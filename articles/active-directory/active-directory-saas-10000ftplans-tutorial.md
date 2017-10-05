---
title: "Självstudier: Azure Active Directory-integrering med 10 000 ft planer | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och 10 000 ft planer."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b60c955e-8fa3-4872-a897-c4e81fd7beac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: c81a6adb3abad58af149bbef6f5dff736c142f51
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-10000ft-plans"></a><span data-ttu-id="ea256-103">Självstudier: Azure Active Directory-integrering med 10 000 ft planer</span><span class="sxs-lookup"><span data-stu-id="ea256-103">Tutorial: Azure Active Directory integration with 10,000ft Plans</span></span>

<span data-ttu-id="ea256-104">I kursen får lära du att integrera 10 000 ft planer med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ea256-104">In this tutorial, you learn how to integrate 10,000ft Plans with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ea256-105">10 000 ft planer integrera med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ea256-105">Integrating 10,000ft Plans with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ea256-106">Du kan styra i Azure AD som har åtkomst till 10 000 ft planer</span><span class="sxs-lookup"><span data-stu-id="ea256-106">You can control in Azure AD who has access to 10,000ft Plans</span></span>
- <span data-ttu-id="ea256-107">Du kan aktivera användarna att automatiskt hämta loggat in på 10 000 ft planer (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ea256-107">You can enable your users to automatically get signed-on to 10,000ft Plans (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ea256-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ea256-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ea256-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ea256-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ea256-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ea256-110">Prerequisites</span></span>

<span data-ttu-id="ea256-111">För att konfigurera Azure AD-integrering med 10 000 ft planer, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="ea256-111">To configure Azure AD integration with 10,000ft Plans, you need the following items:</span></span>

- <span data-ttu-id="ea256-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ea256-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ea256-113">En 10 000 ft planer för enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="ea256-113">A 10,000ft Plans single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ea256-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ea256-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ea256-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ea256-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ea256-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ea256-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ea256-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här [utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ea256-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ea256-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ea256-118">Scenario description</span></span>
<span data-ttu-id="ea256-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ea256-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ea256-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ea256-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ea256-121">Lägger till 10 000 ft planer från galleriet</span><span class="sxs-lookup"><span data-stu-id="ea256-121">Adding 10,000ft Plans from the gallery</span></span>
2. <span data-ttu-id="ea256-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ea256-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-10000ft-plans-from-the-gallery"></a><span data-ttu-id="ea256-123">Lägger till 10 000 ft planer från galleriet</span><span class="sxs-lookup"><span data-stu-id="ea256-123">Adding 10,000ft Plans from the gallery</span></span>
<span data-ttu-id="ea256-124">Du måste lägga till 10 000 ft planer från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av 10 000 ft planer i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ea256-124">To configure the integration of 10,000ft Plans into Azure AD, you need to add 10,000ft Plans from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ea256-125">**Utför följande steg för att lägga till 10 000 ft planer från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="ea256-125">**To add 10,000ft Plans from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ea256-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ea256-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ea256-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ea256-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ea256-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ea256-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="ea256-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ea256-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="ea256-133">I sökrutan skriver **10 000 ft planer**.</span><span class="sxs-lookup"><span data-stu-id="ea256-133">In the search box, type **10,000ft Plans**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_search.png)

5. <span data-ttu-id="ea256-135">Välj i resultatpanelen **10 000 ft planer**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="ea256-135">In the results panel, select **10,000ft Plans**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ea256-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ea256-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ea256-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med 10 000 ft planer baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ea256-138">In this section, you configure and test Azure AD single sign-on with 10,000ft Plans based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ea256-139">Azure AD måste du känna till användaren i 10 000 ft planer motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="ea256-139">For single sign-on to work, Azure AD needs to know what the counterpart user in 10,000ft Plans is to a user in Azure AD.</span></span> <span data-ttu-id="ea256-140">Med andra ord en länk förhållandet mellan en Azure AD-användare och relaterade användaren i 10 000 ft planer måste upprättas.</span><span class="sxs-lookup"><span data-stu-id="ea256-140">In other words, a link relationship between an Azure AD user and the related user in 10,000ft Plans needs to be established.</span></span>

<span data-ttu-id="ea256-141">I 10 000 ft planer, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="ea256-141">In 10,000ft Plans, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ea256-142">Om du vill konfigurera och testa Azure AD enkel inloggning med 10 000 ft planer, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ea256-142">To configure and test Azure AD single sign-on with 10,000ft Plans, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ea256-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ea256-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ea256-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ea256-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ea256-145">**[Skapa en 10 000 ft planer testanvändare](#creating-a-10000ft-plans-test-user)**  – du har en motsvarighet för Britta Simon i 10 000 ft planer som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ea256-145">**[Creating a 10,000ft Plans test user](#creating-a-10000ft-plans-test-user)** - to have a counterpart of Britta Simon in 10,000ft Plans that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ea256-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ea256-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ea256-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ea256-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ea256-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ea256-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ea256-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i 10 000 ft planer för programmet.</span><span class="sxs-lookup"><span data-stu-id="ea256-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your 10,000ft Plans application.</span></span>

<span data-ttu-id="ea256-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med 10 000 ft planer:**</span><span class="sxs-lookup"><span data-stu-id="ea256-150">**To configure Azure AD single sign-on with 10,000ft Plans, perform the following steps:**</span></span>

1. <span data-ttu-id="ea256-151">I Azure-portalen på den **10 000 ft planer** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ea256-151">In the Azure portal, on the **10,000ft Plans** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="ea256-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ea256-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_samlbase.png)

3. <span data-ttu-id="ea256-155">På den **10 000 ft planer domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ea256-155">On the **10,000ft Plans Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_url.png)

    <span data-ttu-id="ea256-157">a.</span><span class="sxs-lookup"><span data-stu-id="ea256-157">a.</span></span> <span data-ttu-id="ea256-158">I den **inloggnings-URL** textruta anger du URL:`https://app.10000ft.com`</span><span class="sxs-lookup"><span data-stu-id="ea256-158">In the **Sign-on URL** textbox, type the URL: `https://app.10000ft.com`</span></span>

    <span data-ttu-id="ea256-159">b.</span><span class="sxs-lookup"><span data-stu-id="ea256-159">b.</span></span> <span data-ttu-id="ea256-160">I den **identifierare** textruta anger du URL:`https://app.10000ft.com/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="ea256-160">In the **Identifier** textbox, type the URL: `https://app.10000ft.com/saml/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ea256-161">Värdet för **identifierare** skiljer sig om du har en anpassad domän.</span><span class="sxs-lookup"><span data-stu-id="ea256-161">The value for **Identifier** is different if you have a custom domain.</span></span> <span data-ttu-id="ea256-162">Kontakta [10 000 ft planer supportteamet](https://www.10000ft.com/plans/support) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="ea256-162">Contact [10,000ft Plans support team](https://www.10000ft.com/plans/support) to get this value.</span></span> 
 
4. <span data-ttu-id="ea256-163">På den **SAML-signeringscertifikat** klickar du på **Certificate(Raw)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="ea256-163">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_certificate.png) 

5. <span data-ttu-id="ea256-165">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ea256-165">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ea256-167">På den **10 000 ft planer Configuration** klickar du på **konfigurera 10 000 ft planer** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="ea256-167">On the **10,000ft Plans Configuration** section, click **Configure 10,000ft Plans** to open **Configure sign-on** window.</span></span> <span data-ttu-id="ea256-168">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="ea256-168">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_configure.png) 

7. <span data-ttu-id="ea256-170">Konfigurera enkel inloggning på **10 000 ft planer** sida, måste du skicka den hämtade **Certificate(Raw), Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** till [10 000 ft planer supportteamet](https://www.10000ft.com/plans/support).</span><span class="sxs-lookup"><span data-stu-id="ea256-170">To configure single sign-on on **10,000ft Plans** side, you need to send the downloaded **Certificate(Raw), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [10,000ft Plans support team](https://www.10000ft.com/plans/support).</span></span>

> [!TIP]
> <span data-ttu-id="ea256-171">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="ea256-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ea256-172">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="ea256-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ea256-173">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ea256-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ea256-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ea256-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="ea256-175">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ea256-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="ea256-177">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="ea256-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ea256-178">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ea256-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ea256-180">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ea256-180">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ea256-182">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ea256-182">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ea256-184">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ea256-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-10000ftplans-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ea256-186">a.</span><span class="sxs-lookup"><span data-stu-id="ea256-186">a.</span></span> <span data-ttu-id="ea256-187">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ea256-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ea256-188">b.</span><span class="sxs-lookup"><span data-stu-id="ea256-188">b.</span></span> <span data-ttu-id="ea256-189">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ea256-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ea256-190">c.</span><span class="sxs-lookup"><span data-stu-id="ea256-190">c.</span></span> <span data-ttu-id="ea256-191">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ea256-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ea256-192">d.</span><span class="sxs-lookup"><span data-stu-id="ea256-192">d.</span></span> <span data-ttu-id="ea256-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ea256-193">Click **Create**.</span></span>
 
### <a name="creating-a-10000ft-plans-test-user"></a><span data-ttu-id="ea256-194">Skapa en 10 000 ft testanvändare planer</span><span class="sxs-lookup"><span data-stu-id="ea256-194">Creating a 10,000ft Plans test user</span></span>

<span data-ttu-id="ea256-195">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i 10 000 ft planer.</span><span class="sxs-lookup"><span data-stu-id="ea256-195">The objective of this section is to create a user called Britta Simon in 10,000ft Plans.</span></span> <span data-ttu-id="ea256-196">10 000 ft planer stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="ea256-196">10,000ft Plans supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="ea256-197">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ea256-197">There is no action item for you in this section.</span></span> <span data-ttu-id="ea256-198">En ny användare skapas under ett försök att få åtkomst till 10 000 ft planer om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="ea256-198">A new user is created during an attempt to access 10,000ft Plans if it doesn't exist yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="ea256-199">Om du behöver skapa en användare manuellt, måste du kontakta den [10 000 ft planer supportteamet](https://www.10000ft.com/plans/support).</span><span class="sxs-lookup"><span data-stu-id="ea256-199">If you need to create a user manually, you need to contact the [10,000ft Plans support team](https://www.10000ft.com/plans/support).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ea256-200">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="ea256-200">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ea256-201">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till 10 000 ft planer.</span><span class="sxs-lookup"><span data-stu-id="ea256-201">In this section, you enable Britta Simon to use Azure single sign-on by granting access to 10,000ft Plans.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="ea256-203">**Om du vill tilldela 10 000 ft planer Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ea256-203">**To assign Britta Simon to 10,000ft Plans, perform the following steps:**</span></span>

1. <span data-ttu-id="ea256-204">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ea256-204">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ea256-206">Välj i listan med program **10 000 ft planer**.</span><span class="sxs-lookup"><span data-stu-id="ea256-206">In the applications list, select **10,000ft Plans**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-10000ftplans-tutorial/tutorial_10,000ftplans_app.png) 

3. <span data-ttu-id="ea256-208">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ea256-208">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="ea256-210">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ea256-210">Click **Add** button.</span></span> <span data-ttu-id="ea256-211">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ea256-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="ea256-213">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="ea256-213">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ea256-214">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ea256-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ea256-215">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ea256-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ea256-216">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ea256-216">Testing single sign-on</span></span>

<span data-ttu-id="ea256-217">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="ea256-217">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="ea256-218">När du klickar på panelen 10 000 ft planer på åtkomstpanelen du bör få automatiskt loggat in på 10 000 ft planer för programmet.</span><span class="sxs-lookup"><span data-stu-id="ea256-218">When you click the 10,000ft Plans tile in the Access Panel, you should get automatically signed-on to your 10,000ft Plans application.</span></span>
 
## <a name="additional-resources"></a><span data-ttu-id="ea256-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ea256-219">Additional resources</span></span>

* [<span data-ttu-id="ea256-220">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ea256-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ea256-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ea256-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-10000ftplans-tutorial/tutorial_general_203.png

