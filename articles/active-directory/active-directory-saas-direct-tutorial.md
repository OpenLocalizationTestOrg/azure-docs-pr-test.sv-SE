---
title: "Självstudier: Azure Active Directory-integrering med Direct | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och direkt."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7c2cd1f0-d14c-42f0-94a8-9b800008b285
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 84582492592613320bd3ec2bdffe08519852d7c9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-direct"></a><span data-ttu-id="8ffa0-103">Självstudier: Azure Active Directory-integrering med Direct</span><span class="sxs-lookup"><span data-stu-id="8ffa0-103">Tutorial: Azure Active Directory integration with Direct</span></span>

<span data-ttu-id="8ffa0-104">I kursen får du lära dig hur integreras direkt med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="8ffa0-104">In this tutorial, you learn how to integrate Direct with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8ffa0-105">Integrera direkt med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="8ffa0-105">Integrating Direct with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8ffa0-106">Du kan styra i Azure AD som har åtkomst till Direct</span><span class="sxs-lookup"><span data-stu-id="8ffa0-106">You can control in Azure AD who has access to Direct</span></span>
- <span data-ttu-id="8ffa0-107">Du kan aktivera användarna att automatiskt hämta inloggade att dirigera (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="8ffa0-107">You can enable your users to automatically get signed-on to Direct (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8ffa0-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8ffa0-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8ffa0-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8ffa0-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8ffa0-110">Krav</span><span class="sxs-lookup"><span data-stu-id="8ffa0-110">Prerequisites</span></span>

<span data-ttu-id="8ffa0-111">För att konfigurera Azure AD-integrering med Direct, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="8ffa0-111">To configure Azure AD integration with Direct, you need the following items:</span></span>

- <span data-ttu-id="8ffa0-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="8ffa0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8ffa0-113">En direkt enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="8ffa0-113">A Direct single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8ffa0-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8ffa0-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="8ffa0-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8ffa0-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8ffa0-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8ffa0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8ffa0-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="8ffa0-118">Scenario description</span></span>
<span data-ttu-id="8ffa0-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8ffa0-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="8ffa0-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8ffa0-121">Lägger till direkt från galleriet</span><span class="sxs-lookup"><span data-stu-id="8ffa0-121">Adding Direct from the gallery</span></span>
2. <span data-ttu-id="8ffa0-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8ffa0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-direct-from-the-gallery"></a><span data-ttu-id="8ffa0-123">Lägger till direkt från galleriet</span><span class="sxs-lookup"><span data-stu-id="8ffa0-123">Adding Direct from the gallery</span></span>
<span data-ttu-id="8ffa0-124">Du måste lägga till direkt från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av direkt i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-124">To configure the integration of Direct into Azure AD, you need to add Direct from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8ffa0-125">**Utför följande steg för att lägga till direkt från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="8ffa0-125">**To add Direct from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8ffa0-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8ffa0-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8ffa0-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="8ffa0-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="8ffa0-133">I sökrutan skriver **direkt**.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-133">In the search box, type **Direct**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-direct-tutorial/tutorial_direct_search.png)

5. <span data-ttu-id="8ffa0-135">Välj i resultatpanelen **direkt**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-135">In the results panel, select **Direct**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-direct-tutorial/tutorial_direct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8ffa0-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8ffa0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8ffa0-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Direct baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-138">In this section, you configure and test Azure AD single sign-on with Direct based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8ffa0-139">Azure AD måste du känna till användaren i motsvarande direkt till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Direct is to a user in Azure AD.</span></span> <span data-ttu-id="8ffa0-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Direct upprättas.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-140">In other words, a link relationship between an Azure AD user and the related user in Direct needs to be established.</span></span>

<span data-ttu-id="8ffa0-141">I direkta, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-141">In Direct, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8ffa0-142">Om du vill konfigurera och testa Azure AD enkel inloggning med direkt, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="8ffa0-142">To configure and test Azure AD single sign-on with Direct, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8ffa0-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8ffa0-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8ffa0-145">**[Skapa en direkt testanvändare](#creating-a-direct-test-user)**  – har en motsvarighet för Britta Simon Direct som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-145">**[Creating a Direct test user](#creating-a-direct-test-user)** - to have a counterpart of Britta Simon in Direct that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8ffa0-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8ffa0-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8ffa0-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8ffa0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8ffa0-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet direkt.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Direct application.</span></span>

<span data-ttu-id="8ffa0-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Direct:**</span><span class="sxs-lookup"><span data-stu-id="8ffa0-150">**To configure Azure AD single sign-on with Direct, perform the following steps:**</span></span>

1. <span data-ttu-id="8ffa0-151">I Azure-portalen på den **direkt** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-151">In the Azure portal, on the **Direct** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="8ffa0-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-direct-tutorial/tutorial_direct_samlbase.png)

3. <span data-ttu-id="8ffa0-155">På den **direkt domän och URL: er** om du vill konfigurera programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="8ffa0-155">On the **Direct Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-direct-tutorial/tutorial_direct_url.png)

    <span data-ttu-id="8ffa0-157">I den **identifierare** textruta anger du URL:`https://direct4b.com/`</span><span class="sxs-lookup"><span data-stu-id="8ffa0-157">In the **Identifier** textbox, type the URL: `https://direct4b.com/`</span></span>

4. <span data-ttu-id="8ffa0-158">Kontrollera **visa avancerade inställningar för URL: en**, om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="8ffa0-158">Check **Show advanced URL settings**, If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-direct-tutorial/tutorial_direct_url1.png)

     <span data-ttu-id="8ffa0-160">I den **inloggnings-URL** textruta anger du URL:`https://direct4b.com/sso`</span><span class="sxs-lookup"><span data-stu-id="8ffa0-160">In the **Sign-on URL** textbox, type the URL: `https://direct4b.com/sso`</span></span> 
    
5. <span data-ttu-id="8ffa0-161">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-direct-tutorial/tutorial_direct_certificate.png) 

6. <span data-ttu-id="8ffa0-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-direct-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="8ffa0-165">Konfigurera enkel inloggning på **direkt** sida, måste du skicka den hämtade **XML-Metadata för** till [direkt supportteamet](https://direct4b.com/ja/support.html#inquiry).</span><span class="sxs-lookup"><span data-stu-id="8ffa0-165">To configure single sign-on on **Direct** side, you need to send the downloaded **Metadata XML** to [Direct support team](https://direct4b.com/ja/support.html#inquiry).</span></span> 

> [!TIP]
> <span data-ttu-id="8ffa0-166">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="8ffa0-166">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8ffa0-167">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-167">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8ffa0-168">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8ffa0-168">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8ffa0-169">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="8ffa0-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="8ffa0-170">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-170">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="8ffa0-172">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="8ffa0-172">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8ffa0-173">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-173">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8ffa0-175">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-175">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8ffa0-177">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-177">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8ffa0-179">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8ffa0-179">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8ffa0-181">a.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-181">a.</span></span> <span data-ttu-id="8ffa0-182">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-182">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8ffa0-183">b.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-183">b.</span></span> <span data-ttu-id="8ffa0-184">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-184">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8ffa0-185">c.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-185">c.</span></span> <span data-ttu-id="8ffa0-186">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-186">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8ffa0-187">d.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-187">d.</span></span> <span data-ttu-id="8ffa0-188">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-188">Click **Create**.</span></span>
 
### <a name="creating-a-direct-test-user"></a><span data-ttu-id="8ffa0-189">Skapa en direkt testanvändare</span><span class="sxs-lookup"><span data-stu-id="8ffa0-189">Creating a Direct test user</span></span>

<span data-ttu-id="8ffa0-190">I det här avsnittet kan du skapa en användare som kallas Britta Simon i direkt.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-190">In this section, you create a user called Britta Simon in Direct.</span></span> <span data-ttu-id="8ffa0-191">Arbeta med [direkt supportteamet](https://direct4b.com/ja/support.html#inquiry) att lägga till användare i plattformen direkt.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-191">Work with [Direct support team](https://direct4b.com/ja/support.html#inquiry) to add the users in the Direct platform.</span></span> <span data-ttu-id="8ffa0-192">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-192">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8ffa0-193">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="8ffa0-193">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8ffa0-194">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till direkt.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-194">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Direct.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="8ffa0-196">**Om du vill tilldela Direct Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8ffa0-196">**To assign Britta Simon to Direct, perform the following steps:**</span></span>

1. <span data-ttu-id="8ffa0-197">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-197">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="8ffa0-199">Välj i listan med program **direkt**.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-199">In the applications list, select **Direct**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-direct-tutorial/tutorial_direct_app.png) 

3. <span data-ttu-id="8ffa0-201">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-201">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="8ffa0-203">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-203">Click **Add** button.</span></span> <span data-ttu-id="8ffa0-204">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="8ffa0-206">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-206">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8ffa0-207">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8ffa0-208">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8ffa0-209">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8ffa0-209">Testing single sign-on</span></span>

<span data-ttu-id="8ffa0-210">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-210">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

1. <span data-ttu-id="8ffa0-211">Om du vill testa i **IDP initierade läge**:</span><span class="sxs-lookup"><span data-stu-id="8ffa0-211">If you wish to test in **IDP Initiated Mode**:</span></span>

    <span data-ttu-id="8ffa0-212">När du klickar på den **direkt** panelen på panelen åtkomst som du bör få automatiskt loggat in på ditt **direkt** program.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-212">When you click the **Direct** tile in the Access Panel, you should get automatically signed-on to your **Direct** application.</span></span>

2. <span data-ttu-id="8ffa0-213">Om du vill testa i **SP-initierad läge**:</span><span class="sxs-lookup"><span data-stu-id="8ffa0-213">If you wish to test in **SP Initiated Mode**:</span></span>
    
    <span data-ttu-id="8ffa0-214">a.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-214">a.</span></span> <span data-ttu-id="8ffa0-215">Klicka på den **direkt** panelen på åtkomstpanelen och du omdirigeras till sidan program inloggning.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-215">Click on the **Direct** tile in the Access Panel and you will be redirected to the application sign-on page.</span></span>

    <span data-ttu-id="8ffa0-216">b.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-216">b.</span></span> <span data-ttu-id="8ffa0-217">Ange din `subdomain` i textrutan som visas och tryck på '次へ (nästa), och du bör få automatiskt loggat in på ditt **direkt** program.</span><span class="sxs-lookup"><span data-stu-id="8ffa0-217">Input your `subdomain` in the textbox displayed and press '次へ (Next)' and you should get automatically signed-on to your **Direct** application .</span></span>
    
<span data-ttu-id="8ffa0-218">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8ffa0-218">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8ffa0-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="8ffa0-219">Additional resources</span></span>

* [<span data-ttu-id="8ffa0-220">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8ffa0-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8ffa0-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8ffa0-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-direct-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-direct-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-direct-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-direct-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-direct-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-direct-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-direct-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-direct-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-direct-tutorial/tutorial_general_203.png

