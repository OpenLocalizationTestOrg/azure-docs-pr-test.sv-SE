---
title: "Självstudier: Azure Active Directory-integrering med New Relic | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och New Relic."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3186b9a8-f4d8-45e2-ad82-6275f95e7aa6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: 605e85c23a849f70fcc0237361d7a891f716ca3a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-new-relic"></a><span data-ttu-id="b7189-103">Självstudier: Azure Active Directory-integrering med New Relic</span><span class="sxs-lookup"><span data-stu-id="b7189-103">Tutorial: Azure Active Directory integration with New Relic</span></span>

<span data-ttu-id="b7189-104">I kursen får lära du att integrera New Relic med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b7189-104">In this tutorial, you learn how to integrate New Relic with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b7189-105">Integrera New Relic med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b7189-105">Integrating New Relic with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b7189-106">Du kan styra i Azure AD som har åtkomst till New Relic</span><span class="sxs-lookup"><span data-stu-id="b7189-106">You can control in Azure AD who has access to New Relic</span></span>
- <span data-ttu-id="b7189-107">Du kan aktivera användarna att automatiskt hämta loggat in på New Relic (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b7189-107">You can enable your users to automatically get signed-on to New Relic (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b7189-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b7189-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b7189-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b7189-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b7189-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b7189-110">Prerequisites</span></span>

<span data-ttu-id="b7189-111">Om du vill konfigurera Azure AD-integrering med New Relic behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="b7189-111">To configure Azure AD integration with New Relic, you need the following items:</span></span>

- <span data-ttu-id="b7189-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b7189-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b7189-113">Ett New Relic enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="b7189-113">A New Relic single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b7189-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b7189-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b7189-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b7189-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b7189-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b7189-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b7189-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b7189-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b7189-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b7189-118">Scenario description</span></span>
<span data-ttu-id="b7189-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b7189-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b7189-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b7189-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b7189-121">Att lägga till New Relic från galleriet</span><span class="sxs-lookup"><span data-stu-id="b7189-121">Adding New Relic from the gallery</span></span>
2. <span data-ttu-id="b7189-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7189-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-new-relic-from-the-gallery"></a><span data-ttu-id="b7189-123">Att lägga till New Relic från galleriet</span><span class="sxs-lookup"><span data-stu-id="b7189-123">Adding New Relic from the gallery</span></span>
<span data-ttu-id="b7189-124">Du måste lägga till New Relic från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av New Relic i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b7189-124">To configure the integration of New Relic into Azure AD, you need to add New Relic from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b7189-125">**Utför följande steg för att lägga till New Relic från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="b7189-125">**To add New Relic from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b7189-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b7189-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b7189-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="b7189-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b7189-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b7189-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="b7189-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7189-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="b7189-133">I sökrutan skriver **New Relic**.</span><span class="sxs-lookup"><span data-stu-id="b7189-133">In the search box, type **New Relic**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_search.png)

5. <span data-ttu-id="b7189-135">Välj i resultatpanelen **New Relic**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="b7189-135">In the results panel, select **New Relic**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b7189-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7189-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b7189-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med New Relic baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b7189-138">In this section, you configure and test Azure AD single sign-on with New Relic based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b7189-139">Azure AD måste du känna till användaren i New Relic motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="b7189-139">For single sign-on to work, Azure AD needs to know what the counterpart user in New Relic is to a user in Azure AD.</span></span> <span data-ttu-id="b7189-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i New Relic upprättas.</span><span class="sxs-lookup"><span data-stu-id="b7189-140">In other words, a link relationship between an Azure AD user and the related user in New Relic needs to be established.</span></span>

<span data-ttu-id="b7189-141">I New Relic tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="b7189-141">In New Relic, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="b7189-142">Om du vill konfigurera och testa Azure AD enkel inloggning med New Relic, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b7189-142">To configure and test Azure AD single sign-on with New Relic, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b7189-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b7189-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b7189-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b7189-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b7189-145">**[Skapa en testanvändare New Relic](#creating-a-new-relic-test-user)**  – du har en motsvarighet för Britta Simon i New Relic som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="b7189-145">**[Creating a New Relic test user](#creating-a-new-relic-test-user)** - to have a counterpart of Britta Simon in New Relic that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b7189-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b7189-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b7189-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b7189-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b7189-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7189-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b7189-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i New Relic-program.</span><span class="sxs-lookup"><span data-stu-id="b7189-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your New Relic application.</span></span>

<span data-ttu-id="b7189-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med New Relic:**</span><span class="sxs-lookup"><span data-stu-id="b7189-150">**To configure Azure AD single sign-on with New Relic, perform the following steps:**</span></span>

1. <span data-ttu-id="b7189-151">I Azure-portalen på den **New Relic** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b7189-151">In the Azure portal, on the **New Relic** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="b7189-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b7189-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_samlbase.png)

3. <span data-ttu-id="b7189-155">På den **ny Relic domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b7189-155">On the **New Relic Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_url.png)

    <span data-ttu-id="b7189-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.newrelic.com`</span><span class="sxs-lookup"><span data-stu-id="b7189-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.newrelic.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b7189-158">Värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="b7189-158">The value is not real.</span></span> <span data-ttu-id="b7189-159">Uppdatera värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="b7189-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="b7189-160">Kontakta [ny Relic klient supportteamet](https://support.newrelic.com/) värdet hämtas.</span><span class="sxs-lookup"><span data-stu-id="b7189-160">Contact [New Relic Client support team](https://support.newrelic.com/) to get the value.</span></span> 
 
4. <span data-ttu-id="b7189-161">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="b7189-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_certificate.png) 

5. <span data-ttu-id="b7189-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="b7189-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-new-relic-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b7189-165">På den **nya Relic konfigurationen** klickar du på **konfigurera New Relic** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="b7189-165">On the **New Relic Configuration** section, click **Configure New Relic** to open **Configure sign-on** window.</span></span> <span data-ttu-id="b7189-166">Kopiera den **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="b7189-166">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_configure.png) 

7. <span data-ttu-id="b7189-168">I en annan webbläsarfönstret, logga in på ditt **New Relic** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="b7189-168">In a different web browser window, sign on to your **New Relic** company site as administrator.</span></span>

8. <span data-ttu-id="b7189-169">Klicka på menyn högst upp **kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="b7189-169">In the menu on the top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="b7189-170">![Kontoinställningar](./media/active-directory-saas-new-relic-tutorial/ic797036.png "kontoinställningar")</span><span class="sxs-lookup"><span data-stu-id="b7189-170">![Account Settings](./media/active-directory-saas-new-relic-tutorial/ic797036.png "Account Settings")</span></span>

9. <span data-ttu-id="b7189-171">Klicka på den **säkerhet och autentisering** fliken och klicka sedan på den **enkel inloggning** fliken.</span><span class="sxs-lookup"><span data-stu-id="b7189-171">Click the **Security and authentication** tab, and then click the **Single sign on** tab.</span></span>
   
    <span data-ttu-id="b7189-172">![Enkel inloggning](./media/active-directory-saas-new-relic-tutorial/ic797037.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="b7189-172">![Single Sign-On](./media/active-directory-saas-new-relic-tutorial/ic797037.png "Single Sign-On")</span></span>

10. <span data-ttu-id="b7189-173">På sidan SAML dialogrutan utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="b7189-173">On the SAML dialog page, perform the following steps:</span></span>
   
    <span data-ttu-id="b7189-174">![SAML](./media/active-directory-saas-new-relic-tutorial/ic797038.png "SAML")</span><span class="sxs-lookup"><span data-stu-id="b7189-174">![SAML](./media/active-directory-saas-new-relic-tutorial/ic797038.png "SAML")</span></span>
   
   <span data-ttu-id="b7189-175">a.</span><span class="sxs-lookup"><span data-stu-id="b7189-175">a.</span></span> <span data-ttu-id="b7189-176">Klicka på **Välj fil** överföra ditt hämtade Azure Active Directory-certifikat.</span><span class="sxs-lookup"><span data-stu-id="b7189-176">Click **Choose File** to upload your downloaded Azure Active Directory certificate.</span></span>

   <span data-ttu-id="b7189-177">b.</span><span class="sxs-lookup"><span data-stu-id="b7189-177">b.</span></span> <span data-ttu-id="b7189-178">I den **fjärrinloggning URL** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b7189-178">In the **Remote login URL** textbox,  paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="b7189-179">c.</span><span class="sxs-lookup"><span data-stu-id="b7189-179">c.</span></span> <span data-ttu-id="b7189-180">I den **logga ut hamnar URL** textruta klistra in värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b7189-180">In the **Logout landing URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

   <span data-ttu-id="b7189-181">d.</span><span class="sxs-lookup"><span data-stu-id="b7189-181">d.</span></span> <span data-ttu-id="b7189-182">Klicka på **spara mina ändringar**.</span><span class="sxs-lookup"><span data-stu-id="b7189-182">Click **Save my changes**.</span></span>

> [!TIP]
> <span data-ttu-id="b7189-183">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="b7189-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b7189-184">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="b7189-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b7189-185">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b7189-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b7189-186">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b7189-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="b7189-187">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b7189-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="b7189-189">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="b7189-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b7189-190">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b7189-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b7189-192">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b7189-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b7189-194">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7189-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b7189-196">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b7189-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b7189-198">a.</span><span class="sxs-lookup"><span data-stu-id="b7189-198">a.</span></span> <span data-ttu-id="b7189-199">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b7189-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b7189-200">b.</span><span class="sxs-lookup"><span data-stu-id="b7189-200">b.</span></span> <span data-ttu-id="b7189-201">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b7189-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b7189-202">c.</span><span class="sxs-lookup"><span data-stu-id="b7189-202">c.</span></span> <span data-ttu-id="b7189-203">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b7189-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b7189-204">d.</span><span class="sxs-lookup"><span data-stu-id="b7189-204">d.</span></span> <span data-ttu-id="b7189-205">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b7189-205">Click **Create**.</span></span>
 
### <a name="creating-a-new-relic-test-user"></a><span data-ttu-id="b7189-206">Skapa en New Relic testanvändare</span><span class="sxs-lookup"><span data-stu-id="b7189-206">Creating a New Relic test user</span></span>

<span data-ttu-id="b7189-207">För att aktivera Azure Active Directory-användare kan logga in till New Relic etableras de till New Relic.</span><span class="sxs-lookup"><span data-stu-id="b7189-207">In order to enable Azure Active Directory users to log in to New Relic, they must be provisioned into New Relic.</span></span> <span data-ttu-id="b7189-208">New Relic är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="b7189-208">In the case of New Relic, provisioning is a manual task.</span></span>

<span data-ttu-id="b7189-209">**Utför följande steg för att etablera ett användarkonto till New Relic:**</span><span class="sxs-lookup"><span data-stu-id="b7189-209">**To provision a user account to New Relic, perform the following steps:**</span></span>

1. <span data-ttu-id="b7189-210">Logga in på ditt **New Relic** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="b7189-210">Log in to your **New Relic** company site as administrator.</span></span>

2. <span data-ttu-id="b7189-211">Klicka på menyn högst upp **kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="b7189-211">In the menu on the top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="b7189-212">![Kontoinställningar](./media/active-directory-saas-new-relic-tutorial/ic797040.png "kontoinställningar")</span><span class="sxs-lookup"><span data-stu-id="b7189-212">![Account Settings](./media/active-directory-saas-new-relic-tutorial/ic797040.png "Account Settings")</span></span>

3. <span data-ttu-id="b7189-213">I den **konto** vänster, klicka på **sammanfattning**, och klicka sedan på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="b7189-213">In the **Account** pane on the left side, click **Summary**, and then click **Add user**.</span></span>
   
    <span data-ttu-id="b7189-214">![Kontoinställningar](./media/active-directory-saas-new-relic-tutorial/ic797041.png "kontoinställningar")</span><span class="sxs-lookup"><span data-stu-id="b7189-214">![Account Settings](./media/active-directory-saas-new-relic-tutorial/ic797041.png "Account Settings")</span></span>

4. <span data-ttu-id="b7189-215">På den **aktiva användare** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b7189-215">On the **Active users** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="b7189-216">![Aktiva användare](./media/active-directory-saas-new-relic-tutorial/ic797042.png "aktiva användare")</span><span class="sxs-lookup"><span data-stu-id="b7189-216">![Active Users](./media/active-directory-saas-new-relic-tutorial/ic797042.png "Active Users")</span></span>
   
    <span data-ttu-id="b7189-217">a.</span><span class="sxs-lookup"><span data-stu-id="b7189-217">a.</span></span> <span data-ttu-id="b7189-218">I den **e-post** textruta, ange en giltig Azure Active Directory-användare som du vill etablera e-postadress.</span><span class="sxs-lookup"><span data-stu-id="b7189-218">In the **Email** textbox, type the email address of a valid Azure Active Directory user you want to provision.</span></span>

    <span data-ttu-id="b7189-219">b.</span><span class="sxs-lookup"><span data-stu-id="b7189-219">b.</span></span> <span data-ttu-id="b7189-220">Som **rollen** Välj **användaren**.</span><span class="sxs-lookup"><span data-stu-id="b7189-220">As **Role** select **User**.</span></span>

    <span data-ttu-id="b7189-221">c.</span><span class="sxs-lookup"><span data-stu-id="b7189-221">c.</span></span> <span data-ttu-id="b7189-222">Klicka på **Lägg till användaren**.</span><span class="sxs-lookup"><span data-stu-id="b7189-222">Click **Add this user**.</span></span>

>[!NOTE]
><span data-ttu-id="b7189-223">Du kan använda något annat New Relic användarens konto skapas verktyg eller API: er som tillhandahålls av New Relic etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="b7189-223">You can use any other New Relic user account creation tools or APIs provided by New Relic to provision AAD user accounts.</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b7189-224">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="b7189-224">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b7189-225">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till New Relic.</span><span class="sxs-lookup"><span data-stu-id="b7189-225">In this section, you enable Britta Simon to use Azure single sign-on by granting access to New Relic.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="b7189-227">**Om du vill tilldela New Relic Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b7189-227">**To assign Britta Simon to New Relic, perform the following steps:**</span></span>

1. <span data-ttu-id="b7189-228">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b7189-228">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b7189-230">Välj i listan med program **New Relic**.</span><span class="sxs-lookup"><span data-stu-id="b7189-230">In the applications list, select **New Relic**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_app.png) 

3. <span data-ttu-id="b7189-232">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b7189-232">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="b7189-234">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="b7189-234">Click **Add** button.</span></span> <span data-ttu-id="b7189-235">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7189-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="b7189-237">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="b7189-237">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b7189-238">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7189-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b7189-239">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7189-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b7189-240">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7189-240">Testing single sign-on</span></span>

<span data-ttu-id="b7189-241">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="b7189-241">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b7189-242">När du klickar på panelen New Relic på åtkomstpanelen du bör få automatiskt loggat in på ditt New Relic-program.</span><span class="sxs-lookup"><span data-stu-id="b7189-242">When you click the New Relic tile in the Access Panel, you should get automatically signed-on to your New Relic application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b7189-243">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b7189-243">Additional resources</span></span>

* [<span data-ttu-id="b7189-244">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b7189-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b7189-245">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b7189-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_203.png

