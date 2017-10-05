---
title: "Självstudier: Azure Active Directory-integrering med Learning på arbetet | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Learning på arbetet."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d607174-bea1-4f40-8233-54cabe02c66a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: jeedes
ms.openlocfilehash: 941832740689c583a8e857d706c35f3076fa754f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learning-at-work"></a><span data-ttu-id="6c1b7-103">Självstudier: Azure Active Directory-integrering med Learning på arbetet</span><span class="sxs-lookup"><span data-stu-id="6c1b7-103">Tutorial: Azure Active Directory integration with Learning at Work</span></span>

<span data-ttu-id="6c1b7-104">I kursen får lära du att integrera Learning på arbetet med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6c1b7-104">In this tutorial, you learn how to integrate Learning at Work with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6c1b7-105">Integrera Learning på arbetet med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="6c1b7-105">Integrating Learning at Work with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="6c1b7-106">Du kan styra i Azure AD som har åtkomst till Learning på arbetet</span><span class="sxs-lookup"><span data-stu-id="6c1b7-106">You can control in Azure AD who has access to Learning at Work</span></span>
- <span data-ttu-id="6c1b7-107">Du kan aktivera användarna att automatiskt hämta loggat in på Learning på arbetet (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="6c1b7-107">You can enable your users to automatically get signed-on to Learning at Work (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6c1b7-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6c1b7-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="6c1b7-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6c1b7-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6c1b7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="6c1b7-110">Prerequisites</span></span>

<span data-ttu-id="6c1b7-111">För att konfigurera Azure AD-integrering med Learning på arbetet, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="6c1b7-111">To configure Azure AD integration with Learning at Work, you need the following items:</span></span>

- <span data-ttu-id="6c1b7-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="6c1b7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6c1b7-113">En Learning vid arbete enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="6c1b7-113">A Learning at Work single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6c1b7-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6c1b7-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="6c1b7-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6c1b7-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6c1b7-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6c1b7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6c1b7-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="6c1b7-118">Scenario description</span></span>
<span data-ttu-id="6c1b7-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6c1b7-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="6c1b7-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6c1b7-121">Att lägga till Learning på arbetet från galleriet</span><span class="sxs-lookup"><span data-stu-id="6c1b7-121">Adding Learning at Work from the gallery</span></span>
2. <span data-ttu-id="6c1b7-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6c1b7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learning-at-work-from-the-gallery"></a><span data-ttu-id="6c1b7-123">Att lägga till Learning på arbetet från galleriet</span><span class="sxs-lookup"><span data-stu-id="6c1b7-123">Adding Learning at Work from the gallery</span></span>
<span data-ttu-id="6c1b7-124">Du måste lägga till Learning på arbetet från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Learning på arbetet till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-124">To configure the integration of Learning at Work into Azure AD, you need to add Learning at Work from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6c1b7-125">**Utför följande steg för att lägga till Learning på arbetet från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="6c1b7-125">**To add Learning at Work from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="6c1b7-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6c1b7-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="6c1b7-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="6c1b7-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="6c1b7-133">I sökrutan skriver **Learning på arbetet**.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-133">In the search box, type **Learning at Work**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_search.png)

5. <span data-ttu-id="6c1b7-135">Välj i resultatpanelen **Learning på arbetet**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-135">In the results panel, select **Learning at Work**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6c1b7-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6c1b7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6c1b7-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Learning på arbetet baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-138">In this section, you configure and test Azure AD single sign-on with Learning at Work based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6c1b7-139">Azure AD måste du känna till motsvarande användaren i Learning på jobbet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Learning at Work is to a user in Azure AD.</span></span> <span data-ttu-id="6c1b7-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Learning på arbetet upprättas.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-140">In other words, a link relationship between an Azure AD user and the related user in Learning at Work needs to be established.</span></span>

<span data-ttu-id="6c1b7-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Learning på arbetet.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Learning at Work.</span></span>

<span data-ttu-id="6c1b7-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Learning på arbetet, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="6c1b7-142">To configure and test Azure AD single sign-on with Learning at Work, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="6c1b7-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="6c1b7-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6c1b7-145">**[Skapa en Learning vid arbete testanvändare](#creating-a-learning-at-work-test-user)**  – du har en motsvarighet för Britta Simon i Learning på arbetet som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-145">**[Creating a Learning at Work test user](#creating-a-learning-at-work-test-user)** - to have a counterpart of Britta Simon in Learning at Work that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="6c1b7-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6c1b7-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6c1b7-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6c1b7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6c1b7-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din utbildning vid arbete program.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Learning at Work application.</span></span>

<span data-ttu-id="6c1b7-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Learning på arbetet:**</span><span class="sxs-lookup"><span data-stu-id="6c1b7-150">**To configure Azure AD single sign-on with Learning at Work, perform the following steps:**</span></span>

1. <span data-ttu-id="6c1b7-151">I Azure-portalen på den **Learning på arbetet** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-151">In the Azure portal, on the **Learning at Work** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="6c1b7-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_samlbase.png)

3. <span data-ttu-id="6c1b7-155">På den **Learning vid arbete domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="6c1b7-155">On the **Learning at Work Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_url.png)

    <span data-ttu-id="6c1b7-157">a.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-157">a.</span></span> <span data-ttu-id="6c1b7-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.sabacloud.com/Saba/Web/<company code>`</span><span class="sxs-lookup"><span data-stu-id="6c1b7-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.sabacloud.com/Saba/Web/<company code>`</span></span>

    <span data-ttu-id="6c1b7-159">b.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-159">b.</span></span> <span data-ttu-id="6c1b7-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.sabacloud.com/Saba/saml/SSO/alias/<company name>`</span><span class="sxs-lookup"><span data-stu-id="6c1b7-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.sabacloud.com/Saba/saml/SSO/alias/<company name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6c1b7-161">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-161">These values are not the real.</span></span> <span data-ttu-id="6c1b7-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="6c1b7-163">Kontakta [Learning vid arbete klienten supportteamet](https://www.learninga-z.com/site/contact/support) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-163">Contact [Learning at Work Client support team](https://www.learninga-z.com/site/contact/support) to get these values.</span></span> 
 
4. <span data-ttu-id="6c1b7-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_certificate.png) 

5. <span data-ttu-id="6c1b7-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6c1b7-168">På den **Learning vid arbete konfigurationen** klickar du på **konfigurera Learning på arbetet** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-168">On the **Learning at Work Configuration** section, click **Configure Learning at Work** to open **Configure sign-on** window.</span></span> <span data-ttu-id="6c1b7-169">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="6c1b7-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_configure.png) 

7. <span data-ttu-id="6c1b7-171">Konfigurera enkel inloggning på **Learning på arbetet** sida, måste du skicka den hämtade **XML-Metadata för**, **SAML enhets-ID**, **SAML enkel inloggning Tjänstwebbadress**, och **Sign-Out URL** till [Learning på arbetet support](https://www.learninga-z.com/site/contact/support).</span><span class="sxs-lookup"><span data-stu-id="6c1b7-171">To configure single sign-on on **Learning at Work** side, you need to send the downloaded **Metadata XML**, **SAML Entity ID**, **SAML Single Sign-On Service URL**, and **Sign-Out URL** to [Learning at Work support](https://www.learninga-z.com/site/contact/support).</span></span>

> [!TIP]
> <span data-ttu-id="6c1b7-172">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="6c1b7-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="6c1b7-173">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="6c1b7-174">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6c1b7-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6c1b7-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="6c1b7-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="6c1b7-176">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="6c1b7-178">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="6c1b7-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="6c1b7-179">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6c1b7-181">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6c1b7-183">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6c1b7-185">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="6c1b7-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6c1b7-187">a.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-187">a.</span></span> <span data-ttu-id="6c1b7-188">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6c1b7-189">b.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-189">b.</span></span> <span data-ttu-id="6c1b7-190">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6c1b7-191">c.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-191">c.</span></span> <span data-ttu-id="6c1b7-192">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="6c1b7-193">d.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-193">d.</span></span> <span data-ttu-id="6c1b7-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-194">Click **Create**.</span></span>
 
### <a name="creating-a-learning-at-work-test-user"></a><span data-ttu-id="6c1b7-195">Skapa en Learning vid arbete testanvändare</span><span class="sxs-lookup"><span data-stu-id="6c1b7-195">Creating a Learning at Work test user</span></span>

<span data-ttu-id="6c1b7-196">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Learning på arbetet.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-196">In this section, you create a user called Britta Simon in Learning at Work.</span></span> <span data-ttu-id="6c1b7-197">Arbeta med [Learning på arbetet support](https://www.learninga-z.com/site/contact/support) att lägga till användare i Learning vid arbete plattform.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-197">Work with [Learning at Work support](https://www.learninga-z.com/site/contact/support) to add the users in the Learning at Work platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="6c1b7-198">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="6c1b7-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="6c1b7-199">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Learning på arbetet.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Learning at Work.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="6c1b7-201">**Om du vill tilldela Learning på arbetet Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6c1b7-201">**To assign Britta Simon to Learning at Work, perform the following steps:**</span></span>

1. <span data-ttu-id="6c1b7-202">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="6c1b7-204">Välj i listan med program **Learning på arbetet**.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-204">In the applications list, select **Learning at Work**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_app.png) 

3. <span data-ttu-id="6c1b7-206">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="6c1b7-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-208">Click **Add** button.</span></span> <span data-ttu-id="6c1b7-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="6c1b7-211">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="6c1b7-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6c1b7-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6c1b7-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6c1b7-214">Testing single sign-on</span></span>

<span data-ttu-id="6c1b7-215">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-215">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="6c1b7-216">När du klickar på Learning vid arbete panelen på åtkomstpanelen du bör få automatiskt loggat in på din utbildning vid arbete program.</span><span class="sxs-lookup"><span data-stu-id="6c1b7-216">When you click the Learning at Work tile in the Access Panel, you should get automatically signed-on to your Learning at Work application.</span></span>
<span data-ttu-id="6c1b7-217">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6c1b7-217">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6c1b7-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="6c1b7-218">Additional resources</span></span>

* [<span data-ttu-id="6c1b7-219">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6c1b7-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6c1b7-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6c1b7-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_203.png

