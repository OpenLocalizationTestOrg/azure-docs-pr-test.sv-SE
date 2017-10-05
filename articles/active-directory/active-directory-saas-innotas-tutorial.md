---
title: "Självstudier: Azure Active Directory-integrering med Innotas | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Innotas."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: eb9e9c2c-4b09-4177-bbab-423fd657448e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 674d01b2c0818dc10fdab5844a23c5ebf29bb2d2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-innotas"></a><span data-ttu-id="b5422-103">Självstudier: Azure Active Directory-integrering med Innotas</span><span class="sxs-lookup"><span data-stu-id="b5422-103">Tutorial: Azure Active Directory integration with Innotas</span></span>

<span data-ttu-id="b5422-104">I kursen får lära du att integrera Innotas med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b5422-104">In this tutorial, you learn how to integrate Innotas with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b5422-105">Integrera Innotas med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b5422-105">Integrating Innotas with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b5422-106">Du kan styra i Azure AD som har åtkomst till Innotas</span><span class="sxs-lookup"><span data-stu-id="b5422-106">You can control in Azure AD who has access to Innotas</span></span>
- <span data-ttu-id="b5422-107">Du kan aktivera användarna att automatiskt hämta loggat in på Innotas (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b5422-107">You can enable your users to automatically get signed-on to Innotas (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b5422-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b5422-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b5422-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b5422-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b5422-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b5422-110">Prerequisites</span></span>

<span data-ttu-id="b5422-111">För att konfigurera Azure AD-integrering med Innotas, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="b5422-111">To configure Azure AD integration with Innotas, you need the following items:</span></span>

- <span data-ttu-id="b5422-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b5422-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b5422-113">En Innotas enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="b5422-113">An Innotas single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b5422-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b5422-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b5422-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b5422-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b5422-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b5422-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b5422-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b5422-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b5422-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b5422-118">Scenario description</span></span>

<span data-ttu-id="b5422-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b5422-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b5422-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b5422-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b5422-121">Att lägga till Innotas från galleriet</span><span class="sxs-lookup"><span data-stu-id="b5422-121">Adding Innotas from the gallery</span></span>
2. <span data-ttu-id="b5422-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b5422-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-innotas-from-the-gallery"></a><span data-ttu-id="b5422-123">Att lägga till Innotas från galleriet</span><span class="sxs-lookup"><span data-stu-id="b5422-123">Adding Innotas from the gallery</span></span>
<span data-ttu-id="b5422-124">Du måste lägga till Innotas från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Innotas i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b5422-124">To configure the integration of Innotas into Azure AD, you need to add Innotas from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b5422-125">**Utför följande steg för att lägga till Innotas från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="b5422-125">**To add Innotas from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b5422-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b5422-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b5422-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="b5422-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b5422-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b5422-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="b5422-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b5422-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="b5422-133">I sökrutan skriver **Innotas**.</span><span class="sxs-lookup"><span data-stu-id="b5422-133">In the search box, type **Innotas**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-innotas-tutorial/tutorial_innotas_search.png)

5. <span data-ttu-id="b5422-135">Välj i resultatpanelen **Innotas**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="b5422-135">In the results panel, select **Innotas**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-innotas-tutorial/tutorial_innotas_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b5422-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b5422-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="b5422-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Innotas baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b5422-138">In this section, you configure and test Azure AD single sign-on with Innotas based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b5422-139">Azure AD måste du känna till användaren i Innotas motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="b5422-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Innotas is to a user in Azure AD.</span></span> <span data-ttu-id="b5422-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Innotas upprättas.</span><span class="sxs-lookup"><span data-stu-id="b5422-140">In other words, a link relationship between an Azure AD user and the related user in Innotas needs to be established.</span></span>

<span data-ttu-id="b5422-141">I Innotas, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="b5422-141">In Innotas, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="b5422-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Innotas, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b5422-142">To configure and test Azure AD single sign-on with Innotas, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b5422-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b5422-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b5422-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b5422-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b5422-145">**[Skapa en testanvändare Innotas](#creating-an-innotas-test-user)**  – du har en motsvarighet för Britta Simon i Innotas som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="b5422-145">**[Creating an Innotas test user](#creating-an-innotas-test-user)** - to have a counterpart of Britta Simon in Innotas that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b5422-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b5422-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b5422-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b5422-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b5422-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b5422-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b5422-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Innotas program.</span><span class="sxs-lookup"><span data-stu-id="b5422-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Innotas application.</span></span>

<span data-ttu-id="b5422-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Innotas:**</span><span class="sxs-lookup"><span data-stu-id="b5422-150">**To configure Azure AD single sign-on with Innotas, perform the following steps:**</span></span>

1. <span data-ttu-id="b5422-151">I Azure-portalen på den **Innotas** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b5422-151">In the Azure portal, on the **Innotas** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="b5422-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b5422-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-innotas-tutorial/tutorial_innotas_samlbase.png)

3. <span data-ttu-id="b5422-155">På den **Innotas domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b5422-155">On the **Innotas Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-innotas-tutorial/tutorial_innotas_url.png)

    <span data-ttu-id="b5422-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<tenant-name>.Innotas.com`</span><span class="sxs-lookup"><span data-stu-id="b5422-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.Innotas.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b5422-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="b5422-158">This value is not real.</span></span> <span data-ttu-id="b5422-159">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="b5422-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="b5422-160">Kontakta [Innotas klienten supportteamet](https://www.innotas.com/contact) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="b5422-160">Contact [Innotas Client support team](https://www.innotas.com/contact) to get this value.</span></span> 
 
4. <span data-ttu-id="b5422-161">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="b5422-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-innotas-tutorial/tutorial_innotas_certificate.png) 

5. <span data-ttu-id="b5422-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="b5422-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-innotas-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b5422-165">Konfigurera enkel inloggning på **Innotas** sida, måste du skicka den hämtade **XML-Metadata för** till [Innotas supportteam](https://www.innotas.com/contact).</span><span class="sxs-lookup"><span data-stu-id="b5422-165">To configure single sign-on on **Innotas** side, you need to send the downloaded **Metadata XML** to [Innotas support team](https://www.innotas.com/contact).</span></span> <span data-ttu-id="b5422-166">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="b5422-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="b5422-167">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="b5422-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b5422-168">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="b5422-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b5422-169">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b5422-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b5422-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b5422-170">Creating an Azure AD test user</span></span>

<span data-ttu-id="b5422-171">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b5422-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="b5422-173">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="b5422-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b5422-174">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b5422-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-innotas-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b5422-176">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b5422-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-innotas-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b5422-178">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b5422-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-innotas-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b5422-180">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b5422-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-innotas-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b5422-182">a.</span><span class="sxs-lookup"><span data-stu-id="b5422-182">a.</span></span> <span data-ttu-id="b5422-183">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b5422-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b5422-184">b.</span><span class="sxs-lookup"><span data-stu-id="b5422-184">b.</span></span> <span data-ttu-id="b5422-185">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b5422-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b5422-186">c.</span><span class="sxs-lookup"><span data-stu-id="b5422-186">c.</span></span> <span data-ttu-id="b5422-187">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b5422-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b5422-188">d.</span><span class="sxs-lookup"><span data-stu-id="b5422-188">d.</span></span> <span data-ttu-id="b5422-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b5422-189">Click **Create**.</span></span>
 
### <a name="creating-an-innotas-test-user"></a><span data-ttu-id="b5422-190">Skapa en testanvändare Innotas</span><span class="sxs-lookup"><span data-stu-id="b5422-190">Creating an Innotas test user</span></span>

<span data-ttu-id="b5422-191">Det finns inga uppgiften som du kan konfigurera användaretablering till Innotas.</span><span class="sxs-lookup"><span data-stu-id="b5422-191">There is no action item for you to configure user provisioning to Innotas.</span></span>  
<span data-ttu-id="b5422-192">När en tilldelad användare försöker logga in med hjälp av åtkomstpanelen Innotas kontrollerar Innotas om användaren finns.</span><span class="sxs-lookup"><span data-stu-id="b5422-192">When an assigned user tries to log in to Innotas using the access panel, Innotas checks whether the user exists.</span></span>  

>[!NOTE]
><span data-ttu-id="b5422-193">Om det finns inget användarkonto ännu, skapas den automatiskt av Innotas.</span><span class="sxs-lookup"><span data-stu-id="b5422-193">If there is no user account available yet, it is automatically created by Innotas.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b5422-194">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="b5422-194">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b5422-195">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Innotas.</span><span class="sxs-lookup"><span data-stu-id="b5422-195">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Innotas.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="b5422-197">**Om du vill tilldela Innotas Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b5422-197">**To assign Britta Simon to Innotas, perform the following steps:**</span></span>

1. <span data-ttu-id="b5422-198">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b5422-198">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b5422-200">Välj i listan med program **Innotas**.</span><span class="sxs-lookup"><span data-stu-id="b5422-200">In the applications list, select **Innotas**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-innotas-tutorial/tutorial_innotas_app.png) 

3. <span data-ttu-id="b5422-202">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b5422-202">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="b5422-204">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="b5422-204">Click **Add** button.</span></span> <span data-ttu-id="b5422-205">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b5422-205">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="b5422-207">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="b5422-207">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b5422-208">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b5422-208">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b5422-209">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b5422-209">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b5422-210">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b5422-210">Testing single sign-on</span></span>

<span data-ttu-id="b5422-211">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="b5422-211">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b5422-212">När du klickar på panelen Innotas på åtkomstpanelen du bör få automatiskt loggat in på ditt Innotas program.</span><span class="sxs-lookup"><span data-stu-id="b5422-212">When you click the Innotas tile in the Access Panel, you should get automatically signed-on to your Innotas application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5422-213">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b5422-213">Additional resources</span></span>

* [<span data-ttu-id="b5422-214">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b5422-214">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b5422-215">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b5422-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-innotas-tutorial/tutorial_general_203.png

