---
title: "Självstudier: Azure Active Directory-integrering med BeeLine | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och BeeLine."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0726859d-1dac-44a0-810b-da56d89039ee
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 93acbd90bbe5f0a40bf3f56edb766a0fdd30f68f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-beeline"></a><span data-ttu-id="ed70b-103">Självstudier: Azure Active Directory-integrering med BeeLine</span><span class="sxs-lookup"><span data-stu-id="ed70b-103">Tutorial: Azure Active Directory integration with BeeLine</span></span>

<span data-ttu-id="ed70b-104">I kursen får lära du att integrera BeeLine med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ed70b-104">In this tutorial, you learn how to integrate BeeLine with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ed70b-105">Integrera BeeLine med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ed70b-105">Integrating BeeLine with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ed70b-106">Du kan styra i Azure AD som har åtkomst till BeeLine</span><span class="sxs-lookup"><span data-stu-id="ed70b-106">You can control in Azure AD who has access to BeeLine</span></span>
- <span data-ttu-id="ed70b-107">Du kan aktivera användarna att automatiskt hämta loggat in på BeeLine (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ed70b-107">You can enable your users to automatically get signed-on to BeeLine (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ed70b-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ed70b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ed70b-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ed70b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ed70b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ed70b-110">Prerequisites</span></span>

<span data-ttu-id="ed70b-111">För att konfigurera Azure AD-integrering med BeeLine, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="ed70b-111">To configure Azure AD integration with BeeLine, you need the following items:</span></span>

- <span data-ttu-id="ed70b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ed70b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ed70b-113">En BeeLine enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="ed70b-113">A BeeLine single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ed70b-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ed70b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ed70b-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ed70b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ed70b-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ed70b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ed70b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ed70b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ed70b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ed70b-118">Scenario description</span></span>
<span data-ttu-id="ed70b-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ed70b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ed70b-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ed70b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ed70b-121">Att lägga till BeeLine från galleriet</span><span class="sxs-lookup"><span data-stu-id="ed70b-121">Adding BeeLine from the gallery</span></span>
2. <span data-ttu-id="ed70b-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ed70b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-beeline-from-the-gallery"></a><span data-ttu-id="ed70b-123">Att lägga till BeeLine från galleriet</span><span class="sxs-lookup"><span data-stu-id="ed70b-123">Adding BeeLine from the gallery</span></span>
<span data-ttu-id="ed70b-124">Du måste lägga till BeeLine från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av BeeLine i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ed70b-124">To configure the integration of BeeLine into Azure AD, you need to add BeeLine from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ed70b-125">**Utför följande steg för att lägga till BeeLine från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="ed70b-125">**To add BeeLine from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ed70b-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ed70b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ed70b-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ed70b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ed70b-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ed70b-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="ed70b-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ed70b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="ed70b-133">I sökrutan skriver **BeeLine**.</span><span class="sxs-lookup"><span data-stu-id="ed70b-133">In the search box, type **BeeLine**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_search.png)

5. <span data-ttu-id="ed70b-135">Välj i resultatpanelen **BeeLine**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="ed70b-135">In the results panel, select **BeeLine**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ed70b-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ed70b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ed70b-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med BeeLine baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ed70b-138">In this section, you configure and test Azure AD single sign-on with BeeLine based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ed70b-139">Azure AD måste du känna till användaren i BeeLine motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="ed70b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BeeLine is to a user in Azure AD.</span></span> <span data-ttu-id="ed70b-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i BeeLine upprättas.</span><span class="sxs-lookup"><span data-stu-id="ed70b-140">In other words, a link relationship between an Azure AD user and the related user in BeeLine needs to be established.</span></span>

<span data-ttu-id="ed70b-141">I BeeLine, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="ed70b-141">In BeeLine, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ed70b-142">Om du vill konfigurera och testa Azure AD enkel inloggning med BeeLine, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ed70b-142">To configure and test Azure AD single sign-on with BeeLine, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ed70b-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ed70b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ed70b-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ed70b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ed70b-145">**[Skapa en testanvändare BeeLine](#creating-a-beeline-test-user)**  – du har en motsvarighet för Britta Simon i BeeLine som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ed70b-145">**[Creating a BeeLine test user](#creating-a-beeline-test-user)** - to have a counterpart of Britta Simon in BeeLine that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ed70b-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ed70b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ed70b-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ed70b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ed70b-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ed70b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ed70b-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt BeeLine program.</span><span class="sxs-lookup"><span data-stu-id="ed70b-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BeeLine application.</span></span>

<span data-ttu-id="ed70b-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med BeeLine:**</span><span class="sxs-lookup"><span data-stu-id="ed70b-150">**To configure Azure AD single sign-on with BeeLine, perform the following steps:**</span></span>

1. <span data-ttu-id="ed70b-151">I Azure-portalen på den **BeeLine** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ed70b-151">In the Azure portal, on the **BeeLine** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="ed70b-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ed70b-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_samlbase.png)

3. <span data-ttu-id="ed70b-155">På den **BeeLine domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ed70b-155">On the **BeeLine Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_url.png)

    <span data-ttu-id="ed70b-157">a.</span><span class="sxs-lookup"><span data-stu-id="ed70b-157">a.</span></span> <span data-ttu-id="ed70b-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://projects.beeline.net/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="ed70b-158">In the **Identifier** textbox, type a URL using the following pattern: `https://projects.beeline.net/<instancename>`</span></span>

    <span data-ttu-id="ed70b-159">b.</span><span class="sxs-lookup"><span data-stu-id="ed70b-159">b.</span></span> <span data-ttu-id="ed70b-160">I den **Reply URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="ed70b-160">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://projects.beeline.net/<instancename>/SSO_External.ashx`|
    | `https://projects.beeline.net/<companyname>/SSO_External.ashx` |

    > [!NOTE] 
    > <span data-ttu-id="ed70b-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="ed70b-161">These values are not real.</span></span> <span data-ttu-id="ed70b-162">Uppdatera dessa värden med den faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="ed70b-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="ed70b-163">Kontakta [BeeLine supportteamet](https://www.beeline.com/contact-us/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="ed70b-163">Contact [BeeLine support team](https://www.beeline.com/contact-us/) to get these values.</span></span>
 
4. <span data-ttu-id="ed70b-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="ed70b-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_certificate.png) 

5. <span data-ttu-id="ed70b-166">Tillämpningsprogrammet Beeline förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="ed70b-166">Your Beeline application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="ed70b-167">Se tillsammans med [BeeLine supportteamet](https://www.beeline.com/contact-us/) först för att identifiera rätt användar-ID som ska mappas till programmet.</span><span class="sxs-lookup"><span data-stu-id="ed70b-167">Please work with [BeeLine support team](https://www.beeline.com/contact-us/) first to identify the correct user identifier which will be mapped into the application.</span></span> <span data-ttu-id="ed70b-168">Utför även vägledning från [BeeLine supportteamet](https://www.beeline.com/contact-us/) om attributet som de vill använda för att mappningen.</span><span class="sxs-lookup"><span data-stu-id="ed70b-168">Also please take the guidance from [BeeLine support team](https://www.beeline.com/contact-us/) about the attribute which they want to use for this mapping.</span></span> <span data-ttu-id="ed70b-169">Du kan hantera värdet för det här attributet från den **användarattribut** för programmet.</span><span class="sxs-lookup"><span data-stu-id="ed70b-169">You can manage the value of this attribute from the **User Attributes** tab of the application.</span></span> <span data-ttu-id="ed70b-170">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="ed70b-170">The following screenshot shows an example for this.</span></span> <span data-ttu-id="ed70b-171">Här vi har mappat den **användar-ID** anspråk med den **userprincipalname** attribut som innehåller unika användar-ID, som kommer att skickas till programmet Beeline i varje lyckad SAML-svaret.</span><span class="sxs-lookup"><span data-stu-id="ed70b-171">Here we have mapped the **User Identifier** claim with the **userprincipalname** attribute, which provides unique user ID, which will be sent to the Beeline application in the every successful SAML Response.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-beeline-tutorial/tutorial_attribute.png)  

6. <span data-ttu-id="ed70b-173">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ed70b-173">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-beeline-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="ed70b-175">På den **BeeLine Configuration** klickar du på **konfigurera BeeLine** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="ed70b-175">On the **BeeLine Configuration** section, click **Configure BeeLine** to open **Configure sign-on** window.</span></span> <span data-ttu-id="ed70b-176">Kopiera den **Sign-Out URL** och **SAML enhets-ID** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="ed70b-176">Copy the **Sign-Out URL** and **SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_configure.png) 

8. <span data-ttu-id="ed70b-178">Konfigurera enkel inloggning på **BeeLine** sida, måste du skicka den hämtade **XML-Metadata för** och **SAML enhets-ID**, **Sign-Out URL** till [BeeLine supportteamet](https://www.beeline.com/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="ed70b-178">To configure single sign-on on **BeeLine** side, you need to send the downloaded **Metadata XML** and **SAML Entity ID**, **Sign-Out URL** to [BeeLine support team](https://www.beeline.com/contact-us/).</span></span>

> [!TIP]
> <span data-ttu-id="ed70b-179">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="ed70b-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ed70b-180">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="ed70b-180">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ed70b-181">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ed70b-181">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ed70b-182">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ed70b-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="ed70b-183">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ed70b-183">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="ed70b-185">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="ed70b-185">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ed70b-186">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ed70b-186">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ed70b-188">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ed70b-188">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ed70b-190">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ed70b-190">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ed70b-192">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ed70b-192">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ed70b-194">a.</span><span class="sxs-lookup"><span data-stu-id="ed70b-194">a.</span></span> <span data-ttu-id="ed70b-195">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ed70b-195">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ed70b-196">b.</span><span class="sxs-lookup"><span data-stu-id="ed70b-196">b.</span></span> <span data-ttu-id="ed70b-197">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ed70b-197">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ed70b-198">c.</span><span class="sxs-lookup"><span data-stu-id="ed70b-198">c.</span></span> <span data-ttu-id="ed70b-199">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ed70b-199">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ed70b-200">d.</span><span class="sxs-lookup"><span data-stu-id="ed70b-200">d.</span></span> <span data-ttu-id="ed70b-201">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ed70b-201">Click **Create**.</span></span>
 
### <a name="creating-a-beeline-test-user"></a><span data-ttu-id="ed70b-202">Skapa en testanvändare BeeLine</span><span class="sxs-lookup"><span data-stu-id="ed70b-202">Creating a BeeLine test user</span></span>

<span data-ttu-id="ed70b-203">I det här avsnittet skapar du en användare som kallas Britta Simon i Beeline.</span><span class="sxs-lookup"><span data-stu-id="ed70b-203">In this section, you create a user called Britta Simon in Beeline.</span></span> <span data-ttu-id="ed70b-204">Beeline programmet måste alla användare som ska etableras i programmet innan du utför enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ed70b-204">Beeline application needs all the users to be provisioned in the application before doing Single Sign On.</span></span> <span data-ttu-id="ed70b-205">Så fungerar med den [BeeLine supportteamet](https://www.beeline.com/contact-us/) att etablera dessa användare till programmet.</span><span class="sxs-lookup"><span data-stu-id="ed70b-205">So work with the [BeeLine support team](https://www.beeline.com/contact-us/) to provision all these users into the application.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ed70b-206">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="ed70b-206">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ed70b-207">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till BeeLine.</span><span class="sxs-lookup"><span data-stu-id="ed70b-207">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BeeLine.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="ed70b-209">**Om du vill tilldela BeeLine Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ed70b-209">**To assign Britta Simon to BeeLine, perform the following steps:**</span></span>

1. <span data-ttu-id="ed70b-210">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ed70b-210">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ed70b-212">Välj i listan med program **BeeLine**.</span><span class="sxs-lookup"><span data-stu-id="ed70b-212">In the applications list, select **BeeLine**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_app.png) 

3. <span data-ttu-id="ed70b-214">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ed70b-214">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="ed70b-216">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ed70b-216">Click **Add** button.</span></span> <span data-ttu-id="ed70b-217">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ed70b-217">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="ed70b-219">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="ed70b-219">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ed70b-220">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ed70b-220">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ed70b-221">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ed70b-221">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ed70b-222">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ed70b-222">Testing single sign-on</span></span>

<span data-ttu-id="ed70b-223">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="ed70b-223">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span> <span data-ttu-id="ed70b-224">När du klickar på panelen Beeline på åtkomstpanelen du bör få automatiskt loggat in på ditt Beeline program.</span><span class="sxs-lookup"><span data-stu-id="ed70b-224">When you click the Beeline tile in the Access Panel, you should get automatically signed-on to your Beeline application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed70b-225">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ed70b-225">Additional resources</span></span>

* [<span data-ttu-id="ed70b-226">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ed70b-226">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ed70b-227">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ed70b-227">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_203.png

