---
title: "Självstudier: Azure Active Directory-integrering med Nexonia | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Nexonia."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a93b771a-9bc3-444a-bdc0-457f8bb7e780
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/1/2017
ms.author: jeedes
ms.openlocfilehash: 1370fa64c2ddc25d3121c567ceea4828b1e50921
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-nexonia"></a><span data-ttu-id="48dd4-103">Självstudier: Azure Active Directory-integrering med Nexonia</span><span class="sxs-lookup"><span data-stu-id="48dd4-103">Tutorial: Azure Active Directory integration with Nexonia</span></span>

<span data-ttu-id="48dd4-104">I kursen får lära du att integrera Nexonia med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="48dd4-104">In this tutorial, you learn how to integrate Nexonia with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="48dd4-105">Integrera Nexonia med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="48dd4-105">Integrating Nexonia with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="48dd4-106">Du kan styra i Azure AD som har åtkomst till Nexonia</span><span class="sxs-lookup"><span data-stu-id="48dd4-106">You can control in Azure AD who has access to Nexonia</span></span>
- <span data-ttu-id="48dd4-107">Du kan aktivera användarna att automatiskt hämta loggat in på Nexonia (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="48dd4-107">You can enable your users to automatically get signed-on to Nexonia (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="48dd4-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="48dd4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="48dd4-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns i.</span><span class="sxs-lookup"><span data-stu-id="48dd4-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="48dd4-110">[Vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="48dd4-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48dd4-111">Krav</span><span class="sxs-lookup"><span data-stu-id="48dd4-111">Prerequisites</span></span>

<span data-ttu-id="48dd4-112">För att konfigurera Azure AD-integrering med Nexonia, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="48dd4-112">To configure Azure AD integration with Nexonia, you need the following items:</span></span>

- <span data-ttu-id="48dd4-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="48dd4-113">An Azure AD subscription</span></span>
- <span data-ttu-id="48dd4-114">En Nexonia enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="48dd4-114">A Nexonia single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="48dd4-115">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="48dd4-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="48dd4-116">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="48dd4-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="48dd4-117">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="48dd4-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="48dd4-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="48dd4-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="48dd4-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="48dd4-119">Scenario description</span></span>
<span data-ttu-id="48dd4-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="48dd4-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="48dd4-121">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="48dd4-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="48dd4-122">Att lägga till Nexonia från galleriet</span><span class="sxs-lookup"><span data-stu-id="48dd4-122">Adding Nexonia from the gallery</span></span>
2. <span data-ttu-id="48dd4-123">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="48dd4-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-nexonia-from-the-gallery"></a><span data-ttu-id="48dd4-124">Att lägga till Nexonia från galleriet</span><span class="sxs-lookup"><span data-stu-id="48dd4-124">Adding Nexonia from the gallery</span></span>
<span data-ttu-id="48dd4-125">Du måste lägga till Nexonia från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Nexonia i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="48dd4-125">To configure the integration of Nexonia into Azure AD, you need to add Nexonia from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="48dd4-126">**Utför följande steg för att lägga till Nexonia från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="48dd4-126">**To add Nexonia from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="48dd4-127">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="48dd4-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="48dd4-129">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="48dd4-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="48dd4-130">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="48dd4-130">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="48dd4-132">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="48dd4-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="48dd4-134">I sökrutan skriver **Nexonia**.</span><span class="sxs-lookup"><span data-stu-id="48dd4-134">In the search box, type **Nexonia**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_search.png)

5. <span data-ttu-id="48dd4-136">Välj i resultatpanelen **Nexonia**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="48dd4-136">In the results panel, select **Nexonia**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="48dd4-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="48dd4-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="48dd4-139">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Nexonia baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="48dd4-139">In this section, you configure and test Azure AD single sign-on with Nexonia based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="48dd4-140">Azure AD måste du känna till användaren i Nexonia motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="48dd4-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Nexonia is to a user in Azure AD.</span></span> <span data-ttu-id="48dd4-141">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Nexonia upprättas.</span><span class="sxs-lookup"><span data-stu-id="48dd4-141">In other words, a link relationship between an Azure AD user and the related user in Nexonia needs to be established.</span></span>

<span data-ttu-id="48dd4-142">I Nexonia, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="48dd4-142">In Nexonia, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="48dd4-143">Om du vill konfigurera och testa Azure AD enkel inloggning med Nexonia, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="48dd4-143">To configure and test Azure AD single sign-on with Nexonia, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="48dd4-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="48dd4-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="48dd4-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="48dd4-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="48dd4-146">**[Skapa en testanvändare Nexonia](#creating-a-nexonia-test-user)**  – du har en motsvarighet för Britta Simon i Nexonia som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="48dd4-146">**[Creating a Nexonia test user](#creating-a-nexonia-test-user)** - to have a counterpart of Britta Simon in Nexonia that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="48dd4-147">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="48dd4-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="48dd4-148">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="48dd4-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="48dd4-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="48dd4-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="48dd4-150">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Nexonia program.</span><span class="sxs-lookup"><span data-stu-id="48dd4-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Nexonia application.</span></span>

>[!Note]
><span data-ttu-id="48dd4-151">Om du har problem i integrationen och sedan refererar detta [länken](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?WT.mc_id=UI_AAD_Enterprise_Apps_SupportOrTroubleshooting) för guide för felsökning.</span><span class="sxs-lookup"><span data-stu-id="48dd4-151">If you have issues in the integration then refer this [link](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?WT.mc_id=UI_AAD_Enterprise_Apps_SupportOrTroubleshooting) for troubleshooting guide.</span></span> <span data-ttu-id="48dd4-152">Om du fortfarande inte har hittat lösningen, öka du supportbegäran från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="48dd4-152">If you still have not found the solution, then raise the support request from Azure portal.</span></span>

<span data-ttu-id="48dd4-153">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Nexonia:**</span><span class="sxs-lookup"><span data-stu-id="48dd4-153">**To configure Azure AD single sign-on with Nexonia, perform the following steps:**</span></span>

1. <span data-ttu-id="48dd4-154">I Azure-portalen på den **Nexonia** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="48dd4-154">In the Azure portal, on the **Nexonia** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="48dd4-156">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="48dd4-156">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_samlbase.png)

3. <span data-ttu-id="48dd4-158">På den **Nexonia domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="48dd4-158">On the **Nexonia Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_url.png)

    <span data-ttu-id="48dd4-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://system.nexonia.com/assistant/saml.do?orgCode=<organizationcode>`</span><span class="sxs-lookup"><span data-stu-id="48dd4-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://system.nexonia.com/assistant/saml.do?orgCode=<organizationcode>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="48dd4-161">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="48dd4-161">This value is not real.</span></span> <span data-ttu-id="48dd4-162">Uppdatera det här värdet med det faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="48dd4-162">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="48dd4-163">Kontakta [Nexonia supportteamet](https://nexonia.zendesk.com/hc/requests/new) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="48dd4-163">Contact [Nexonia support team](https://nexonia.zendesk.com/hc/requests/new) to get this value.</span></span> 


4. <span data-ttu-id="48dd4-164">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="48dd4-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_certificate.png) 

5. <span data-ttu-id="48dd4-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="48dd4-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-nexonia-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="48dd4-168">På den **Nexonia Configuration** klickar du på **konfigurera Nexonia** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="48dd4-168">On the **Nexonia Configuration** section, click **Configure Nexonia** to open **Configure sign-on** window.</span></span> <span data-ttu-id="48dd4-169">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="48dd4-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_configure.png) 

7. <span data-ttu-id="48dd4-171">För att få SSO konfigurerats för ditt program, kontakta [Nexonia supportteamet](https://nexonia.zendesk.com/hc/requests/new) och ge dem med följande:</span><span class="sxs-lookup"><span data-stu-id="48dd4-171">To get SSO configured for your application, contact [Nexonia support team](https://nexonia.zendesk.com/hc/requests/new) and provide them with the following:</span></span>

    <span data-ttu-id="48dd4-172">• Den hämtade **certifikat**</span><span class="sxs-lookup"><span data-stu-id="48dd4-172">• The downloaded **certificate**</span></span>

    <span data-ttu-id="48dd4-173">• Den **SAML enhets-ID**</span><span class="sxs-lookup"><span data-stu-id="48dd4-173">• The **SAML Entity ID**</span></span>

    <span data-ttu-id="48dd4-174">• Den **URL för SAML-tjänst för enkel inloggning**</span><span class="sxs-lookup"><span data-stu-id="48dd4-174">• The **SAML Single Sign-On Service URL**</span></span>

    <span data-ttu-id="48dd4-175">• Den **URL för utloggning**</span><span class="sxs-lookup"><span data-stu-id="48dd4-175">• The **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="48dd4-176">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="48dd4-176">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="48dd4-177">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="48dd4-177">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="48dd4-178">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="48dd4-178">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="48dd4-179">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="48dd4-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="48dd4-180">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="48dd4-180">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="48dd4-182">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="48dd4-182">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="48dd4-183">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="48dd4-183">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-nexonia-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="48dd4-185">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="48dd4-185">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-nexonia-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="48dd4-187">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="48dd4-187">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-nexonia-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="48dd4-189">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="48dd4-189">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-nexonia-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="48dd4-191">a.</span><span class="sxs-lookup"><span data-stu-id="48dd4-191">a.</span></span> <span data-ttu-id="48dd4-192">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="48dd4-192">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="48dd4-193">b.</span><span class="sxs-lookup"><span data-stu-id="48dd4-193">b.</span></span> <span data-ttu-id="48dd4-194">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="48dd4-194">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="48dd4-195">c.</span><span class="sxs-lookup"><span data-stu-id="48dd4-195">c.</span></span> <span data-ttu-id="48dd4-196">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="48dd4-196">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="48dd4-197">d.</span><span class="sxs-lookup"><span data-stu-id="48dd4-197">d.</span></span> <span data-ttu-id="48dd4-198">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="48dd4-198">Click **Create**.</span></span>
 
### <a name="creating-a-nexonia-test-user"></a><span data-ttu-id="48dd4-199">Skapa en testanvändare Nexonia</span><span class="sxs-lookup"><span data-stu-id="48dd4-199">Creating a Nexonia test user</span></span>

<span data-ttu-id="48dd4-200">I det här avsnittet skapar du en användare som kallas Britta Simon i Nexonia.</span><span class="sxs-lookup"><span data-stu-id="48dd4-200">In this section, you create a user called Britta Simon in Nexonia.</span></span> <span data-ttu-id="48dd4-201">Arbeta med [Nexonia supportteamet](https://nexonia.zendesk.com/hc/requests/new) att lägga till användare i Nexonia-plattformen.</span><span class="sxs-lookup"><span data-stu-id="48dd4-201">Work with [Nexonia support team](https://nexonia.zendesk.com/hc/requests/new) to add the users in the Nexonia platform.</span></span> <span data-ttu-id="48dd4-202">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="48dd4-202">Users must be created and activated before you use single sign-on.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="48dd4-203">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="48dd4-203">Assigning the Azure AD test user</span></span>

<span data-ttu-id="48dd4-204">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Nexonia.</span><span class="sxs-lookup"><span data-stu-id="48dd4-204">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Nexonia.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="48dd4-206">**Om du vill tilldela Nexonia Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="48dd4-206">**To assign Britta Simon to Nexonia, perform the following steps:**</span></span>

1. <span data-ttu-id="48dd4-207">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="48dd4-207">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="48dd4-209">Välj i listan med program **Nexonia**.</span><span class="sxs-lookup"><span data-stu-id="48dd4-209">In the applications list, select **Nexonia**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_app.png) 

3. <span data-ttu-id="48dd4-211">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="48dd4-211">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="48dd4-213">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="48dd4-213">Click **Add** button.</span></span> <span data-ttu-id="48dd4-214">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="48dd4-214">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="48dd4-216">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="48dd4-216">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="48dd4-217">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="48dd4-217">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="48dd4-218">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="48dd4-218">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="48dd4-219">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="48dd4-219">Testing single sign-on</span></span>

<span data-ttu-id="48dd4-220">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="48dd4-220">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="48dd4-221">När du klickar på panelen Nexonia på åtkomstpanelen du bör få automatiskt loggat in på ditt Nexonia program.</span><span class="sxs-lookup"><span data-stu-id="48dd4-221">When you click the Nexonia tile in the Access Panel, you should get automatically signed-on to your Nexonia application.</span></span>
<span data-ttu-id="48dd4-222">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="48dd4-222">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48dd4-223">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="48dd4-223">Additional resources</span></span>

* [<span data-ttu-id="48dd4-224">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="48dd4-224">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="48dd4-225">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="48dd4-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_203.png

