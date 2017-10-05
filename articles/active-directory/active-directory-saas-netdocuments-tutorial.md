---
title: "Självstudier: Azure Active Directory-integrering med NetDocuments | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och NetDocuments."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1a47dc42-1a17-48a2-965e-eca4cfb2f197
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 87c3338d611daa837aa5f079c4b68e0e6fc58455
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-netdocuments"></a><span data-ttu-id="d6170-103">Självstudier: Azure Active Directory-integrering med NetDocuments</span><span class="sxs-lookup"><span data-stu-id="d6170-103">Tutorial: Azure Active Directory integration with NetDocuments</span></span>

<span data-ttu-id="d6170-104">I kursen får lära du att integrera NetDocuments med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d6170-104">In this tutorial, you learn how to integrate NetDocuments with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d6170-105">Integrera NetDocuments med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d6170-105">Integrating NetDocuments with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d6170-106">Du kan styra i Azure AD som har åtkomst till NetDocuments</span><span class="sxs-lookup"><span data-stu-id="d6170-106">You can control in Azure AD who has access to NetDocuments</span></span>
- <span data-ttu-id="d6170-107">Du kan aktivera användarna att automatiskt hämta loggat in på NetDocuments (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d6170-107">You can enable your users to automatically get signed-on to NetDocuments (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d6170-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d6170-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d6170-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d6170-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d6170-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d6170-110">Prerequisites</span></span>

<span data-ttu-id="d6170-111">För att konfigurera Azure AD-integrering med NetDocuments, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="d6170-111">To configure Azure AD integration with NetDocuments, you need the following items:</span></span>

- <span data-ttu-id="d6170-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d6170-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d6170-113">En NetDocuments enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="d6170-113">A NetDocuments single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d6170-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d6170-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d6170-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d6170-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d6170-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d6170-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d6170-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d6170-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d6170-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d6170-118">Scenario description</span></span>
<span data-ttu-id="d6170-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d6170-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d6170-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d6170-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d6170-121">Att lägga till NetDocuments från galleriet</span><span class="sxs-lookup"><span data-stu-id="d6170-121">Adding NetDocuments from the gallery</span></span>
2. <span data-ttu-id="d6170-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d6170-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-netdocuments-from-the-gallery"></a><span data-ttu-id="d6170-123">Att lägga till NetDocuments från galleriet</span><span class="sxs-lookup"><span data-stu-id="d6170-123">Adding NetDocuments from the gallery</span></span>
<span data-ttu-id="d6170-124">Du måste lägga till NetDocuments från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av NetDocuments i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d6170-124">To configure the integration of NetDocuments into Azure AD, you need to add NetDocuments from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d6170-125">**Utför följande steg för att lägga till NetDocuments från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="d6170-125">**To add NetDocuments from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d6170-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d6170-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d6170-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d6170-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d6170-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d6170-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d6170-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d6170-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d6170-133">I sökrutan skriver **NetDocuments**.</span><span class="sxs-lookup"><span data-stu-id="d6170-133">In the search box, type **NetDocuments**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_search.png)

5. <span data-ttu-id="d6170-135">Välj i resultatpanelen **NetDocuments**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="d6170-135">In the results panel, select **NetDocuments**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d6170-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d6170-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d6170-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med NetDocuments baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d6170-138">In this section, you configure and test Azure AD single sign-on with NetDocuments based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d6170-139">Azure AD måste du känna till användaren i NetDocuments motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="d6170-139">For single sign-on to work, Azure AD needs to know what the counterpart user in NetDocuments is to a user in Azure AD.</span></span> <span data-ttu-id="d6170-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i NetDocuments upprättas.</span><span class="sxs-lookup"><span data-stu-id="d6170-140">In other words, a link relationship between an Azure AD user and the related user in NetDocuments needs to be established.</span></span>

<span data-ttu-id="d6170-141">I NetDocuments, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="d6170-141">In NetDocuments, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d6170-142">Om du vill konfigurera och testa Azure AD enkel inloggning med NetDocuments, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d6170-142">To configure and test Azure AD single sign-on with NetDocuments, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d6170-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d6170-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d6170-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d6170-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d6170-145">**[Skapa en testanvändare NetDocuments](#creating-a-netdocuments-test-user)**  – du har en motsvarighet för Britta Simon i NetDocuments som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d6170-145">**[Creating a NetDocuments test user](#creating-a-netdocuments-test-user)** - to have a counterpart of Britta Simon in NetDocuments that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d6170-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d6170-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d6170-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d6170-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d6170-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d6170-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d6170-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt NetDocuments program.</span><span class="sxs-lookup"><span data-stu-id="d6170-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your NetDocuments application.</span></span>

<span data-ttu-id="d6170-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med NetDocuments:**</span><span class="sxs-lookup"><span data-stu-id="d6170-150">**To configure Azure AD single sign-on with NetDocuments, perform the following steps:**</span></span>

1. <span data-ttu-id="d6170-151">I Azure-portalen på den **NetDocuments** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d6170-151">In the Azure portal, on the **NetDocuments** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d6170-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d6170-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_samlbase.png)

3. <span data-ttu-id="d6170-155">På den **NetDocuments domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d6170-155">On the **NetDocuments Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_url.png)

    <span data-ttu-id="d6170-157">a.</span><span class="sxs-lookup"><span data-stu-id="d6170-157">a.</span></span> <span data-ttu-id="d6170-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span><span class="sxs-lookup"><span data-stu-id="d6170-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span></span>

    <span data-ttu-id="d6170-159">b.</span><span class="sxs-lookup"><span data-stu-id="d6170-159">b.</span></span> <span data-ttu-id="d6170-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span><span class="sxs-lookup"><span data-stu-id="d6170-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://vault.netvoyage.com/neWeb2/docCent.aspx?whr=<user identifier>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d6170-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="d6170-161">These values are not real.</span></span> <span data-ttu-id="d6170-162">Uppdatera dessa värden med den faktiska inloggnings-URL och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="d6170-162">Update these values with the actual Sign-on URL and Reply URL.</span></span> <span data-ttu-id="d6170-163">Kontakta [NetDocuments supportteam](https://support.netdocuments.com/hc/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="d6170-163">Contact [NetDocuments support team](https://support.netdocuments.com/hc/) to get these values.</span></span>
 
4. <span data-ttu-id="d6170-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="d6170-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_certificate.png) 

5. <span data-ttu-id="d6170-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d6170-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netdocuments-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d6170-168">Logga in på webbplatsen NetDocuments företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="d6170-168">In a different web browser window, log into your NetDocuments company site as an administrator.</span></span>

7. <span data-ttu-id="d6170-169">Gå till **Admin**.</span><span class="sxs-lookup"><span data-stu-id="d6170-169">Go to **Admin**.</span></span>

8. <span data-ttu-id="d6170-170">Klicka på **Lägg till och ta bort användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d6170-170">Click **Add and remove users and groups**.</span></span>
   
    <span data-ttu-id="d6170-171">![Databasen](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "databasen")</span><span class="sxs-lookup"><span data-stu-id="d6170-171">![Repository](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "Repository")</span></span>

9. <span data-ttu-id="d6170-172">Klicka på **konfigurera avancerade autentiseringsalternativ**.</span><span class="sxs-lookup"><span data-stu-id="d6170-172">Click **Configure advanced authentication options**.</span></span>
    
    <span data-ttu-id="d6170-173">![Konfigurera avancerade autentiseringsalternativ](./media/active-directory-saas-netdocuments-tutorial/ic795048.png "konfigurera avancerade autentiseringsalternativ")</span><span class="sxs-lookup"><span data-stu-id="d6170-173">![Configure advanced authentication options](./media/active-directory-saas-netdocuments-tutorial/ic795048.png "Configure advanced authentication options")</span></span>

10. <span data-ttu-id="d6170-174">På den **federerad identitet** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d6170-174">On the **Federated Identity** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="d6170-175">![Federerad Identitty](./media/active-directory-saas-netdocuments-tutorial/ic795049.png "federerad Identitty")</span><span class="sxs-lookup"><span data-stu-id="d6170-175">![Federated Identitty](./media/active-directory-saas-netdocuments-tutorial/ic795049.png "Federated Identitty")</span></span>
   
    <span data-ttu-id="d6170-176">a.</span><span class="sxs-lookup"><span data-stu-id="d6170-176">a.</span></span> <span data-ttu-id="d6170-177">Som **federerad identitet servertyp**väljer **Active Directory Federation Services**.</span><span class="sxs-lookup"><span data-stu-id="d6170-177">As **Federated identity server type**, select **Active Directory Federation Services**.</span></span>
   
    <span data-ttu-id="d6170-178">b.</span><span class="sxs-lookup"><span data-stu-id="d6170-178">b.</span></span> <span data-ttu-id="d6170-179">Klicka på **Välj fil**, för att överföra metadatafilen hämtade som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d6170-179">Click **Choose file**, to upload the downloaded metadata file which you have downloaded from Azure portal.</span></span>
   
    <span data-ttu-id="d6170-180">c.</span><span class="sxs-lookup"><span data-stu-id="d6170-180">c.</span></span> <span data-ttu-id="d6170-181">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6170-181">Click **OK**.</span></span>

> [!TIP]
> <span data-ttu-id="d6170-182">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="d6170-182">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d6170-183">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="d6170-183">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d6170-184">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d6170-184">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d6170-185">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d6170-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="d6170-186">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d6170-186">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d6170-188">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="d6170-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d6170-189">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d6170-189">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d6170-191">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d6170-191">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d6170-193">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d6170-193">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d6170-195">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d6170-195">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netdocuments-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d6170-197">a.</span><span class="sxs-lookup"><span data-stu-id="d6170-197">a.</span></span> <span data-ttu-id="d6170-198">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d6170-198">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d6170-199">b.</span><span class="sxs-lookup"><span data-stu-id="d6170-199">b.</span></span> <span data-ttu-id="d6170-200">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d6170-200">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d6170-201">c.</span><span class="sxs-lookup"><span data-stu-id="d6170-201">c.</span></span> <span data-ttu-id="d6170-202">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d6170-202">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d6170-203">d.</span><span class="sxs-lookup"><span data-stu-id="d6170-203">d.</span></span> <span data-ttu-id="d6170-204">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d6170-204">Click **Create**.</span></span>
 
### <a name="creating-a-netdocuments-test-user"></a><span data-ttu-id="d6170-205">Skapa en testanvändare NetDocuments</span><span class="sxs-lookup"><span data-stu-id="d6170-205">Creating a NetDocuments test user</span></span>

<span data-ttu-id="d6170-206">Om du vill aktivera Azure AD-användare kan logga in på NetDocuments etableras de i NetDocuments.</span><span class="sxs-lookup"><span data-stu-id="d6170-206">To enable Azure AD users to log in to NetDocuments, they must be provisioned into NetDocuments.</span></span>  
<span data-ttu-id="d6170-207">När det gäller NetDocuments är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="d6170-207">In the case of NetDocuments, provisioning is a manual task.</span></span>

<span data-ttu-id="d6170-208">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="d6170-208">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="d6170-209">Till att använda din **NetDocuments** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="d6170-209">Sing on to your **NetDocuments** company site as administrator.</span></span>

2. <span data-ttu-id="d6170-210">Klicka på menyn högst upp **Admin**.</span><span class="sxs-lookup"><span data-stu-id="d6170-210">In the menu on the top, click **Admin**.</span></span>
   
    <span data-ttu-id="d6170-211">![Admin](./media/active-directory-saas-netdocuments-tutorial/ic795051.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="d6170-211">![Admin](./media/active-directory-saas-netdocuments-tutorial/ic795051.png "Admin")</span></span>

3. <span data-ttu-id="d6170-212">Klicka på **Lägg till och ta bort användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d6170-212">Click **Add and remove users and groups**.</span></span>
   
    <span data-ttu-id="d6170-213">![Databasen](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "databasen")</span><span class="sxs-lookup"><span data-stu-id="d6170-213">![Repository](./media/active-directory-saas-netdocuments-tutorial/ic795047.png "Repository")</span></span>

4. <span data-ttu-id="d6170-214">I den **e-postadress** textruta, ange ett giltigt Azure Active Directory-konto du vill etablera och klicka sedan på e-postadress **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="d6170-214">In the **Email Address** textbox, type the email address of a valid Azure Active Directory account you want to provision, and then click **Add User**.</span></span>
   
    <span data-ttu-id="d6170-215">![E-postadress](./media/active-directory-saas-netdocuments-tutorial/ic795053.png "e-postadress")</span><span class="sxs-lookup"><span data-stu-id="d6170-215">![Email Address](./media/active-directory-saas-netdocuments-tutorial/ic795053.png "Email Address")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="d6170-216">Azure Active Directory kontoinnehavaren får ett e-postmeddelande som innehåller en länk för att bekräfta kontot innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="d6170-216">The Azure Active Directory account holder will get an email that includes a link to confirm the account before it becomes active.</span></span> <span data-ttu-id="d6170-217">Du kan använda något annat NetDocuments användarens konto skapas verktyg eller API: er som tillhandahålls av NetDocuments för att etablera Azure Active Directory användarkonton.</span><span class="sxs-lookup"><span data-stu-id="d6170-217">You can use any other NetDocuments user account creation tools or APIs provided by NetDocuments to provision Azure Active Directory user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d6170-218">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="d6170-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d6170-219">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till NetDocuments.</span><span class="sxs-lookup"><span data-stu-id="d6170-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to NetDocuments.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d6170-221">**Om du vill tilldela NetDocuments Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d6170-221">**To assign Britta Simon to NetDocuments, perform the following steps:**</span></span>

1. <span data-ttu-id="d6170-222">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d6170-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d6170-224">Välj i listan med program **NetDocuments**.</span><span class="sxs-lookup"><span data-stu-id="d6170-224">In the applications list, select **NetDocuments**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netdocuments-tutorial/tutorial_netdocuments_app.png) 

3. <span data-ttu-id="d6170-226">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d6170-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d6170-228">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d6170-228">Click **Add** button.</span></span> <span data-ttu-id="d6170-229">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d6170-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d6170-231">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="d6170-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d6170-232">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d6170-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d6170-233">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d6170-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d6170-234">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d6170-234">Testing single sign-on</span></span>

<span data-ttu-id="d6170-235">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="d6170-235">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d6170-236">När du klickar på panelen NetDocuments på åtkomstpanelen du bör få automatiskt loggat in på ditt NetDocuments program.</span><span class="sxs-lookup"><span data-stu-id="d6170-236">When you click the NetDocuments tile in the Access Panel, you should get automatically signed-on to your NetDocuments application.</span></span>
<span data-ttu-id="d6170-237">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d6170-237">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d6170-238">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d6170-238">Additional resources</span></span>

* [<span data-ttu-id="d6170-239">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d6170-239">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d6170-240">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d6170-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netdocuments-tutorial/tutorial_general_203.png

