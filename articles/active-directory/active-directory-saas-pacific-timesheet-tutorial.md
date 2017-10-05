---
title: "Självstudier: Azure Active Directory-integrering med Pacific tidrapporter | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Pacific tidrapporter."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e546e8ba-821a-4942-9545-c84b0670beab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: fda06c340430d19bea035a2cab2f318fe8a5998c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pacific-timesheet"></a><span data-ttu-id="73dbe-103">Självstudier: Azure Active Directory-integrering med Pacific tidrapporter</span><span class="sxs-lookup"><span data-stu-id="73dbe-103">Tutorial: Azure Active Directory integration with Pacific Timesheet</span></span>

<span data-ttu-id="73dbe-104">I kursen får lära du att integrera Pacific tidrapporter med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="73dbe-104">In this tutorial, you learn how to integrate Pacific Timesheet with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="73dbe-105">Integrera Pacific tidrapporter med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="73dbe-105">Integrating Pacific Timesheet with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="73dbe-106">Du kan styra i Azure AD som har åtkomst till Pacific tidrapporter</span><span class="sxs-lookup"><span data-stu-id="73dbe-106">You can control in Azure AD who has access to Pacific Timesheet</span></span>
- <span data-ttu-id="73dbe-107">Du kan aktivera användarna att automatiskt hämta loggat in på Pacific tidrapporter (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="73dbe-107">You can enable your users to automatically get signed-on to Pacific Timesheet (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="73dbe-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="73dbe-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="73dbe-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="73dbe-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="73dbe-110">Krav</span><span class="sxs-lookup"><span data-stu-id="73dbe-110">Prerequisites</span></span>

<span data-ttu-id="73dbe-111">För att konfigurera Azure AD-integrering med Pacific tidrapporter, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="73dbe-111">To configure Azure AD integration with Pacific Timesheet, you need the following items:</span></span>

- <span data-ttu-id="73dbe-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="73dbe-112">An Azure AD subscription</span></span>
- <span data-ttu-id="73dbe-113">En Pacific tidrapporter enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="73dbe-113">A Pacific Timesheet single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="73dbe-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="73dbe-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="73dbe-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="73dbe-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="73dbe-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="73dbe-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="73dbe-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="73dbe-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="73dbe-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="73dbe-118">Scenario description</span></span>
<span data-ttu-id="73dbe-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="73dbe-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="73dbe-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="73dbe-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="73dbe-121">Att lägga till Pacific tidrapporter från galleriet</span><span class="sxs-lookup"><span data-stu-id="73dbe-121">Adding Pacific Timesheet from the gallery</span></span>
2. <span data-ttu-id="73dbe-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="73dbe-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pacific-timesheet-from-the-gallery"></a><span data-ttu-id="73dbe-123">Att lägga till Pacific tidrapporter från galleriet</span><span class="sxs-lookup"><span data-stu-id="73dbe-123">Adding Pacific Timesheet from the gallery</span></span>
<span data-ttu-id="73dbe-124">Du måste lägga till Pacific tidrapporter från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Pacific tidrapporter i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="73dbe-124">To configure the integration of Pacific Timesheet into Azure AD, you need to add Pacific Timesheet from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="73dbe-125">**Utför följande steg för att lägga till Pacific tidrapporter från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="73dbe-125">**To add Pacific Timesheet from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="73dbe-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="73dbe-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="73dbe-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="73dbe-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="73dbe-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="73dbe-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="73dbe-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="73dbe-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="73dbe-133">I sökrutan skriver **Pacific tidrapporter**.</span><span class="sxs-lookup"><span data-stu-id="73dbe-133">In the search box, type **Pacific Timesheet**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_search.png)

5. <span data-ttu-id="73dbe-135">Välj i resultatpanelen **Pacific tidrapporter**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="73dbe-135">In the results panel, select **Pacific Timesheet**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="73dbe-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="73dbe-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="73dbe-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Pacific tidrapporter baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="73dbe-138">In this section, you configure and test Azure AD single sign-on with Pacific Timesheet based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="73dbe-139">Azure AD måste du känna till användaren i Pacific tidrapporter motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="73dbe-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Pacific Timesheet is to a user in Azure AD.</span></span> <span data-ttu-id="73dbe-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i Pacific tidrapporter upprättas.</span><span class="sxs-lookup"><span data-stu-id="73dbe-140">In other words, a link relationship between an Azure AD user and the related user in Pacific Timesheet needs to be established.</span></span>

<span data-ttu-id="73dbe-141">I Pacific tidrapporter tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="73dbe-141">In Pacific Timesheet, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="73dbe-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Pacific tidrapporter, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="73dbe-142">To configure and test Azure AD single sign-on with Pacific Timesheet, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="73dbe-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="73dbe-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="73dbe-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="73dbe-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="73dbe-145">**[Skapa en testanvändare Pacific tidrapporter](#creating-a-pacific-timesheet-test-user)**  – du har en motsvarighet för Britta Simon i Pacific tidrapporter som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="73dbe-145">**[Creating a Pacific Timesheet test user](#creating-a-pacific-timesheet-test-user)** - to have a counterpart of Britta Simon in Pacific Timesheet that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="73dbe-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="73dbe-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="73dbe-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="73dbe-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="73dbe-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="73dbe-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="73dbe-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Pacific tidrapporter.</span><span class="sxs-lookup"><span data-stu-id="73dbe-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Pacific Timesheet application.</span></span>

<span data-ttu-id="73dbe-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Pacific tidrapporter:**</span><span class="sxs-lookup"><span data-stu-id="73dbe-150">**To configure Azure AD single sign-on with Pacific Timesheet, perform the following steps:**</span></span>

1. <span data-ttu-id="73dbe-151">I Azure-portalen på den **Pacific tidrapporter** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="73dbe-151">In the Azure portal, on the **Pacific Timesheet** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="73dbe-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="73dbe-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_samlbase.png)

3. <span data-ttu-id="73dbe-155">På den **Pacific tidrapporter domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="73dbe-155">On the **Pacific Timesheet Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_url.png)

    <span data-ttu-id="73dbe-157">a.</span><span class="sxs-lookup"><span data-stu-id="73dbe-157">a.</span></span> <span data-ttu-id="73dbe-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`</span><span class="sxs-lookup"><span data-stu-id="73dbe-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`</span></span>

    <span data-ttu-id="73dbe-159">b.</span><span class="sxs-lookup"><span data-stu-id="73dbe-159">b.</span></span> <span data-ttu-id="73dbe-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`</span><span class="sxs-lookup"><span data-stu-id="73dbe-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="73dbe-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="73dbe-161">These values are not real.</span></span> <span data-ttu-id="73dbe-162">Uppdatera dessa värden med den faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="73dbe-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="73dbe-163">Kontakta [Pacific tidrapporter supportteamet](http://www.pacifictimesheet.com/support) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="73dbe-163">Contact [Pacific Timesheet support team](http://www.pacifictimesheet.com/support) to get these values.</span></span>
 
4. <span data-ttu-id="73dbe-164">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="73dbe-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_certificate.png) 

5. <span data-ttu-id="73dbe-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="73dbe-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="73dbe-168">På den **Pacific tidrapporter Configuration** klickar du på **konfigurera Pacific tidrapporter** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="73dbe-168">On the **Pacific Timesheet Configuration** section, click **Configure Pacific Timesheet** to open **Configure sign-on** window.</span></span> <span data-ttu-id="73dbe-169">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="73dbe-169">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_configure.png) 

7. <span data-ttu-id="73dbe-171">Konfigurera enkel inloggning på **Pacific tidrapporter** sida, måste du skicka den hämtade **certifikat (Base64)**, **SAML enkel inloggning Tjänstwebbadress**, och **SAML enhets-ID** till [Pacific tidrapporter supportteamet](http://www.pacifictimesheet.com/support).</span><span class="sxs-lookup"><span data-stu-id="73dbe-171">To configure single sign-on on **Pacific Timesheet** side, you need to send the downloaded **Certificate (Base64)**, **SAML Single Sign-On Service URL**, and **SAML Entity ID** to [Pacific Timesheet support team](http://www.pacifictimesheet.com/support).</span></span> <span data-ttu-id="73dbe-172">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="73dbe-172">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="73dbe-173">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="73dbe-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="73dbe-174">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="73dbe-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="73dbe-175">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="73dbe-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="73dbe-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="73dbe-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="73dbe-177">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="73dbe-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="73dbe-179">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="73dbe-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="73dbe-180">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="73dbe-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="73dbe-182">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="73dbe-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="73dbe-184">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="73dbe-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="73dbe-186">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="73dbe-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="73dbe-188">a.</span><span class="sxs-lookup"><span data-stu-id="73dbe-188">a.</span></span> <span data-ttu-id="73dbe-189">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="73dbe-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="73dbe-190">b.</span><span class="sxs-lookup"><span data-stu-id="73dbe-190">b.</span></span> <span data-ttu-id="73dbe-191">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="73dbe-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="73dbe-192">c.</span><span class="sxs-lookup"><span data-stu-id="73dbe-192">c.</span></span> <span data-ttu-id="73dbe-193">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="73dbe-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="73dbe-194">d.</span><span class="sxs-lookup"><span data-stu-id="73dbe-194">d.</span></span> <span data-ttu-id="73dbe-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="73dbe-195">Click **Create**.</span></span>
 
### <a name="creating-a-pacific-timesheet-test-user"></a><span data-ttu-id="73dbe-196">Skapa en testanvändare Pacific tidrapporter</span><span class="sxs-lookup"><span data-stu-id="73dbe-196">Creating a Pacific Timesheet test user</span></span>

<span data-ttu-id="73dbe-197">I det här avsnittet skapar du en användare som kallas Britta Simon i Pacific tidrapporter.</span><span class="sxs-lookup"><span data-stu-id="73dbe-197">In this section, you create a user called Britta Simon in Pacific Timesheet.</span></span> <span data-ttu-id="73dbe-198">Arbeta med [Pacific tidrapporter supportteamet](http://www.pacifictimesheet.com/support) att skapa en användare i programmet.</span><span class="sxs-lookup"><span data-stu-id="73dbe-198">Work with [Pacific Timesheet support team](http://www.pacifictimesheet.com/support) to create a user in the application.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="73dbe-199">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="73dbe-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="73dbe-200">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Pacific tidrapporter.</span><span class="sxs-lookup"><span data-stu-id="73dbe-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Pacific Timesheet.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="73dbe-202">**Om du vill tilldela Pacific tidrapporter Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="73dbe-202">**To assign Britta Simon to Pacific Timesheet, perform the following steps:**</span></span>

1. <span data-ttu-id="73dbe-203">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="73dbe-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="73dbe-205">Välj i listan med program **Pacific tidrapporter**.</span><span class="sxs-lookup"><span data-stu-id="73dbe-205">In the applications list, select **Pacific Timesheet**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacifictimesheet_app.png) 

3. <span data-ttu-id="73dbe-207">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="73dbe-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="73dbe-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="73dbe-209">Click **Add** button.</span></span> <span data-ttu-id="73dbe-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="73dbe-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="73dbe-212">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="73dbe-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="73dbe-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="73dbe-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="73dbe-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="73dbe-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="73dbe-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="73dbe-215">Testing single sign-on</span></span>

<span data-ttu-id="73dbe-216">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="73dbe-216">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="73dbe-217">När du klickar på panelen Pacific tidrapporter på åtkomstpanelen du bör få automatiskt loggat in på ditt Pacific tidrapporter program.</span><span class="sxs-lookup"><span data-stu-id="73dbe-217">When you click the Pacific Timesheet tile in the Access Panel, you should get automatically signed-on to your Pacific Timesheet application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="73dbe-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="73dbe-218">Additional resources</span></span>

* [<span data-ttu-id="73dbe-219">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="73dbe-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="73dbe-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="73dbe-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_203.png

