---
title: "Självstudier: Azure Active Directory-integrering med Jive | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Jive."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9fc5659a-c116-4a1b-a601-333325a26b46
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 6d2d4b777d7afd74598d2eba4a7e3571a8a18d6f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jive"></a><span data-ttu-id="7dd71-103">Självstudier: Azure Active Directory-integrering med Jive</span><span class="sxs-lookup"><span data-stu-id="7dd71-103">Tutorial: Azure Active Directory integration with Jive</span></span>

<span data-ttu-id="7dd71-104">I kursen får lära du att integrera Jive med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7dd71-104">In this tutorial, you learn how to integrate Jive with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7dd71-105">Integrera Jive med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7dd71-105">Integrating Jive with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7dd71-106">Du kan styra i Azure AD som har åtkomst till Jive</span><span class="sxs-lookup"><span data-stu-id="7dd71-106">You can control in Azure AD who has access to Jive</span></span>
- <span data-ttu-id="7dd71-107">Du kan aktivera användarna att automatiskt hämta loggat in på Jive (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="7dd71-107">You can enable your users to automatically get signed-on to Jive (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7dd71-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7dd71-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="7dd71-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7dd71-109">If you want to know more information about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7dd71-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7dd71-110">Prerequisites</span></span>

<span data-ttu-id="7dd71-111">För att konfigurera Azure AD-integrering med Jive, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="7dd71-111">To configure Azure AD integration with Jive, you need the following items:</span></span>

- <span data-ttu-id="7dd71-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7dd71-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7dd71-113">En Jive enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="7dd71-113">A Jive single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7dd71-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7dd71-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7dd71-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7dd71-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7dd71-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="7dd71-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7dd71-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7dd71-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7dd71-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="7dd71-118">Scenario description</span></span>
<span data-ttu-id="7dd71-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7dd71-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7dd71-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="7dd71-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7dd71-121">Att lägga till Jive från galleriet</span><span class="sxs-lookup"><span data-stu-id="7dd71-121">Adding Jive from the gallery</span></span>
2. <span data-ttu-id="7dd71-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7dd71-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jive-from-the-gallery"></a><span data-ttu-id="7dd71-123">Att lägga till Jive från galleriet</span><span class="sxs-lookup"><span data-stu-id="7dd71-123">Adding Jive from the gallery</span></span>
<span data-ttu-id="7dd71-124">Du måste lägga till Jive från galleriet i listan över hanterade SaaS-appar för att konfigurera Jive till Azure AD-integrering.</span><span class="sxs-lookup"><span data-stu-id="7dd71-124">To configure the integration of Jive into Azure AD, you need to add Jive from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7dd71-125">**Utför följande steg för att lägga till Jive från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="7dd71-125">**To add Jive from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7dd71-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7dd71-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7dd71-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7dd71-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7dd71-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7dd71-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="7dd71-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7dd71-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="7dd71-133">I sökrutan skriver **Jive**.</span><span class="sxs-lookup"><span data-stu-id="7dd71-133">In the search box, type **Jive**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jive-tutorial/tutorial_jive_search.png)

5. <span data-ttu-id="7dd71-135">Välj i resultatpanelen **Jive**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="7dd71-135">In the results panel, select **Jive**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jive-tutorial/tutorial_jive_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7dd71-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7dd71-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7dd71-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Jive baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="7dd71-138">In this section, you configure and test Azure AD single sign-on with Jive based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="7dd71-139">Azure AD måste du känna till användaren i Jive motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="7dd71-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Jive is to a user in Azure AD.</span></span> <span data-ttu-id="7dd71-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Jive upprättas.</span><span class="sxs-lookup"><span data-stu-id="7dd71-140">In other words, a link relationship between an Azure AD user and the related user in Jive needs to be established.</span></span>

<span data-ttu-id="7dd71-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Jive.</span><span class="sxs-lookup"><span data-stu-id="7dd71-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Jive.</span></span>

<span data-ttu-id="7dd71-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Jive, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="7dd71-142">To configure and test Azure AD single sign-on with Jive, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7dd71-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7dd71-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7dd71-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7dd71-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7dd71-145">**[Skapa en testanvändare Jive](#creating-a-jive-test-user)**  – du har en motsvarighet för Britta Simon i Jive som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="7dd71-145">**[Creating a Jive test user](#creating-a-jive-test-user)** - to have a counterpart of Britta Simon in Jive that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7dd71-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7dd71-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7dd71-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="7dd71-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7dd71-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7dd71-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7dd71-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Jive program.</span><span class="sxs-lookup"><span data-stu-id="7dd71-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Jive application.</span></span>

<span data-ttu-id="7dd71-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Jive:**</span><span class="sxs-lookup"><span data-stu-id="7dd71-150">**To configure Azure AD single sign-on with Jive, perform the following steps:**</span></span>

1. <span data-ttu-id="7dd71-151">I Azure-portalen på den **Jive** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7dd71-151">In the Azure portal, on the **Jive** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="7dd71-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7dd71-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-jive-tutorial/tutorial_jive_samlbase.png)

3. <span data-ttu-id="7dd71-155">På den **Jive domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7dd71-155">On the **Jive Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jive-tutorial/tutorial_jive_url.png)

    <span data-ttu-id="7dd71-157">a.</span><span class="sxs-lookup"><span data-stu-id="7dd71-157">a.</span></span> <span data-ttu-id="7dd71-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<instance name>.jivecustom.com`</span><span class="sxs-lookup"><span data-stu-id="7dd71-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<instance name>.jivecustom.com`</span></span>

    <span data-ttu-id="7dd71-159">b.</span><span class="sxs-lookup"><span data-stu-id="7dd71-159">b.</span></span> <span data-ttu-id="7dd71-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<instance name>.jiveon.com`</span><span class="sxs-lookup"><span data-stu-id="7dd71-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<instance name>.jiveon.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7dd71-161">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="7dd71-161">These values are not the real.</span></span> <span data-ttu-id="7dd71-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="7dd71-162">Update these values with the actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="7dd71-163">Kontakta [Jive klienten supportteamet](https://www.jivesoftware.com/services-support/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="7dd71-163">Contact [Jive Client support team](https://www.jivesoftware.com/services-support/) to get these values.</span></span> 
 
4. <span data-ttu-id="7dd71-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="7dd71-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jive-tutorial/tutorial_jive_certificate.png) 

5. <span data-ttu-id="7dd71-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="7dd71-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jive-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7dd71-168">Konfigurera enkel inloggning på **Jive** sida, inloggning till din Jive klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="7dd71-168">To configure single sign-on on **Jive** side, sign-on to your Jive tenant as an administrator.</span></span>

7. <span data-ttu-id="7dd71-169">Klicka på menyn högst upp ”**Saml**”.</span><span class="sxs-lookup"><span data-stu-id="7dd71-169">In the menu on the top, Click "**Saml**."</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-jive-tutorial/tutorial_jive_002.png)

    <span data-ttu-id="7dd71-171">a.</span><span class="sxs-lookup"><span data-stu-id="7dd71-171">a.</span></span> <span data-ttu-id="7dd71-172">Välj **aktiverad** under den **allmänna** fliken.</span><span class="sxs-lookup"><span data-stu-id="7dd71-172">Select **Enabled** under the **General** tab.</span></span>   
    <span data-ttu-id="7dd71-173">b.</span><span class="sxs-lookup"><span data-stu-id="7dd71-173">b.</span></span> <span data-ttu-id="7dd71-174">Klicka på ”**spara alla inställningar för saml**” knappen.</span><span class="sxs-lookup"><span data-stu-id="7dd71-174">Click the "**Save all saml settings**" button.</span></span>

8. <span data-ttu-id="7dd71-175">Navigera till den ”**Idp Metadata**” fliken.</span><span class="sxs-lookup"><span data-stu-id="7dd71-175">Navigate to the "**Idp Metadata**" tab.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-jive-tutorial/tutorial_jive_003.png)
   
    <span data-ttu-id="7dd71-177">a.</span><span class="sxs-lookup"><span data-stu-id="7dd71-177">a.</span></span> <span data-ttu-id="7dd71-178">Kopiera innehållet i den hämta metadata XML-filen och klistrar in det i den **identitet Provider (IDP) Metadata** textruta.</span><span class="sxs-lookup"><span data-stu-id="7dd71-178">Copy the content of the downloaded metadata XML file, and then paste it into the **Identity Provider (IDP) Metadata** textbox.</span></span>
    
    <span data-ttu-id="7dd71-179">b.</span><span class="sxs-lookup"><span data-stu-id="7dd71-179">b.</span></span> <span data-ttu-id="7dd71-180">Klicka på ”**spara alla inställningar för saml**” knappen.</span><span class="sxs-lookup"><span data-stu-id="7dd71-180">Click the "**Save all saml settings**" button.</span></span> 

9. <span data-ttu-id="7dd71-181">Gå till den ”**mappning av attributet**” fliken.</span><span class="sxs-lookup"><span data-stu-id="7dd71-181">Go to the "**User Attribute Mapping**" tab.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-jive-tutorial/tutorial_jive_004.png)
   
    <span data-ttu-id="7dd71-183">a.</span><span class="sxs-lookup"><span data-stu-id="7dd71-183">a.</span></span> <span data-ttu-id="7dd71-184">I den **e-post** textruta, kopiera och klistra in attributnamnet för **e** värde.</span><span class="sxs-lookup"><span data-stu-id="7dd71-184">In the **Email** textbox, copy and paste the attribute name of **mail** value.</span></span>
   
    <span data-ttu-id="7dd71-185">b.</span><span class="sxs-lookup"><span data-stu-id="7dd71-185">b.</span></span> <span data-ttu-id="7dd71-186">I den **Förnamn** textruta, kopiera och klistra in attributnamnet för **givenname** värde.</span><span class="sxs-lookup"><span data-stu-id="7dd71-186">In the **First Name** textbox, copy and paste the attribute name of **givenname** value.</span></span>
   
    <span data-ttu-id="7dd71-187">c.</span><span class="sxs-lookup"><span data-stu-id="7dd71-187">c.</span></span> <span data-ttu-id="7dd71-188">I den **efternamn** textruta, kopiera och klistra in attributnamnet för **efternamn** värde.</span><span class="sxs-lookup"><span data-stu-id="7dd71-188">In the **Last Name** textbox, copy and paste the attribute name of **surname** value.</span></span>

> [!TIP]
> <span data-ttu-id="7dd71-189">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="7dd71-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7dd71-190">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="7dd71-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7dd71-191">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7dd71-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7dd71-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7dd71-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="7dd71-193">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7dd71-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="7dd71-195">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="7dd71-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7dd71-196">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7dd71-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7dd71-198">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="7dd71-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7dd71-200">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7dd71-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7dd71-202">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7dd71-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jive-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7dd71-204">a.</span><span class="sxs-lookup"><span data-stu-id="7dd71-204">a.</span></span> <span data-ttu-id="7dd71-205">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7dd71-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7dd71-206">b.</span><span class="sxs-lookup"><span data-stu-id="7dd71-206">b.</span></span> <span data-ttu-id="7dd71-207">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7dd71-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7dd71-208">c.</span><span class="sxs-lookup"><span data-stu-id="7dd71-208">c.</span></span> <span data-ttu-id="7dd71-209">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="7dd71-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="7dd71-210">d.</span><span class="sxs-lookup"><span data-stu-id="7dd71-210">d.</span></span> <span data-ttu-id="7dd71-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7dd71-211">Click **Create**.</span></span>
 
### <a name="creating-a-jive-test-user"></a><span data-ttu-id="7dd71-212">Skapa en testanvändare Jive</span><span class="sxs-lookup"><span data-stu-id="7dd71-212">Creating a Jive test user</span></span>

<span data-ttu-id="7dd71-213">Arbeta med [Jive klienten supportteamet](https://www.jivesoftware.com/services-support/) att lägga till användare i Jive-plattformen.</span><span class="sxs-lookup"><span data-stu-id="7dd71-213">Work with [Jive Client support team](https://www.jivesoftware.com/services-support/) to add the users in the Jive platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="7dd71-214">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="7dd71-214">Assigning the Azure AD test user</span></span>

<span data-ttu-id="7dd71-215">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Jive.</span><span class="sxs-lookup"><span data-stu-id="7dd71-215">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Jive.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="7dd71-217">**Om du vill tilldela Jive Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7dd71-217">**To assign Britta Simon to Jive, perform the following steps:**</span></span>

1. <span data-ttu-id="7dd71-218">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7dd71-218">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="7dd71-220">Välj i listan med program **Jive**.</span><span class="sxs-lookup"><span data-stu-id="7dd71-220">In the applications list, select **Jive**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jive-tutorial/tutorial_jive_app.png) 

3. <span data-ttu-id="7dd71-222">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7dd71-222">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="7dd71-224">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="7dd71-224">Click **Add** button.</span></span> <span data-ttu-id="7dd71-225">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7dd71-225">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="7dd71-227">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="7dd71-227">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7dd71-228">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7dd71-228">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7dd71-229">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7dd71-229">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7dd71-230">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7dd71-230">Testing single sign-on</span></span>

<span data-ttu-id="7dd71-231">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="7dd71-231">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="7dd71-232">När du klickar på panelen Jive på åtkomstpanelen du bör få automatiskt loggat in på ditt Jive program.</span><span class="sxs-lookup"><span data-stu-id="7dd71-232">When you click the Jive tile in the Access Panel, you should get automatically signed-on to your Jive application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7dd71-233">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7dd71-233">Additional resources</span></span>

* [<span data-ttu-id="7dd71-234">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7dd71-234">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7dd71-235">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7dd71-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="7dd71-236">Konfigurera Användaretablering</span><span class="sxs-lookup"><span data-stu-id="7dd71-236">Configure User Provisioning</span></span>](active-directory-saas-jive-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-jive-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jive-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jive-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jive-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jive-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jive-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jive-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jive-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jive-tutorial/tutorial_general_203.png

