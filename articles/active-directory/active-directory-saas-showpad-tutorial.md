---
title: "Självstudier: Azure Active Directory-integrering med Showpad | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Showpad."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 48b6bee0-dbc5-4863-964d-75b25e517741
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: c8b39c9215675d8073f896f934339e7cd55334cc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-showpad"></a><span data-ttu-id="64d0c-103">Självstudier: Azure Active Directory-integrering med Showpad</span><span class="sxs-lookup"><span data-stu-id="64d0c-103">Tutorial: Azure Active Directory integration with Showpad</span></span>

<span data-ttu-id="64d0c-104">I kursen får lära du att integrera Showpad med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="64d0c-104">In this tutorial, you learn how to integrate Showpad with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="64d0c-105">Integrera Showpad med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="64d0c-105">Integrating Showpad with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="64d0c-106">Du kan styra i Azure AD som har åtkomst till Showpad</span><span class="sxs-lookup"><span data-stu-id="64d0c-106">You can control in Azure AD who has access to Showpad</span></span>
- <span data-ttu-id="64d0c-107">Du kan aktivera användarna att automatiskt hämta loggat in på Showpad (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="64d0c-107">You can enable your users to automatically get signed-on to Showpad (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="64d0c-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="64d0c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="64d0c-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="64d0c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64d0c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="64d0c-110">Prerequisites</span></span>

<span data-ttu-id="64d0c-111">För att konfigurera Azure AD-integrering med Showpad, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="64d0c-111">To configure Azure AD integration with Showpad, you need the following items:</span></span>

- <span data-ttu-id="64d0c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="64d0c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="64d0c-113">En Showpad enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="64d0c-113">A Showpad single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="64d0c-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="64d0c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="64d0c-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="64d0c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="64d0c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="64d0c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="64d0c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="64d0c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="64d0c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="64d0c-118">Scenario description</span></span>
<span data-ttu-id="64d0c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="64d0c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="64d0c-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="64d0c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="64d0c-121">Att lägga till Showpad från galleriet</span><span class="sxs-lookup"><span data-stu-id="64d0c-121">Adding Showpad from the gallery</span></span>
2. <span data-ttu-id="64d0c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="64d0c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-showpad-from-the-gallery"></a><span data-ttu-id="64d0c-123">Att lägga till Showpad från galleriet</span><span class="sxs-lookup"><span data-stu-id="64d0c-123">Adding Showpad from the gallery</span></span>

<span data-ttu-id="64d0c-124">Du måste lägga till Showpad från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Showpad i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="64d0c-124">To configure the integration of Showpad into Azure AD, you need to add Showpad from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="64d0c-125">**Utför följande steg för att lägga till Showpad från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="64d0c-125">**To add Showpad from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="64d0c-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="64d0c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="64d0c-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="64d0c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="64d0c-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="64d0c-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="64d0c-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="64d0c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="64d0c-133">I sökrutan skriver **Showpad**.</span><span class="sxs-lookup"><span data-stu-id="64d0c-133">In the search box, type **Showpad**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_search.png)

5. <span data-ttu-id="64d0c-135">Välj i resultatpanelen **Showpad**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="64d0c-135">In the results panel, select **Showpad**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="64d0c-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="64d0c-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="64d0c-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Showpad baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="64d0c-138">In this section, you configure and test Azure AD single sign-on with Showpad based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="64d0c-139">Azure AD måste du känna till användaren i Showpad motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="64d0c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Showpad is to a user in Azure AD.</span></span> <span data-ttu-id="64d0c-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Showpad upprättas.</span><span class="sxs-lookup"><span data-stu-id="64d0c-140">In other words, a link relationship between an Azure AD user and the related user in Showpad needs to be established.</span></span>

<span data-ttu-id="64d0c-141">I Showpad, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="64d0c-141">In Showpad, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="64d0c-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Showpad, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="64d0c-142">To configure and test Azure AD single sign-on with Showpad, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="64d0c-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="64d0c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="64d0c-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="64d0c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="64d0c-145">**[Skapa en testanvändare Showpad](#creating-a-showpad-test-user)**  – du har en motsvarighet för Britta Simon i Showpad som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="64d0c-145">**[Creating a Showpad test user](#creating-a-showpad-test-user)** - to have a counterpart of Britta Simon in Showpad that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="64d0c-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="64d0c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="64d0c-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="64d0c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="64d0c-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="64d0c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="64d0c-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Showpad program.</span><span class="sxs-lookup"><span data-stu-id="64d0c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Showpad application.</span></span>

<span data-ttu-id="64d0c-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Showpad:**</span><span class="sxs-lookup"><span data-stu-id="64d0c-150">**To configure Azure AD single sign-on with Showpad, perform the following steps:**</span></span>

1. <span data-ttu-id="64d0c-151">I Azure-portalen på den **Showpad** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="64d0c-151">In the Azure portal, on the **Showpad** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="64d0c-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="64d0c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_samlbase.png)

3. <span data-ttu-id="64d0c-155">På den **Showpad domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="64d0c-155">On the **Showpad Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_url.png)

    <span data-ttu-id="64d0c-157">a.</span><span class="sxs-lookup"><span data-stu-id="64d0c-157">a.</span></span> <span data-ttu-id="64d0c-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<comapany-name>.showpad.biz/login`</span><span class="sxs-lookup"><span data-stu-id="64d0c-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<comapany-name>.showpad.biz/login`</span></span>

    <span data-ttu-id="64d0c-159">b.</span><span class="sxs-lookup"><span data-stu-id="64d0c-159">b.</span></span> <span data-ttu-id="64d0c-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<company-name>.showpad.biz`</span><span class="sxs-lookup"><span data-stu-id="64d0c-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<company-name>.showpad.biz`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="64d0c-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="64d0c-161">These values are not real.</span></span> <span data-ttu-id="64d0c-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="64d0c-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="64d0c-163">Kontakta [Showpad supportteamet](https://help.showpad.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="64d0c-163">Contact [Showpad support team](https://help.showpad.com) to get these values.</span></span> 
 


4. <span data-ttu-id="64d0c-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="64d0c-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_certificate.png) 

5. <span data-ttu-id="64d0c-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="64d0c-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-showpad-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="64d0c-168">Inloggning till Showpad-klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="64d0c-168">Sign-on to your Showpad tenant as an administrator.</span></span>

7. <span data-ttu-id="64d0c-169">Klicka på menyn högst upp i **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="64d0c-169">In the menu on the top, click the **Settings**.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_001.png) 

8. <span data-ttu-id="64d0c-171">Gå till ”**enkel inloggning**” och klicka på ”**aktivera**”.</span><span class="sxs-lookup"><span data-stu-id="64d0c-171">Navigate to "**Single Sign-On**" and click "**Enable**."</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_002.png)

9. <span data-ttu-id="64d0c-173">På den **lägga till en tjänst för SAML 2.0** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="64d0c-173">On the **Add a SAML 2.0 Service** dialog, perform the following steps:</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_003.png) 
   
    <span data-ttu-id="64d0c-175">a.</span><span class="sxs-lookup"><span data-stu-id="64d0c-175">a.</span></span> <span data-ttu-id="64d0c-176">I den **namn** textruta skriver du namnet på ID-providern (till exempel: företagets namn).</span><span class="sxs-lookup"><span data-stu-id="64d0c-176">In the **Name** textbox, type the name of Identifier Provider (for example: your company name).</span></span>
   
    <span data-ttu-id="64d0c-177">b.</span><span class="sxs-lookup"><span data-stu-id="64d0c-177">b.</span></span> <span data-ttu-id="64d0c-178">Som **Metadatakälla**väljer **XML**.</span><span class="sxs-lookup"><span data-stu-id="64d0c-178">As **Metadata Source**, select **XML**.</span></span>
   
    <span data-ttu-id="64d0c-179">c.</span><span class="sxs-lookup"><span data-stu-id="64d0c-179">c.</span></span> <span data-ttu-id="64d0c-180">Kopiera innehållet i metadata XML-fil som du har hämtat från Azure-portalen och klistrar in det i den **XML-Metadata för** textruta.</span><span class="sxs-lookup"><span data-stu-id="64d0c-180">Copy the content of metadata XML file, which you have downloaded from the Azure portal, and then paste it into the **Metadata XML** textbox.</span></span>
   
    <span data-ttu-id="64d0c-181">d.</span><span class="sxs-lookup"><span data-stu-id="64d0c-181">d.</span></span> <span data-ttu-id="64d0c-182">Välj **automatiskt etablera konton för nya användare när de loggar in**.</span><span class="sxs-lookup"><span data-stu-id="64d0c-182">Select **Auto-provision accounts for new users when they log in**.</span></span>
   
    <span data-ttu-id="64d0c-183">e.</span><span class="sxs-lookup"><span data-stu-id="64d0c-183">e.</span></span> <span data-ttu-id="64d0c-184">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="64d0c-184">Click **Submit**.</span></span>

> [!TIP]
> <span data-ttu-id="64d0c-185">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="64d0c-185">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="64d0c-186">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="64d0c-186">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="64d0c-187">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="64d0c-187">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="64d0c-188">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="64d0c-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="64d0c-189">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="64d0c-189">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="64d0c-191">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="64d0c-191">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="64d0c-192">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="64d0c-192">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="64d0c-194">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="64d0c-194">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="64d0c-196">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="64d0c-196">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="64d0c-198">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="64d0c-198">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="64d0c-200">a.</span><span class="sxs-lookup"><span data-stu-id="64d0c-200">a.</span></span> <span data-ttu-id="64d0c-201">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="64d0c-201">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="64d0c-202">b.</span><span class="sxs-lookup"><span data-stu-id="64d0c-202">b.</span></span> <span data-ttu-id="64d0c-203">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="64d0c-203">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="64d0c-204">c.</span><span class="sxs-lookup"><span data-stu-id="64d0c-204">c.</span></span> <span data-ttu-id="64d0c-205">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="64d0c-205">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="64d0c-206">d.</span><span class="sxs-lookup"><span data-stu-id="64d0c-206">d.</span></span> <span data-ttu-id="64d0c-207">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="64d0c-207">Click **Create**.</span></span>
 
### <a name="creating-a-showpad-test-user"></a><span data-ttu-id="64d0c-208">Skapa en testanvändare Showpad</span><span class="sxs-lookup"><span data-stu-id="64d0c-208">Creating a Showpad test user</span></span>

<span data-ttu-id="64d0c-209">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Showpad.</span><span class="sxs-lookup"><span data-stu-id="64d0c-209">The objective of this section is to create a user called Britta Simon in Showpad.</span></span> 

<span data-ttu-id="64d0c-210">Showpad stöder just-in-time-etablering.</span><span class="sxs-lookup"><span data-stu-id="64d0c-210">Showpad supports just-in-time provisioning.</span></span> <span data-ttu-id="64d0c-211">Du har aktiverat allokering i  **[konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**.</span><span class="sxs-lookup"><span data-stu-id="64d0c-211">You have enabled provisioning in **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**.</span></span> 

<span data-ttu-id="64d0c-212">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="64d0c-212">There is no action item for you in this section.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="64d0c-213">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="64d0c-213">Assigning the Azure AD test user</span></span>

<span data-ttu-id="64d0c-214">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Showpad.</span><span class="sxs-lookup"><span data-stu-id="64d0c-214">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Showpad.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="64d0c-216">**Om du vill tilldela Showpad Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="64d0c-216">**To assign Britta Simon to Showpad, perform the following steps:**</span></span>

1. <span data-ttu-id="64d0c-217">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="64d0c-217">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="64d0c-219">Välj i listan med program **Showpad**.</span><span class="sxs-lookup"><span data-stu-id="64d0c-219">In the applications list, select **Showpad**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_app.png) 

3. <span data-ttu-id="64d0c-221">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="64d0c-221">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="64d0c-223">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="64d0c-223">Click **Add** button.</span></span> <span data-ttu-id="64d0c-224">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="64d0c-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="64d0c-226">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="64d0c-226">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="64d0c-227">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="64d0c-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="64d0c-228">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="64d0c-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="64d0c-229">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="64d0c-229">Testing single sign-on</span></span>

<span data-ttu-id="64d0c-230">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="64d0c-230">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="64d0c-231">När du klickar på panelen Showpad på åtkomstpanelen du bör få automatiskt loggat in på Showpad program.</span><span class="sxs-lookup"><span data-stu-id="64d0c-231">When you click the Showpad tile in the Access Panel, you should get automatically signed-on to Showpad application.</span></span>
<span data-ttu-id="64d0c-232">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="64d0c-232">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="64d0c-233">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="64d0c-233">Additional resources</span></span>

* [<span data-ttu-id="64d0c-234">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="64d0c-234">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="64d0c-235">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="64d0c-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_203.png

