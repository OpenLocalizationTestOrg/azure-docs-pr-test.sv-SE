---
title: "Självstudier: Azure Active Directory-integrering med Trakopolis | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Trakopolis."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 73d67c3e-4b4b-4d3b-aa58-6699ea1ccea3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 3887cf8c085c30eb01ac769944da2fcfe3df81f3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-trakopolis"></a><span data-ttu-id="c0a3a-103">Självstudier: Azure Active Directory-integrering med Trakopolis</span><span class="sxs-lookup"><span data-stu-id="c0a3a-103">Tutorial: Azure Active Directory integration with Trakopolis</span></span>

<span data-ttu-id="c0a3a-104">I kursen får lära du att integrera Trakopolis med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c0a3a-104">In this tutorial, you learn how to integrate Trakopolis with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c0a3a-105">Integrera Trakopolis med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c0a3a-105">Integrating Trakopolis with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c0a3a-106">Du kan styra i Azure AD som har åtkomst till Trakopolis</span><span class="sxs-lookup"><span data-stu-id="c0a3a-106">You can control in Azure AD who has access to Trakopolis</span></span>
- <span data-ttu-id="c0a3a-107">Du kan aktivera användarna att automatiskt hämta loggat in på Trakopolis (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c0a3a-107">You can enable your users to automatically get signed-on to Trakopolis (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c0a3a-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c0a3a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c0a3a-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c0a3a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c0a3a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c0a3a-110">Prerequisites</span></span>

<span data-ttu-id="c0a3a-111">För att konfigurera Azure AD-integrering med Trakopolis, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="c0a3a-111">To configure Azure AD integration with Trakopolis, you need the following items:</span></span>

- <span data-ttu-id="c0a3a-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c0a3a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c0a3a-113">En Trakopolis enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="c0a3a-113">A Trakopolis single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c0a3a-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c0a3a-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c0a3a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c0a3a-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c0a3a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c0a3a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c0a3a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c0a3a-118">Scenario description</span></span>
<span data-ttu-id="c0a3a-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c0a3a-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c0a3a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c0a3a-121">Att lägga till Trakopolis från galleriet</span><span class="sxs-lookup"><span data-stu-id="c0a3a-121">Adding Trakopolis from the gallery</span></span>
2. <span data-ttu-id="c0a3a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c0a3a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-trakopolis-from-the-gallery"></a><span data-ttu-id="c0a3a-123">Att lägga till Trakopolis från galleriet</span><span class="sxs-lookup"><span data-stu-id="c0a3a-123">Adding Trakopolis from the gallery</span></span>
<span data-ttu-id="c0a3a-124">Du måste lägga till Trakopolis från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Trakopolis i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-124">To configure the integration of Trakopolis into Azure AD, you need to add Trakopolis from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c0a3a-125">**Utför följande steg för att lägga till Trakopolis från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="c0a3a-125">**To add Trakopolis from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c0a3a-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c0a3a-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c0a3a-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="c0a3a-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="c0a3a-133">I sökrutan skriver **Trakopolis**.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-133">In the search box, type **Trakopolis**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trakopolis-tutorial/tutorial_trakopolis_search.png)

5. <span data-ttu-id="c0a3a-135">Välj i resultatpanelen **Trakopolis**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-135">In the results panel, select **Trakopolis**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trakopolis-tutorial/tutorial_trakopolis_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c0a3a-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c0a3a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c0a3a-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Trakopolis baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-138">In this section, you configure and test Azure AD single sign-on with Trakopolis based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c0a3a-139">Azure AD måste du känna till användaren i Trakopolis motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Trakopolis is to a user in Azure AD.</span></span> <span data-ttu-id="c0a3a-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Trakopolis upprättas.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-140">In other words, a link relationship between an Azure AD user and the related user in Trakopolis needs to be established.</span></span>

<span data-ttu-id="c0a3a-141">I Trakopolis, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-141">In Trakopolis, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c0a3a-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Trakopolis, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c0a3a-142">To configure and test Azure AD single sign-on with Trakopolis, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c0a3a-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c0a3a-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c0a3a-145">**[Skapa en testanvändare Trakopolis](#creating-a-trakopolis-test-user)**  – du har en motsvarighet för Britta Simon i Trakopolis som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-145">**[Creating a Trakopolis test user](#creating-a-trakopolis-test-user)** - to have a counterpart of Britta Simon in Trakopolis that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c0a3a-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c0a3a-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c0a3a-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c0a3a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c0a3a-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Trakopolis program.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Trakopolis application.</span></span>

<span data-ttu-id="c0a3a-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Trakopolis:**</span><span class="sxs-lookup"><span data-stu-id="c0a3a-150">**To configure Azure AD single sign-on with Trakopolis, perform the following steps:**</span></span>

1. <span data-ttu-id="c0a3a-151">I Azure-portalen på den **Trakopolis** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-151">In the Azure portal, on the **Trakopolis** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c0a3a-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-trakopolis-tutorial/tutorial_trakopolis_samlbase.png)

3. <span data-ttu-id="c0a3a-155">På den **Trakopolis domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c0a3a-155">On the **Trakopolis Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trakopolis-tutorial/tutorial_trakopolis_url.png)

    <span data-ttu-id="c0a3a-157">a.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-157">a.</span></span> <span data-ttu-id="c0a3a-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<company name>.trakopolis.com/`</span><span class="sxs-lookup"><span data-stu-id="c0a3a-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.trakopolis.com/`</span></span>

    <span data-ttu-id="c0a3a-159">b.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-159">b.</span></span> <span data-ttu-id="c0a3a-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<company name>.trakopolis.com`</span><span class="sxs-lookup"><span data-stu-id="c0a3a-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.trakopolis.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c0a3a-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-161">These values are not real.</span></span> <span data-ttu-id="c0a3a-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="c0a3a-163">Kontakta [Trakopolis klienten supportteamet](mailto:support@cantelematics.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-163">Contact [Trakopolis Client support team](mailto:support@cantelematics.com) to get these values.</span></span> 

4. <span data-ttu-id="c0a3a-164">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trakopolis-tutorial/tutorial_trakopolis_certificate.png) 

5. <span data-ttu-id="c0a3a-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trakopolis-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c0a3a-168">På den **Trakopolis Configuration** klickar du på **konfigurera Trakopolis** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-168">On the **Trakopolis Configuration** section, click **Configure Trakopolis** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c0a3a-169">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="c0a3a-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trakopolis-tutorial/tutorial_trakopolis_configure.png) 

7. <span data-ttu-id="c0a3a-171">Konfigurera enkel inloggning på **Trakopolis** sida, måste du skicka den hämtade **XML-Metadata, Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** till [Trakopolis stöd team](mailto:support@cantelematics.com).</span><span class="sxs-lookup"><span data-stu-id="c0a3a-171">To configure single sign-on on **Trakopolis** side, you need to send the downloaded **Metadata XML, Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Trakopolis support team](mailto:support@cantelematics.com).</span></span> <span data-ttu-id="c0a3a-172">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-172">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="c0a3a-173">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="c0a3a-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c0a3a-174">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c0a3a-175">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c0a3a-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c0a3a-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c0a3a-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="c0a3a-177">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="c0a3a-179">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="c0a3a-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c0a3a-180">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trakopolis-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c0a3a-182">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trakopolis-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c0a3a-184">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trakopolis-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c0a3a-186">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c0a3a-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trakopolis-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c0a3a-188">a.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-188">a.</span></span> <span data-ttu-id="c0a3a-189">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c0a3a-190">b.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-190">b.</span></span> <span data-ttu-id="c0a3a-191">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c0a3a-192">c.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-192">c.</span></span> <span data-ttu-id="c0a3a-193">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c0a3a-194">d.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-194">d.</span></span> <span data-ttu-id="c0a3a-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-195">Click **Create**.</span></span>
 
### <a name="creating-a-trakopolis-test-user"></a><span data-ttu-id="c0a3a-196">Skapa en testanvändare Trakopolis</span><span class="sxs-lookup"><span data-stu-id="c0a3a-196">Creating a Trakopolis test user</span></span>

<span data-ttu-id="c0a3a-197">I det här avsnittet skapar du en användare som kallas Britta Simon i Trakopolis.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-197">In this section, you create a user called Britta Simon in Trakopolis.</span></span> <span data-ttu-id="c0a3a-198">Arbeta med [Trakopolis supportteam](mailto:support@cantelematics.com) att lägga till användare i Trakopolis-plattformen.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-198">Work with [Trakopolis support team](mailto:support@cantelematics.com) to add the users in the Trakopolis platform.</span></span> <span data-ttu-id="c0a3a-199">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-199">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c0a3a-200">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="c0a3a-200">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c0a3a-201">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Trakopolis.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-201">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Trakopolis.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="c0a3a-203">**Om du vill tilldela Trakopolis Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c0a3a-203">**To assign Britta Simon to Trakopolis, perform the following steps:**</span></span>

1. <span data-ttu-id="c0a3a-204">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-204">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c0a3a-206">Välj i listan med program **Trakopolis**.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-206">In the applications list, select **Trakopolis**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trakopolis-tutorial/tutorial_trakopolis_app.png) 

3. <span data-ttu-id="c0a3a-208">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-208">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="c0a3a-210">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-210">Click **Add** button.</span></span> <span data-ttu-id="c0a3a-211">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="c0a3a-213">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-213">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c0a3a-214">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c0a3a-215">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c0a3a-216">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c0a3a-216">Testing single sign-on</span></span>

<span data-ttu-id="c0a3a-217">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-217">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="c0a3a-218">När du klickar på panelen Trakopolis på åtkomstpanelen du bör få automatiskt loggat in på ditt Trakopolis program.</span><span class="sxs-lookup"><span data-stu-id="c0a3a-218">When you click the Trakopolis tile in the Access Panel, you should get automatically signed-on to your Trakopolis application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c0a3a-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c0a3a-219">Additional resources</span></span>

* [<span data-ttu-id="c0a3a-220">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c0a3a-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c0a3a-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c0a3a-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-trakopolis-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-trakopolis-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-trakopolis-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-trakopolis-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-trakopolis-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-trakopolis-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-trakopolis-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-trakopolis-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-trakopolis-tutorial/tutorial_general_203.png

