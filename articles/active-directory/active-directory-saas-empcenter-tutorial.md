---
title: "Självstudier: Azure Active Directory-integrering med EmpCenter | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och EmpCenter."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a00ecf6e-917a-4284-b998-41506931585e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: aa27175165f4b15477bef4e70ad1c25016db31a2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-empcenter"></a><span data-ttu-id="ec9d4-103">Självstudier: Azure Active Directory-integrering med EmpCenter</span><span class="sxs-lookup"><span data-stu-id="ec9d4-103">Tutorial: Azure Active Directory integration with EmpCenter</span></span>

<span data-ttu-id="ec9d4-104">I kursen får lära du att integrera EmpCenter med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ec9d4-104">In this tutorial, you learn how to integrate EmpCenter with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ec9d4-105">Integrera EmpCenter med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ec9d4-105">Integrating EmpCenter with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ec9d4-106">Du kan styra i Azure AD som har åtkomst till EmpCenter</span><span class="sxs-lookup"><span data-stu-id="ec9d4-106">You can control in Azure AD who has access to EmpCenter</span></span>
- <span data-ttu-id="ec9d4-107">Du kan aktivera användarna att automatiskt hämta loggat in på EmpCenter (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ec9d4-107">You can enable your users to automatically get signed-on to EmpCenter (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ec9d4-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ec9d4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ec9d4-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ec9d4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ec9d4-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ec9d4-110">Prerequisites</span></span>

<span data-ttu-id="ec9d4-111">För att konfigurera Azure AD-integrering med EmpCenter, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="ec9d4-111">To configure Azure AD integration with EmpCenter, you need the following items:</span></span>

- <span data-ttu-id="ec9d4-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ec9d4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ec9d4-113">En EmpCenter enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="ec9d4-113">An EmpCenter single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ec9d4-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ec9d4-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ec9d4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ec9d4-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ec9d4-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ec9d4-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ec9d4-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ec9d4-118">Scenario description</span></span>
<span data-ttu-id="ec9d4-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ec9d4-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ec9d4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ec9d4-121">Att lägga till EmpCenter från galleriet</span><span class="sxs-lookup"><span data-stu-id="ec9d4-121">Adding EmpCenter from the gallery</span></span>
2. <span data-ttu-id="ec9d4-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ec9d4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-empcenter-from-the-gallery"></a><span data-ttu-id="ec9d4-123">Att lägga till EmpCenter från galleriet</span><span class="sxs-lookup"><span data-stu-id="ec9d4-123">Adding EmpCenter from the gallery</span></span>
<span data-ttu-id="ec9d4-124">Du måste lägga till EmpCenter från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av EmpCenter i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-124">To configure the integration of EmpCenter into Azure AD, you need to add EmpCenter from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ec9d4-125">**Utför följande steg för att lägga till EmpCenter från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="ec9d4-125">**To add EmpCenter from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ec9d4-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ec9d4-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ec9d4-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="ec9d4-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="ec9d4-133">I sökrutan skriver **EmpCenter**.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-133">In the search box, type **EmpCenter**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-EmpCenter-tutorial/tutorial_EmpCenter_search.png)

5. <span data-ttu-id="ec9d4-135">Välj i resultatpanelen **EmpCenter**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-135">In the results panel, select **EmpCenter**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-EmpCenter-tutorial/tutorial_EmpCenter_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ec9d4-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ec9d4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ec9d4-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med EmpCenter baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-138">In this section, you configure and test Azure AD single sign-on with EmpCenter based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ec9d4-139">Azure AD måste du känna till användaren i EmpCenter motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in EmpCenter is to a user in Azure AD.</span></span> <span data-ttu-id="ec9d4-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i EmpCenter upprättas.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-140">In other words, a link relationship between an Azure AD user and the related user in EmpCenter needs to be established.</span></span>

<span data-ttu-id="ec9d4-141">I EmpCenter, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-141">In EmpCenter, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ec9d4-142">Om du vill konfigurera och testa Azure AD enkel inloggning med EmpCenter, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ec9d4-142">To configure and test Azure AD single sign-on with EmpCenter, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ec9d4-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ec9d4-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ec9d4-145">**[Skapa en testanvändare EmpCenter](#creating-an-empcenter-test-user)**  – du har en motsvarighet för Britta Simon i EmpCenter som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-145">**[Creating an EmpCenter test user](#creating-an-empcenter-test-user)** - to have a counterpart of Britta Simon in EmpCenter that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ec9d4-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ec9d4-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ec9d4-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ec9d4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ec9d4-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt EmpCenter program.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your EmpCenter application.</span></span>

<span data-ttu-id="ec9d4-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med EmpCenter:**</span><span class="sxs-lookup"><span data-stu-id="ec9d4-150">**To configure Azure AD single sign-on with EmpCenter, perform the following steps:**</span></span>

1. <span data-ttu-id="ec9d4-151">I Azure-portalen på den **EmpCenter** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-151">In the Azure portal, on the **EmpCenter** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="ec9d4-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-EmpCenter-tutorial/tutorial_EmpCenter_samlbase.png)

3. <span data-ttu-id="ec9d4-155">På den **EmpCenter domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ec9d4-155">On the **EmpCenter Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-EmpCenter-tutorial/tutorial_EmpCenter_url.png)

    <span data-ttu-id="ec9d4-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="ec9d4-157">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<subdomain>.EmpCenter.com/<instancename>` |
    | `https://<subdomain>.workforcehosting.com/<instancename>` |

    > [!NOTE] 
    > <span data-ttu-id="ec9d4-158">Värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-158">The value is not real.</span></span> <span data-ttu-id="ec9d4-159">Uppdatera värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="ec9d4-160">Kontakta [EmpCenter klienten supportteamet](http://www.workforcesoftware.com/services/customer-support/) värdet hämtas.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-160">Contact [EmpCenter Client support team](http://www.workforcesoftware.com/services/customer-support/) to get the value.</span></span> 
 
4. <span data-ttu-id="ec9d4-161">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-EmpCenter-tutorial/tutorial_EmpCenter_certificate.png) 

5. <span data-ttu-id="ec9d4-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ec9d4-165">Konfigurera enkel inloggning på **EmpCenter** sida, måste du skicka den hämtade **XML-Metadata för** till [EmpCenter supportteamet](http://www.workforcesoftware.com/services/customer-support/).</span><span class="sxs-lookup"><span data-stu-id="ec9d4-165">To configure single sign-on on **EmpCenter** side, you need to send the downloaded **Metadata XML** to [EmpCenter support team](http://www.workforcesoftware.com/services/customer-support/).</span></span> <span data-ttu-id="ec9d4-166">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="ec9d4-167">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="ec9d4-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ec9d4-168">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ec9d4-169">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ec9d4-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ec9d4-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ec9d4-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="ec9d4-171">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="ec9d4-173">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="ec9d4-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ec9d4-174">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-EmpCenter-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ec9d4-176">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-EmpCenter-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ec9d4-178">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-EmpCenter-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ec9d4-180">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ec9d4-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-EmpCenter-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ec9d4-182">a.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-182">a.</span></span> <span data-ttu-id="ec9d4-183">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ec9d4-184">b.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-184">b.</span></span> <span data-ttu-id="ec9d4-185">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ec9d4-186">c.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-186">c.</span></span> <span data-ttu-id="ec9d4-187">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ec9d4-188">d.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-188">d.</span></span> <span data-ttu-id="ec9d4-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-189">Click **Create**.</span></span>
 
### <a name="creating-an-empcenter-test-user"></a><span data-ttu-id="ec9d4-190">Skapa en testanvändare EmpCenter</span><span class="sxs-lookup"><span data-stu-id="ec9d4-190">Creating an EmpCenter test user</span></span>

<span data-ttu-id="ec9d4-191">För att aktivera Azure AD-användare kan logga in på EmpCenter etableras de i EmpCenter.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-191">In order to enable Azure AD users to log in to EmpCenter, they must be provisioned into EmpCenter.</span></span> <span data-ttu-id="ec9d4-192">När det gäller EmpCenter, behöver användarkonton skapas av din [EmpCenter supportteamet](http://www.workforcesoftware.com/services/customer-support/).</span><span class="sxs-lookup"><span data-stu-id="ec9d4-192">In the case of EmpCenter, the user accounts need to be created by your [EmpCenter support team](http://www.workforcesoftware.com/services/customer-support/).</span></span>

> [!NOTE]
> <span data-ttu-id="ec9d4-193">Du kan använda något annat EmpCenter användarens konto skapas verktyg eller API: er som tillhandahålls av EmpCenter för att etablera Azure Active Directory användarkonton.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-193">You can use any other EmpCenter user account creation tools or APIs provided by EmpCenter to provision Azure Active Directory user accounts.</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ec9d4-194">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="ec9d4-194">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ec9d4-195">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till EmpCenter.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-195">In this section, you enable Britta Simon to use Azure single sign-on by granting access to EmpCenter.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="ec9d4-197">**Om du vill tilldela EmpCenter Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ec9d4-197">**To assign Britta Simon to EmpCenter, perform the following steps:**</span></span>

1. <span data-ttu-id="ec9d4-198">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-198">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ec9d4-200">Välj i listan med program **EmpCenter**.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-200">In the applications list, select **EmpCenter**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-EmpCenter-tutorial/tutorial_EmpCenter_app.png) 

3. <span data-ttu-id="ec9d4-202">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-202">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="ec9d4-204">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-204">Click **Add** button.</span></span> <span data-ttu-id="ec9d4-205">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-205">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="ec9d4-207">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-207">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ec9d4-208">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-208">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ec9d4-209">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-209">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ec9d4-210">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ec9d4-210">Testing single sign-on</span></span>


<span data-ttu-id="ec9d4-211">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-211">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="ec9d4-212">När du klickar på panelen EmpCenter på åtkomstpanelen du bör få automatiskt loggat in på ditt EmpCenter program.</span><span class="sxs-lookup"><span data-stu-id="ec9d4-212">When you click the EmpCenter tile in the Access Panel, you should get automatically signed-on to your EmpCenter application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ec9d4-213">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ec9d4-213">Additional resources</span></span>

* [<span data-ttu-id="ec9d4-214">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ec9d4-214">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ec9d4-215">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ec9d4-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-EmpCenter-tutorial/tutorial_general_203.png

