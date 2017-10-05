---
title: "Självstudier: Azure Active Directory-integrering med Origami | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Origami."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a28bb0ba-b564-46ba-accc-e587699295d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 3420409b72ff032e64ac59365083dd141dfc3c1b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-origami"></a><span data-ttu-id="2d0ae-103">Självstudier: Azure Active Directory-integrering med Origami</span><span class="sxs-lookup"><span data-stu-id="2d0ae-103">Tutorial: Azure Active Directory integration with Origami</span></span>

<span data-ttu-id="2d0ae-104">I kursen får lära du att integrera Origami med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="2d0ae-104">In this tutorial, you learn how to integrate Origami with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2d0ae-105">Integrera Origami med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="2d0ae-105">Integrating Origami with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2d0ae-106">Du kan styra i Azure AD som har åtkomst till Origami</span><span class="sxs-lookup"><span data-stu-id="2d0ae-106">You can control in Azure AD who has access to Origami</span></span>
- <span data-ttu-id="2d0ae-107">Du kan aktivera användarna att automatiskt hämta loggat in på Origami (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="2d0ae-107">You can enable your users to automatically get signed-on to Origami (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2d0ae-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2d0ae-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="2d0ae-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2d0ae-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2d0ae-110">Krav</span><span class="sxs-lookup"><span data-stu-id="2d0ae-110">Prerequisites</span></span>

<span data-ttu-id="2d0ae-111">Om du vill konfigurera Azure AD-integrering med Origami behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="2d0ae-111">To configure Azure AD integration with Origami, you need the following items:</span></span>

- <span data-ttu-id="2d0ae-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="2d0ae-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2d0ae-113">En Origami enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="2d0ae-113">An Origami single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2d0ae-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2d0ae-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="2d0ae-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2d0ae-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2d0ae-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2d0ae-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2d0ae-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="2d0ae-118">Scenario description</span></span>
<span data-ttu-id="2d0ae-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2d0ae-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="2d0ae-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2d0ae-121">Att lägga till Origami från galleriet</span><span class="sxs-lookup"><span data-stu-id="2d0ae-121">Adding Origami from the gallery</span></span>
2. <span data-ttu-id="2d0ae-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2d0ae-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-origami-from-the-gallery"></a><span data-ttu-id="2d0ae-123">Att lägga till Origami från galleriet</span><span class="sxs-lookup"><span data-stu-id="2d0ae-123">Adding Origami from the gallery</span></span>
<span data-ttu-id="2d0ae-124">Du måste lägga till Origami från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Origami i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-124">To configure the integration of Origami into Azure AD, you need to add Origami from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2d0ae-125">**Utför följande steg för att lägga till Origami från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="2d0ae-125">**To add Origami from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2d0ae-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2d0ae-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="2d0ae-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="2d0ae-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="2d0ae-133">I sökrutan skriver **Origami**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-133">In the search box, type **Origami**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-origami-tutorial/tutorial_origami_search.png)

5. <span data-ttu-id="2d0ae-135">Välj i resultatpanelen **Origami**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-135">In the results panel, select **Origami**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-origami-tutorial/tutorial_origami_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2d0ae-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2d0ae-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2d0ae-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Origami baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-138">In this section, you configure and test Azure AD single sign-on with Origami based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2d0ae-139">Azure AD måste du känna till användaren i Origami motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Origami is to a user in Azure AD.</span></span> <span data-ttu-id="2d0ae-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Origami upprättas.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-140">In other words, a link relationship between an Azure AD user and the related user in Origami needs to be established.</span></span>

<span data-ttu-id="2d0ae-141">I Origami, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-141">In Origami, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="2d0ae-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Origami, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="2d0ae-142">To configure and test Azure AD single sign-on with Origami, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2d0ae-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2d0ae-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2d0ae-145">**[Skapa en testanvändare Origami](#creating-an-origami-test-user)**  – har en motsvarighet för Britta Simon Origami som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-145">**[Creating an Origami test user](#creating-an-origami-test-user)** - to have a counterpart of Britta Simon in Origami that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="2d0ae-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2d0ae-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2d0ae-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2d0ae-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2d0ae-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Origami.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Origami application.</span></span>

<span data-ttu-id="2d0ae-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Origami:**</span><span class="sxs-lookup"><span data-stu-id="2d0ae-150">**To configure Azure AD single sign-on with Origami, perform the following steps:**</span></span>

1. <span data-ttu-id="2d0ae-151">I Azure-portalen på den **Origami** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-151">In the Azure portal, on the **Origami** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="2d0ae-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-origami-tutorial/tutorial_origami_samlbase.png)

3. <span data-ttu-id="2d0ae-155">På den **Origami domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2d0ae-155">On the **Origami Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-origami-tutorial/tutorial_origami_url.png)

    <span data-ttu-id="2d0ae-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://live.origamirisk.com/origami/account/login?account=<companyname>`</span><span class="sxs-lookup"><span data-stu-id="2d0ae-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://live.origamirisk.com/origami/account/login?account=<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2d0ae-158">Värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-158">The value is not real.</span></span> <span data-ttu-id="2d0ae-159">Uppdatera värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="2d0ae-160">Kontakta [Origami klienten supportteamet](https://wordpress.org/support/theme/origami) värdet hämtas.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-160">Contact [Origami Client support team](https://wordpress.org/support/theme/origami) to get the value.</span></span> 
 
4. <span data-ttu-id="2d0ae-161">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-origami-tutorial/tutorial_origami_certificate.png) 

5. <span data-ttu-id="2d0ae-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-origami-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2d0ae-165">På den **Origami Configuration** klickar du på **konfigurera Origami** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-165">On the **Origami Configuration** section, click **Configure Origami** to open **Configure sign-on** window.</span></span> <span data-ttu-id="2d0ae-166">Kopiera den **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="2d0ae-166">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-origami-tutorial/tutorial_origami_configure.png) 

7. <span data-ttu-id="2d0ae-168">Logga in på Origami-konto med administratörsrättigheter.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-168">Log in to the Origami account with Admin rights.</span></span>

8. <span data-ttu-id="2d0ae-169">Klicka på menyn högst upp **Admin**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-169">In the menu on the top, click **Admin**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

9. <span data-ttu-id="2d0ae-171">Utför följande steg på enkel inloggning på sidan Inställningar av dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="2d0ae-171">On the Single Sign On Setup dialog page, perform the following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-origami-tutorial/tutorial_origami_531.png)

    <span data-ttu-id="2d0ae-173">a.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-173">a.</span></span> <span data-ttu-id="2d0ae-174">Välj **aktivera enkel inloggning på**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-174">Select **Enable Single Sign On**.</span></span>

    <span data-ttu-id="2d0ae-175">b.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-175">b.</span></span> <span data-ttu-id="2d0ae-176">I den **identitetsleverantören logga in Sidadress** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-176">In the **Identity Provider's Sign-in Page URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="2d0ae-177">c.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-177">c.</span></span> <span data-ttu-id="2d0ae-178">I den **identitetsleverantörens Sign-out Sidadress** textruta klistra in värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-178">In the **Identity Provider's Sign-out Page URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="2d0ae-179">d.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-179">d.</span></span> <span data-ttu-id="2d0ae-180">Klicka på **Bläddra** att ladda upp det certifikat som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-180">Click **Browse** to upload the certificate you have downloaded from the Azure portal.</span></span>

    <span data-ttu-id="2d0ae-181">e.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-181">e.</span></span> <span data-ttu-id="2d0ae-182">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-182">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="2d0ae-183">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="2d0ae-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="2d0ae-184">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="2d0ae-185">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2d0ae-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2d0ae-186">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="2d0ae-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="2d0ae-187">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="2d0ae-189">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="2d0ae-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2d0ae-190">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-origami-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2d0ae-192">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-origami-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2d0ae-194">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-origami-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2d0ae-196">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2d0ae-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-origami-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2d0ae-198">a.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-198">a.</span></span> <span data-ttu-id="2d0ae-199">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2d0ae-200">b.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-200">b.</span></span> <span data-ttu-id="2d0ae-201">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2d0ae-202">c.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-202">c.</span></span> <span data-ttu-id="2d0ae-203">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="2d0ae-204">d.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-204">d.</span></span> <span data-ttu-id="2d0ae-205">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-205">Click **Create**.</span></span>
 
### <a name="creating-an-origami-test-user"></a><span data-ttu-id="2d0ae-206">Skapa en testanvändare Origami</span><span class="sxs-lookup"><span data-stu-id="2d0ae-206">Creating an Origami test user</span></span>

<span data-ttu-id="2d0ae-207">I det här avsnittet skapar du en användare som kallas Britta Simon i Origami.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-207">In this section, you create a user called Britta Simon in Origami.</span></span> 

1. <span data-ttu-id="2d0ae-208">Logga in på Origami-konto med administratörsrättigheter.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-208">Log in to the Origami account with Admin rights.</span></span>

2. <span data-ttu-id="2d0ae-209">Klicka på menyn högst upp **Admin**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-209">In the menu on the top, click **Admin**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-origami-tutorial/tutorial_origami_51.png)

3. <span data-ttu-id="2d0ae-211">På den **användare och säkerhet** dialogrutan klickar du på **användare**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-211">On the **Users and Security** dialog, click **Users**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-origami-tutorial/tutorial_origami_54.png)

4. <span data-ttu-id="2d0ae-213">Klicka på **lägga till nya användare**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-213">Click **Add New User**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-origami-tutorial/tutorial_origami_55.png)

5. <span data-ttu-id="2d0ae-215">I dialogrutan Lägg till ny användare utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="2d0ae-215">On the Add New User dialog, perform the following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-origami-tutorial/tutorial_origami_56.png)

    <span data-ttu-id="2d0ae-217">a.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-217">a.</span></span> <span data-ttu-id="2d0ae-218">I den **användarnamn** textruta ange e-postadress för användaren som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="2d0ae-218">In the **User Name** textbox, enter the email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="2d0ae-219">b.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-219">b.</span></span> <span data-ttu-id="2d0ae-220">I den **lösenord** textruta, ange ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-220">In the **Password** textbox, type a password.</span></span>

    <span data-ttu-id="2d0ae-221">c.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-221">c.</span></span> <span data-ttu-id="2d0ae-222">I den **Bekräfta lösenord** textruta, Skriv in lösenordet igen.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-222">In the **Confirm Password** textbox, type the password again.</span></span>

    <span data-ttu-id="2d0ae-223">d.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-223">d.</span></span> <span data-ttu-id="2d0ae-224">I den **Förnamn** textruta Ange först namnet på användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-224">In the **First Name** textbox, enter the first name of user like **Britta**.</span></span>

    <span data-ttu-id="2d0ae-225">e.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-225">e.</span></span> <span data-ttu-id="2d0ae-226">I den **efternamn** textruta Ange efternamn för användaren som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-226">In the **Last Name** textbox, enter the last name of user like **Simon**.</span></span>

    <span data-ttu-id="2d0ae-227">f.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-227">f.</span></span> <span data-ttu-id="2d0ae-228">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-228">Click **Save**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-origami-tutorial/tutorial_origami_57.png)

6. <span data-ttu-id="2d0ae-230">Tilldela **användarroller** och **klientåtkomst** för användaren.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-230">Assign **User Roles** and **Client Access** to the user.</span></span> 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-origami-tutorial/tutorial_origami_58.png)

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="2d0ae-232">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="2d0ae-232">Assigning the Azure AD test user</span></span>

<span data-ttu-id="2d0ae-233">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Origami.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Origami.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="2d0ae-235">**Om du vill tilldela Origami Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2d0ae-235">**To assign Britta Simon to Origami, perform the following steps:**</span></span>

1. <span data-ttu-id="2d0ae-236">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="2d0ae-238">Välj i listan med program **Origami**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-238">In the applications list, select **Origami**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-origami-tutorial/tutorial_origami_app.png) 

3. <span data-ttu-id="2d0ae-240">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="2d0ae-242">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-242">Click **Add** button.</span></span> <span data-ttu-id="2d0ae-243">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="2d0ae-245">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="2d0ae-246">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2d0ae-247">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2d0ae-248">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2d0ae-248">Testing single sign-on</span></span>

<span data-ttu-id="2d0ae-249">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-249">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="2d0ae-250">När du klickar på panelen Origami på åtkomstpanelen du bör få automatiskt loggat in på ditt Origami program.</span><span class="sxs-lookup"><span data-stu-id="2d0ae-250">When you click the Origami tile in the Access Panel, you should get automatically signed-on to your Origami application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2d0ae-251">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2d0ae-251">Additional resources</span></span>

* [<span data-ttu-id="2d0ae-252">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2d0ae-252">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2d0ae-253">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2d0ae-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-origami-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-origami-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-origami-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-origami-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-origami-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-origami-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-origami-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-origami-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-origami-tutorial/tutorial_general_203.png

