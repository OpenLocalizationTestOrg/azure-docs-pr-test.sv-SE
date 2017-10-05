---
title: "Självstudier: Azure Active Directory-integrering med Printix | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Printix."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4aea7320-b2d5-49e0-9b63-aeaff0f6fe66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 97dbb3fa0531f2f679badb6bb9752f2e42fc9cb3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-printix"></a><span data-ttu-id="d41e9-103">Självstudier: Azure Active Directory-integrering med Printix</span><span class="sxs-lookup"><span data-stu-id="d41e9-103">Tutorial: Azure Active Directory integration with Printix</span></span>

<span data-ttu-id="d41e9-104">I kursen får lära du att integrera Printix med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d41e9-104">In this tutorial, you learn how to integrate Printix with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d41e9-105">Integrera Printix med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d41e9-105">Integrating Printix with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d41e9-106">Du kan styra i Azure AD som har åtkomst till Printix</span><span class="sxs-lookup"><span data-stu-id="d41e9-106">You can control in Azure AD who has access to Printix</span></span>
- <span data-ttu-id="d41e9-107">Du kan aktivera användarna att automatiskt hämta loggat in på Printix (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d41e9-107">You can enable your users to automatically get signed-on to Printix (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d41e9-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d41e9-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d41e9-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d41e9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d41e9-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d41e9-110">Prerequisites</span></span>

<span data-ttu-id="d41e9-111">För att konfigurera Azure AD-integrering med Printix, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="d41e9-111">To configure Azure AD integration with Printix, you need the following items:</span></span>

- <span data-ttu-id="d41e9-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d41e9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d41e9-113">En Printix enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="d41e9-113">A Printix single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d41e9-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d41e9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d41e9-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d41e9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d41e9-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d41e9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d41e9-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d41e9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d41e9-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d41e9-118">Scenario description</span></span>
<span data-ttu-id="d41e9-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d41e9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d41e9-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d41e9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d41e9-121">Att lägga till Printix från galleriet</span><span class="sxs-lookup"><span data-stu-id="d41e9-121">Adding Printix from the gallery</span></span>
2. <span data-ttu-id="d41e9-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d41e9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-printix-from-the-gallery"></a><span data-ttu-id="d41e9-123">Att lägga till Printix från galleriet</span><span class="sxs-lookup"><span data-stu-id="d41e9-123">Adding Printix from the gallery</span></span>
<span data-ttu-id="d41e9-124">Du måste lägga till Printix från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Printix i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d41e9-124">To configure the integration of Printix into Azure AD, you need to add Printix from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d41e9-125">**Utför följande steg för att lägga till Printix från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="d41e9-125">**To add Printix from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d41e9-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d41e9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d41e9-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d41e9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d41e9-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d41e9-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d41e9-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d41e9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d41e9-133">I sökrutan skriver **Printix**.</span><span class="sxs-lookup"><span data-stu-id="d41e9-133">In the search box, type **Printix**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-printix-tutorial/tutorial_printix_search.png)

5. <span data-ttu-id="d41e9-135">Välj i resultatpanelen **Printix**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="d41e9-135">In the results panel, select **Printix**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-printix-tutorial/tutorial_printix_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d41e9-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d41e9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d41e9-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Printix baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d41e9-138">In this section, you configure and test Azure AD single sign-on with Printix based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d41e9-139">Azure AD måste du känna till användaren i Printix motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="d41e9-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Printix is to a user in Azure AD.</span></span> <span data-ttu-id="d41e9-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Printix upprättas.</span><span class="sxs-lookup"><span data-stu-id="d41e9-140">In other words, a link relationship between an Azure AD user and the related user in Printix needs to be established.</span></span>

<span data-ttu-id="d41e9-141">I Printix, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="d41e9-141">In Printix, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d41e9-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Printix, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d41e9-142">To configure and test Azure AD single sign-on with Printix, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d41e9-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d41e9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d41e9-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d41e9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d41e9-145">**[Skapa en testanvändare Printix](#creating-a-printix-test-user)**  – du har en motsvarighet för Britta Simon i Printix som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d41e9-145">**[Creating a Printix test user](#creating-a-printix-test-user)** - to have a counterpart of Britta Simon in Printix that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d41e9-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d41e9-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d41e9-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d41e9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d41e9-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d41e9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d41e9-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Printix program.</span><span class="sxs-lookup"><span data-stu-id="d41e9-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Printix application.</span></span>

<span data-ttu-id="d41e9-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Printix:**</span><span class="sxs-lookup"><span data-stu-id="d41e9-150">**To configure Azure AD single sign-on with Printix, perform the following steps:**</span></span>

1. <span data-ttu-id="d41e9-151">I Azure-portalen på den **Printix** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d41e9-151">In the Azure portal, on the **Printix** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d41e9-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d41e9-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-printix-tutorial/tutorial_printix_samlbase.png)

3. <span data-ttu-id="d41e9-155">På den **Printix domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d41e9-155">On the **Printix Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-printix-tutorial/tutorial_printix_url.png)

    <span data-ttu-id="d41e9-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.printix.net`</span><span class="sxs-lookup"><span data-stu-id="d41e9-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.printix.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d41e9-158">Värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="d41e9-158">The value is not real.</span></span> <span data-ttu-id="d41e9-159">Uppdatera värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="d41e9-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="d41e9-160">Kontakta [Printix klienten supportteamet](mailto:support@printix.net) värdet hämtas.</span><span class="sxs-lookup"><span data-stu-id="d41e9-160">Contact [Printix Client support team](mailto:support@printix.net) to get the value.</span></span> 
 
4. <span data-ttu-id="d41e9-161">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="d41e9-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-printix-tutorial/tutorial_printix_certificate.png) 

5. <span data-ttu-id="d41e9-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d41e9-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-printix-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d41e9-165">Inloggning till Printix-klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="d41e9-165">Sign-on to your Printix tenant as an administrator.</span></span>

7. <span data-ttu-id="d41e9-166">Klicka på ikonen i det övre högra hörnet på överst menyn och välj ”**autentisering**”.</span><span class="sxs-lookup"><span data-stu-id="d41e9-166">In the menu on the top, click the icon at the upper right corner and select "**Authentication**".</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-printix-tutorial/tutorial_printix_06.png)

8. <span data-ttu-id="d41e9-168">På den **installationsprogrammet** väljer **aktivera Azure/Office 365-autentisering**</span><span class="sxs-lookup"><span data-stu-id="d41e9-168">On the **Setup** tab, select **Enable Azure/Office 365 authentication**</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-printix-tutorial/tutorial_printix_07.png)

9. <span data-ttu-id="d41e9-170">På den **Azure** fliken, inkommande URL: en i federation metadata till textrutan för ”**Federation Metadatadokumentet**”.</span><span class="sxs-lookup"><span data-stu-id="d41e9-170">On the **Azure** tab, input federation metadata URL to the textbox of "**Federation metadata document**".</span></span> 

    <span data-ttu-id="d41e9-171">Koppla metadata xml-filen som du hämtade från Azure AD för att [Printix supportteamet](mailto:support@printix.net).</span><span class="sxs-lookup"><span data-stu-id="d41e9-171">Attach the metadata xml file which you downloaded from Azure AD to [Printix support team](mailto:support@printix.net).</span></span> <span data-ttu-id="d41e9-172">Sedan de överför XML-filen och ange en Webbadress för federation metadata.</span><span class="sxs-lookup"><span data-stu-id="d41e9-172">Then they upload the xml file and provide a federation metadata URL.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-printix-tutorial/tutorial_printix_08.png)
   
10. <span data-ttu-id="d41e9-174">Klicka på ”**testa**” och klicka sedan på ”**OK**” knappen om testet lyckades.</span><span class="sxs-lookup"><span data-stu-id="d41e9-174">Click the "**Test**" button and click "**OK**" button if the test was successful.</span></span>
   
     <span data-ttu-id="d41e9-175">Azure active directory-sidan visas när du klickar på den **testa** knappen.</span><span class="sxs-lookup"><span data-stu-id="d41e9-175">Azure active directory page will show after clicking the **test** button.</span></span> <span data-ttu-id="d41e9-176">”Testet lyckades” innebär här att när du har angett autentiseringsuppgifterna för ditt testkonto för Azure som det visas ett meddelande ”inställningar testas OK”. Klicka på den **OK** knappen.</span><span class="sxs-lookup"><span data-stu-id="d41e9-176">"The test was successful" here means after entering the credentials of your Azure test account it will pop up a message "Settings tested OK".Then click the **OK** button.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-printix-tutorial/tutorial_printix_09.png)

11. <span data-ttu-id="d41e9-178">Klicka på den **spara** på knappen ”**autentisering**” sidan.</span><span class="sxs-lookup"><span data-stu-id="d41e9-178">Click the **Save** button on "**Authentication**" page.</span></span>


> [!TIP]
> <span data-ttu-id="d41e9-179">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="d41e9-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d41e9-180">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="d41e9-180">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d41e9-181">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d41e9-181">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d41e9-182">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d41e9-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="d41e9-183">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d41e9-183">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d41e9-185">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="d41e9-185">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d41e9-186">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d41e9-186">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d41e9-188">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d41e9-188">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d41e9-190">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d41e9-190">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d41e9-192">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d41e9-192">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-printix-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d41e9-194">a.</span><span class="sxs-lookup"><span data-stu-id="d41e9-194">a.</span></span> <span data-ttu-id="d41e9-195">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d41e9-195">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d41e9-196">b.</span><span class="sxs-lookup"><span data-stu-id="d41e9-196">b.</span></span> <span data-ttu-id="d41e9-197">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d41e9-197">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d41e9-198">c.</span><span class="sxs-lookup"><span data-stu-id="d41e9-198">c.</span></span> <span data-ttu-id="d41e9-199">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d41e9-199">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d41e9-200">d.</span><span class="sxs-lookup"><span data-stu-id="d41e9-200">d.</span></span> <span data-ttu-id="d41e9-201">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d41e9-201">Click **Create**.</span></span>
 
### <a name="creating-a-printix-test-user"></a><span data-ttu-id="d41e9-202">Skapa en testanvändare Printix</span><span class="sxs-lookup"><span data-stu-id="d41e9-202">Creating a Printix test user</span></span>

<span data-ttu-id="d41e9-203">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Printix.</span><span class="sxs-lookup"><span data-stu-id="d41e9-203">The objective of this section is to create a user called Britta Simon in Printix.</span></span> <span data-ttu-id="d41e9-204">Printix stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="d41e9-204">Printix supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="d41e9-205">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="d41e9-205">There is no action item for you in this section.</span></span> <span data-ttu-id="d41e9-206">En ny användare skapas under ett försök att komma åt Printix om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="d41e9-206">A new user is created during an attempt to access Printix if it doesn't exist yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="d41e9-207">Om du behöver skapa en användare manuellt, måste du kontakta den [Printix supportteamet](mailto:support@printix.net).</span><span class="sxs-lookup"><span data-stu-id="d41e9-207">If you need to create a user manually, you need to contact the [Printix support team](mailto:support@printix.net).</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d41e9-208">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="d41e9-208">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d41e9-209">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Printix.</span><span class="sxs-lookup"><span data-stu-id="d41e9-209">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Printix.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d41e9-211">**Om du vill tilldela Printix Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d41e9-211">**To assign Britta Simon to Printix, perform the following steps:**</span></span>

1. <span data-ttu-id="d41e9-212">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d41e9-212">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d41e9-214">Välj i listan med program **Printix**.</span><span class="sxs-lookup"><span data-stu-id="d41e9-214">In the applications list, select **Printix**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-printix-tutorial/tutorial_printix_app.png) 

3. <span data-ttu-id="d41e9-216">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d41e9-216">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d41e9-218">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d41e9-218">Click **Add** button.</span></span> <span data-ttu-id="d41e9-219">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d41e9-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d41e9-221">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="d41e9-221">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d41e9-222">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d41e9-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d41e9-223">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d41e9-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d41e9-224">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d41e9-224">Testing single sign-on</span></span>

<span data-ttu-id="d41e9-225">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="d41e9-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d41e9-226">När du klickar på panelen Printix på åtkomstpanelen du bör få automatiskt loggat in på ditt Printix program.</span><span class="sxs-lookup"><span data-stu-id="d41e9-226">When you click the Printix tile in the Access Panel, you should get automatically signed-on to your Printix application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d41e9-227">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d41e9-227">Additional resources</span></span>

* [<span data-ttu-id="d41e9-228">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d41e9-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d41e9-229">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d41e9-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-printix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-printix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-printix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-printix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-printix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-printix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-printix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-printix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-printix-tutorial/tutorial_general_203.png

