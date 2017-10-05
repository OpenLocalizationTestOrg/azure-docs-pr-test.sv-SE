---
title: "Självstudier: Azure Active Directory-integrering med Projectplace | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Projectplace."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 298059ca-b652-4577-916a-c31393d53d7a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: bb9dd10c887cb0e42e544066d9b0dcfa554e10ce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-projectplace"></a><span data-ttu-id="f190a-103">Självstudier: Azure Active Directory-integrering med Projectplace</span><span class="sxs-lookup"><span data-stu-id="f190a-103">Tutorial: Azure Active Directory integration with Projectplace</span></span>

<span data-ttu-id="f190a-104">I kursen får lära du att integrera Projectplace med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="f190a-104">In this tutorial, you learn how to integrate Projectplace with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f190a-105">Integrera Projectplace med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f190a-105">Integrating Projectplace with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f190a-106">Du kan styra i Azure AD som har åtkomst till Projectplace</span><span class="sxs-lookup"><span data-stu-id="f190a-106">You can control in Azure AD who has access to Projectplace</span></span>
- <span data-ttu-id="f190a-107">Du kan aktivera användarna att automatiskt hämta loggat in på Projectplace (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="f190a-107">You can enable your users to automatically get signed-on to Projectplace (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f190a-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f190a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f190a-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f190a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f190a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f190a-110">Prerequisites</span></span>

<span data-ttu-id="f190a-111">För att konfigurera Azure AD-integrering med Projectplace, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="f190a-111">To configure Azure AD integration with Projectplace, you need the following items:</span></span>

- <span data-ttu-id="f190a-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f190a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f190a-113">En Projectplace enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="f190a-113">A Projectplace single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f190a-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f190a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f190a-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f190a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f190a-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f190a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f190a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f190a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f190a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="f190a-118">Scenario description</span></span>
<span data-ttu-id="f190a-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="f190a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f190a-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="f190a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f190a-121">Att lägga till Projectplace från galleriet</span><span class="sxs-lookup"><span data-stu-id="f190a-121">Adding Projectplace from the gallery</span></span>
2. <span data-ttu-id="f190a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f190a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-projectplace-from-the-gallery"></a><span data-ttu-id="f190a-123">Att lägga till Projectplace från galleriet</span><span class="sxs-lookup"><span data-stu-id="f190a-123">Adding Projectplace from the gallery</span></span>
<span data-ttu-id="f190a-124">Du måste lägga till Projectplace från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Projectplace i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f190a-124">To configure the integration of Projectplace into Azure AD, you need to add Projectplace from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f190a-125">**Utför följande steg för att lägga till Projectplace från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="f190a-125">**To add Projectplace from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f190a-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f190a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f190a-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f190a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f190a-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f190a-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="f190a-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f190a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="f190a-133">I sökrutan skriver **Projectplace**.</span><span class="sxs-lookup"><span data-stu-id="f190a-133">In the search box, type **Projectplace**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_search.png)

5. <span data-ttu-id="f190a-135">Välj i resultatpanelen **Projectplace**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="f190a-135">In the results panel, select **Projectplace**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f190a-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f190a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f190a-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Projectplace baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="f190a-138">In this section, you configure and test Azure AD single sign-on with Projectplace based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f190a-139">Azure AD måste du känna till användaren i Projectplace motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="f190a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Projectplace is to a user in Azure AD.</span></span> <span data-ttu-id="f190a-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Projectplace upprättas.</span><span class="sxs-lookup"><span data-stu-id="f190a-140">In other words, a link relationship between an Azure AD user and the related user in Projectplace needs to be established.</span></span>

<span data-ttu-id="f190a-141">I Projectplace, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="f190a-141">In Projectplace, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="f190a-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Projectplace, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="f190a-142">To configure and test Azure AD single sign-on with Projectplace, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f190a-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f190a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f190a-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f190a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f190a-145">**[Skapa en testanvändare Projectplace](#creating-a-projectplace-test-user)**  – du har en motsvarighet för Britta Simon i Projectplace som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="f190a-145">**[Creating a Projectplace test user](#creating-a-projectplace-test-user)** - to have a counterpart of Britta Simon in Projectplace that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f190a-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f190a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f190a-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f190a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f190a-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f190a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f190a-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Projectplace-program.</span><span class="sxs-lookup"><span data-stu-id="f190a-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Projectplace application.</span></span>

<span data-ttu-id="f190a-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Projectplace:**</span><span class="sxs-lookup"><span data-stu-id="f190a-150">**To configure Azure AD single sign-on with Projectplace, perform the following steps:**</span></span>

1. <span data-ttu-id="f190a-151">I Azure-portalen på den **Projectplace** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f190a-151">In the Azure portal, on the **Projectplace** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="f190a-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f190a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_samlbase.png)

3. <span data-ttu-id="f190a-155">På den **Projectplace-domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f190a-155">On the **Projectplace Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_url.png)

    <span data-ttu-id="f190a-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<company>.projectplace.com`</span><span class="sxs-lookup"><span data-stu-id="f190a-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company>.projectplace.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f190a-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="f190a-158">This value is not real.</span></span> <span data-ttu-id="f190a-159">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="f190a-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="f190a-160">Kontakta [Projectplace klienten supportteamet](https://success.planview.com/Projectplace/Support) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="f190a-160">Contact [Projectplace Client support team](https://success.planview.com/Projectplace/Support) to get this value.</span></span> 
 
4. <span data-ttu-id="f190a-161">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="f190a-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_certificate.png) 

5. <span data-ttu-id="f190a-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f190a-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-projectplace-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="f190a-165">Konfigurera enkel inloggning på **Projectplace** sida, måste du skicka den hämtade **XML-Metadata för** till [Projectplace-supportteamet](https://success.planview.com/Projectplace/Support).</span><span class="sxs-lookup"><span data-stu-id="f190a-165">To configure single sign-on on **Projectplace** side, you need to send the downloaded **Metadata XML** to [Projectplace support team](https://success.planview.com/Projectplace/Support).</span></span> <span data-ttu-id="f190a-166">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="f190a-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

>[!NOTE]
><span data-ttu-id="f190a-167">Konfigurationen för enkel inloggning måste utföras av den [Projectplace-supportteamet](https://success.planview.com/Projectplace/Support).</span><span class="sxs-lookup"><span data-stu-id="f190a-167">The single sign-on configuration has to be performed by the [Projectplace support team](https://success.planview.com/Projectplace/Support).</span></span> <span data-ttu-id="f190a-168">Du får ett meddelande så snart som konfigurationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="f190a-168">You will get a notification as soon as the configuration has been completed.</span></span>

> [!TIP]
> <span data-ttu-id="f190a-169">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="f190a-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f190a-170">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="f190a-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f190a-171">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f190a-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f190a-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f190a-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="f190a-173">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f190a-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="f190a-175">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="f190a-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f190a-176">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f190a-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-projectplace-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f190a-178">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f190a-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-projectplace-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f190a-180">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f190a-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-projectplace-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f190a-182">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f190a-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-projectplace-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f190a-184">a.</span><span class="sxs-lookup"><span data-stu-id="f190a-184">a.</span></span> <span data-ttu-id="f190a-185">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f190a-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f190a-186">b.</span><span class="sxs-lookup"><span data-stu-id="f190a-186">b.</span></span> <span data-ttu-id="f190a-187">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f190a-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f190a-188">c.</span><span class="sxs-lookup"><span data-stu-id="f190a-188">c.</span></span> <span data-ttu-id="f190a-189">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="f190a-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f190a-190">d.</span><span class="sxs-lookup"><span data-stu-id="f190a-190">d.</span></span> <span data-ttu-id="f190a-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f190a-191">Click **Create**.</span></span>
 
### <a name="creating-a-projectplace-test-user"></a><span data-ttu-id="f190a-192">Skapa en Projectplace-testanvändare</span><span class="sxs-lookup"><span data-stu-id="f190a-192">Creating a Projectplace test user</span></span>

<span data-ttu-id="f190a-193">För att aktivera Azure AD-användare att logga in på Projectplace etableras de i Projectplace.</span><span class="sxs-lookup"><span data-stu-id="f190a-193">In order to enable Azure AD users to log into Projectplace, they must be provisioned into Projectplace.</span></span> <span data-ttu-id="f190a-194">När det gäller Projectplace är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="f190a-194">In the case of Projectplace, provisioning is a manual task.</span></span>

<span data-ttu-id="f190a-195">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="f190a-195">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="f190a-196">Logga in på ditt **Projectplace** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="f190a-196">Log in to your **Projectplace** company site as an administrator.</span></span>

2. <span data-ttu-id="f190a-197">Gå till **personer**, och klicka sedan på **medlemmar**.</span><span class="sxs-lookup"><span data-stu-id="f190a-197">Go to **People**, and then click **Members**.</span></span>
   
    <span data-ttu-id="f190a-198">![Personer](./media/active-directory-saas-projectplace-tutorial/ic790228.png "personer")</span><span class="sxs-lookup"><span data-stu-id="f190a-198">![People](./media/active-directory-saas-projectplace-tutorial/ic790228.png "People")</span></span>

3. <span data-ttu-id="f190a-199">Klicka på **lägga till medlem**.</span><span class="sxs-lookup"><span data-stu-id="f190a-199">Click **Add Member**.</span></span>
   
    <span data-ttu-id="f190a-200">![Lägga till medlemmar](./media/active-directory-saas-projectplace-tutorial/ic790232.png "lägga till medlemmar")</span><span class="sxs-lookup"><span data-stu-id="f190a-200">![Add Members](./media/active-directory-saas-projectplace-tutorial/ic790232.png "Add Members")</span></span>

4. <span data-ttu-id="f190a-201">I den **Lägg till medlem** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f190a-201">In the **Add Member** section, perform the following steps:</span></span>
   
    <span data-ttu-id="f190a-202">![Nya medlemmar](./media/active-directory-saas-projectplace-tutorial/ic790233.png "nya medlemmar")</span><span class="sxs-lookup"><span data-stu-id="f190a-202">![New Members](./media/active-directory-saas-projectplace-tutorial/ic790233.png "New Members")</span></span>
   
    <span data-ttu-id="f190a-203">a.</span><span class="sxs-lookup"><span data-stu-id="f190a-203">a.</span></span> <span data-ttu-id="f190a-204">I den **nya medlemmar** textruta, ange en giltig AAD-konto som du vill etablera i relaterade textrutor e-postadress.</span><span class="sxs-lookup"><span data-stu-id="f190a-204">In the **New Members** textbox, type the email address of a valid AAD account you want to provision into the related textboxes.</span></span>
   
    <span data-ttu-id="f190a-205">b.</span><span class="sxs-lookup"><span data-stu-id="f190a-205">b.</span></span> <span data-ttu-id="f190a-206">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="f190a-206">Click **Send**.</span></span>

   <span data-ttu-id="f190a-207">Ett e-postmeddelande med en länk för att bekräfta kontot innan det blir aktiv skickas till innehavare för Azure Active Directory-konto.</span><span class="sxs-lookup"><span data-stu-id="f190a-207">An email including a link to confirm the account before it becomes active is sent to the Azure Active Directory account holder.</span></span>

>[!NOTE]
><span data-ttu-id="f190a-208">Du kan använda något annat Projectplace användarens konto skapas verktyg eller API: er som tillhandahålls av Projectplace att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="f190a-208">You can use any other Projectplace user account creation tools or APIs provided by Projectplace to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f190a-209">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="f190a-209">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f190a-210">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Projectplace.</span><span class="sxs-lookup"><span data-stu-id="f190a-210">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Projectplace.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="f190a-212">**Om du vill tilldela Projectplace Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f190a-212">**To assign Britta Simon to Projectplace, perform the following steps:**</span></span>

1. <span data-ttu-id="f190a-213">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f190a-213">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="f190a-215">Välj i listan med program **Projectplace**.</span><span class="sxs-lookup"><span data-stu-id="f190a-215">In the applications list, select **Projectplace**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-projectplace-tutorial/tutorial_projectplace_app.png) 

3. <span data-ttu-id="f190a-217">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f190a-217">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="f190a-219">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="f190a-219">Click **Add** button.</span></span> <span data-ttu-id="f190a-220">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f190a-220">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="f190a-222">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="f190a-222">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f190a-223">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f190a-223">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f190a-224">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f190a-224">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f190a-225">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f190a-225">Testing single sign-on</span></span>

<span data-ttu-id="f190a-226">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f190a-226">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f190a-227">När du klickar på panelen Projectplace på åtkomstpanelen du bör få automatiskt loggat in på ditt Projectplace-program.</span><span class="sxs-lookup"><span data-stu-id="f190a-227">When you click the Projectplace tile in the Access Panel, you should get automatically signed-on to your Projectplace application.</span></span>
<span data-ttu-id="f190a-228">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f190a-228">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f190a-229">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f190a-229">Additional resources</span></span>

* [<span data-ttu-id="f190a-230">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f190a-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f190a-231">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f190a-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-projectplace-tutorial/tutorial_general_203.png

