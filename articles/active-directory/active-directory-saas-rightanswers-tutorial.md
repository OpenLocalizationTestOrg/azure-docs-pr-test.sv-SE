---
title: "Självstudier: Azure Active Directory-integrering med RightAnswers | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och RightAnswers."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7f09e25a-a716-41e1-8ca3-fd00e3d1b8cc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: e5985831598a0e5b1277d2c6cd02b03c919aad4d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rightanswers"></a><span data-ttu-id="3f1cf-103">Självstudier: Azure Active Directory-integrering med RightAnswers</span><span class="sxs-lookup"><span data-stu-id="3f1cf-103">Tutorial: Azure Active Directory integration with RightAnswers</span></span>

<span data-ttu-id="3f1cf-104">I kursen får lära du att integrera RightAnswers med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3f1cf-104">In this tutorial, you learn how to integrate RightAnswers with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3f1cf-105">Integrera RightAnswers med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3f1cf-105">Integrating RightAnswers with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3f1cf-106">Du kan styra i Azure AD som har åtkomst till RightAnswers</span><span class="sxs-lookup"><span data-stu-id="3f1cf-106">You can control in Azure AD who has access to RightAnswers</span></span>
- <span data-ttu-id="3f1cf-107">Du kan aktivera användarna att automatiskt hämta loggat in på RightAnswers (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="3f1cf-107">You can enable your users to automatically get signed-on to RightAnswers (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3f1cf-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3f1cf-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3f1cf-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3f1cf-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f1cf-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3f1cf-110">Prerequisites</span></span>

<span data-ttu-id="3f1cf-111">För att konfigurera Azure AD-integrering med RightAnswers, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="3f1cf-111">To configure Azure AD integration with RightAnswers, you need the following items:</span></span>

- <span data-ttu-id="3f1cf-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3f1cf-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3f1cf-113">En RightAnswers enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="3f1cf-113">A RightAnswers single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3f1cf-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3f1cf-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="3f1cf-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3f1cf-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3f1cf-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3f1cf-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3f1cf-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="3f1cf-118">Scenario description</span></span>
<span data-ttu-id="3f1cf-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3f1cf-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="3f1cf-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3f1cf-121">Att lägga till RightAnswers från galleriet</span><span class="sxs-lookup"><span data-stu-id="3f1cf-121">Adding RightAnswers from the gallery</span></span>
2. <span data-ttu-id="3f1cf-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3f1cf-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-rightanswers-from-the-gallery"></a><span data-ttu-id="3f1cf-123">Att lägga till RightAnswers från galleriet</span><span class="sxs-lookup"><span data-stu-id="3f1cf-123">Adding RightAnswers from the gallery</span></span>
<span data-ttu-id="3f1cf-124">Du måste lägga till RightAnswers från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av RightAnswers i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-124">To configure the integration of RightAnswers into Azure AD, you need to add RightAnswers from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3f1cf-125">**Utför följande steg för att lägga till RightAnswers från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="3f1cf-125">**To add RightAnswers from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3f1cf-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3f1cf-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3f1cf-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="3f1cf-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="3f1cf-133">I sökrutan skriver **RightAnswers**.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-133">In the search box, type **RightAnswers**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_search.png)

5. <span data-ttu-id="3f1cf-135">Välj i resultatpanelen **RightAnswers**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-135">In the results panel, select **RightAnswers**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3f1cf-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3f1cf-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3f1cf-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med RightAnswers baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-138">In this section, you configure and test Azure AD single sign-on with RightAnswers based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3f1cf-139">Azure AD måste du känna till användaren i RightAnswers motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-139">For single sign-on to work, Azure AD needs to know what the counterpart user in RightAnswers is to a user in Azure AD.</span></span> <span data-ttu-id="3f1cf-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i RightAnswers upprättas.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-140">In other words, a link relationship between an Azure AD user and the related user in RightAnswers needs to be established.</span></span>

<span data-ttu-id="3f1cf-141">I RightAnswers, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-141">In RightAnswers, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3f1cf-142">Om du vill konfigurera och testa Azure AD enkel inloggning med RightAnswers, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="3f1cf-142">To configure and test Azure AD single sign-on with RightAnswers, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3f1cf-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3f1cf-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3f1cf-145">**[Skapa en testanvändare RightAnswers](#creating-a-rightanswers-test-user)**  – du har en motsvarighet för Britta Simon i RightAnswers som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-145">**[Creating a RightAnswers test user](#creating-a-rightanswers-test-user)** - to have a counterpart of Britta Simon in RightAnswers that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3f1cf-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3f1cf-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3f1cf-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3f1cf-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3f1cf-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt RightAnswers program.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your RightAnswers application.</span></span>

<span data-ttu-id="3f1cf-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med RightAnswers:**</span><span class="sxs-lookup"><span data-stu-id="3f1cf-150">**To configure Azure AD single sign-on with RightAnswers, perform the following steps:**</span></span>

1. <span data-ttu-id="3f1cf-151">I Azure-portalen på den **RightAnswers** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-151">In the Azure portal, on the **RightAnswers** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="3f1cf-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_samlbase.png)

3. <span data-ttu-id="3f1cf-155">På den **RightAnswers domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3f1cf-155">On the **RightAnswers Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_url.png)

    <span data-ttu-id="3f1cf-157">a.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-157">a.</span></span> <span data-ttu-id="3f1cf-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.rightanswers.com/portal/ss/`</span><span class="sxs-lookup"><span data-stu-id="3f1cf-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.rightanswers.com/portal/ss/`</span></span>

    <span data-ttu-id="3f1cf-159">b.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-159">b.</span></span> <span data-ttu-id="3f1cf-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.rightanswers.com:<identifier>/portal`</span><span class="sxs-lookup"><span data-stu-id="3f1cf-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.rightanswers.com:<identifier>/portal`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3f1cf-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-161">These values are not real.</span></span> <span data-ttu-id="3f1cf-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="3f1cf-163">Kontakta [RightAnswers klienten supportteamet](https://www.rightanswers.com/contact-us/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-163">Contact [RightAnswers Client support team](https://www.rightanswers.com/contact-us/) to get these values.</span></span> 
 
4. <span data-ttu-id="3f1cf-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_certificate.png) 

5. <span data-ttu-id="3f1cf-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightanswers-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3f1cf-168">Konfigurera enkel inloggning på **RightAnswers** sida, måste du skicka den hämtade **XML-Metadata för** till [RightAnswers supportteam](https://www.rightanswers.com/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="3f1cf-168">To configure single sign-on on **RightAnswers** side, you need to send the downloaded **Metadata XML** to [RightAnswers support team](https://www.rightanswers.com/contact-us/).</span></span>

    >[!NOTE]
    ><span data-ttu-id="3f1cf-169">Supportteamet RightAnswers har att göra den faktiska SSO-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-169">Your RightAnswers support team has to do the actual SSO configuration.</span></span>
    ><span data-ttu-id="3f1cf-170">Du får ett meddelande när enkel inloggning har aktiverats för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-170">You will get a notification when SSO has been enabled for your subscription.</span></span>

> [!TIP]
> <span data-ttu-id="3f1cf-171">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="3f1cf-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3f1cf-172">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3f1cf-173">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3f1cf-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3f1cf-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="3f1cf-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="3f1cf-175">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="3f1cf-177">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="3f1cf-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3f1cf-178">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rightanswers-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3f1cf-180">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-180">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rightanswers-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3f1cf-182">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-182">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rightanswers-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3f1cf-184">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3f1cf-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rightanswers-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3f1cf-186">a.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-186">a.</span></span> <span data-ttu-id="3f1cf-187">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3f1cf-188">b.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-188">b.</span></span> <span data-ttu-id="3f1cf-189">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3f1cf-190">c.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-190">c.</span></span> <span data-ttu-id="3f1cf-191">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3f1cf-192">d.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-192">d.</span></span> <span data-ttu-id="3f1cf-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-193">Click **Create**.</span></span>
 
### <a name="creating-a-rightanswers-test-user"></a><span data-ttu-id="3f1cf-194">Skapa en testanvändare RightAnswers</span><span class="sxs-lookup"><span data-stu-id="3f1cf-194">Creating a RightAnswers test user</span></span>

<span data-ttu-id="3f1cf-195">Om du vill aktivera Azure AD-användare kan logga in på RightAnswers etableras de i RightAnswers.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-195">To enable Azure AD users to log in to RightAnswers, they must be provisioned into RightAnswers.</span></span> <span data-ttu-id="3f1cf-196">När RightAnswers, etablering är en automatisk uppgift så att det finns ingen åtgärd-objekt.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-196">When RightAnswers, provisioning is an automated task so there is no action item for you.</span></span>

<span data-ttu-id="3f1cf-197">Användare skapas automatiskt om det behövs under det första enkla inloggning försöket.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-197">Users are automatically created if necessary during the first single sign-on attempt.</span></span>

>[!NOTE]
><span data-ttu-id="3f1cf-198">Du kan använda något annat RightAnswers användarens konto skapas verktyg eller API: er som tillhandahålls av RightAnswers att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-198">You can use any other RightAnswers user account creation tools or APIs provided by RightAnswers to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3f1cf-199">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="3f1cf-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3f1cf-200">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till RightAnswers.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to RightAnswers.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="3f1cf-202">**Om du vill tilldela RightAnswers Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3f1cf-202">**To assign Britta Simon to RightAnswers, perform the following steps:**</span></span>

1. <span data-ttu-id="3f1cf-203">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="3f1cf-205">Välj i listan med program **RightAnswers**.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-205">In the applications list, select **RightAnswers**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_app.png) 

3. <span data-ttu-id="3f1cf-207">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="3f1cf-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-209">Click **Add** button.</span></span> <span data-ttu-id="3f1cf-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="3f1cf-212">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3f1cf-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3f1cf-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3f1cf-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3f1cf-215">Testing single sign-on</span></span>

<span data-ttu-id="3f1cf-216">Om du vill testa inställningarna för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="3f1cf-216">If you want to test your SSO settings, open the Access Panel.</span></span> <span data-ttu-id="3f1cf-217">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3f1cf-217">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>
## <a name="additional-resources"></a><span data-ttu-id="3f1cf-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3f1cf-218">Additional resources</span></span>

* [<span data-ttu-id="3f1cf-219">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3f1cf-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3f1cf-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3f1cf-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_203.png

