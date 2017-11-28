---
title: "Självstudier: Azure Active Directory-integrering med Intralinks | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Intralinks."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 147f2bf9-166b-402e-adc4-4b19dd336883
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: ee7fd5b88ac806104002ffb41af11bab4fd1b2dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intralinks"></a><span data-ttu-id="452e8-103">Självstudier: Azure Active Directory-integrering med Intralinks</span><span class="sxs-lookup"><span data-stu-id="452e8-103">Tutorial: Azure Active Directory integration with Intralinks</span></span>

<span data-ttu-id="452e8-104">I kursen får lära du att integrera Intralinks med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="452e8-104">In this tutorial, you learn how to integrate Intralinks with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="452e8-105">Integrera Intralinks med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="452e8-105">Integrating Intralinks with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="452e8-106">Du kan styra i Azure AD som har åtkomst till Intralinks</span><span class="sxs-lookup"><span data-stu-id="452e8-106">You can control in Azure AD who has access to Intralinks</span></span>
- <span data-ttu-id="452e8-107">Du kan aktivera användarna att automatiskt hämta loggat in på Intralinks (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="452e8-107">You can enable your users to automatically get signed-on to Intralinks (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="452e8-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="452e8-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="452e8-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="452e8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="452e8-110">Krav</span><span class="sxs-lookup"><span data-stu-id="452e8-110">Prerequisites</span></span>

<span data-ttu-id="452e8-111">För att konfigurera Azure AD-integrering med Intralinks, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="452e8-111">To configure Azure AD integration with Intralinks, you need the following items:</span></span>

- <span data-ttu-id="452e8-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="452e8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="452e8-113">En Intralinks enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="452e8-113">An Intralinks single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="452e8-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="452e8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="452e8-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="452e8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="452e8-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="452e8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="452e8-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="452e8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="452e8-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="452e8-118">Scenario description</span></span>
<span data-ttu-id="452e8-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="452e8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="452e8-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="452e8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="452e8-121">Att lägga till Intralinks från galleriet</span><span class="sxs-lookup"><span data-stu-id="452e8-121">Adding Intralinks from the gallery</span></span>
2. <span data-ttu-id="452e8-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="452e8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-intralinks-from-the-gallery"></a><span data-ttu-id="452e8-123">Att lägga till Intralinks från galleriet</span><span class="sxs-lookup"><span data-stu-id="452e8-123">Adding Intralinks from the gallery</span></span>
<span data-ttu-id="452e8-124">Du måste lägga till Intralinks från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Intralinks i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="452e8-124">To configure the integration of Intralinks into Azure AD, you need to add Intralinks from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="452e8-125">**Utför följande steg för att lägga till Intralinks från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="452e8-125">**To add Intralinks from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="452e8-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="452e8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="452e8-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="452e8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="452e8-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="452e8-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="452e8-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="452e8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="452e8-133">I sökrutan skriver **Intralinks**.</span><span class="sxs-lookup"><span data-stu-id="452e8-133">In the search box, type **Intralinks**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_search.png)

5. <span data-ttu-id="452e8-135">Välj i resultatpanelen **Intralinks**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="452e8-135">In the results panel, select **Intralinks**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="452e8-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="452e8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="452e8-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Intralinks baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="452e8-138">In this section, you configure and test Azure AD single sign-on with Intralinks based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="452e8-139">Azure AD måste du känna till användaren i Intralinks motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="452e8-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Intralinks is to a user in Azure AD.</span></span> <span data-ttu-id="452e8-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Intralinks upprättas.</span><span class="sxs-lookup"><span data-stu-id="452e8-140">In other words, a link relationship between an Azure AD user and the related user in Intralinks needs to be established.</span></span>

<span data-ttu-id="452e8-141">I Intralinks, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="452e8-141">In Intralinks, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="452e8-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Intralinks, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="452e8-142">To configure and test Azure AD single sign-on with Intralinks, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="452e8-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="452e8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="452e8-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="452e8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="452e8-145">**[Skapa en testanvändare Intralinks](#creating-an-intralinks-test-user)**  – du har en motsvarighet för Britta Simon i Intralinks som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="452e8-145">**[Creating an Intralinks test user](#creating-an-intralinks-test-user)** - to have a counterpart of Britta Simon in Intralinks that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="452e8-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="452e8-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="452e8-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="452e8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="452e8-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="452e8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="452e8-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Intralinks program.</span><span class="sxs-lookup"><span data-stu-id="452e8-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Intralinks application.</span></span>

<span data-ttu-id="452e8-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Intralinks:**</span><span class="sxs-lookup"><span data-stu-id="452e8-150">**To configure Azure AD single sign-on with Intralinks, perform the following steps:**</span></span>

1. <span data-ttu-id="452e8-151">I Azure-portalen på den **Intralinks** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="452e8-151">In the Azure portal, on the **Intralinks** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="452e8-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="452e8-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_samlbase.png)

3. <span data-ttu-id="452e8-155">På den **Intralinks domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="452e8-155">On the **Intralinks Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_url.png)

    <span data-ttu-id="452e8-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`</span><span class="sxs-lookup"><span data-stu-id="452e8-157">In the **Sign-on URL** textbox, type a URL using the following pattern:  `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="452e8-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="452e8-158">This value is not real.</span></span> <span data-ttu-id="452e8-159">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="452e8-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="452e8-160">Kontakta [Intralinks klienten supportteamet](https://www.intralinks.com/contact-1) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="452e8-160">Contact [Intralinks Client support team](https://www.intralinks.com/contact-1) to get this value.</span></span> 
 
4. <span data-ttu-id="452e8-161">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="452e8-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_certificate.png) 

5. <span data-ttu-id="452e8-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="452e8-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="452e8-165">Konfigurera enkel inloggning på **Intralinks** sida, måste du skicka den hämtade **XML-Metadata för** [Intralinks supportteam](https://www.intralinks.com/contact-1).</span><span class="sxs-lookup"><span data-stu-id="452e8-165">To configure single sign-on on **Intralinks** side, you need to send the downloaded **Metadata XML** [Intralinks support team](https://www.intralinks.com/contact-1).</span></span> <span data-ttu-id="452e8-166">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="452e8-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="452e8-167">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="452e8-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="452e8-168">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="452e8-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="452e8-169">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="452e8-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="452e8-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="452e8-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="452e8-171">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="452e8-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="452e8-173">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="452e8-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="452e8-174">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="452e8-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="452e8-176">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="452e8-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="452e8-178">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="452e8-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="452e8-180">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="452e8-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="452e8-182">a.</span><span class="sxs-lookup"><span data-stu-id="452e8-182">a.</span></span> <span data-ttu-id="452e8-183">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="452e8-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="452e8-184">b.</span><span class="sxs-lookup"><span data-stu-id="452e8-184">b.</span></span> <span data-ttu-id="452e8-185">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="452e8-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="452e8-186">c.</span><span class="sxs-lookup"><span data-stu-id="452e8-186">c.</span></span> <span data-ttu-id="452e8-187">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="452e8-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="452e8-188">d.</span><span class="sxs-lookup"><span data-stu-id="452e8-188">d.</span></span> <span data-ttu-id="452e8-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="452e8-189">Click **Create**.</span></span>
 
### <a name="creating-an-intralinks-test-user"></a><span data-ttu-id="452e8-190">Skapa en testanvändare Intralinks</span><span class="sxs-lookup"><span data-stu-id="452e8-190">Creating an Intralinks test user</span></span>

<span data-ttu-id="452e8-191">I det här avsnittet skapar du en användare som kallas Britta Simon i Intralinks.</span><span class="sxs-lookup"><span data-stu-id="452e8-191">In this section, you create a user called Britta Simon in Intralinks.</span></span> <span data-ttu-id="452e8-192">Se tillsammans med [Intralinks supportteam](https://www.intralinks.com/contact-1) att lägga till användare i Intralinks-plattformen.</span><span class="sxs-lookup"><span data-stu-id="452e8-192">Please work with [Intralinks support team](https://www.intralinks.com/contact-1) to add the users in the Intralinks platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="452e8-193">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="452e8-193">Assigning the Azure AD test user</span></span>

<span data-ttu-id="452e8-194">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Intralinks.</span><span class="sxs-lookup"><span data-stu-id="452e8-194">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Intralinks.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="452e8-196">**Om du vill tilldela Intralinks Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="452e8-196">**To assign Britta Simon to Intralinks, perform the following steps:**</span></span>

1. <span data-ttu-id="452e8-197">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="452e8-197">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="452e8-199">Välj i listan med program **Intralinks**.</span><span class="sxs-lookup"><span data-stu-id="452e8-199">In the applications list, select **Intralinks**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_app.png) 

3. <span data-ttu-id="452e8-201">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="452e8-201">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="452e8-203">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="452e8-203">Click **Add** button.</span></span> <span data-ttu-id="452e8-204">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="452e8-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="452e8-206">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="452e8-206">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="452e8-207">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="452e8-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="452e8-208">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="452e8-208">Click **Assign** button on **Add Assignment** dialog.</span></span>

### <a name="add-intralinks-via-or-elite-application"></a><span data-ttu-id="452e8-209">Lägg till Intralinks VIA eller Elite program</span><span class="sxs-lookup"><span data-stu-id="452e8-209">Add Intralinks VIA or Elite application</span></span>

<span data-ttu-id="452e8-210">Intralinks använder samma SSO-identitetsplattformen för alla andra Intralinks program utom affären Nexus program.</span><span class="sxs-lookup"><span data-stu-id="452e8-210">Intralinks uses the same SSO identity platform for all other Intralinks applications excluding Deal Nexus application.</span></span> <span data-ttu-id="452e8-211">Så om du planerar att använda något annat program för Intralinks sedan du först konfigurera enkel inloggning för en primär Intralinks program med hjälp av proceduren ovan.</span><span class="sxs-lookup"><span data-stu-id="452e8-211">So if you plan to use any other Intralinks application then first you have to configure SSO for one Primary Intralinks application using the procedure described above.</span></span>

<span data-ttu-id="452e8-212">Efter det att du kan följa det under proceduren för att lägga till en annan Intralinks program i din klient som kan använda den här primära programmet för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="452e8-212">After that you can follow the below procedure to add another Intralinks application in your tenant which can leverage this primary application for SSO.</span></span> 

>[!NOTE]
><span data-ttu-id="452e8-213">Den här funktionen är endast tillgänglig för Azure AD Premium-SKU kunder och inte tillgängliga för kostnadsfri eller grundläggande SKU-kunder.</span><span class="sxs-lookup"><span data-stu-id="452e8-213">This feature is available only to Azure AD Premium SKU Customers and not available for Free or Basic SKU customers.</span></span>

1. <span data-ttu-id="452e8-214">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="452e8-214">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]


2. <span data-ttu-id="452e8-216">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="452e8-216">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="452e8-217">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="452e8-217">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="452e8-219">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="452e8-219">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="452e8-221">I sökrutan skriver **Intralinks**.</span><span class="sxs-lookup"><span data-stu-id="452e8-221">In the search box, type **Intralinks**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_search.png)

5. <span data-ttu-id="452e8-223">På **Intralinks Lägg till app** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="452e8-223">On **Intralinks Add app** perform the following steps:</span></span>

    ![Lägga till Intralinks VIA eller Elite program](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_addapp.png)

    <span data-ttu-id="452e8-225">a.</span><span class="sxs-lookup"><span data-stu-id="452e8-225">a.</span></span> <span data-ttu-id="452e8-226">I **namn** textruta, t.ex. Ange rätt namn på programmet **Intralinks Elite**.</span><span class="sxs-lookup"><span data-stu-id="452e8-226">In **Name** textbox, enter appropriate name of the application e.g. **Intralinks Elite**.</span></span>

    <span data-ttu-id="452e8-227">b.</span><span class="sxs-lookup"><span data-stu-id="452e8-227">b.</span></span> <span data-ttu-id="452e8-228">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="452e8-228">Click **Add** button.</span></span>

6.  <span data-ttu-id="452e8-229">I Azure-portalen på den **Intralinks** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="452e8-229">In the Azure portal, on the **Intralinks** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

7. <span data-ttu-id="452e8-231">På den **enkel inloggning** markerar **läge** som **inloggning länkade**.</span><span class="sxs-lookup"><span data-stu-id="452e8-231">On the **Single sign-on** dialog, select **Mode** as **Linked Sign-on**.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_linkedsignon.png)

8. <span data-ttu-id="452e8-233">Hämta den SP initierad SSO URL från [Intralinks team](https://www.intralinks.com/contact-1) för andra Intralinks program och ange den i **konfigurera inloggnings-URL** enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="452e8-233">Get the the SP Initiated SSO URL from [Intralinks team](https://www.intralinks.com/contact-1) for the other Intralinks application and enter it in **Configure Sign-on URL** as shown below.</span></span> 
    
     ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_customappurl.png)
    
     <span data-ttu-id="452e8-235">Skriv den URL som används av dina användare logga in i tillämpningsprogrammet Intralinks med hjälp av följande mönster i textrutan loggar URL:</span><span class="sxs-lookup"><span data-stu-id="452e8-235">In the Sign On URL textbox, type the URL used by your users to sign-on to your Intralinks application using the following pattern:</span></span>
   
    `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`

9. <span data-ttu-id="452e8-236">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="452e8-236">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_general_400.png)

10. <span data-ttu-id="452e8-238">Tilldela program till användare eller grupper som visas i avsnittet  **[tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**.</span><span class="sxs-lookup"><span data-stu-id="452e8-238">Assign the application to user or groups as shown in the section **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)**.</span></span>

### <a name="testing-single-sign-on"></a><span data-ttu-id="452e8-239">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="452e8-239">Testing single sign-on</span></span>

<span data-ttu-id="452e8-240">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="452e8-240">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="452e8-241">När du klickar på panelen Intralinks på åtkomstpanelen du bör få automatiskt loggat in på ditt Intralinks program.</span><span class="sxs-lookup"><span data-stu-id="452e8-241">When you click the Intralinks tile in the Access Panel, you should get automatically signed-on to your Intralinks application.</span></span>
<span data-ttu-id="452e8-242">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="452e8-242">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="452e8-243">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="452e8-243">Additional resources</span></span>

* [<span data-ttu-id="452e8-244">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="452e8-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="452e8-245">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="452e8-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_203.png

