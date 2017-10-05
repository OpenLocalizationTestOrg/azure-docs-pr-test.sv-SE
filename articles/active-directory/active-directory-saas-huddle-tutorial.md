---
title: "Självstudier: Azure Active Directory-integrering med Huddle | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Huddle."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8389ba4c-f5f8-4ede-b2f4-32eae844ceb0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 59d4019545d39ec76bf401696338140f430630c9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-huddle"></a><span data-ttu-id="563a5-103">Självstudier: Azure Active Directory-integrering med Huddle</span><span class="sxs-lookup"><span data-stu-id="563a5-103">Tutorial: Azure Active Directory integration with Huddle</span></span>

<span data-ttu-id="563a5-104">I kursen får lära du att integrera Huddle med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="563a5-104">In this tutorial, you learn how to integrate Huddle with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="563a5-105">Integrera Huddle med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="563a5-105">Integrating Huddle with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="563a5-106">Du kan styra i Azure AD som har åtkomst till Huddle</span><span class="sxs-lookup"><span data-stu-id="563a5-106">You can control in Azure AD who has access to Huddle</span></span>
- <span data-ttu-id="563a5-107">Du kan aktivera användarna att automatiskt hämta loggat in på Huddle (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="563a5-107">You can enable your users to automatically get signed-on to Huddle (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="563a5-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="563a5-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="563a5-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="563a5-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="563a5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="563a5-110">Prerequisites</span></span>

<span data-ttu-id="563a5-111">För att konfigurera Azure AD-integrering med Huddle, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="563a5-111">To configure Azure AD integration with Huddle, you need the following items:</span></span>

- <span data-ttu-id="563a5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="563a5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="563a5-113">En Huddle enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="563a5-113">A Huddle single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="563a5-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="563a5-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="563a5-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="563a5-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="563a5-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="563a5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="563a5-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="563a5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="563a5-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="563a5-118">Scenario description</span></span>

<span data-ttu-id="563a5-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="563a5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="563a5-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="563a5-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="563a5-121">Att lägga till Huddle från galleriet</span><span class="sxs-lookup"><span data-stu-id="563a5-121">Adding Huddle from the gallery</span></span>
2. <span data-ttu-id="563a5-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="563a5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-huddle-from-the-gallery"></a><span data-ttu-id="563a5-123">Att lägga till Huddle från galleriet</span><span class="sxs-lookup"><span data-stu-id="563a5-123">Adding Huddle from the gallery</span></span>
<span data-ttu-id="563a5-124">Du måste lägga till Huddle från galleriet i listan över hanterade SaaS-appar för att konfigurera Huddle till Azure AD-integrering.</span><span class="sxs-lookup"><span data-stu-id="563a5-124">To configure the integration of Huddle into Azure AD, you need to add Huddle from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="563a5-125">**Utför följande steg för att lägga till Huddle från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="563a5-125">**To add Huddle from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="563a5-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="563a5-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="563a5-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="563a5-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="563a5-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="563a5-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="563a5-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="563a5-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="563a5-133">I sökrutan skriver **Huddle**.</span><span class="sxs-lookup"><span data-stu-id="563a5-133">In the search box, type **Huddle**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_search.png)

5. <span data-ttu-id="563a5-135">Välj i resultatpanelen **Huddle**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="563a5-135">In the results panel, select **Huddle**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="563a5-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="563a5-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="563a5-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Huddle baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="563a5-138">In this section, you configure and test Azure AD single sign-on with Huddle based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="563a5-139">Azure AD måste du känna till användaren i Huddle motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="563a5-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Huddle is to a user in Azure AD.</span></span> <span data-ttu-id="563a5-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Huddle upprättas.</span><span class="sxs-lookup"><span data-stu-id="563a5-140">In other words, a link relationship between an Azure AD user and the related user in Huddle needs to be established.</span></span>

<span data-ttu-id="563a5-141">I Huddle, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="563a5-141">In Huddle, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="563a5-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Huddle, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="563a5-142">To configure and test Azure AD single sign-on with Huddle, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="563a5-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="563a5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>

2. <span data-ttu-id="563a5-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="563a5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>

3. <span data-ttu-id="563a5-145">**[Skapa en testanvändare Huddle](#creating-a-huddle-test-user)**  – du har en motsvarighet för Britta Simon i Huddle som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="563a5-145">**[Creating a Huddle test user](#creating-a-huddle-test-user)** - to have a counterpart of Britta Simon in Huddle that is linked to the Azure AD representation of user.</span></span>

4. <span data-ttu-id="563a5-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="563a5-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>

5. <span data-ttu-id="563a5-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="563a5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="563a5-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="563a5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="563a5-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Huddle program.</span><span class="sxs-lookup"><span data-stu-id="563a5-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Huddle application.</span></span>

<span data-ttu-id="563a5-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Huddle:**</span><span class="sxs-lookup"><span data-stu-id="563a5-150">**To configure Azure AD single sign-on with Huddle, perform the following steps:**</span></span>

1. <span data-ttu-id="563a5-151">I Azure-portalen på den **Huddle** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="563a5-151">In the Azure portal, on the **Huddle** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="563a5-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="563a5-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_samlbase.png)

3. <span data-ttu-id="563a5-155">På den **Huddle domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="563a5-155">On the **Huddle Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_url.png)

    <span data-ttu-id="563a5-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`http://<company name>.huddle.com`</span><span class="sxs-lookup"><span data-stu-id="563a5-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `http://<company name>.huddle.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="563a5-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="563a5-158">This value is not real.</span></span> <span data-ttu-id="563a5-159">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="563a5-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="563a5-160">Kontakta [Huddle klienten supportteamet](https://huddle.zendesk.com) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="563a5-160">Contact [Huddle Client support team](https://huddle.zendesk.com) to get this value.</span></span> 

4. <span data-ttu-id="563a5-161">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="563a5-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_certificate.png) 

5. <span data-ttu-id="563a5-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="563a5-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-huddle-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="563a5-165">På den **Huddle Configuration** klickar du på **konfigurera Huddle** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="563a5-165">On the **Huddle Configuration** section, click **Configure Huddle** to open **Configure sign-on** window.</span></span> <span data-ttu-id="563a5-166">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="563a5-166">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_configure.png) 
    
7. <span data-ttu-id="563a5-168">Om du vill konfigurera enkel inloggning på Huddle sida, måste du skicka den hämtade **certifikat**, **SAML enkel inloggning Tjänstwebbadress**, och **SAML enhets-ID** till [Huddle klienten supportteamet](https://huddle.zendesk.com).</span><span class="sxs-lookup"><span data-stu-id="563a5-168">To configure single sign-on on Huddle side, you need to send the downloaded  **Certificate**, **SAML Single Sign-On Service URL**, and **SAML Entity ID** to [Huddle Client support team](https://huddle.zendesk.com).</span></span> <span data-ttu-id="563a5-169">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="563a5-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>  
   
    >[!NOTE]
    > <span data-ttu-id="563a5-170">Enkel inloggning måste aktiveras av supportteamet Huddle.</span><span class="sxs-lookup"><span data-stu-id="563a5-170">Single sign-on needs to be enabled by the Huddle support team.</span></span> <span data-ttu-id="563a5-171">Du får ett meddelande när konfigurationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="563a5-171">You get a notification when the configuration has been completed.</span></span> 
    > 

> [!TIP]
> <span data-ttu-id="563a5-172">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="563a5-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="563a5-173">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="563a5-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="563a5-174">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="563a5-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 
   
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="563a5-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="563a5-175">Creating an Azure AD test user</span></span>

<span data-ttu-id="563a5-176">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="563a5-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="563a5-178">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="563a5-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="563a5-179">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="563a5-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-huddle-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="563a5-181">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="563a5-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-huddle-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="563a5-183">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="563a5-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-huddle-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="563a5-185">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="563a5-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-huddle-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="563a5-187">a.</span><span class="sxs-lookup"><span data-stu-id="563a5-187">a.</span></span> <span data-ttu-id="563a5-188">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="563a5-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="563a5-189">b.</span><span class="sxs-lookup"><span data-stu-id="563a5-189">b.</span></span> <span data-ttu-id="563a5-190">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="563a5-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="563a5-191">c.</span><span class="sxs-lookup"><span data-stu-id="563a5-191">c.</span></span> <span data-ttu-id="563a5-192">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="563a5-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="563a5-193">d.</span><span class="sxs-lookup"><span data-stu-id="563a5-193">d.</span></span> <span data-ttu-id="563a5-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="563a5-194">Click **Create**.</span></span>
 
### <a name="creating-a-huddle-test-user"></a><span data-ttu-id="563a5-195">Skapa en testanvändare Huddle</span><span class="sxs-lookup"><span data-stu-id="563a5-195">Creating a Huddle test user</span></span>

<span data-ttu-id="563a5-196">Om du vill aktivera Azure AD-användare kan logga in på Huddle etableras de i Huddle.</span><span class="sxs-lookup"><span data-stu-id="563a5-196">To enable Azure AD users to log in to Huddle, they must be provisioned into Huddle.</span></span> <span data-ttu-id="563a5-197">När det gäller Huddle är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="563a5-197">In the case of Huddle, provisioning is a manual task.</span></span>

<span data-ttu-id="563a5-198">**Utför följande steg för att konfigurera användaretablering:**</span><span class="sxs-lookup"><span data-stu-id="563a5-198">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="563a5-199">Logga in på ditt **Huddle** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="563a5-199">Log in to your **Huddle** company site as administrator.</span></span>
2. <span data-ttu-id="563a5-200">Klicka på **arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="563a5-200">Click **Workspace**.</span></span>
3. <span data-ttu-id="563a5-201">Klicka på **personer \> bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="563a5-201">Click **People \> Invite People**.</span></span>
   
   <span data-ttu-id="563a5-202">![Personer](./media/active-directory-saas-huddle-tutorial/IC787838.png "personer")</span><span class="sxs-lookup"><span data-stu-id="563a5-202">![People](./media/active-directory-saas-huddle-tutorial/IC787838.png "People")</span></span>

4. <span data-ttu-id="563a5-203">I den **skapa en ny inbjudan** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="563a5-203">In the **Create a new invitation** section, perform the following steps:</span></span>
   
   <span data-ttu-id="563a5-204">![Ny inbjudan](./media/active-directory-saas-huddle-tutorial/IC787839.png "ny inbjudan")</span><span class="sxs-lookup"><span data-stu-id="563a5-204">![New Invitation](./media/active-directory-saas-huddle-tutorial/IC787839.png "New Invitation")</span></span>
   
   <span data-ttu-id="563a5-205">a.</span><span class="sxs-lookup"><span data-stu-id="563a5-205">a.</span></span> <span data-ttu-id="563a5-206">I den **väljer du ett team att bjuda in personer att delta i** väljer **team**.</span><span class="sxs-lookup"><span data-stu-id="563a5-206">In the **Choose a team to invite people to join** list, select **team**.</span></span>

   <span data-ttu-id="563a5-207">b.</span><span class="sxs-lookup"><span data-stu-id="563a5-207">b.</span></span> <span data-ttu-id="563a5-208">Typ av **e-postadress** av ett giltigt Azure AD-kontot som du vill etablera i att **ange e-postadress till personer som du vill bjuda in** textruta.</span><span class="sxs-lookup"><span data-stu-id="563a5-208">Type the **Email Address** of a valid Azure AD account you want to provision in to **Enter email address for people you'd like to invite** textbox.</span></span>

   <span data-ttu-id="563a5-209">c.</span><span class="sxs-lookup"><span data-stu-id="563a5-209">c.</span></span> <span data-ttu-id="563a5-210">Klicka på **bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="563a5-210">Click **Invite**.</span></span>   
   
    >[!NOTE]
    > <span data-ttu-id="563a5-211">Azure AD-kontoinnehavaren får ett e-postmeddelande med en länk för att bekräfta kontot innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="563a5-211">The Azure AD account holder will receive an email including a link to confirm the account before it becomes active.</span></span> 
    > 

>[!NOTE]
><span data-ttu-id="563a5-212">Du kan använda andra Huddle användare skapa verktyg eller API: er som tillhandahålls av Huddle för att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="563a5-212">You can use any other Huddle user account creation tools or APIs provided by Huddle to provision Azure AD user accounts.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="563a5-213">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="563a5-213">Assigning the Azure AD test user</span></span>

<span data-ttu-id="563a5-214">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Huddle.</span><span class="sxs-lookup"><span data-stu-id="563a5-214">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Huddle.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="563a5-216">**Om du vill tilldela Huddle Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="563a5-216">**To assign Britta Simon to Huddle, perform the following steps:**</span></span>

1. <span data-ttu-id="563a5-217">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="563a5-217">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="563a5-219">Välj i listan med program **Huddle**.</span><span class="sxs-lookup"><span data-stu-id="563a5-219">In the applications list, select **Huddle**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_app.png) 

3. <span data-ttu-id="563a5-221">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="563a5-221">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="563a5-223">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="563a5-223">Click **Add** button.</span></span> <span data-ttu-id="563a5-224">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="563a5-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="563a5-226">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="563a5-226">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="563a5-227">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="563a5-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="563a5-228">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="563a5-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="563a5-229">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="563a5-229">Testing single sign-on</span></span>

<span data-ttu-id="563a5-230">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="563a5-230">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="563a5-231">När du klickar på panelen Huddle på åtkomstpanelen ska du automatiskt hämta inloggningssidan för Huddle program.</span><span class="sxs-lookup"><span data-stu-id="563a5-231">When you click the Huddle tile in the Access Panel, you should get automatically login page of Huddle application.</span></span>
<span data-ttu-id="563a5-232">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="563a5-232">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="563a5-233">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="563a5-233">Additional resources</span></span>

* [<span data-ttu-id="563a5-234">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="563a5-234">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="563a5-235">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="563a5-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_04.png
[100]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_100.png
[200]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_203.png
