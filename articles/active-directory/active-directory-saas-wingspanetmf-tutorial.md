---
title: "Självstudier: Azure Active Directory-integrering med Wingspan eTMF | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Wingspan eTMF."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ace320d3-521c-449c-992f-feabe7538de7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: jeedes
ms.openlocfilehash: 8c76fb64229abcad0cabb910e7c170979a79d839
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wingspan-etmf"></a><span data-ttu-id="e742e-103">Självstudier: Azure Active Directory-integrering med Wingspan eTMF</span><span class="sxs-lookup"><span data-stu-id="e742e-103">Tutorial: Azure Active Directory integration with Wingspan eTMF</span></span>

<span data-ttu-id="e742e-104">I kursen får lära du att integrera Wingspan eTMF med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e742e-104">In this tutorial, you learn how to integrate Wingspan eTMF with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e742e-105">Integrera Wingspan eTMF med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e742e-105">Integrating Wingspan eTMF with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e742e-106">Du kan styra i Azure AD som har åtkomst till Wingspan eTMF</span><span class="sxs-lookup"><span data-stu-id="e742e-106">You can control in Azure AD who has access to Wingspan eTMF</span></span>
- <span data-ttu-id="e742e-107">Du kan aktivera användarna att automatiskt hämta loggat in på Wingspan eTMF (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e742e-107">You can enable your users to automatically get signed-on to Wingspan eTMF (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e742e-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e742e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e742e-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e742e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e742e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e742e-110">Prerequisites</span></span>

<span data-ttu-id="e742e-111">För att konfigurera Azure AD-integrering med Wingspan eTMF, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="e742e-111">To configure Azure AD integration with Wingspan eTMF, you need the following items:</span></span>

- <span data-ttu-id="e742e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e742e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e742e-113">En Wingspan eTMF enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="e742e-113">A Wingspan eTMF single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e742e-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e742e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e742e-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e742e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e742e-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e742e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e742e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e742e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e742e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e742e-118">Scenario description</span></span>
<span data-ttu-id="e742e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e742e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e742e-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e742e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e742e-121">Att lägga till Wingspan eTMF från galleriet</span><span class="sxs-lookup"><span data-stu-id="e742e-121">Adding Wingspan eTMF from the gallery</span></span>
2. <span data-ttu-id="e742e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e742e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-wingspan-etmf-from-the-gallery"></a><span data-ttu-id="e742e-123">Att lägga till Wingspan eTMF från galleriet</span><span class="sxs-lookup"><span data-stu-id="e742e-123">Adding Wingspan eTMF from the gallery</span></span>
<span data-ttu-id="e742e-124">Du måste lägga till Wingspan eTMF från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Wingspan eTMF i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e742e-124">To configure the integration of Wingspan eTMF into Azure AD, you need to add Wingspan eTMF from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e742e-125">**Utför följande steg för att lägga till Wingspan eTMF från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="e742e-125">**To add Wingspan eTMF from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e742e-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e742e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e742e-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e742e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e742e-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e742e-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="e742e-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e742e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="e742e-133">I sökrutan skriver **Wingspan eTMF**.</span><span class="sxs-lookup"><span data-stu-id="e742e-133">In the search box, type **Wingspan eTMF**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_search.png)

5. <span data-ttu-id="e742e-135">Välj i resultatpanelen **Wingspan eTMF**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="e742e-135">In the results panel, select **Wingspan eTMF**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e742e-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e742e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e742e-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Wingspan eTMF baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e742e-138">In this section, you configure and test Azure AD single sign-on with Wingspan eTMF based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e742e-139">Azure AD måste du känna till användaren i Wingspan eTMF motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="e742e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Wingspan eTMF is to a user in Azure AD.</span></span> <span data-ttu-id="e742e-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Wingspan eTMF upprättas.</span><span class="sxs-lookup"><span data-stu-id="e742e-140">In other words, a link relationship between an Azure AD user and the related user in Wingspan eTMF needs to be established.</span></span>

<span data-ttu-id="e742e-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Wingspan eTMF.</span><span class="sxs-lookup"><span data-stu-id="e742e-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Wingspan eTMF.</span></span>

<span data-ttu-id="e742e-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Wingspan eTMF, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e742e-142">To configure and test Azure AD single sign-on with Wingspan eTMF, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e742e-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e742e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e742e-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e742e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e742e-145">**[Skapa en testanvändare för Wingspan eTMF](#creating-a-wingspan-etmf-test-user)**  – du har en motsvarighet för Britta Simon i Wingspan eTMF som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="e742e-145">**[Creating a Wingspan eTMF test user](#creating-a-wingspan-etmf-test-user)** - to have a counterpart of Britta Simon in Wingspan eTMF that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e742e-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e742e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e742e-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e742e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e742e-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e742e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e742e-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Wingspan eTMF program.</span><span class="sxs-lookup"><span data-stu-id="e742e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Wingspan eTMF application.</span></span>

<span data-ttu-id="e742e-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Wingspan eTMF:**</span><span class="sxs-lookup"><span data-stu-id="e742e-150">**To configure Azure AD single sign-on with Wingspan eTMF, perform the following steps:**</span></span>

1. <span data-ttu-id="e742e-151">I Azure-portalen på den **Wingspan eTMF** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e742e-151">In the Azure portal, on the **Wingspan eTMF** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="e742e-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e742e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_samlbase.png)

3. <span data-ttu-id="e742e-155">På den **Wingspan eTMF domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e742e-155">On the **Wingspan eTMF Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_url11.png)

    <span data-ttu-id="e742e-157">a.</span><span class="sxs-lookup"><span data-stu-id="e742e-157">a.</span></span> <span data-ttu-id="e742e-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<customer name>.<instance name>.mywingspan.com/saml`</span><span class="sxs-lookup"><span data-stu-id="e742e-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<customer name>.<instance name>.mywingspan.com/saml`</span></span>

    <span data-ttu-id="e742e-159">b.</span><span class="sxs-lookup"><span data-stu-id="e742e-159">b.</span></span> <span data-ttu-id="e742e-160">I den **identifierare** textruta Skriv en URL med följande mönster:`http://saml.<instance name>.wingspan.com/shibboleth`</span><span class="sxs-lookup"><span data-stu-id="e742e-160">In the **Identifier** textbox, type a URL using the following pattern: `http://saml.<instance name>.wingspan.com/shibboleth`</span></span>

    <span data-ttu-id="e742e-161">c.</span><span class="sxs-lookup"><span data-stu-id="e742e-161">c.</span></span> <span data-ttu-id="e742e-162">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<customer name>.<instance name>.mywingspan.com/`</span><span class="sxs-lookup"><span data-stu-id="e742e-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://<customer name>.<instance name>.mywingspan.com/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="e742e-163">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="e742e-163">These values are not the real.</span></span> <span data-ttu-id="e742e-164">Uppdatera dessa värden med faktiska inloggnings-URL, identifierare och Reply URL inklusive faktiska kund- och instansnamn.</span><span class="sxs-lookup"><span data-stu-id="e742e-164">Update these values with the actual Sign-On URL, Identifier and Reply URL including the actual customer name and instance name.</span></span> <span data-ttu-id="e742e-165">Kontakta [Wingspan eTMF klienten supportteamet](http://www.wingspan.com/contact-us/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="e742e-165">Contact [Wingspan eTMF Client support team](http://www.wingspan.com/contact-us/) to get these values.</span></span> 

4. <span data-ttu-id="e742e-166">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="e742e-166">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_certificate.png) 

5. <span data-ttu-id="e742e-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e742e-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e742e-170">Konfigurera enkel inloggning på **Wingspan eTMF** sida, måste du skicka den hämtade **XML-Metadata för** till [Wingspan eTMF support](http://www.wingspan.com/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="e742e-170">To configure single sign-on on **Wingspan eTMF** side, you need to send the downloaded **Metadata XML** to [Wingspan eTMF support](http://www.wingspan.com/contact-us/).</span></span> <span data-ttu-id="e742e-171">De ställa in för att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="e742e-171">They set this up to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="e742e-172">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="e742e-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e742e-173">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="e742e-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e742e-174">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e742e-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e742e-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e742e-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="e742e-176">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e742e-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="e742e-178">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="e742e-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e742e-179">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e742e-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e742e-181">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e742e-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e742e-183">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e742e-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e742e-185">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e742e-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wingspanetmf-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e742e-187">a.</span><span class="sxs-lookup"><span data-stu-id="e742e-187">a.</span></span> <span data-ttu-id="e742e-188">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e742e-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e742e-189">b.</span><span class="sxs-lookup"><span data-stu-id="e742e-189">b.</span></span> <span data-ttu-id="e742e-190">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e742e-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e742e-191">c.</span><span class="sxs-lookup"><span data-stu-id="e742e-191">c.</span></span> <span data-ttu-id="e742e-192">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e742e-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e742e-193">d.</span><span class="sxs-lookup"><span data-stu-id="e742e-193">d.</span></span> <span data-ttu-id="e742e-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e742e-194">Click **Create**.</span></span>
 
### <a name="creating-a-wingspan-etmf-test-user"></a><span data-ttu-id="e742e-195">Skapa en Wingspan eTMF testanvändare</span><span class="sxs-lookup"><span data-stu-id="e742e-195">Creating a Wingspan eTMF test user</span></span>

<span data-ttu-id="e742e-196">I det här avsnittet skapar du en användare som kallas Britta Simon i Wingspan eTMF.</span><span class="sxs-lookup"><span data-stu-id="e742e-196">In this section, you create a user called Britta Simon in Wingspan eTMF.</span></span> <span data-ttu-id="e742e-197">Arbeta med [Wingspan eTMF stöd](http://www.wingspan.com/contact-us/) att lägga till användare i Wingspan eTMF program.</span><span class="sxs-lookup"><span data-stu-id="e742e-197">Work with [Wingspan eTMF support](http://www.wingspan.com/contact-us/) to add the users in the Wingspan eTMF application.</span></span> <span data-ttu-id="e742e-198">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e742e-198">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e742e-199">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="e742e-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e742e-200">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Wingspan eTMF.</span><span class="sxs-lookup"><span data-stu-id="e742e-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Wingspan eTMF.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="e742e-202">**Om du vill tilldela Wingspan eTMF Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e742e-202">**To assign Britta Simon to Wingspan eTMF, perform the following steps:**</span></span>

1. <span data-ttu-id="e742e-203">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e742e-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e742e-205">Välj i listan med program **Wingspan eTMF**.</span><span class="sxs-lookup"><span data-stu-id="e742e-205">In the applications list, select **Wingspan eTMF**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wingspanetmf-tutorial/tutorial_wingspanetmf_app.png) 

3. <span data-ttu-id="e742e-207">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e742e-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="e742e-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e742e-209">Click **Add** button.</span></span> <span data-ttu-id="e742e-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e742e-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="e742e-212">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="e742e-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e742e-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e742e-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e742e-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e742e-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e742e-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e742e-215">Testing single sign-on</span></span>

<span data-ttu-id="e742e-216">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e742e-216">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span> 

<span data-ttu-id="e742e-217">Klicka på panelen Wingspan eTMF på åtkomstpanelen, du omdirigeras till organisation inloggning på sidan.</span><span class="sxs-lookup"><span data-stu-id="e742e-217">Click the Wingspan eTMF tile in the Access Panel, you will be redirected to Organization sign on page.</span></span> <span data-ttu-id="e742e-218">Efter genomförd inloggning du kommer att loggat in på ditt Wingspan eTMF program.</span><span class="sxs-lookup"><span data-stu-id="e742e-218">After successful login, you will be signed-on to your Wingspan eTMF application.</span></span> <span data-ttu-id="e742e-219">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="e742e-219">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e742e-220">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e742e-220">Additional resources</span></span>

* [<span data-ttu-id="e742e-221">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e742e-221">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e742e-222">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e742e-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wingspanetmf-tutorial/tutorial_general_203.png

