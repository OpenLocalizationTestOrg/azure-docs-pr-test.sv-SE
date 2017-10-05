---
title: "Självstudier: Azure Active Directory-integrering med Tango Analytics | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Tango analyser."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2f7555d3-e9ba-40b2-9b3a-2f0ab38a4c08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 46f6facc3c86630a9252340b2e89634368c0263d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tango-analytics"></a><span data-ttu-id="8dd69-103">Självstudier: Azure Active Directory-integrering med Tango Analytics</span><span class="sxs-lookup"><span data-stu-id="8dd69-103">Tutorial: Azure Active Directory integration with Tango Analytics</span></span>

<span data-ttu-id="8dd69-104">I kursen får lära du att integrera Tango Analytics med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="8dd69-104">In this tutorial, you learn how to integrate Tango Analytics with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8dd69-105">Integrera Tango Analytics med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="8dd69-105">Integrating Tango Analytics with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8dd69-106">Du kan styra i Azure AD som har åtkomst till Tango Analytics</span><span class="sxs-lookup"><span data-stu-id="8dd69-106">You can control in Azure AD who has access to Tango Analytics</span></span>
- <span data-ttu-id="8dd69-107">Du kan aktivera användarna att automatiskt hämta loggat in på Tango Analytics (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="8dd69-107">You can enable your users to automatically get signed-on to Tango Analytics (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8dd69-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8dd69-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8dd69-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8dd69-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8dd69-110">Krav</span><span class="sxs-lookup"><span data-stu-id="8dd69-110">Prerequisites</span></span>

<span data-ttu-id="8dd69-111">Om du vill konfigurera Azure AD-integrering med Tango Analytics behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="8dd69-111">To configure Azure AD integration with Tango Analytics, you need the following items:</span></span>

- <span data-ttu-id="8dd69-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="8dd69-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8dd69-113">En Tango Analytics enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="8dd69-113">A Tango Analytics single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8dd69-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="8dd69-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8dd69-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="8dd69-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8dd69-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="8dd69-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8dd69-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8dd69-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8dd69-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="8dd69-118">Scenario description</span></span>
<span data-ttu-id="8dd69-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="8dd69-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8dd69-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="8dd69-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8dd69-121">Att lägga till Tango Analytics från galleriet</span><span class="sxs-lookup"><span data-stu-id="8dd69-121">Adding Tango Analytics from the gallery</span></span>
2. <span data-ttu-id="8dd69-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8dd69-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tango-analytics-from-the-gallery"></a><span data-ttu-id="8dd69-123">Att lägga till Tango Analytics från galleriet</span><span class="sxs-lookup"><span data-stu-id="8dd69-123">Adding Tango Analytics from the gallery</span></span>
<span data-ttu-id="8dd69-124">Du måste lägga till Tango Analytics från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Tango Analytics till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8dd69-124">To configure the integration of Tango Analytics into Azure AD, you need to add Tango Analytics from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8dd69-125">**Utför följande steg för att lägga till Tango Analytics från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="8dd69-125">**To add Tango Analytics from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8dd69-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8dd69-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8dd69-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="8dd69-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8dd69-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8dd69-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="8dd69-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8dd69-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="8dd69-133">I sökrutan skriver **Tango Analytics**.</span><span class="sxs-lookup"><span data-stu-id="8dd69-133">In the search box, type **Tango Analytics**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_search.png)

5. <span data-ttu-id="8dd69-135">Välj i resultatpanelen **Tango Analytics**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="8dd69-135">In the results panel, select **Tango Analytics**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8dd69-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8dd69-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8dd69-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Tango Analytics baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="8dd69-138">In this section, you configure and test Azure AD single sign-on with Tango Analytics based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8dd69-139">Azure AD måste du känna till användaren i Tango Analytics motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="8dd69-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Tango Analytics is to a user in Azure AD.</span></span> <span data-ttu-id="8dd69-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i Tango Analytics upprättas.</span><span class="sxs-lookup"><span data-stu-id="8dd69-140">In other words, a link relationship between an Azure AD user and the related user in Tango Analytics needs to be established.</span></span>

<span data-ttu-id="8dd69-141">I Tango Analytics, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="8dd69-141">In Tango Analytics, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8dd69-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Tango Analytics, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="8dd69-142">To configure and test Azure AD single sign-on with Tango Analytics, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8dd69-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="8dd69-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8dd69-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8dd69-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8dd69-145">**[Skapa en testanvändare Tango Analytics](#creating-a-tango-analytics-test-user)**  – du har en motsvarighet för Britta Simon i Tango Analytics som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="8dd69-145">**[Creating a Tango Analytics test user](#creating-a-tango-analytics-test-user)** - to have a counterpart of Britta Simon in Tango Analytics that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8dd69-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8dd69-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8dd69-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="8dd69-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8dd69-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8dd69-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8dd69-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Tango Analytics.</span><span class="sxs-lookup"><span data-stu-id="8dd69-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Tango Analytics application.</span></span>

<span data-ttu-id="8dd69-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Tango Analytics:**</span><span class="sxs-lookup"><span data-stu-id="8dd69-150">**To configure Azure AD single sign-on with Tango Analytics, perform the following steps:**</span></span>

1. <span data-ttu-id="8dd69-151">I Azure-portalen på den **Tango Analytics** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="8dd69-151">In the Azure portal, on the **Tango Analytics** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="8dd69-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8dd69-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_samlbase.png)

3. <span data-ttu-id="8dd69-155">På den **Tango Analytics domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8dd69-155">On the **Tango Analytics Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_url.png)

    <span data-ttu-id="8dd69-157">a.</span><span class="sxs-lookup"><span data-stu-id="8dd69-157">a.</span></span> <span data-ttu-id="8dd69-158">I den **identifierare** textruta Skriv värdet`TACORE_SSO`</span><span class="sxs-lookup"><span data-stu-id="8dd69-158">In the **Identifier** textbox, type the value `TACORE_SSO`</span></span>

    <span data-ttu-id="8dd69-159">b.</span><span class="sxs-lookup"><span data-stu-id="8dd69-159">b.</span></span> <span data-ttu-id="8dd69-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://mts.tangoanalytics.com/saml2/sp/acs/post`</span><span class="sxs-lookup"><span data-stu-id="8dd69-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://mts.tangoanalytics.com/saml2/sp/acs/post`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8dd69-161">Reply URL-värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="8dd69-161">The Reply URL value is not real.</span></span> <span data-ttu-id="8dd69-162">Uppdatera den med den faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="8dd69-162">Update this with the actual Reply URL.</span></span> <span data-ttu-id="8dd69-163">Kontakta [Tango Analytics supportteam](mailto:support@tangoanalytics.com) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="8dd69-163">Contact [Tango Analytics support team](mailto:support@tangoanalytics.com) to get this value.</span></span>

4. <span data-ttu-id="8dd69-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="8dd69-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_certificate.png) 

5. <span data-ttu-id="8dd69-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="8dd69-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8dd69-168">Konfigurera enkel inloggning på **Tango Analytics** sida, måste du skicka den hämtade **XML-Metadata för** till [Tango Analytics supportteam](mailto:support@tangoanalytics.com).</span><span class="sxs-lookup"><span data-stu-id="8dd69-168">To configure single sign-on on **Tango Analytics** side, you need to send the downloaded **Metadata XML** to [Tango Analytics support team](mailto:support@tangoanalytics.com).</span></span> <span data-ttu-id="8dd69-169">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="8dd69-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="8dd69-170">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="8dd69-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8dd69-171">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="8dd69-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8dd69-172">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8dd69-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8dd69-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="8dd69-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="8dd69-174">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8dd69-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="8dd69-176">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="8dd69-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8dd69-177">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8dd69-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tangoanalytics-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8dd69-179">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="8dd69-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tangoanalytics-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8dd69-181">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8dd69-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tangoanalytics-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8dd69-183">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8dd69-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tangoanalytics-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8dd69-185">a.</span><span class="sxs-lookup"><span data-stu-id="8dd69-185">a.</span></span> <span data-ttu-id="8dd69-186">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8dd69-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8dd69-187">b.</span><span class="sxs-lookup"><span data-stu-id="8dd69-187">b.</span></span> <span data-ttu-id="8dd69-188">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8dd69-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8dd69-189">c.</span><span class="sxs-lookup"><span data-stu-id="8dd69-189">c.</span></span> <span data-ttu-id="8dd69-190">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="8dd69-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8dd69-191">d.</span><span class="sxs-lookup"><span data-stu-id="8dd69-191">d.</span></span> <span data-ttu-id="8dd69-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="8dd69-192">Click **Create**.</span></span>
 
### <a name="creating-a-tango-analytics-test-user"></a><span data-ttu-id="8dd69-193">Skapa en testanvändare Tango Analytics</span><span class="sxs-lookup"><span data-stu-id="8dd69-193">Creating a Tango Analytics test user</span></span>

<span data-ttu-id="8dd69-194">I det här avsnittet skapar du en användare som kallas Britta Simon i Tango Analytics.</span><span class="sxs-lookup"><span data-stu-id="8dd69-194">In this section, you create a user called Britta Simon in Tango Analytics.</span></span> <span data-ttu-id="8dd69-195">Arbeta med [Tango Analytics supportteam](mailto:support@tangoanalytics.com) att lägga till användare i Tango Analytics platform.</span><span class="sxs-lookup"><span data-stu-id="8dd69-195">Work with [Tango Analytics support team](mailto:support@tangoanalytics.com) to add the users in the Tango Analytics platform.</span></span> <span data-ttu-id="8dd69-196">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8dd69-196">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8dd69-197">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="8dd69-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8dd69-198">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Tango Analytics.</span><span class="sxs-lookup"><span data-stu-id="8dd69-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Tango Analytics.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="8dd69-200">**Om du vill tilldela Tango Analytics Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8dd69-200">**To assign Britta Simon to Tango Analytics, perform the following steps:**</span></span>

1. <span data-ttu-id="8dd69-201">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8dd69-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="8dd69-203">Välj i listan med program **Tango Analytics**.</span><span class="sxs-lookup"><span data-stu-id="8dd69-203">In the applications list, select **Tango Analytics**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tangoanalytics-tutorial/tutorial_tangoanalytics_app.png) 

3. <span data-ttu-id="8dd69-205">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="8dd69-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="8dd69-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="8dd69-207">Click **Add** button.</span></span> <span data-ttu-id="8dd69-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8dd69-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="8dd69-210">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="8dd69-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8dd69-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8dd69-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8dd69-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8dd69-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8dd69-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8dd69-213">Testing single sign-on</span></span>

<span data-ttu-id="8dd69-214">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8dd69-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="8dd69-215">När du klickar på panelen Tango Analytics på åtkomstpanelen du bör få automatiskt loggat in på ditt Tango Analytics-program.</span><span class="sxs-lookup"><span data-stu-id="8dd69-215">When you click the Tango Analytics tile in the Access Panel, you should get automatically signed-on to your Tango Analytics application.</span></span>
<span data-ttu-id="8dd69-216">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8dd69-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8dd69-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="8dd69-217">Additional resources</span></span>

* [<span data-ttu-id="8dd69-218">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8dd69-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8dd69-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8dd69-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tangoanalytics-tutorial/tutorial_general_203.png

