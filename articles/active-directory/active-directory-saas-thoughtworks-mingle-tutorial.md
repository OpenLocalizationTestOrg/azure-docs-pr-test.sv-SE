---
title: "Självstudier: Azure Active Directory-integrering med Thoughtworks Mingle | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Thoughtworks Mingle."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 268ae5affb88a718f68c08daa94fe7aba4a99c11
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a><span data-ttu-id="52c2a-103">Självstudier: Azure Active Directory-integrering med Thoughtworks Mingle</span><span class="sxs-lookup"><span data-stu-id="52c2a-103">Tutorial: Azure Active Directory integration with Thoughtworks Mingle</span></span>

<span data-ttu-id="52c2a-104">I kursen får lära du att integrera Thoughtworks Mingle med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="52c2a-104">In this tutorial, you learn how to integrate Thoughtworks Mingle with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="52c2a-105">Integrera Thoughtworks Mingle med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="52c2a-105">Integrating Thoughtworks Mingle with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="52c2a-106">Du kan styra i Azure AD som har åtkomst till Thoughtworks Mingle</span><span class="sxs-lookup"><span data-stu-id="52c2a-106">You can control in Azure AD who has access to Thoughtworks Mingle</span></span>
- <span data-ttu-id="52c2a-107">Du kan aktivera användarna att automatiskt hämta loggat in på Thoughtworks Mingle (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="52c2a-107">You can enable your users to automatically get signed-on to Thoughtworks Mingle (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="52c2a-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="52c2a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="52c2a-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="52c2a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="52c2a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="52c2a-110">Prerequisites</span></span>

<span data-ttu-id="52c2a-111">För att konfigurera Azure AD-integrering med Thoughtworks Mingle, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="52c2a-111">To configure Azure AD integration with Thoughtworks Mingle, you need the following items:</span></span>

- <span data-ttu-id="52c2a-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="52c2a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="52c2a-113">En Thoughtworks Mingle enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="52c2a-113">A Thoughtworks Mingle single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="52c2a-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="52c2a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="52c2a-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="52c2a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="52c2a-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="52c2a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="52c2a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="52c2a-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="52c2a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="52c2a-118">Scenario description</span></span>
<span data-ttu-id="52c2a-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="52c2a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="52c2a-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="52c2a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="52c2a-121">Att lägga till Thoughtworks Mingle från galleriet</span><span class="sxs-lookup"><span data-stu-id="52c2a-121">Adding Thoughtworks Mingle from the gallery</span></span>
2. <span data-ttu-id="52c2a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="52c2a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-thoughtworks-mingle-from-the-gallery"></a><span data-ttu-id="52c2a-123">Att lägga till Thoughtworks Mingle från galleriet</span><span class="sxs-lookup"><span data-stu-id="52c2a-123">Adding Thoughtworks Mingle from the gallery</span></span>
<span data-ttu-id="52c2a-124">Du måste lägga till Thoughtworks Mingle från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Thoughtworks Mingle i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="52c2a-124">To configure the integration of Thoughtworks Mingle into Azure AD, you need to add Thoughtworks Mingle from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="52c2a-125">**Om du vill lägga till Thoughtworks Mingle från galleriet, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="52c2a-125">**To add Thoughtworks Mingle from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="52c2a-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="52c2a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="52c2a-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="52c2a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="52c2a-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="52c2a-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="52c2a-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="52c2a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="52c2a-133">I sökrutan skriver **Thoughtworks Mingle**väljer **Thoughtworks Mingle** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="52c2a-133">In the search box, type **Thoughtworks Mingle**, select **Thoughtworks Mingle** from result panel then click **Add** button to add the application.</span></span>

    ![Thoughtworks Mingle i resultatlistan](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="52c2a-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="52c2a-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="52c2a-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Thoughtworks Mingle baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="52c2a-136">In this section, you configure and test Azure AD single sign-on with Thoughtworks Mingle based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="52c2a-137">Azure AD måste du känna till användaren i Thoughtworks Mingle motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="52c2a-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Thoughtworks Mingle is to a user in Azure AD.</span></span> <span data-ttu-id="52c2a-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Thoughtworks Mingle upprättas.</span><span class="sxs-lookup"><span data-stu-id="52c2a-138">In other words, a link relationship between an Azure AD user and the related user in Thoughtworks Mingle needs to be established.</span></span>

<span data-ttu-id="52c2a-139">I Thoughtworks Mingle, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="52c2a-139">In Thoughtworks Mingle, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="52c2a-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Thoughtworks Mingle, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="52c2a-140">To configure and test Azure AD single sign-on with Thoughtworks Mingle, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="52c2a-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="52c2a-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="52c2a-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="52c2a-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="52c2a-143">**[Skapa en testanvändare Thoughtworks Mingle](#create-a-thoughtworks-mingle-test-user)**  – har en motsvarighet för Britta Simon Thoughtworks Mingle som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="52c2a-143">**[Create a Thoughtworks Mingle test user](#create-a-thoughtworks-mingle-test-user)** - to have a counterpart of Britta Simon in Thoughtworks Mingle that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="52c2a-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="52c2a-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="52c2a-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="52c2a-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="52c2a-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="52c2a-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="52c2a-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Thoughtworks Mingle program.</span><span class="sxs-lookup"><span data-stu-id="52c2a-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Thoughtworks Mingle application.</span></span>

<span data-ttu-id="52c2a-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Thoughtworks Mingle:**</span><span class="sxs-lookup"><span data-stu-id="52c2a-148">**To configure Azure AD single sign-on with Thoughtworks Mingle, perform the following steps:**</span></span>

1. <span data-ttu-id="52c2a-149">I Azure-portalen på den **Thoughtworks Mingle** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="52c2a-149">In the Azure portal, on the **Thoughtworks Mingle** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="52c2a-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="52c2a-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_samlbase.png)

3. <span data-ttu-id="52c2a-153">På den **Thoughtworks Mingle domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="52c2a-153">On the **Thoughtworks Mingle Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och Thoughtworks Mingle domän med enkel inloggning information](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_url.png)

    <span data-ttu-id="52c2a-155">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.mingle.thoughtworks.com`</span><span class="sxs-lookup"><span data-stu-id="52c2a-155">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.mingle.thoughtworks.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="52c2a-156">Värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="52c2a-156">The value is not real.</span></span> <span data-ttu-id="52c2a-157">Uppdatera värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="52c2a-157">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="52c2a-158">Kontakta [Thoughtworks Mingle klienten supportteamet](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) värdet hämtas.</span><span class="sxs-lookup"><span data-stu-id="52c2a-158">Contact [Thoughtworks Mingle Client support team](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) to get the value.</span></span> 
 
4. <span data-ttu-id="52c2a-159">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="52c2a-159">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_certificate.png) 

5. <span data-ttu-id="52c2a-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="52c2a-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="52c2a-163">Logga in på ditt **Thoughtworks Mingle** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="52c2a-163">Log in to your **Thoughtworks Mingle** company site as administrator.</span></span>

7. <span data-ttu-id="52c2a-164">Klicka på den **Admin** fliken och klicka sedan på **SSO Config**.</span><span class="sxs-lookup"><span data-stu-id="52c2a-164">Click the **Admin** tab, and then, click **SSO Config**.</span></span>
   
    <span data-ttu-id="52c2a-165">![Fliken Administration](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785157.png "SSO Config")</span><span class="sxs-lookup"><span data-stu-id="52c2a-165">![Admin tab](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785157.png "SSO Config")</span></span>

8. <span data-ttu-id="52c2a-166">I den **SSO Config** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="52c2a-166">In the **SSO Config** section, perform the following steps:</span></span>
   
    <span data-ttu-id="52c2a-167">![SSO Config](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785158.png "SSO Config")</span><span class="sxs-lookup"><span data-stu-id="52c2a-167">![SSO Config](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785158.png "SSO Config")</span></span>
    
    <span data-ttu-id="52c2a-168">a.</span><span class="sxs-lookup"><span data-stu-id="52c2a-168">a.</span></span> <span data-ttu-id="52c2a-169">Om du vill överföra metadatafilen, klickar du på **Välj fil**.</span><span class="sxs-lookup"><span data-stu-id="52c2a-169">To upload the metadata file, click **Choose file**.</span></span> 

    <span data-ttu-id="52c2a-170">b.</span><span class="sxs-lookup"><span data-stu-id="52c2a-170">b.</span></span> <span data-ttu-id="52c2a-171">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="52c2a-171">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="52c2a-172">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="52c2a-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="52c2a-173">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="52c2a-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="52c2a-174">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="52c2a-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="52c2a-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="52c2a-175">Create an Azure AD test user</span></span>
<span data-ttu-id="52c2a-176">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="52c2a-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="52c2a-178">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="52c2a-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="52c2a-179">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="52c2a-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="52c2a-181">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="52c2a-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="52c2a-183">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="52c2a-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Knappen Lägg till](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="52c2a-185">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="52c2a-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Dialogrutan användare](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="52c2a-187">a.</span><span class="sxs-lookup"><span data-stu-id="52c2a-187">a.</span></span> <span data-ttu-id="52c2a-188">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="52c2a-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="52c2a-189">b.</span><span class="sxs-lookup"><span data-stu-id="52c2a-189">b.</span></span> <span data-ttu-id="52c2a-190">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="52c2a-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="52c2a-191">c.</span><span class="sxs-lookup"><span data-stu-id="52c2a-191">c.</span></span> <span data-ttu-id="52c2a-192">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="52c2a-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="52c2a-193">d.</span><span class="sxs-lookup"><span data-stu-id="52c2a-193">d.</span></span> <span data-ttu-id="52c2a-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="52c2a-194">Click **Create**.</span></span>
 
### <a name="create-a-thoughtworks-mingle-test-user"></a><span data-ttu-id="52c2a-195">Skapa en Thoughtworks Mingle testanvändare</span><span class="sxs-lookup"><span data-stu-id="52c2a-195">Create a Thoughtworks Mingle test user</span></span>

<span data-ttu-id="52c2a-196">För Azure AD-användare för att kunna logga in, måste de etableras till Thoughtworks Mingle programmet med hjälp av deras användarnamn för Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="52c2a-196">For Azure AD users to be able to sign in, they must be provisioned to the Thoughtworks Mingle application using their Azure Active Directory user names.</span></span> <span data-ttu-id="52c2a-197">När det gäller Thoughtworks Mingle är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="52c2a-197">In the case of Thoughtworks Mingle, provisioning is a manual task.</span></span>

<span data-ttu-id="52c2a-198">**Utför följande steg för att konfigurera användaretablering:**</span><span class="sxs-lookup"><span data-stu-id="52c2a-198">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="52c2a-199">Logga in på webbplatsen Thoughtworks Mingle företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="52c2a-199">Log in to your Thoughtworks Mingle company site as administrator.</span></span>

2. <span data-ttu-id="52c2a-200">Klicka på **profil**.</span><span class="sxs-lookup"><span data-stu-id="52c2a-200">Click **Profile**.</span></span>
   
    <span data-ttu-id="52c2a-201">![Ditt första projekt](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785160.png "ditt första projekt")</span><span class="sxs-lookup"><span data-stu-id="52c2a-201">![Your First Project](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785160.png "Your First Project")</span></span>

3. <span data-ttu-id="52c2a-202">Klicka på den **Admin** fliken och klicka sedan på **användare**.</span><span class="sxs-lookup"><span data-stu-id="52c2a-202">Click the **Admin** tab, and then click **Users**.</span></span>
   
    <span data-ttu-id="52c2a-203">![Användare](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785161.png "användare")</span><span class="sxs-lookup"><span data-stu-id="52c2a-203">![Users](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785161.png "Users")</span></span>

4. <span data-ttu-id="52c2a-204">Klicka på **ny användare**.</span><span class="sxs-lookup"><span data-stu-id="52c2a-204">Click **New User**.</span></span>
   
    <span data-ttu-id="52c2a-205">![Ny användare](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785162.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="52c2a-205">![New User](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785162.png "New User")</span></span>

5. <span data-ttu-id="52c2a-206">På den **ny användare** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="52c2a-206">On the **New User** dialog page, perform the following steps:</span></span>
   
    <span data-ttu-id="52c2a-207">![Dialogrutan Ny användare](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785163.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="52c2a-207">![New User dialog](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785163.png "New User")</span></span>  
 
    <span data-ttu-id="52c2a-208">a.</span><span class="sxs-lookup"><span data-stu-id="52c2a-208">a.</span></span> <span data-ttu-id="52c2a-209">Typ av **inloggningsnamn**, **visningsnamn**, **Välj lösenord**, **Bekräfta lösenord** av ett giltigt Azure AD-kontot som du vill etablera i relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="52c2a-209">Type the **Sign-in name**, **Display name**, **Choose password**, **Confirm password** of a valid Azure AD account you want to provision into the related textboxes.</span></span> 

    <span data-ttu-id="52c2a-210">b.</span><span class="sxs-lookup"><span data-stu-id="52c2a-210">b.</span></span> <span data-ttu-id="52c2a-211">Som **användartyp**väljer **fullständig användaren**.</span><span class="sxs-lookup"><span data-stu-id="52c2a-211">As **User type**, select **Full user**.</span></span>

    <span data-ttu-id="52c2a-212">c.</span><span class="sxs-lookup"><span data-stu-id="52c2a-212">c.</span></span> <span data-ttu-id="52c2a-213">Klicka på **skapa den här profilen**.</span><span class="sxs-lookup"><span data-stu-id="52c2a-213">Click **Create This Profile**.</span></span>

>[!NOTE]
><span data-ttu-id="52c2a-214">Du kan använda något annat Thoughtworks Mingle användarens konto skapas verktyg eller API: er som tillhandahålls av Thoughtworks Mingle att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="52c2a-214">You can use any other Thoughtworks Mingle user account creation tools or APIs provided by Thoughtworks Mingle to provision AAD user accounts.</span></span>
> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="52c2a-215">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="52c2a-215">Assign the Azure AD test user</span></span>

<span data-ttu-id="52c2a-216">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Thoughtworks Mingle.</span><span class="sxs-lookup"><span data-stu-id="52c2a-216">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Thoughtworks Mingle.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="52c2a-218">**Om du vill tilldela Britta Simon Thoughtworks Mingle, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="52c2a-218">**To assign Britta Simon to Thoughtworks Mingle, perform the following steps:**</span></span>

1. <span data-ttu-id="52c2a-219">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="52c2a-219">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="52c2a-221">Välj i listan med program **Thoughtworks Mingle**.</span><span class="sxs-lookup"><span data-stu-id="52c2a-221">In the applications list, select **Thoughtworks Mingle**.</span></span>

    ![Länken Thoughtworks Mingle i listan med program](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_app.png) 

3. <span data-ttu-id="52c2a-223">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="52c2a-223">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202] 

4. <span data-ttu-id="52c2a-225">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="52c2a-225">Click **Add** button.</span></span> <span data-ttu-id="52c2a-226">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="52c2a-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="52c2a-228">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="52c2a-228">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="52c2a-229">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="52c2a-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="52c2a-230">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="52c2a-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="52c2a-231">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="52c2a-231">Test single sign-on</span></span>

<span data-ttu-id="52c2a-232">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="52c2a-232">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="52c2a-233">När du klickar på panelen Thoughtworks Mingle på åtkomstpanelen du bör få automatiskt loggat in på ditt Thoughtworks Mingle program.</span><span class="sxs-lookup"><span data-stu-id="52c2a-233">When you click the Thoughtworks Mingle tile in the Access Panel, you should get automatically signed-on to your Thoughtworks Mingle application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="52c2a-234">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="52c2a-234">Additional resources</span></span>

* [<span data-ttu-id="52c2a-235">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="52c2a-235">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="52c2a-236">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="52c2a-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_203.png

