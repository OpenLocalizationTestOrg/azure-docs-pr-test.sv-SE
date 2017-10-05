---
title: "Självstudier: Azure Active Directory-integrering med Microsoft instruktioner | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och anvisningarna på Microsoft."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e0c8986f-2acd-418d-a306-437abc44b640
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: f9c068c71eb00a4c779c91c8ee0f0dc9d6ba85ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a><span data-ttu-id="09bcc-103">Självstudier: Azure Active Directory-integrering med Microsoft instruktioner</span><span class="sxs-lookup"><span data-stu-id="09bcc-103">Tutorial: Azure Active Directory integration with Directions on Microsoft</span></span>

<span data-ttu-id="09bcc-104">I kursen får lära du att integrera riktningar i Microsoft Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="09bcc-104">In this tutorial, you learn how to integrate Directions on Microsoft with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="09bcc-105">Integrera anvisningarna på Microsoft med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="09bcc-105">Integrating Directions on Microsoft with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="09bcc-106">Du kan styra i Azure AD som har åtkomst till anvisningarna på Microsoft</span><span class="sxs-lookup"><span data-stu-id="09bcc-106">You can control in Azure AD who has access to Directions on Microsoft</span></span>
- <span data-ttu-id="09bcc-107">Du kan aktivera användarna att automatiskt hämta loggat in på riktningar i Microsoft (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="09bcc-107">You can enable your users to automatically get signed-on to Directions on Microsoft (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="09bcc-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="09bcc-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="09bcc-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="09bcc-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="09bcc-110">Krav</span><span class="sxs-lookup"><span data-stu-id="09bcc-110">Prerequisites</span></span>

<span data-ttu-id="09bcc-111">För att konfigurera Azure AD-integrering med Microsoft instruktioner, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="09bcc-111">To configure Azure AD integration with Directions on Microsoft, you need the following items:</span></span>

- <span data-ttu-id="09bcc-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="09bcc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="09bcc-113">En anvisningarna på Microsoft enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="09bcc-113">A Directions on Microsoft single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="09bcc-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="09bcc-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="09bcc-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="09bcc-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="09bcc-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="09bcc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="09bcc-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="09bcc-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="09bcc-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="09bcc-118">Scenario description</span></span>
<span data-ttu-id="09bcc-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="09bcc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="09bcc-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="09bcc-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="09bcc-121">Lägger till anvisningarna på Microsoft från galleriet</span><span class="sxs-lookup"><span data-stu-id="09bcc-121">Adding Directions on Microsoft from the gallery</span></span>
2. <span data-ttu-id="09bcc-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="09bcc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-directions-on-microsoft-from-the-gallery"></a><span data-ttu-id="09bcc-123">Lägger till anvisningarna på Microsoft från galleriet</span><span class="sxs-lookup"><span data-stu-id="09bcc-123">Adding Directions on Microsoft from the gallery</span></span>
<span data-ttu-id="09bcc-124">Du måste lägga till anvisningarna på Microsoft från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av anvisningar i Microsoft till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="09bcc-124">To configure the integration of Directions on Microsoft into Azure AD, you need to add Directions on Microsoft from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="09bcc-125">**Utför följande steg för att lägga till anvisningarna på Microsoft från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="09bcc-125">**To add Directions on Microsoft from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="09bcc-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="09bcc-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="09bcc-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="09bcc-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="09bcc-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="09bcc-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="09bcc-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="09bcc-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="09bcc-133">I sökrutan skriver **riktningar i Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="09bcc-133">In the search box, type **Directions on Microsoft**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_search.png)

5. <span data-ttu-id="09bcc-135">Välj i resultatpanelen **riktningar i Microsoft**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="09bcc-135">In the results panel, select **Directions on Microsoft**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="09bcc-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="09bcc-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="09bcc-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med instruktioner Microsoft utifrån en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="09bcc-138">In this section, you configure and test Azure AD single sign-on with Directions on Microsoft based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="09bcc-139">Azure AD måste du känna till motsvarande användaren i anvisningarna i Microsoft till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="09bcc-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Directions on Microsoft is to a user in Azure AD.</span></span> <span data-ttu-id="09bcc-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i anvisningarna i Microsoft upprättas.</span><span class="sxs-lookup"><span data-stu-id="09bcc-140">In other words, a link relationship between an Azure AD user and the related user in Directions on Microsoft needs to be established.</span></span>

<span data-ttu-id="09bcc-141">I anvisningarna på Microsoft, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="09bcc-141">In Directions on Microsoft, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="09bcc-142">För att konfigurera och testa Azure AD enkel inloggning med Microsoft instruktioner, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="09bcc-142">To configure and test Azure AD single sign-on with Directions on Microsoft, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="09bcc-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="09bcc-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="09bcc-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="09bcc-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="09bcc-145">**[Skapa en anvisningarna på Microsoft testanvändare](#creating-a-directions-on-microsoft-test-user)**  – du har en motsvarighet för Britta Simon i anvisningarna på Microsoft som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="09bcc-145">**[Creating a Directions on Microsoft test user](#creating-a-directions-on-microsoft-test-user)** - to have a counterpart of Britta Simon in Directions on Microsoft that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="09bcc-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="09bcc-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="09bcc-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="09bcc-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="09bcc-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="09bcc-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="09bcc-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din riktningar på Microsoft-program.</span><span class="sxs-lookup"><span data-stu-id="09bcc-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Directions on Microsoft application.</span></span>

<span data-ttu-id="09bcc-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med instruktionerna på Microsoft:**</span><span class="sxs-lookup"><span data-stu-id="09bcc-150">**To configure Azure AD single sign-on with Directions on Microsoft, perform the following steps:**</span></span>

1. <span data-ttu-id="09bcc-151">I Azure-portalen på den **riktningar i Microsoft** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="09bcc-151">In the Azure portal, on the **Directions on Microsoft** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="09bcc-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="09bcc-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_samlbase.png)

3. <span data-ttu-id="09bcc-155">På den **anvisningarna på URL: er och Microsoft Domain** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="09bcc-155">On the **Directions on Microsoft Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_url.png)

    <span data-ttu-id="09bcc-157">a.</span><span class="sxs-lookup"><span data-stu-id="09bcc-157">a.</span></span> <span data-ttu-id="09bcc-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="09bcc-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>
    |  |
    | --- |
    | `https://www.directionsonmicrosoft.com/user/login` |
    | `https://<subdomain>.devcloud.acquia-sites.com/<companyname>` |

    <span data-ttu-id="09bcc-159">b.</span><span class="sxs-lookup"><span data-stu-id="09bcc-159">b.</span></span> <span data-ttu-id="09bcc-160">I den **identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="09bcc-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    |  |
    | --- |
    | `https://rhelmdirectionsonmicrosoftcomtest.devcloud.acquia-sites.com/simplesaml/<companyname>` |
    | `https://www.directionsonmicrosoft.com/simplesaml/<companyname>` |

    > [!NOTE] 
    > <span data-ttu-id="09bcc-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="09bcc-161">These values are not real.</span></span> <span data-ttu-id="09bcc-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="09bcc-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="09bcc-163">Kontakta [anvisningarna på Microsoft Client supportteam](mailto:service@DirectionsOnMicrosoft.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="09bcc-163">Contact [Directions on Microsoft Client support team](mailto:service@DirectionsOnMicrosoft.com) to get these values.</span></span> 
 
4. <span data-ttu-id="09bcc-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="09bcc-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_certificate.png) 

5. <span data-ttu-id="09bcc-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="09bcc-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="09bcc-168">Konfigurera enkel inloggning på **riktningar i Microsoft** sida, måste du skicka den hämtade **XML-Metadata för** till [anvisningarna på Microsoft supportteam](mailto:service@DirectionsOnMicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="09bcc-168">To configure single sign-on on **Directions on Microsoft** side, you need to send the downloaded **Metadata XML** to [Directions on Microsoft support team](mailto:service@DirectionsOnMicrosoft.com).</span></span> <span data-ttu-id="09bcc-169">Om du vill aktivera anvisningarna på Microsoft support-teamet att hitta ditt federerade medlemskap, inkludera företagets information i din e-post.</span><span class="sxs-lookup"><span data-stu-id="09bcc-169">To enable the Directions on Microsoft support team to locate your federated site membership, include your company information in your email.</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="09bcc-170">Enkel inloggning för anvisningar i Microsoft måste aktiveras av den [anvisningarna på Microsoft Client supportteam](mailto:service@DirectionsOnMicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="09bcc-170">Single sign-on for Directions on Microsoft needs to be enabled by the [Directions on Microsoft Client support team](mailto:service@DirectionsOnMicrosoft.com).</span></span> <span data-ttu-id="09bcc-171">Du får ett meddelande när enkel inloggning har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="09bcc-171">You will receive a notification when single sign-on has been enabled.</span></span>

> [!TIP]
> <span data-ttu-id="09bcc-172">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="09bcc-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="09bcc-173">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="09bcc-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="09bcc-174">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="09bcc-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="09bcc-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="09bcc-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="09bcc-176">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="09bcc-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="09bcc-178">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="09bcc-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="09bcc-179">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="09bcc-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="09bcc-181">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="09bcc-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="09bcc-183">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="09bcc-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="09bcc-185">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="09bcc-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-directions-microsoft-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="09bcc-187">a.</span><span class="sxs-lookup"><span data-stu-id="09bcc-187">a.</span></span> <span data-ttu-id="09bcc-188">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="09bcc-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="09bcc-189">b.</span><span class="sxs-lookup"><span data-stu-id="09bcc-189">b.</span></span> <span data-ttu-id="09bcc-190">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="09bcc-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="09bcc-191">c.</span><span class="sxs-lookup"><span data-stu-id="09bcc-191">c.</span></span> <span data-ttu-id="09bcc-192">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="09bcc-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="09bcc-193">d.</span><span class="sxs-lookup"><span data-stu-id="09bcc-193">d.</span></span> <span data-ttu-id="09bcc-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="09bcc-194">Click **Create**.</span></span>
 
### <a name="creating-a-directions-on-microsoft-test-user"></a><span data-ttu-id="09bcc-195">Skapa en anvisningarna på Microsoft testanvändare</span><span class="sxs-lookup"><span data-stu-id="09bcc-195">Creating a Directions on Microsoft test user</span></span>

<span data-ttu-id="09bcc-196">Det finns inget objekt för åtgärden att konfigurera användaretablering till anvisningarna på Microsoft.</span><span class="sxs-lookup"><span data-stu-id="09bcc-196">There is no action item for you to configure user provisioning to Directions on Microsoft.</span></span>  

<span data-ttu-id="09bcc-197">När en tilldelad användare försöker logga in på anvisningarna på Microsoft med hjälp av åtkomstpanelen anvisningar i Microsoft kontrollerar om användaren finns.</span><span class="sxs-lookup"><span data-stu-id="09bcc-197">When an assigned user tries to log in to Directions on Microsoft using the access panel, Directions on Microsoft checks whether the user exists.</span></span> <span data-ttu-id="09bcc-198">Om det finns inget användarkonto ännu, skapas den automatiskt av instruktionerna på Microsoft.</span><span class="sxs-lookup"><span data-stu-id="09bcc-198">If there is no user account available yet, it is automatically created by Directions on Microsoft.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="09bcc-199">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="09bcc-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="09bcc-200">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till anvisningarna på Microsoft.</span><span class="sxs-lookup"><span data-stu-id="09bcc-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Directions on Microsoft.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="09bcc-202">**Om du vill tilldela anvisningarna på Microsoft Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="09bcc-202">**To assign Britta Simon to Directions on Microsoft, perform the following steps:**</span></span>

1. <span data-ttu-id="09bcc-203">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="09bcc-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="09bcc-205">Välj i listan med program **riktningar i Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="09bcc-205">In the applications list, select **Directions on Microsoft**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-directions-microsoft-tutorial/tutorial_directionsonmicrosoft_app.png) 

3. <span data-ttu-id="09bcc-207">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="09bcc-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="09bcc-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="09bcc-209">Click **Add** button.</span></span> <span data-ttu-id="09bcc-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="09bcc-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="09bcc-212">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="09bcc-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="09bcc-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="09bcc-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="09bcc-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="09bcc-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="09bcc-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="09bcc-215">Testing single sign-on</span></span>

<span data-ttu-id="09bcc-216">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="09bcc-216">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>
 
<span data-ttu-id="09bcc-217">När du klickar på anvisningarna på Microsoft-panelen på åtkomstpanelen du bör få automatiskt loggat in på ditt anvisningarna på Microsoft-program.</span><span class="sxs-lookup"><span data-stu-id="09bcc-217">When you click the Directions on Microsoft tile in the Access Panel, you should get automatically signed-on to your Directions on Microsoft application.</span></span>

<span data-ttu-id="09bcc-218">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="09bcc-218">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="09bcc-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="09bcc-219">Additional resources</span></span>

* [<span data-ttu-id="09bcc-220">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="09bcc-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="09bcc-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="09bcc-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-directions-microsoft-tutorial/tutorial_general_203.png

