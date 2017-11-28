---
title: "Självstudier: Azure Active Directory-integrering med Tidemark | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Tidemark."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5cf80d4e-6e8b-48ec-81c8-27872af5e5d5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 170dc58363b12ec671c2fab8c80c7720d3dbf352
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tidemark"></a><span data-ttu-id="00333-103">Självstudier: Azure Active Directory-integrering med Tidemark</span><span class="sxs-lookup"><span data-stu-id="00333-103">Tutorial: Azure Active Directory integration with Tidemark</span></span>

<span data-ttu-id="00333-104">I kursen får lära du att integrera Tidemark med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="00333-104">In this tutorial, you learn how to integrate Tidemark with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="00333-105">Integrera Tidemark med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="00333-105">Integrating Tidemark with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="00333-106">Du kan styra i Azure AD som har åtkomst till Tidemark</span><span class="sxs-lookup"><span data-stu-id="00333-106">You can control in Azure AD who has access to Tidemark</span></span>
- <span data-ttu-id="00333-107">Du kan aktivera användarna att automatiskt hämta loggat in på Tidemark (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="00333-107">You can enable your users to automatically get signed-on to Tidemark (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="00333-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="00333-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="00333-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="00333-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00333-110">Krav</span><span class="sxs-lookup"><span data-stu-id="00333-110">Prerequisites</span></span>

<span data-ttu-id="00333-111">För att konfigurera Azure AD-integrering med Tidemark, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="00333-111">To configure Azure AD integration with Tidemark, you need the following items:</span></span>

- <span data-ttu-id="00333-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="00333-112">An Azure AD subscription</span></span>
- <span data-ttu-id="00333-113">En Tidemark enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="00333-113">A Tidemark single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="00333-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="00333-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="00333-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="00333-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="00333-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="00333-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="00333-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="00333-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="00333-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="00333-118">Scenario description</span></span>
<span data-ttu-id="00333-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="00333-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="00333-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="00333-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="00333-121">Att lägga till Tidemark från galleriet</span><span class="sxs-lookup"><span data-stu-id="00333-121">Adding Tidemark from the gallery</span></span>
2. <span data-ttu-id="00333-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="00333-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tidemark-from-the-gallery"></a><span data-ttu-id="00333-123">Att lägga till Tidemark från galleriet</span><span class="sxs-lookup"><span data-stu-id="00333-123">Adding Tidemark from the gallery</span></span>
<span data-ttu-id="00333-124">Du måste lägga till Tidemark från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Tidemark i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="00333-124">To configure the integration of Tidemark into Azure AD, you need to add Tidemark from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="00333-125">**Utför följande steg för att lägga till Tidemark från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="00333-125">**To add Tidemark from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="00333-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="00333-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="00333-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="00333-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="00333-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="00333-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="00333-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="00333-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="00333-133">I sökrutan skriver **Tidemark**.</span><span class="sxs-lookup"><span data-stu-id="00333-133">In the search box, type **Tidemark**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_search.png)

5. <span data-ttu-id="00333-135">Välj i resultatpanelen **Tidemark**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="00333-135">In the results panel, select **Tidemark**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="00333-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="00333-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="00333-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Tidemark baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="00333-138">In this section, you configure and test Azure AD single sign-on with Tidemark based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="00333-139">Azure AD måste du känna till användaren i Tidemark motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="00333-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Tidemark is to a user in Azure AD.</span></span> <span data-ttu-id="00333-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Tidemark upprättas.</span><span class="sxs-lookup"><span data-stu-id="00333-140">In other words, a link relationship between an Azure AD user and the related user in Tidemark needs to be established.</span></span>

<span data-ttu-id="00333-141">I Tidemark, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="00333-141">In Tidemark, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="00333-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Tidemark, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="00333-142">To configure and test Azure AD single sign-on with Tidemark, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="00333-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="00333-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="00333-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="00333-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="00333-145">**[Skapa en testanvändare Tidemark](#creating-a-tidemark-test-user)**  – du har en motsvarighet för Britta Simon i Tidemark som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="00333-145">**[Creating a Tidemark test user](#creating-a-tidemark-test-user)** - to have a counterpart of Britta Simon in Tidemark that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="00333-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="00333-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="00333-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="00333-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="00333-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="00333-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="00333-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Tidemark program.</span><span class="sxs-lookup"><span data-stu-id="00333-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Tidemark application.</span></span>

<span data-ttu-id="00333-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Tidemark:**</span><span class="sxs-lookup"><span data-stu-id="00333-150">**To configure Azure AD single sign-on with Tidemark, perform the following steps:**</span></span>

1. <span data-ttu-id="00333-151">I Azure-portalen på den **Tidemark** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="00333-151">In the Azure portal, on the **Tidemark** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="00333-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="00333-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_samlbase.png)

3. <span data-ttu-id="00333-155">På den **Tidemark domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="00333-155">On the **Tidemark Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_url.png)

    <span data-ttu-id="00333-157">a.</span><span class="sxs-lookup"><span data-stu-id="00333-157">a.</span></span> <span data-ttu-id="00333-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="00333-158">In the **Sign-on URL** textbox, type a URL using the following patterns:</span></span> 
    | |
    |--|
    | `https://<subdomain>.tidemark.com/login` |
    | `https://<subdomain>.tidemark.net/login` |

    <span data-ttu-id="00333-159">b.</span><span class="sxs-lookup"><span data-stu-id="00333-159">b.</span></span> <span data-ttu-id="00333-160">I den **identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="00333-160">In the **Identifier** textbox, type a URL using the following patterns:</span></span> 
    | |
    |--|
    | `https://<subdomain>.tidemark.com/saml` |
    | `https://<subdomain>.tidemark.net/saml` |

    > [!NOTE] 
    > <span data-ttu-id="00333-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="00333-161">These values are not real.</span></span> <span data-ttu-id="00333-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="00333-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="00333-163">Kontakta [Tidemark klienten supportteamet](http://www.tidemark.com/contact-us) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="00333-163">Contact [Tidemark Client support team](http://www.tidemark.com/contact-us) to get these values.</span></span> 
 
4. <span data-ttu-id="00333-164">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="00333-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_certificate.png) 

5. <span data-ttu-id="00333-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="00333-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tidemark-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="00333-168">På den **Tidemark Configuration** klickar du på **konfigurera Tidemark** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="00333-168">On the **Tidemark Configuration** section, click **Configure Tidemark** to open **Configure sign-on** window.</span></span> <span data-ttu-id="00333-169">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="00333-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_configure.png) 

7. <span data-ttu-id="00333-171">Konfigurera enkel inloggning på **Tidemark** sida, måste du skicka den hämtade **Certificate(Base64), SAML enhets-ID, Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** till [Tidemark supportteam](http://www.tidemark.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="00333-171">To configure single sign-on on **Tidemark** side, you need to send the downloaded **Certificate(Base64), SAML Entity ID, Sign-Out URL and SAML Single Sign-On Service URL** to [Tidemark support team](http://www.tidemark.com/contact-us).</span></span> <span data-ttu-id="00333-172">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="00333-172">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="00333-173">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="00333-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="00333-174">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="00333-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="00333-175">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="00333-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="00333-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="00333-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="00333-177">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="00333-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="00333-179">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="00333-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="00333-180">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="00333-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="00333-182">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="00333-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="00333-184">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="00333-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="00333-186">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="00333-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tidemark-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="00333-188">a.</span><span class="sxs-lookup"><span data-stu-id="00333-188">a.</span></span> <span data-ttu-id="00333-189">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="00333-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="00333-190">b.</span><span class="sxs-lookup"><span data-stu-id="00333-190">b.</span></span> <span data-ttu-id="00333-191">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="00333-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="00333-192">c.</span><span class="sxs-lookup"><span data-stu-id="00333-192">c.</span></span> <span data-ttu-id="00333-193">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="00333-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="00333-194">d.</span><span class="sxs-lookup"><span data-stu-id="00333-194">d.</span></span> <span data-ttu-id="00333-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="00333-195">Click **Create**.</span></span>
 
### <a name="creating-a-tidemark-test-user"></a><span data-ttu-id="00333-196">Skapa en testanvändare Tidemark</span><span class="sxs-lookup"><span data-stu-id="00333-196">Creating a Tidemark test user</span></span>

<span data-ttu-id="00333-197">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Tidemark.</span><span class="sxs-lookup"><span data-stu-id="00333-197">The objective of this section is to create a user called Britta Simon in Tidemark.</span></span> <span data-ttu-id="00333-198">Se tillsammans med [Tidemark supportteamet](http://www.tidemark.com/contact-us) att lägga till användare i Tidemark-konto.</span><span class="sxs-lookup"><span data-stu-id="00333-198">Please work with [Tidemark support team](http://www.tidemark.com/contact-us) to add the users in the Tidemark account.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="00333-199">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="00333-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="00333-200">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Tidemark.</span><span class="sxs-lookup"><span data-stu-id="00333-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Tidemark.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="00333-202">**Om du vill tilldela Tidemark Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="00333-202">**To assign Britta Simon to Tidemark, perform the following steps:**</span></span>

1. <span data-ttu-id="00333-203">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="00333-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="00333-205">Välj i listan med program **Tidemark**.</span><span class="sxs-lookup"><span data-stu-id="00333-205">In the applications list, select **Tidemark**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tidemark-tutorial/tutorial_tidemark_app.png) 

3. <span data-ttu-id="00333-207">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="00333-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="00333-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="00333-209">Click **Add** button.</span></span> <span data-ttu-id="00333-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="00333-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="00333-212">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="00333-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="00333-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="00333-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="00333-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="00333-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="00333-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="00333-215">Testing single sign-on</span></span>

<span data-ttu-id="00333-216">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="00333-216">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="00333-217">När du klickar på panelen Tidemark på åtkomstpanelen du bör få automatiskt loggat in på ditt Tidemark program.</span><span class="sxs-lookup"><span data-stu-id="00333-217">When you click the Tidemark tile in the Access Panel, you should get automatically signed-on to your Tidemark application.</span></span>
<span data-ttu-id="00333-218">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="00333-218">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="00333-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="00333-219">Additional resources</span></span>

* [<span data-ttu-id="00333-220">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="00333-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="00333-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="00333-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tidemark-tutorial/tutorial_general_203.png

