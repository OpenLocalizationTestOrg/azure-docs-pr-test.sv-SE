---
title: "Självstudier: Azure Active Directory-integrering med Synergi | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Synergi."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 73c970e1-f1ba-420b-b225-414fdf93b140
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: dedbe96fbb26bc34c4d7e213892b318f0e6fef12
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-synergi"></a><span data-ttu-id="453af-103">Självstudier: Azure Active Directory-integrering med Synergi</span><span class="sxs-lookup"><span data-stu-id="453af-103">Tutorial: Azure Active Directory integration with Synergi</span></span>

<span data-ttu-id="453af-104">I kursen får lära du att integrera Synergi med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="453af-104">In this tutorial, you learn how to integrate Synergi with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="453af-105">Integrera Synergi med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="453af-105">Integrating Synergi with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="453af-106">Du kan styra i Azure AD som har åtkomst till Synergi.</span><span class="sxs-lookup"><span data-stu-id="453af-106">You can control in Azure AD who has access to Synergi.</span></span>
- <span data-ttu-id="453af-107">Du kan aktivera användarna att automatiskt hämta loggat in på Synergi (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="453af-107">You can enable your users to automatically get signed-on to Synergi (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="453af-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="453af-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="453af-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="453af-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="453af-110">Krav</span><span class="sxs-lookup"><span data-stu-id="453af-110">Prerequisites</span></span>

<span data-ttu-id="453af-111">Om du vill konfigurera Azure AD-integrering med Synergi behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="453af-111">To configure Azure AD integration with Synergi, you need the following items:</span></span>

- <span data-ttu-id="453af-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="453af-112">An Azure AD subscription</span></span>
- <span data-ttu-id="453af-113">En Synergi enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="453af-113">A Synergi single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="453af-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="453af-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="453af-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="453af-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="453af-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="453af-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="453af-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="453af-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="453af-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="453af-118">Scenario description</span></span>
<span data-ttu-id="453af-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="453af-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="453af-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="453af-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="453af-121">Att lägga till Synergi från galleriet</span><span class="sxs-lookup"><span data-stu-id="453af-121">Adding Synergi from the gallery</span></span>
2. <span data-ttu-id="453af-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="453af-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-synergi-from-the-gallery"></a><span data-ttu-id="453af-123">Att lägga till Synergi från galleriet</span><span class="sxs-lookup"><span data-stu-id="453af-123">Adding Synergi from the gallery</span></span>
<span data-ttu-id="453af-124">Du måste lägga till Synergi från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Synergi i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="453af-124">To configure the integration of Synergi into Azure AD, you need to add Synergi from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="453af-125">**Utför följande steg för att lägga till Synergi från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="453af-125">**To add Synergi from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="453af-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="453af-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="453af-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="453af-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="453af-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="453af-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="453af-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="453af-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="453af-133">I sökrutan skriver **Synergi**väljer **Synergi** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="453af-133">In the search box, type **Synergi**, select **Synergi** from result panel then click **Add** button to add the application.</span></span>

    ![Synergi i resultatlistan](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="453af-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="453af-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="453af-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Synergi baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="453af-136">In this section, you configure and test Azure AD single sign-on with Synergi based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="453af-137">Azure AD måste du känna till användaren i Synergi motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="453af-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Synergi is to a user in Azure AD.</span></span> <span data-ttu-id="453af-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Synergi upprättas.</span><span class="sxs-lookup"><span data-stu-id="453af-138">In other words, a link relationship between an Azure AD user and the related user in Synergi needs to be established.</span></span>

<span data-ttu-id="453af-139">I Synergi, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="453af-139">In Synergi, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="453af-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Synergi, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="453af-140">To configure and test Azure AD single sign-on with Synergi, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="453af-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="453af-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="453af-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="453af-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="453af-143">**[Skapa en testanvändare Synergi](#create-a-synergi-test-user)**  – har en motsvarighet för Britta Simon Synergi som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="453af-143">**[Create a Synergi test user](#create-a-synergi-test-user)** - to have a counterpart of Britta Simon in Synergi that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="453af-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="453af-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="453af-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="453af-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="453af-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="453af-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="453af-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Synergi.</span><span class="sxs-lookup"><span data-stu-id="453af-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Synergi application.</span></span>

<span data-ttu-id="453af-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Synergi:**</span><span class="sxs-lookup"><span data-stu-id="453af-148">**To configure Azure AD single sign-on with Synergi, perform the following steps:**</span></span>

1. <span data-ttu-id="453af-149">I Azure-portalen på den **Synergi** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="453af-149">In the Azure portal, on the **Synergi** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="453af-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="453af-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_samlbase.png)

3. <span data-ttu-id="453af-153">På den **Synergi domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="453af-153">On the **Synergi Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och synergi domän med enkel inloggning information](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_url.png)

    <span data-ttu-id="453af-155">a.</span><span class="sxs-lookup"><span data-stu-id="453af-155">a.</span></span> <span data-ttu-id="453af-156">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<company name>.irmsecurity.com`</span><span class="sxs-lookup"><span data-stu-id="453af-156">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.irmsecurity.com`</span></span>

    <span data-ttu-id="453af-157">b.</span><span class="sxs-lookup"><span data-stu-id="453af-157">b.</span></span> <span data-ttu-id="453af-158">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<company name>.irmsecurity.com/sso/<organization id>`</span><span class="sxs-lookup"><span data-stu-id="453af-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.irmsecurity.com/sso/<organization id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="453af-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="453af-159">These values are not real.</span></span> <span data-ttu-id="453af-160">Uppdatera dessa värden med den faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="453af-160">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="453af-161">Kontakta [Synergi supportteamet](https://www.irmsecurity.com/contact/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="453af-161">Contact [Synergi support team](https://www.irmsecurity.com/contact/) to get these values.</span></span>

4. <span data-ttu-id="453af-162">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="453af-162">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_certificate.png) 

5. <span data-ttu-id="453af-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="453af-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-synergi-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="453af-166">På den **Synergi Configuration** klickar du på **konfigurera Synergi** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="453af-166">On the **Synergi Configuration** section, click **Configure Synergi** to open **Configure sign-on** window.</span></span> <span data-ttu-id="453af-167">Kopiera den **Sign-Out URL och SAML enhets-ID** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="453af-167">Copy the **Sign-Out URL and SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Synergi konfiguration](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_configure.png) 

7. <span data-ttu-id="453af-169">Konfigurera enkel inloggning på **Synergi** sida, måste du skicka den hämtade **Certificate(Base64) Sign-Out URL och SAML enhets-ID** till [Synergi supportteamet](https://www.irmsecurity.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="453af-169">To configure single sign-on on **Synergi** side, you need to send the downloaded **Certificate(Base64), Sign-Out URL, and SAML Entity ID** to [Synergi support team](https://www.irmsecurity.com/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="453af-170">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="453af-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="453af-171">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="453af-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="453af-172">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="453af-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="453af-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="453af-173">Create an Azure AD test user</span></span>

<span data-ttu-id="453af-174">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="453af-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="453af-176">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="453af-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="453af-177">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="453af-177">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-synergi-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="453af-179">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="453af-179">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-synergi-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="453af-181">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="453af-181">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-synergi-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="453af-183">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="453af-183">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-synergi-tutorial/create_aaduser_04.png)

    <span data-ttu-id="453af-185">a.</span><span class="sxs-lookup"><span data-stu-id="453af-185">a.</span></span> <span data-ttu-id="453af-186">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="453af-186">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="453af-187">b.</span><span class="sxs-lookup"><span data-stu-id="453af-187">b.</span></span> <span data-ttu-id="453af-188">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="453af-188">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="453af-189">c.</span><span class="sxs-lookup"><span data-stu-id="453af-189">c.</span></span> <span data-ttu-id="453af-190">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="453af-190">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="453af-191">d.</span><span class="sxs-lookup"><span data-stu-id="453af-191">d.</span></span> <span data-ttu-id="453af-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="453af-192">Click **Create**.</span></span>
  
### <a name="create-a-synergi-test-user"></a><span data-ttu-id="453af-193">Skapa en testanvändare Synergi</span><span class="sxs-lookup"><span data-stu-id="453af-193">Create a Synergi test user</span></span>

<span data-ttu-id="453af-194">I det här avsnittet skapar du en användare som kallas Britta Simon i Synergi.</span><span class="sxs-lookup"><span data-stu-id="453af-194">In this section, you create a user called Britta Simon in Synergi.</span></span> <span data-ttu-id="453af-195">Arbeta med [Synergi supportteamet](https://www.irmsecurity.com/contact/) att lägga till användare i Synergi-plattformen.</span><span class="sxs-lookup"><span data-stu-id="453af-195">Work with [Synergi support team](https://www.irmsecurity.com/contact/) to add the users in the Synergi platform.</span></span> <span data-ttu-id="453af-196">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="453af-196">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="453af-197">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="453af-197">Assign the Azure AD test user</span></span>

<span data-ttu-id="453af-198">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Synergi.</span><span class="sxs-lookup"><span data-stu-id="453af-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Synergi.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="453af-200">**Om du vill tilldela Synergi Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="453af-200">**To assign Britta Simon to Synergi, perform the following steps:**</span></span>

1. <span data-ttu-id="453af-201">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="453af-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="453af-203">Välj i listan med program **Synergi**.</span><span class="sxs-lookup"><span data-stu-id="453af-203">In the applications list, select **Synergi**.</span></span>

    ![Länken Synergi i listan med program](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_app.png)  

3. <span data-ttu-id="453af-205">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="453af-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="453af-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="453af-207">Click **Add** button.</span></span> <span data-ttu-id="453af-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="453af-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="453af-210">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="453af-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="453af-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="453af-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="453af-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="453af-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="453af-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="453af-213">Test single sign-on</span></span>

<span data-ttu-id="453af-214">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="453af-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="453af-215">När du klickar på panelen Synergi på åtkomstpanelen du bör få automatiskt loggat in på ditt Synergi program.</span><span class="sxs-lookup"><span data-stu-id="453af-215">When you click the Synergi tile in the Access Panel, you should get automatically signed-on to your Synergi application.</span></span>
<span data-ttu-id="453af-216">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="453af-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="453af-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="453af-217">Additional resources</span></span>

* [<span data-ttu-id="453af-218">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="453af-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="453af-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="453af-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_203.png

