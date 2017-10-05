---
title: "Självstudier: Azure Active Directory-integrering med EthicsPoint Incident Management (EPIM) | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och EthicsPoint Incident Management (EPIM)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8cb31a4c-9309-469b-93ac-daf0d3c7a3e6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: b5ac3afd973b5765ba151e766754934b49ac0e0c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ethicspoint-incident-management-epim"></a><span data-ttu-id="ddf53-103">Självstudier: Azure Active Directory-integrering med EthicsPoint Incident Management (EPIM)</span><span class="sxs-lookup"><span data-stu-id="ddf53-103">Tutorial: Azure Active Directory integration with EthicsPoint Incident Management (EPIM)</span></span>

<span data-ttu-id="ddf53-104">I kursen får lära du att integrera EthicsPoint Incident Management (EPIM) med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ddf53-104">In this tutorial, you learn how to integrate EthicsPoint Incident Management (EPIM) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ddf53-105">Integrera EthicsPoint Incident Management (EPIM) med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ddf53-105">Integrating EthicsPoint Incident Management (EPIM) with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ddf53-106">Du kan styra i Azure AD som har åtkomst till EthicsPoint Incident Management (EPIM)</span><span class="sxs-lookup"><span data-stu-id="ddf53-106">You can control in Azure AD who has access to EthicsPoint Incident Management (EPIM)</span></span>
- <span data-ttu-id="ddf53-107">Du kan aktivera användarna att automatiskt hämta inloggade till EthicsPoint Incident Management (EPIM) (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ddf53-107">You can enable your users to automatically get signed-on to EthicsPoint Incident Management (EPIM) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ddf53-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ddf53-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ddf53-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ddf53-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ddf53-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ddf53-110">Prerequisites</span></span>

<span data-ttu-id="ddf53-111">Om du vill konfigurera Azure AD-integrering med EthicsPoint Incident Management (EPIM) behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="ddf53-111">To configure Azure AD integration with EthicsPoint Incident Management (EPIM), you need the following items:</span></span>

- <span data-ttu-id="ddf53-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ddf53-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ddf53-113">En EthicsPoint Incident Management (EPIM) enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="ddf53-113">A EthicsPoint Incident Management (EPIM) single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ddf53-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ddf53-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ddf53-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ddf53-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ddf53-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ddf53-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ddf53-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ddf53-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ddf53-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ddf53-118">Scenario description</span></span>
<span data-ttu-id="ddf53-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ddf53-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ddf53-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ddf53-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ddf53-121">Att lägga till EthicsPoint Incident Management (EPIM) från galleriet</span><span class="sxs-lookup"><span data-stu-id="ddf53-121">Adding EthicsPoint Incident Management (EPIM) from the gallery</span></span>
2. <span data-ttu-id="ddf53-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ddf53-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ethicspoint-incident-management-epim-from-the-gallery"></a><span data-ttu-id="ddf53-123">Att lägga till EthicsPoint Incident Management (EPIM) från galleriet</span><span class="sxs-lookup"><span data-stu-id="ddf53-123">Adding EthicsPoint Incident Management (EPIM) from the gallery</span></span>
<span data-ttu-id="ddf53-124">Du måste lägga till EthicsPoint Incident Management (EPIM) från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av EthicsPoint Incident Management (EPIM) i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ddf53-124">To configure the integration of EthicsPoint Incident Management (EPIM) into Azure AD, you need to add EthicsPoint Incident Management (EPIM) from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ddf53-125">**Utför följande steg för att lägga till EthicsPoint Incident Management (EPIM) från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="ddf53-125">**To add EthicsPoint Incident Management (EPIM) from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ddf53-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ddf53-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ddf53-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ddf53-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ddf53-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ddf53-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="ddf53-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ddf53-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="ddf53-133">I sökrutan skriver **EthicsPoint Incident Management (EPIM)**.</span><span class="sxs-lookup"><span data-stu-id="ddf53-133">In the search box, type **EthicsPoint Incident Management (EPIM)**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_search.png)

5. <span data-ttu-id="ddf53-135">Välj i resultatpanelen **EthicsPoint Incident Management (EPIM)**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="ddf53-135">In the results panel, select **EthicsPoint Incident Management (EPIM)**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ddf53-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ddf53-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ddf53-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med EthicsPoint Incident Management (EPIM) baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ddf53-138">In this section, you configure and test Azure AD single sign-on with EthicsPoint Incident Management (EPIM) based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ddf53-139">Azure AD måste du känna till motsvarande användaren i EthicsPoint Incident Management (EPIM) till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="ddf53-139">For single sign-on to work, Azure AD needs to know what the counterpart user in EthicsPoint Incident Management (EPIM) is to a user in Azure AD.</span></span> <span data-ttu-id="ddf53-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i EthicsPoint Incident Management (EPIM) upprättas.</span><span class="sxs-lookup"><span data-stu-id="ddf53-140">In other words, a link relationship between an Azure AD user and the related user in EthicsPoint Incident Management (EPIM) needs to be established.</span></span>

<span data-ttu-id="ddf53-141">I EthicsPoint Incident Management (EPIM), tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="ddf53-141">In EthicsPoint Incident Management (EPIM), assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ddf53-142">Om du vill konfigurera och testa Azure AD enkel inloggning med EthicsPoint Incident Management (EPIM), måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ddf53-142">To configure and test Azure AD single sign-on with EthicsPoint Incident Management (EPIM), you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ddf53-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ddf53-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ddf53-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ddf53-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ddf53-145">**[Skapa en testanvändare EthicsPoint Incident Management (EPIM)](#creating-a-ethicspoint-incident-management-epim-test-user)**  – du har en motsvarighet för Britta Simon i EthicsPoint Incident Management (EPIM) och som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ddf53-145">**[Creating a EthicsPoint Incident Management (EPIM) test user](#creating-a-ethicspoint-incident-management-epim-test-user)** - to have a counterpart of Britta Simon in EthicsPoint Incident Management (EPIM) that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ddf53-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ddf53-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ddf53-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ddf53-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ddf53-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ddf53-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ddf53-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet EthicsPoint Incident Management (EPIM).</span><span class="sxs-lookup"><span data-stu-id="ddf53-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your EthicsPoint Incident Management (EPIM) application.</span></span>

<span data-ttu-id="ddf53-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med EthicsPoint Incident Management (EPIM):**</span><span class="sxs-lookup"><span data-stu-id="ddf53-150">**To configure Azure AD single sign-on with EthicsPoint Incident Management (EPIM), perform the following steps:**</span></span>

1. <span data-ttu-id="ddf53-151">I Azure-portalen på den **EthicsPoint Incident Management (EPIM)** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ddf53-151">In the Azure portal, on the **EthicsPoint Incident Management (EPIM)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="ddf53-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ddf53-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_samlbase.png)

3. <span data-ttu-id="ddf53-155">På den **EthicsPoint Incident Management (EPIM)-domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ddf53-155">On the **EthicsPoint Incident Management (EPIM) Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_url.png)

    <span data-ttu-id="ddf53-157">a.</span><span class="sxs-lookup"><span data-stu-id="ddf53-157">a.</span></span> <span data-ttu-id="ddf53-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="ddf53-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.navexglobal.com`|
    | `https://<companyname>.ethicspointvp.com`|

    <span data-ttu-id="ddf53-159">b.</span><span class="sxs-lookup"><span data-stu-id="ddf53-159">b.</span></span> <span data-ttu-id="ddf53-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.navexglobal.com/adfs/services/trust`</span><span class="sxs-lookup"><span data-stu-id="ddf53-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.navexglobal.com/adfs/services/trust`</span></span>

    <span data-ttu-id="ddf53-161">c.</span><span class="sxs-lookup"><span data-stu-id="ddf53-161">c.</span></span> <span data-ttu-id="ddf53-162">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<servername>.navexglobal.com/adfs/ls/`</span><span class="sxs-lookup"><span data-stu-id="ddf53-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://<servername>.navexglobal.com/adfs/ls/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ddf53-163">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="ddf53-163">These values are not real.</span></span> <span data-ttu-id="ddf53-164">Uppdatera dessa värden med den faktiska Reply URL, identifierare och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="ddf53-164">Update these values with the actual Reply URL, Identifier, and Sign-On URL.</span></span> <span data-ttu-id="ddf53-165">Kontakta [EthicsPoint Incident Management (EPIM) klienten supportteamet](http://www.navexglobal.com/company/contact-us) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="ddf53-165">Contact [EthicsPoint Incident Management (EPIM) Client support team](http://www.navexglobal.com/company/contact-us) to get these values.</span></span> 

4. <span data-ttu-id="ddf53-166">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="ddf53-166">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_certificate.png) 

5. <span data-ttu-id="ddf53-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ddf53-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="ddf53-170">Konfigurera enkel inloggning på **EthicsPoint Incident Management (EPIM)** sida, måste du skicka den hämtade **XML-Metadata för** till [supportteamet EthicsPoint Incident Management (EPIM) ](http://www.navexglobal.com/company/contact-us).</span><span class="sxs-lookup"><span data-stu-id="ddf53-170">To configure single sign-on on **EthicsPoint Incident Management (EPIM)** side, you need to send the downloaded **Metadata XML** to [EthicsPoint Incident Management (EPIM) support team](http://www.navexglobal.com/company/contact-us).</span></span>

> [!TIP]
> <span data-ttu-id="ddf53-171">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="ddf53-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ddf53-172">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="ddf53-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ddf53-173">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ddf53-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ddf53-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ddf53-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="ddf53-175">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ddf53-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="ddf53-177">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="ddf53-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ddf53-178">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ddf53-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ddf53-180">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ddf53-180">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ddf53-182">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ddf53-182">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ddf53-184">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ddf53-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ddf53-186">a.</span><span class="sxs-lookup"><span data-stu-id="ddf53-186">a.</span></span> <span data-ttu-id="ddf53-187">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ddf53-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ddf53-188">b.</span><span class="sxs-lookup"><span data-stu-id="ddf53-188">b.</span></span> <span data-ttu-id="ddf53-189">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ddf53-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ddf53-190">c.</span><span class="sxs-lookup"><span data-stu-id="ddf53-190">c.</span></span> <span data-ttu-id="ddf53-191">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ddf53-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ddf53-192">d.</span><span class="sxs-lookup"><span data-stu-id="ddf53-192">d.</span></span> <span data-ttu-id="ddf53-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ddf53-193">Click **Create**.</span></span>
 
### <a name="creating-a-ethicspoint-incident-management-epim-test-user"></a><span data-ttu-id="ddf53-194">Skapa en testanvändare EthicsPoint Incident Management (EPIM)</span><span class="sxs-lookup"><span data-stu-id="ddf53-194">Creating a EthicsPoint Incident Management (EPIM) test user</span></span>

<span data-ttu-id="ddf53-195">I det här avsnittet skapar du en användare som kallas Britta Simon i EthicsPoint Incident Management (EPIM).</span><span class="sxs-lookup"><span data-stu-id="ddf53-195">In this section, you create a user called Britta Simon in EthicsPoint Incident Management (EPIM).</span></span> <span data-ttu-id="ddf53-196">Se tillsammans med [EthicsPoint Incident Management (EPIM) supportteamet](http://www.navexglobal.com/company/contact-us) att lägga till användare i EthicsPoint Incident Management (EPIM)-plattformen.</span><span class="sxs-lookup"><span data-stu-id="ddf53-196">Please work with [EthicsPoint Incident Management (EPIM) support team](http://www.navexglobal.com/company/contact-us) to add the users in the EthicsPoint Incident Management (EPIM) platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ddf53-197">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="ddf53-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ddf53-198">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till EthicsPoint Incident Management (EPIM).</span><span class="sxs-lookup"><span data-stu-id="ddf53-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to EthicsPoint Incident Management (EPIM).</span></span>

![Tilldela användare][200] 

<span data-ttu-id="ddf53-200">**Om du vill tilldela Britta Simon till EthicsPoint Incident Management (EPIM), utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ddf53-200">**To assign Britta Simon to EthicsPoint Incident Management (EPIM), perform the following steps:**</span></span>

1. <span data-ttu-id="ddf53-201">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ddf53-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ddf53-203">Välj i listan med program **EthicsPoint Incident Management (EPIM)**.</span><span class="sxs-lookup"><span data-stu-id="ddf53-203">In the applications list, select **EthicsPoint Incident Management (EPIM)**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspoint_app.png) 

3. <span data-ttu-id="ddf53-205">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ddf53-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="ddf53-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ddf53-207">Click **Add** button.</span></span> <span data-ttu-id="ddf53-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ddf53-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="ddf53-210">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="ddf53-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ddf53-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ddf53-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ddf53-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ddf53-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ddf53-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ddf53-213">Testing single sign-on</span></span>

<span data-ttu-id="ddf53-214">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="ddf53-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>
<span data-ttu-id="ddf53-215">När du klickar på panelen EthicsPoint Incident Management (EPIM) på åtkomstpanelen du ska hämta automatiskt loggat in på ditt program EthicsPoint Incident Management (EPIM).</span><span class="sxs-lookup"><span data-stu-id="ddf53-215">When you click the EthicsPoint Incident Management (EPIM) tile in the Access Panel, you should get automatically signed-on to your EthicsPoint Incident Management (EPIM) application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ddf53-216">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ddf53-216">Additional resources</span></span>

* [<span data-ttu-id="ddf53-217">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ddf53-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ddf53-218">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ddf53-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_203.png

