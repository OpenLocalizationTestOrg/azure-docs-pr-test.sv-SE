---
title: "Självstudier: Azure Active Directory-integrering med Lucidchart | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Lucidchart."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1068d364-11f3-43b5-bd6d-26f00ecd5baa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 2dea669f03c893632c08d30feeb3173efc2d8243
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lucidchart"></a><span data-ttu-id="e2776-103">Självstudier: Azure Active Directory-integrering med Lucidchart</span><span class="sxs-lookup"><span data-stu-id="e2776-103">Tutorial: Azure Active Directory integration with Lucidchart</span></span>

<span data-ttu-id="e2776-104">I kursen får lära du att integrera Lucidchart med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e2776-104">In this tutorial, you learn how to integrate Lucidchart with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e2776-105">Integrera Lucidchart med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e2776-105">Integrating Lucidchart with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e2776-106">Du kan styra i Azure AD som har åtkomst till Lucidchart</span><span class="sxs-lookup"><span data-stu-id="e2776-106">You can control in Azure AD who has access to Lucidchart</span></span>
- <span data-ttu-id="e2776-107">Du kan aktivera användarna att automatiskt hämta loggat in på Lucidchart (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e2776-107">You can enable your users to automatically get signed-on to Lucidchart (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e2776-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e2776-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e2776-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e2776-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e2776-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e2776-110">Prerequisites</span></span>

<span data-ttu-id="e2776-111">För att konfigurera Azure AD-integrering med Lucidchart, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="e2776-111">To configure Azure AD integration with Lucidchart, you need the following items:</span></span>

- <span data-ttu-id="e2776-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e2776-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e2776-113">En Lucidchart enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="e2776-113">A Lucidchart single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e2776-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e2776-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e2776-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e2776-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e2776-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e2776-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e2776-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e2776-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e2776-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e2776-118">Scenario description</span></span>
<span data-ttu-id="e2776-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e2776-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e2776-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e2776-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e2776-121">Att lägga till Lucidchart från galleriet</span><span class="sxs-lookup"><span data-stu-id="e2776-121">Adding Lucidchart from the gallery</span></span>
2. <span data-ttu-id="e2776-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e2776-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lucidchart-from-the-gallery"></a><span data-ttu-id="e2776-123">Att lägga till Lucidchart från galleriet</span><span class="sxs-lookup"><span data-stu-id="e2776-123">Adding Lucidchart from the gallery</span></span>
<span data-ttu-id="e2776-124">Du måste lägga till Lucidchart från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Lucidchart i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e2776-124">To configure the integration of Lucidchart into Azure AD, you need to add Lucidchart from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e2776-125">**Utför följande steg för att lägga till Lucidchart från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="e2776-125">**To add Lucidchart from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e2776-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e2776-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e2776-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e2776-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e2776-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e2776-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="e2776-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e2776-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="e2776-133">I sökrutan skriver **Lucidchart**.</span><span class="sxs-lookup"><span data-stu-id="e2776-133">In the search box, type **Lucidchart**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_search.png)

5. <span data-ttu-id="e2776-135">Välj i resultatpanelen **Lucidchart**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="e2776-135">In the results panel, select **Lucidchart**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e2776-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e2776-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e2776-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Lucidchart baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e2776-138">In this section, you configure and test Azure AD single sign-on with Lucidchart based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e2776-139">Azure AD måste du känna till användaren i Lucidchart motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="e2776-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Lucidchart is to a user in Azure AD.</span></span> <span data-ttu-id="e2776-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Lucidchart upprättas.</span><span class="sxs-lookup"><span data-stu-id="e2776-140">In other words, a link relationship between an Azure AD user and the related user in Lucidchart needs to be established.</span></span>

<span data-ttu-id="e2776-141">I Lucidchart, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="e2776-141">In Lucidchart, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e2776-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Lucidchart, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e2776-142">To configure and test Azure AD single sign-on with Lucidchart, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e2776-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e2776-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e2776-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e2776-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e2776-145">**[Skapa en testanvändare Lucidchart](#creating-a-lucidchart-test-user)**  – du har en motsvarighet för Britta Simon i Lucidchart som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="e2776-145">**[Creating a Lucidchart test user](#creating-a-lucidchart-test-user)** - to have a counterpart of Britta Simon in Lucidchart that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e2776-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e2776-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e2776-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e2776-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e2776-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e2776-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e2776-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Lucidchart program.</span><span class="sxs-lookup"><span data-stu-id="e2776-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Lucidchart application.</span></span>

<span data-ttu-id="e2776-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Lucidchart:**</span><span class="sxs-lookup"><span data-stu-id="e2776-150">**To configure Azure AD single sign-on with Lucidchart, perform the following steps:**</span></span>

1. <span data-ttu-id="e2776-151">I Azure-portalen på den **Lucidchart** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e2776-151">In the Azure portal, on the **Lucidchart** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="e2776-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e2776-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_samlbase.png)

3. <span data-ttu-id="e2776-155">På den **Lucidchart domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e2776-155">On the **Lucidchart Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_url.png)

    <span data-ttu-id="e2776-157">I den **inloggnings-URL** textruta Skriv en URL som:`https://chart2.office.lucidchart.com/saml/sso/azure`</span><span class="sxs-lookup"><span data-stu-id="e2776-157">In the **Sign-on URL** textbox, type a URL as: `https://chart2.office.lucidchart.com/saml/sso/azure`</span></span>

4. <span data-ttu-id="e2776-158">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="e2776-158">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_certificate.png) 

5. <span data-ttu-id="e2776-160">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e2776-160">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lucidchart-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e2776-162">Logga in på webbplatsen Lucidchart företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="e2776-162">In a different web browser window, log into your Lucidchart company site as an administrator.</span></span>

7. <span data-ttu-id="e2776-163">Klicka på menyn högst upp **Team**.</span><span class="sxs-lookup"><span data-stu-id="e2776-163">In the menu on the top, click **Team**.</span></span>
   
    <span data-ttu-id="e2776-164">![Team](./media/active-directory-saas-lucidchart-tutorial/ic791190.png "Team")</span><span class="sxs-lookup"><span data-stu-id="e2776-164">![Team](./media/active-directory-saas-lucidchart-tutorial/ic791190.png "Team")</span></span>

8. <span data-ttu-id="e2776-165">Klicka på **program \> hantera SAML**.</span><span class="sxs-lookup"><span data-stu-id="e2776-165">Click **Applications \> Manage SAML**.</span></span>
   
    <span data-ttu-id="e2776-166">![Hantera SAML](./media/active-directory-saas-lucidchart-tutorial/ic791191.png "hantera SAML")</span><span class="sxs-lookup"><span data-stu-id="e2776-166">![Manage SAML](./media/active-directory-saas-lucidchart-tutorial/ic791191.png "Manage SAML")</span></span>

9. <span data-ttu-id="e2776-167">På den **SAML autentiseringsinställningar** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e2776-167">On the **SAML Authentication Settings** dialog page, perform the following steps:</span></span>
   
    <span data-ttu-id="e2776-168">a.</span><span class="sxs-lookup"><span data-stu-id="e2776-168">a.</span></span> <span data-ttu-id="e2776-169">Välj **aktivera SAML-autentisering**, och klicka sedan på **valfritt**.</span><span class="sxs-lookup"><span data-stu-id="e2776-169">Select **Enable SAML Authentication**, and then click **Optional**.</span></span>

    <span data-ttu-id="e2776-170">![Inställningar för SAML-autentisering](./media/active-directory-saas-lucidchart-tutorial/ic791192.png "inställningar för SAML-autentisering")</span><span class="sxs-lookup"><span data-stu-id="e2776-170">![SAML Authentication Settings](./media/active-directory-saas-lucidchart-tutorial/ic791192.png "SAML Authentication Settings")</span></span>
 
    <span data-ttu-id="e2776-171">b.</span><span class="sxs-lookup"><span data-stu-id="e2776-171">b.</span></span> <span data-ttu-id="e2776-172">I den **domän** textruta Skriv din domän och klicka sedan på **ändra certifikatet**.</span><span class="sxs-lookup"><span data-stu-id="e2776-172">In the **Domain** textbox, type your domain, and then click **Change Certificate**.</span></span>

    <span data-ttu-id="e2776-173">![Ändra certifikat](./media/active-directory-saas-lucidchart-tutorial/ic791193.png "ändra certifikat")</span><span class="sxs-lookup"><span data-stu-id="e2776-173">![Change Certificate](./media/active-directory-saas-lucidchart-tutorial/ic791193.png "Change Certificate")</span></span>
 
    <span data-ttu-id="e2776-174">c.</span><span class="sxs-lookup"><span data-stu-id="e2776-174">c.</span></span> <span data-ttu-id="e2776-175">Öppna metadatafilen hämtade, kopiera innehållet och klistrar in det i den **överföra Metadata** textruta.</span><span class="sxs-lookup"><span data-stu-id="e2776-175">Open your downloaded metadata file, copy the content, and then paste it into the **Upload Metadata** textbox.</span></span>

    <span data-ttu-id="e2776-176">![Överföra Metadata](./media/active-directory-saas-lucidchart-tutorial/ic791194.png "överföra Metadata")</span><span class="sxs-lookup"><span data-stu-id="e2776-176">![Upload Metadata](./media/active-directory-saas-lucidchart-tutorial/ic791194.png "Upload Metadata")</span></span>
 
    <span data-ttu-id="e2776-177">d.</span><span class="sxs-lookup"><span data-stu-id="e2776-177">d.</span></span> <span data-ttu-id="e2776-178">Välj **automatiskt lägga till nya användare till teamet**, och klicka sedan på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="e2776-178">Select **Automatically Add new users to the team**, and then click **Save changes**.</span></span>

    <span data-ttu-id="e2776-179">![Spara ändringarna](./media/active-directory-saas-lucidchart-tutorial/ic791195.png "spara ändringar")</span><span class="sxs-lookup"><span data-stu-id="e2776-179">![Save Changes](./media/active-directory-saas-lucidchart-tutorial/ic791195.png "Save Changes")</span></span>

> [!TIP]
> <span data-ttu-id="e2776-180">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="e2776-180">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e2776-181">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="e2776-181">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e2776-182">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e2776-182">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e2776-183">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e2776-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="e2776-184">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e2776-184">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="e2776-186">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="e2776-186">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e2776-187">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e2776-187">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e2776-189">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e2776-189">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e2776-191">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e2776-191">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e2776-193">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e2776-193">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e2776-195">a.</span><span class="sxs-lookup"><span data-stu-id="e2776-195">a.</span></span> <span data-ttu-id="e2776-196">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e2776-196">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e2776-197">b.</span><span class="sxs-lookup"><span data-stu-id="e2776-197">b.</span></span> <span data-ttu-id="e2776-198">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e2776-198">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e2776-199">c.</span><span class="sxs-lookup"><span data-stu-id="e2776-199">c.</span></span> <span data-ttu-id="e2776-200">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e2776-200">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e2776-201">d.</span><span class="sxs-lookup"><span data-stu-id="e2776-201">d.</span></span> <span data-ttu-id="e2776-202">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e2776-202">Click **Create**.</span></span>
 
### <a name="creating-a-lucidchart-test-user"></a><span data-ttu-id="e2776-203">Skapa en testanvändare Lucidchart</span><span class="sxs-lookup"><span data-stu-id="e2776-203">Creating a Lucidchart test user</span></span>

<span data-ttu-id="e2776-204">Det finns inga uppgiften som du kan konfigurera användaretablering till Lucidchart.</span><span class="sxs-lookup"><span data-stu-id="e2776-204">There is no action item for you to configure user provisioning to Lucidchart.</span></span>  <span data-ttu-id="e2776-205">När en tilldelad användare försöker logga in med hjälp av åtkomstpanelen Lucidchart kontrollerar Lucidchart om användaren finns.</span><span class="sxs-lookup"><span data-stu-id="e2776-205">When an assigned user tries to log into Lucidchart using the access panel, Lucidchart checks whether the user exists.</span></span>  

<span data-ttu-id="e2776-206">Om det finns inget användarkonto ännu, skapas den automatiskt av Lucidchart.</span><span class="sxs-lookup"><span data-stu-id="e2776-206">If there is no user account available yet, it is automatically created by Lucidchart.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e2776-207">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="e2776-207">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e2776-208">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Lucidchart.</span><span class="sxs-lookup"><span data-stu-id="e2776-208">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Lucidchart.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="e2776-210">**Om du vill tilldela Lucidchart Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e2776-210">**To assign Britta Simon to Lucidchart, perform the following steps:**</span></span>

1. <span data-ttu-id="e2776-211">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e2776-211">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e2776-213">Välj i listan med program **Lucidchart**.</span><span class="sxs-lookup"><span data-stu-id="e2776-213">In the applications list, select **Lucidchart**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_app.png) 

3. <span data-ttu-id="e2776-215">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e2776-215">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="e2776-217">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e2776-217">Click **Add** button.</span></span> <span data-ttu-id="e2776-218">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e2776-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="e2776-220">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="e2776-220">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e2776-221">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e2776-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e2776-222">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e2776-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e2776-223">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e2776-223">Testing single sign-on</span></span>

<span data-ttu-id="e2776-224">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e2776-224">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="e2776-225">När du klickar på panelen Lucidchart på åtkomstpanelen du bör få automatiskt loggat in på ditt Lucidchart program.</span><span class="sxs-lookup"><span data-stu-id="e2776-225">When you click the Lucidchart tile in the Access Panel, you should get automatically signed-on to your Lucidchart application.</span></span>
<span data-ttu-id="e2776-226">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e2776-226">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e2776-227">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e2776-227">Additional resources</span></span>

* [<span data-ttu-id="e2776-228">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e2776-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e2776-229">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e2776-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_203.png

