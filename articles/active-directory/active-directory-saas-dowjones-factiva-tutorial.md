---
title: "Självstudier: Azure Active Directory-integrering med önster Karlsson Factiva | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och önster Karlsson Factiva."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b36e97e8-37a6-4096-a894-530427ee1331
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2017
ms.author: jeedes
ms.openlocfilehash: dab48c24ff25fd68df1ee540bb8f0929e7e81bcb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-dow-jones-factiva"></a><span data-ttu-id="70ca6-103">Självstudier: Azure Active Directory-integrering med önster Karlsson Factiva</span><span class="sxs-lookup"><span data-stu-id="70ca6-103">Tutorial: Azure Active Directory integration with Dow Jones Factiva</span></span>

<span data-ttu-id="70ca6-104">I kursen får lära du att integrera önster Karlsson Factiva med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="70ca6-104">In this tutorial, you learn how to integrate Dow Jones Factiva with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="70ca6-105">Integrera önster Karlsson Factiva med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="70ca6-105">Integrating Dow Jones Factiva with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="70ca6-106">Du kan styra i Azure AD som har åtkomst till önster Karlsson Factiva</span><span class="sxs-lookup"><span data-stu-id="70ca6-106">You can control in Azure AD who has access to Dow Jones Factiva</span></span>
- <span data-ttu-id="70ca6-107">Du kan aktivera användarna att automatiskt hämta loggat in på önster Karlsson Factiva (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="70ca6-107">You can enable your users to automatically get signed-on to Dow Jones Factiva (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="70ca6-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="70ca6-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="70ca6-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="70ca6-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="70ca6-110">Krav</span><span class="sxs-lookup"><span data-stu-id="70ca6-110">Prerequisites</span></span>

<span data-ttu-id="70ca6-111">För att konfigurera Azure AD-integrering med önster Karlsson Factiva, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="70ca6-111">To configure Azure AD integration with Dow Jones Factiva, you need the following items:</span></span>

- <span data-ttu-id="70ca6-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="70ca6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="70ca6-113">En önster Karlsson Factiva enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="70ca6-113">A Dow Jones Factiva single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="70ca6-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="70ca6-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="70ca6-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="70ca6-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="70ca6-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="70ca6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="70ca6-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="70ca6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="70ca6-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="70ca6-118">Scenario description</span></span>
<span data-ttu-id="70ca6-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="70ca6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="70ca6-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="70ca6-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="70ca6-121">Att lägga till önster Karlsson Factiva från galleriet</span><span class="sxs-lookup"><span data-stu-id="70ca6-121">Adding Dow Jones Factiva from the gallery</span></span>
2. <span data-ttu-id="70ca6-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="70ca6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-dow-jones-factiva-from-the-gallery"></a><span data-ttu-id="70ca6-123">Att lägga till önster Karlsson Factiva från galleriet</span><span class="sxs-lookup"><span data-stu-id="70ca6-123">Adding Dow Jones Factiva from the gallery</span></span>
<span data-ttu-id="70ca6-124">Du måste lägga till önster Karlsson Factiva från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av önster Karlsson Factiva i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="70ca6-124">To configure the integration of Dow Jones Factiva into Azure AD, you need to add Dow Jones Factiva from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="70ca6-125">**Utför följande steg för att lägga till önster Karlsson Factiva från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="70ca6-125">**To add Dow Jones Factiva from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="70ca6-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="70ca6-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="70ca6-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="70ca6-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="70ca6-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="70ca6-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="70ca6-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="70ca6-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="70ca6-133">I sökrutan skriver **önster Karlsson Factiva**.</span><span class="sxs-lookup"><span data-stu-id="70ca6-133">In the search box, type **Dow Jones Factiva**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_dowjonesfactiva_search.png)

5. <span data-ttu-id="70ca6-135">Välj i resultatpanelen **önster Karlsson Factiva**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="70ca6-135">In the results panel, select **Dow Jones Factiva**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_dowjonesfactiva_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="70ca6-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="70ca6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="70ca6-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med önster Karlsson Factiva baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="70ca6-138">In this section, you configure and test Azure AD single sign-on with Dow Jones Factiva based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="70ca6-139">Azure AD måste du känna till användaren i önster Karlsson Factiva motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="70ca6-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Dow Jones Factiva is to a user in Azure AD.</span></span> <span data-ttu-id="70ca6-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i önster Karlsson Factiva upprättas.</span><span class="sxs-lookup"><span data-stu-id="70ca6-140">In other words, a link relationship between an Azure AD user and the related user in Dow Jones Factiva needs to be established.</span></span>

<span data-ttu-id="70ca6-141">I önster Karlsson Factiva tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="70ca6-141">In Dow Jones Factiva, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="70ca6-142">Om du vill konfigurera och testa Azure AD enkel inloggning med önster Karlsson Factiva, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="70ca6-142">To configure and test Azure AD single sign-on with Dow Jones Factiva, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="70ca6-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="70ca6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="70ca6-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="70ca6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="70ca6-145">**[Skapa en testanvändare önster Karlsson Factiva](#creating-a-dow-jones-factiva-test-user)**  – har en motsvarighet för Britta Simon önster Karlsson Factiva som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="70ca6-145">**[Creating a Dow Jones Factiva test user](#creating-a-dow-jones-factiva-test-user)** - to have a counterpart of Britta Simon in Dow Jones Factiva that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="70ca6-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="70ca6-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="70ca6-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="70ca6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="70ca6-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="70ca6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="70ca6-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet önster Karlsson Factiva.</span><span class="sxs-lookup"><span data-stu-id="70ca6-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Dow Jones Factiva application.</span></span>

<span data-ttu-id="70ca6-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med önster Karlsson Factiva:**</span><span class="sxs-lookup"><span data-stu-id="70ca6-150">**To configure Azure AD single sign-on with Dow Jones Factiva, perform the following steps:**</span></span>

1. <span data-ttu-id="70ca6-151">I Azure-portalen på den **önster Karlsson Factiva** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="70ca6-151">In the Azure portal, on the **Dow Jones Factiva** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="70ca6-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="70ca6-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_dowjonesfactiva_samlbase.png)

3. <span data-ttu-id="70ca6-155">På den **önster Karlsson Factiva domän och URL: er** avsnittet användaren behöver inte utföra några steg som appen före redan är integrerad med Azure.</span><span class="sxs-lookup"><span data-stu-id="70ca6-155">On the **Dow Jones Factiva Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_dowjonesfactiva_url.png)

4. <span data-ttu-id="70ca6-157">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="70ca6-157">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_dowjonesfactiva_certificate.png) 

5. <span data-ttu-id="70ca6-159">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="70ca6-159">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="70ca6-161">Konfigurera enkel inloggning på **önster Karlsson Factiva** sida, måste du skicka den hämtade **XML-Metadata för** till [önster Karlsson Factiva supportteamet](https://www.dowjones.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="70ca6-161">To configure single sign-on on **Dow Jones Factiva** side, you need to send the downloaded **Metadata XML** to [Dow Jones Factiva support team](https://www.dowjones.com/contact/).</span></span> <span data-ttu-id="70ca6-162">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="70ca6-162">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="70ca6-163">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="70ca6-163">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="70ca6-164">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="70ca6-164">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="70ca6-165">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="70ca6-165">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="70ca6-166">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="70ca6-166">Creating an Azure AD test user</span></span>
<span data-ttu-id="70ca6-167">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="70ca6-167">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="70ca6-169">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="70ca6-169">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="70ca6-170">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="70ca6-170">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dowjones-factiva-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="70ca6-172">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="70ca6-172">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dowjones-factiva-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="70ca6-174">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="70ca6-174">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dowjones-factiva-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="70ca6-176">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="70ca6-176">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dowjones-factiva-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="70ca6-178">a.</span><span class="sxs-lookup"><span data-stu-id="70ca6-178">a.</span></span> <span data-ttu-id="70ca6-179">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="70ca6-179">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="70ca6-180">b.</span><span class="sxs-lookup"><span data-stu-id="70ca6-180">b.</span></span> <span data-ttu-id="70ca6-181">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="70ca6-181">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="70ca6-182">c.</span><span class="sxs-lookup"><span data-stu-id="70ca6-182">c.</span></span> <span data-ttu-id="70ca6-183">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="70ca6-183">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="70ca6-184">d.</span><span class="sxs-lookup"><span data-stu-id="70ca6-184">d.</span></span> <span data-ttu-id="70ca6-185">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="70ca6-185">Click **Create**.</span></span>
 
### <a name="creating-a-dow-jones-factiva-test-user"></a><span data-ttu-id="70ca6-186">Skapa en testanvändare önster Karlsson Factiva</span><span class="sxs-lookup"><span data-stu-id="70ca6-186">Creating a Dow Jones Factiva test user</span></span>

<span data-ttu-id="70ca6-187">I det här avsnittet skapar du en användare som kallas Britta Simon i önster Karlsson Factiva.</span><span class="sxs-lookup"><span data-stu-id="70ca6-187">In this section, you create a user called Britta Simon in Dow Jones Factiva.</span></span> <span data-ttu-id="70ca6-188">Se tillsammans med önster [Karlsson Factiva supportteamet](https://www.dowjones.com/contact/) att lägga till användare i önster Karlsson Factiva-plattformen.</span><span class="sxs-lookup"><span data-stu-id="70ca6-188">Please work with Dow [Jones Factiva support team](https://www.dowjones.com/contact/) to add the users in the Dow Jones Factiva platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="70ca6-189">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="70ca6-189">Assigning the Azure AD test user</span></span>

<span data-ttu-id="70ca6-190">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till önster Karlsson Factiva.</span><span class="sxs-lookup"><span data-stu-id="70ca6-190">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Dow Jones Factiva.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="70ca6-192">**Om du vill tilldela önster Karlsson Factiva Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="70ca6-192">**To assign Britta Simon to Dow Jones Factiva, perform the following steps:**</span></span>

1. <span data-ttu-id="70ca6-193">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="70ca6-193">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="70ca6-195">Välj i listan med program **önster Karlsson Factiva**.</span><span class="sxs-lookup"><span data-stu-id="70ca6-195">In the applications list, select **Dow Jones Factiva**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_dowjonesfactiva_app.png) 

3. <span data-ttu-id="70ca6-197">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="70ca6-197">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="70ca6-199">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="70ca6-199">Click **Add** button.</span></span> <span data-ttu-id="70ca6-200">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="70ca6-200">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="70ca6-202">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="70ca6-202">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="70ca6-203">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="70ca6-203">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="70ca6-204">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="70ca6-204">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="70ca6-205">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="70ca6-205">Testing single sign-on</span></span>

<span data-ttu-id="70ca6-206">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="70ca6-206">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="70ca6-207">När du klickar på panelen önster Karlsson Factiva på åtkomstpanelen du bör få automatiskt loggat in på ditt önster Karlsson Factiva program.</span><span class="sxs-lookup"><span data-stu-id="70ca6-207">When you click the Dow Jones Factiva tile in the Access Panel, you should get automatically signed-on to your Dow Jones Factiva application.</span></span>
<span data-ttu-id="70ca6-208">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="70ca6-208">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="70ca6-209">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="70ca6-209">Additional resources</span></span>

* [<span data-ttu-id="70ca6-210">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="70ca6-210">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="70ca6-211">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="70ca6-211">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_203.png

