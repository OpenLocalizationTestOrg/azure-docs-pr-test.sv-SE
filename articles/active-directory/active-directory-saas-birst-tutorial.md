---
title: "Självstudier: Azure Active Directory-integrering med Birst flexibel Företagsanalys | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Birst flexibel Företagsanalys."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 677183b1-5348-4302-88cc-5c8ab63a3c6c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 779f9e0a57ffb2274ea22a90ed9759734ab6916d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-birst-agile-business-analytics"></a><span data-ttu-id="5eefc-103">Självstudier: Azure Active Directory-integrering med Birst flexibel Företagsanalys</span><span class="sxs-lookup"><span data-stu-id="5eefc-103">Tutorial: Azure Active Directory integration with Birst Agile Business Analytics</span></span>

<span data-ttu-id="5eefc-104">I kursen får lära du att integrera Birst flexibel Företagsanalys med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="5eefc-104">In this tutorial, you learn how to integrate Birst Agile Business Analytics with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5eefc-105">Integrera Birst flexibel Företagsanalys med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="5eefc-105">Integrating Birst Agile Business Analytics with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5eefc-106">Du kan styra i Azure AD som har åtkomst till Birst flexibel Företagsanalys</span><span class="sxs-lookup"><span data-stu-id="5eefc-106">You can control in Azure AD who has access to Birst Agile Business Analytics</span></span>
- <span data-ttu-id="5eefc-107">Du kan aktivera användarna att automatiskt hämta loggat in på Birst flexibel Företagsanalys (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="5eefc-107">You can enable your users to automatically get signed-on to Birst Agile Business Analytics (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5eefc-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5eefc-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5eefc-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5eefc-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5eefc-110">Krav</span><span class="sxs-lookup"><span data-stu-id="5eefc-110">Prerequisites</span></span>

<span data-ttu-id="5eefc-111">För att konfigurera Azure AD-integrering med Birst flexibel Företagsanalys, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="5eefc-111">To configure Azure AD integration with Birst Agile Business Analytics, you need the following items:</span></span>

- <span data-ttu-id="5eefc-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5eefc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5eefc-113">En Birst flexibel Företagsanalys enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="5eefc-113">A Birst Agile Business Analytics single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5eefc-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5eefc-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5eefc-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="5eefc-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5eefc-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="5eefc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5eefc-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5eefc-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5eefc-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="5eefc-118">Scenario description</span></span>
<span data-ttu-id="5eefc-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="5eefc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5eefc-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="5eefc-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5eefc-121">Att lägga till Birst flexibel Företagsanalys från galleriet</span><span class="sxs-lookup"><span data-stu-id="5eefc-121">Adding Birst Agile Business Analytics from the gallery</span></span>
2. <span data-ttu-id="5eefc-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5eefc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-birst-agile-business-analytics-from-the-gallery"></a><span data-ttu-id="5eefc-123">Att lägga till Birst flexibel Företagsanalys från galleriet</span><span class="sxs-lookup"><span data-stu-id="5eefc-123">Adding Birst Agile Business Analytics from the gallery</span></span>
<span data-ttu-id="5eefc-124">Du måste lägga till Birst flexibel Företagsanalys från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Birst flexibel Företagsanalys i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5eefc-124">To configure the integration of Birst Agile Business Analytics into Azure AD, you need to add Birst Agile Business Analytics from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5eefc-125">**Utför följande steg för att lägga till Birst flexibel Företagsanalys från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="5eefc-125">**To add Birst Agile Business Analytics from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5eefc-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5eefc-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5eefc-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="5eefc-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5eefc-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5eefc-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="5eefc-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5eefc-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="5eefc-133">I sökrutan skriver **Birst flexibel Företagsanalys**.</span><span class="sxs-lookup"><span data-stu-id="5eefc-133">In the search box, type **Birst Agile Business Analytics**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-birst-tutorial/tutorial_birst_search.png)

5. <span data-ttu-id="5eefc-135">Välj i resultatpanelen **Birst flexibel Företagsanalys**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="5eefc-135">In the results panel, select **Birst Agile Business Analytics**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-birst-tutorial/tutorial_birst_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5eefc-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5eefc-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5eefc-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Birst flexibel Företagsanalys baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="5eefc-138">In this section, you configure and test Azure AD single sign-on with Birst Agile Business Analytics based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5eefc-139">Azure AD måste du känna till användaren i Birst flexibel Företagsanalys motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="5eefc-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Birst Agile Business Analytics is to a user in Azure AD.</span></span> <span data-ttu-id="5eefc-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Birst flexibel Företagsanalys upprättas.</span><span class="sxs-lookup"><span data-stu-id="5eefc-140">In other words, a link relationship between an Azure AD user and the related user in Birst Agile Business Analytics needs to be established.</span></span>

<span data-ttu-id="5eefc-141">I Birst flexibel Företagsanalys, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="5eefc-141">In Birst Agile Business Analytics, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5eefc-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Birst flexibel Företagsanalys, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="5eefc-142">To configure and test Azure AD single sign-on with Birst Agile Business Analytics, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5eefc-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="5eefc-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5eefc-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5eefc-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5eefc-145">**[Skapa en testanvändare Birst flexibel Företagsanalys](#creating-a-birst-agile-business-analytics-test-user)**  – har en motsvarighet för Britta Simon Birst flexibel Företagsanalys som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="5eefc-145">**[Creating a Birst Agile Business Analytics test user](#creating-a-birst-agile-business-analytics-test-user)** - to have a counterpart of Britta Simon in Birst Agile Business Analytics that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5eefc-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5eefc-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5eefc-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="5eefc-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5eefc-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5eefc-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5eefc-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Birst flexibel Företagsanalys.</span><span class="sxs-lookup"><span data-stu-id="5eefc-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Birst Agile Business Analytics application.</span></span>

<span data-ttu-id="5eefc-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Birst flexibel Företagsanalys:**</span><span class="sxs-lookup"><span data-stu-id="5eefc-150">**To configure Azure AD single sign-on with Birst Agile Business Analytics, perform the following steps:**</span></span>

1. <span data-ttu-id="5eefc-151">I Azure-portalen på den **Birst flexibel Företagsanalys** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5eefc-151">In the Azure portal, on the **Birst Agile Business Analytics** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="5eefc-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5eefc-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-birst-tutorial/tutorial_birst_samlbase.png)

3. <span data-ttu-id="5eefc-155">På den **Birst flexibel Business Analytics domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="5eefc-155">On the **Birst Agile Business Analytics Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-birst-tutorial/tutorial_birst_url.png)

     <span data-ttu-id="5eefc-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span><span class="sxs-lookup"><span data-stu-id="5eefc-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span></span>

     <span data-ttu-id="5eefc-158">URL: en är beroende av datacenter att ditt konto Birst finns:</span><span class="sxs-lookup"><span data-stu-id="5eefc-158">The URL depends on the datacenter that your Birst account is located:</span></span> 

     * <span data-ttu-id="5eefc-159">För USA datacenter använder du följande mönstret:`https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span><span class="sxs-lookup"><span data-stu-id="5eefc-159">For US datacenter use following the pattern: `https://login.bws.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span></span> 

     * <span data-ttu-id="5eefc-160">För Europa datacenter använder du följande mönster:`https://login.eu1.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span><span class="sxs-lookup"><span data-stu-id="5eefc-160">For Europe datacenter use the following pattern: `https://login.eu1.birst.com/SAMLSSO/Services.aspx?birst.idpid=TENANTIDPID`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5eefc-161">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="5eefc-161">This value is not real.</span></span> <span data-ttu-id="5eefc-162">Uppdatera värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="5eefc-162">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="5eefc-163">Kontakta [Birst flexibel Business Analytics Client supportteamet](mailto:info@birst.com) värdet hämtas.</span><span class="sxs-lookup"><span data-stu-id="5eefc-163">Contact [Birst Agile Business Analytics Client support team](mailto:info@birst.com) to get the value.</span></span> 
 
4. <span data-ttu-id="5eefc-164">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="5eefc-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-birst-tutorial/tutorial_birst_certificate.png) 

5. <span data-ttu-id="5eefc-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5eefc-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-birst-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5eefc-168">På den **Birst flexibel Business Analytics Configuration** klickar du på **konfigurera Birst flexibel Företagsanalys** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="5eefc-168">On the **Birst Agile Business Analytics Configuration** section, click **Configure Birst Agile Business Analytics** to open **Configure sign-on** window.</span></span> <span data-ttu-id="5eefc-169">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="5eefc-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-birst-tutorial/tutorial_birst_configure.png) 

7. <span data-ttu-id="5eefc-171">Konfigurera enkel inloggning på **Birst flexibel Företagsanalys** sida, måste du skicka den hämtade **certifikat (Base64)**, **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** till [Birst flexibel Företagsanalys supportteam](mailto:info@birst.com).</span><span class="sxs-lookup"><span data-stu-id="5eefc-171">To configure single sign-on on **Birst Agile Business Analytics** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Birst Agile Business Analytics support team](mailto:info@birst.com).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="5eefc-172">Nämnt Birst team som den här integreringen behöver SHA256-algoritmen (SHA1 stöds inte) så att de kan konfigurera SSO på rätt server som **app2101** osv.</span><span class="sxs-lookup"><span data-stu-id="5eefc-172">Mention to Birst team that this integration needs SHA256 Algorithm (SHA1 will not be supported) so that they can set the SSO on the appropriate server like **app2101** etc.</span></span>
  

> [!TIP]
> <span data-ttu-id="5eefc-173">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="5eefc-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5eefc-174">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="5eefc-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5eefc-175">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5eefc-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5eefc-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="5eefc-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="5eefc-177">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5eefc-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="5eefc-179">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="5eefc-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5eefc-180">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5eefc-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-birst-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5eefc-182">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="5eefc-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-birst-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5eefc-184">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5eefc-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-birst-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5eefc-186">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="5eefc-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-birst-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5eefc-188">a.</span><span class="sxs-lookup"><span data-stu-id="5eefc-188">a.</span></span> <span data-ttu-id="5eefc-189">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5eefc-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5eefc-190">b.</span><span class="sxs-lookup"><span data-stu-id="5eefc-190">b.</span></span> <span data-ttu-id="5eefc-191">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5eefc-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5eefc-192">c.</span><span class="sxs-lookup"><span data-stu-id="5eefc-192">c.</span></span> <span data-ttu-id="5eefc-193">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="5eefc-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5eefc-194">d.</span><span class="sxs-lookup"><span data-stu-id="5eefc-194">d.</span></span> <span data-ttu-id="5eefc-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5eefc-195">Click **Create**.</span></span>
 
### <a name="creating-a-birst-agile-business-analytics-test-user"></a><span data-ttu-id="5eefc-196">Skapa en testanvändare Birst flexibel Företagsanalys</span><span class="sxs-lookup"><span data-stu-id="5eefc-196">Creating a Birst Agile Business Analytics test user</span></span>

<span data-ttu-id="5eefc-197">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Birst flexibel Företagsanalys.</span><span class="sxs-lookup"><span data-stu-id="5eefc-197">The objective of this section is to create a user called Britta Simon in Birst Agile Business Analytics.</span></span> <span data-ttu-id="5eefc-198">Arbeta med [Birst flexibel Företagsanalys supportteam](mailto:info@birst.com) att lägga till användare i Birst-konto.</span><span class="sxs-lookup"><span data-stu-id="5eefc-198">Work with [Birst Agile Business Analytics support team](mailto:info@birst.com) to add the users in the Birst account.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5eefc-199">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="5eefc-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5eefc-200">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Birst flexibel Företagsanalys.</span><span class="sxs-lookup"><span data-stu-id="5eefc-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Birst Agile Business Analytics.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="5eefc-202">**Om du vill tilldela Birst flexibel Företagsanalys Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5eefc-202">**To assign Britta Simon to Birst Agile Business Analytics, perform the following steps:**</span></span>

1. <span data-ttu-id="5eefc-203">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5eefc-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="5eefc-205">Välj i listan med program **Birst flexibel Företagsanalys**.</span><span class="sxs-lookup"><span data-stu-id="5eefc-205">In the applications list, select **Birst Agile Business Analytics**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-birst-tutorial/tutorial_birst_app.png) 

3. <span data-ttu-id="5eefc-207">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="5eefc-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="5eefc-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="5eefc-209">Click **Add** button.</span></span> <span data-ttu-id="5eefc-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5eefc-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="5eefc-212">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="5eefc-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5eefc-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5eefc-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5eefc-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5eefc-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5eefc-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5eefc-215">Testing single sign-on</span></span>

<span data-ttu-id="5eefc-216">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="5eefc-216">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="5eefc-217">När du klickar på panelen Birst flexibel Företagsanalys på åtkomstpanelen du bör få automatiskt loggat in på ditt Birst flexibel Företagsanalys program.</span><span class="sxs-lookup"><span data-stu-id="5eefc-217">When you click the Birst Agile Business Analytics tile in the Access Panel, you should get automatically signed-on to your Birst Agile Business Analytics application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5eefc-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="5eefc-218">Additional resources</span></span>

* [<span data-ttu-id="5eefc-219">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5eefc-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5eefc-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5eefc-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-birst-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-birst-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-birst-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-birst-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-birst-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-birst-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-birst-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-birst-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-birst-tutorial/tutorial_general_203.png

