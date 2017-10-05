---
title: "Självstudier: Azure Active Directory-integrering med Kindling | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Kindling."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 71229751-74eb-4c2c-abb4-07caa95754c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 131c2c3f46c60193d512b1779e917c8322732fbc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kindling"></a><span data-ttu-id="f7d94-103">Självstudier: Azure Active Directory-integrering med Kindling</span><span class="sxs-lookup"><span data-stu-id="f7d94-103">Tutorial: Azure Active Directory integration with Kindling</span></span>

<span data-ttu-id="f7d94-104">I kursen får lära du att integrera Kindling med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="f7d94-104">In this tutorial, you learn how to integrate Kindling with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f7d94-105">Integrera Kindling med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f7d94-105">Integrating Kindling with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f7d94-106">Du kan styra i Azure AD som har åtkomst till Kindling</span><span class="sxs-lookup"><span data-stu-id="f7d94-106">You can control in Azure AD who has access to Kindling</span></span>
- <span data-ttu-id="f7d94-107">Du kan ge användarna automatiskt får loggat in på Kindling (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="f7d94-107">You can enable your users to automatically get signed-on to Kindling (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f7d94-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f7d94-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f7d94-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns i.</span><span class="sxs-lookup"><span data-stu-id="f7d94-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="f7d94-110">[Vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f7d94-110">[what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f7d94-111">Krav</span><span class="sxs-lookup"><span data-stu-id="f7d94-111">Prerequisites</span></span>

<span data-ttu-id="f7d94-112">För att konfigurera Azure AD-integrering med Kindling, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="f7d94-112">To configure Azure AD integration with Kindling, you need the following items:</span></span>

- <span data-ttu-id="f7d94-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f7d94-113">An Azure AD subscription</span></span>
- <span data-ttu-id="f7d94-114">En Kindling enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="f7d94-114">A Kindling single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f7d94-115">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f7d94-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f7d94-116">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f7d94-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f7d94-117">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f7d94-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f7d94-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f7d94-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f7d94-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="f7d94-119">Scenario description</span></span>
<span data-ttu-id="f7d94-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="f7d94-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f7d94-121">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="f7d94-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f7d94-122">Att lägga till Kindling från galleriet</span><span class="sxs-lookup"><span data-stu-id="f7d94-122">Adding Kindling from the gallery</span></span>
2. <span data-ttu-id="f7d94-123">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f7d94-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kindling-from-the-gallery"></a><span data-ttu-id="f7d94-124">Att lägga till Kindling från galleriet</span><span class="sxs-lookup"><span data-stu-id="f7d94-124">Adding Kindling from the gallery</span></span>
<span data-ttu-id="f7d94-125">Du måste lägga till Kindling från galleriet i listan över hanterade SaaS-appar för att konfigurera Kindling till Azure AD-integrering.</span><span class="sxs-lookup"><span data-stu-id="f7d94-125">To configure the integration of Kindling into Azure AD, you need to add Kindling from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f7d94-126">**Utför följande steg för att lägga till Kindling från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="f7d94-126">**To add Kindling from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f7d94-127">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f7d94-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f7d94-129">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f7d94-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f7d94-130">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f7d94-130">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="f7d94-132">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f7d94-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="f7d94-134">I sökrutan skriver **Kindling**.</span><span class="sxs-lookup"><span data-stu-id="f7d94-134">In the search box, type **Kindling**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_search.png)

5. <span data-ttu-id="f7d94-136">Välj i resultatpanelen **Kindling**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="f7d94-136">In the results panel, select **Kindling**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f7d94-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f7d94-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f7d94-139">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Kindling baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="f7d94-139">In this section, you configure and test Azure AD single sign-on with Kindling based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f7d94-140">Azure AD måste du känna till användaren i Kindling motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="f7d94-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Kindling is to a user in Azure AD.</span></span> <span data-ttu-id="f7d94-141">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Kindling upprättas.</span><span class="sxs-lookup"><span data-stu-id="f7d94-141">In other words, a link relationship between an Azure AD user and the related user in Kindling needs to be established.</span></span>

<span data-ttu-id="f7d94-142">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Kindling.</span><span class="sxs-lookup"><span data-stu-id="f7d94-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Kindling.</span></span>

<span data-ttu-id="f7d94-143">Om du vill konfigurera och testa Azure AD enkel inloggning med Kindling, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="f7d94-143">To configure and test Azure AD single sign-on with Kindling, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f7d94-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f7d94-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f7d94-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f7d94-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f7d94-146">**[Skapa en testanvändare Kindling](#creating-a-kindling-test-user)**  – du har en motsvarighet för Britta Simon i Kindling som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="f7d94-146">**[Creating a Kindling test user](#creating-a-kindling-test-user)** - to have a counterpart of Britta Simon in Kindling that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f7d94-147">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f7d94-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f7d94-148">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f7d94-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f7d94-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f7d94-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f7d94-150">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Kindling program.</span><span class="sxs-lookup"><span data-stu-id="f7d94-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kindling application.</span></span>

<span data-ttu-id="f7d94-151">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Kindling:**</span><span class="sxs-lookup"><span data-stu-id="f7d94-151">**To configure Azure AD single sign-on with Kindling, perform the following steps:**</span></span>

1. <span data-ttu-id="f7d94-152">I Azure-portalen på den **Kindling** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f7d94-152">In the Azure portal, on the **Kindling** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="f7d94-154">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f7d94-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_samlbase.png)

3. <span data-ttu-id="f7d94-156">På den **Kindling domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f7d94-156">On the **Kindling Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_url.png)

    <span data-ttu-id="f7d94-158">a.</span><span class="sxs-lookup"><span data-stu-id="f7d94-158">a.</span></span> <span data-ttu-id="f7d94-159">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.kindlingapp.com`</span><span class="sxs-lookup"><span data-stu-id="f7d94-159">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.kindlingapp.com`</span></span>

    <span data-ttu-id="f7d94-160">b.</span><span class="sxs-lookup"><span data-stu-id="f7d94-160">b.</span></span>  <span data-ttu-id="f7d94-161">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.kindlingapp.com/saml/module.php/saml/sp/metadata.php/clientIDP`</span><span class="sxs-lookup"><span data-stu-id="f7d94-161">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.kindlingapp.com/saml/module.php/saml/sp/metadata.php/clientIDP`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f7d94-162">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="f7d94-162">These values are not the real.</span></span> <span data-ttu-id="f7d94-163">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="f7d94-163">Update these values with the actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="f7d94-164">Kontakta [Kindling supportteamet](mailto:support@kindlingapp.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="f7d94-164">Contact [Kindling support team](mailto:support@kindlingapp.com) to get these values.</span></span>
 
4. <span data-ttu-id="f7d94-165">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="f7d94-165">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_certificate.png) 

5. <span data-ttu-id="f7d94-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f7d94-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kindling-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f7d94-169">På den **Kindling Configuration** klickar du på **konfigurera Kindling** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="f7d94-169">On the **Kindling Configuration** section, click **Configure Kindling** to open **Configure sign-on** window.</span></span> <span data-ttu-id="f7d94-170">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="f7d94-170">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_configure.png) 

7. <span data-ttu-id="f7d94-172">Konfigurera enkel inloggning på **Kindling** sida, måste du skicka den hämtade **certifikat (Base64)**, **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel**till [Kindling supportteamet](mailto:support@kindlingapp.com).</span><span class="sxs-lookup"><span data-stu-id="f7d94-172">To configure single sign-on on **Kindling** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Kindling support team](mailto:support@kindlingapp.com).</span></span>

> [!TIP]
> <span data-ttu-id="f7d94-173">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="f7d94-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f7d94-174">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="f7d94-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f7d94-175">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f7d94-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f7d94-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f7d94-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="f7d94-177">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f7d94-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="f7d94-179">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="f7d94-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f7d94-180">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f7d94-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kindling-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f7d94-182">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f7d94-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kindling-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f7d94-184">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f7d94-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kindling-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f7d94-186">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f7d94-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kindling-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f7d94-188">a.</span><span class="sxs-lookup"><span data-stu-id="f7d94-188">a.</span></span> <span data-ttu-id="f7d94-189">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f7d94-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f7d94-190">b.</span><span class="sxs-lookup"><span data-stu-id="f7d94-190">b.</span></span> <span data-ttu-id="f7d94-191">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f7d94-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f7d94-192">c.</span><span class="sxs-lookup"><span data-stu-id="f7d94-192">c.</span></span> <span data-ttu-id="f7d94-193">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="f7d94-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f7d94-194">d.</span><span class="sxs-lookup"><span data-stu-id="f7d94-194">d.</span></span> <span data-ttu-id="f7d94-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f7d94-195">Click **Create**.</span></span>
 
### <a name="creating-a-kindling-test-user"></a><span data-ttu-id="f7d94-196">Skapa en testanvändare Kindling</span><span class="sxs-lookup"><span data-stu-id="f7d94-196">Creating a Kindling test user</span></span>

<span data-ttu-id="f7d94-197">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Kindling.</span><span class="sxs-lookup"><span data-stu-id="f7d94-197">The objective of this section is to create a user called Britta Simon in Kindling.</span></span> <span data-ttu-id="f7d94-198">Kindling stöder just-in-time-etablering.</span><span class="sxs-lookup"><span data-stu-id="f7d94-198">Kindling supports just-in-time provisioning.</span></span> <span data-ttu-id="f7d94-199">Du har redan aktiverats i [konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="f7d94-199">You have already enabled it in [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

<span data-ttu-id="f7d94-200">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="f7d94-200">There is no action item for you in this section.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f7d94-201">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="f7d94-201">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f7d94-202">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Kindling.</span><span class="sxs-lookup"><span data-stu-id="f7d94-202">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kindling.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="f7d94-204">**Om du vill tilldela Kindling Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f7d94-204">**To assign Britta Simon to Kindling, perform the following steps:**</span></span>

1. <span data-ttu-id="f7d94-205">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f7d94-205">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="f7d94-207">Välj i listan med program **Kindling**.</span><span class="sxs-lookup"><span data-stu-id="f7d94-207">In the applications list, select **Kindling**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_app.png) 

3. <span data-ttu-id="f7d94-209">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f7d94-209">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="f7d94-211">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="f7d94-211">Click **Add** button.</span></span> <span data-ttu-id="f7d94-212">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f7d94-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="f7d94-214">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="f7d94-214">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f7d94-215">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f7d94-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f7d94-216">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f7d94-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f7d94-217">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f7d94-217">Testing single sign-on</span></span>

<span data-ttu-id="f7d94-218">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f7d94-218">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f7d94-219">När du klickar på panelen Kindling på åtkomstpanelen du bör få automatiskt loggat in på ditt Kindling program.</span><span class="sxs-lookup"><span data-stu-id="f7d94-219">When you click the Kindling tile in the Access Panel, you should get automatically signed-on to your Kindling application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f7d94-220">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f7d94-220">Additional resources</span></span>

* [<span data-ttu-id="f7d94-221">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f7d94-221">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f7d94-222">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f7d94-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_203.png

