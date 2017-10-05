---
title: "Självstudier: Azure Active Directory-integrering med Symantec Web Security Service (WSS) | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Symantec Web Security Service (WSS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: d0738bb750a82863b2f77540e8e7b94cca4b64e3
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a><span data-ttu-id="974ec-103">Självstudier: Azure Active Directory-integrering med Symantec Web Security Service (WSS)</span><span class="sxs-lookup"><span data-stu-id="974ec-103">Tutorial: Azure Active Directory integration with Symantec Web Security Service (WSS)</span></span>

<span data-ttu-id="974ec-104">I kursen får lära du att integrera Symantec Web Security Service (WSS) med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="974ec-104">In this tutorial, you learn how to integrate Symantec Web Security Service (WSS) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="974ec-105">Integrera Symantec Web Security Service (WSS) med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="974ec-105">Integrating Symantec Web Security Service (WSS) with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="974ec-106">Du kan styra i Azure AD som har åtkomst till Symantec Web Security Service (WSS)</span><span class="sxs-lookup"><span data-stu-id="974ec-106">You can control in Azure AD who has access to Symantec Web Security Service (WSS)</span></span>
- <span data-ttu-id="974ec-107">Du kan aktivera användarna att automatiskt hämta inloggade till Symantec Web Security Service (WSS) (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="974ec-107">You can enable your users to automatically get signed-on to Symantec Web Security Service (WSS) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="974ec-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="974ec-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="974ec-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="974ec-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="974ec-110">Krav</span><span class="sxs-lookup"><span data-stu-id="974ec-110">Prerequisites</span></span>

<span data-ttu-id="974ec-111">Om du vill konfigurera Azure AD-integrering med Symantec Web Security Service (WSS) behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="974ec-111">To configure Azure AD integration with Symantec Web Security Service (WSS), you need the following items:</span></span>

- <span data-ttu-id="974ec-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="974ec-112">An Azure AD subscription</span></span>
- <span data-ttu-id="974ec-113">En Symantec Web Security Service (WSS) enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="974ec-113">A Symantec Web Security Service (WSS) single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="974ec-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="974ec-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="974ec-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="974ec-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="974ec-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="974ec-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="974ec-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="974ec-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="974ec-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="974ec-118">Scenario description</span></span>
<span data-ttu-id="974ec-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="974ec-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="974ec-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="974ec-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="974ec-121">Att lägga till Symantec Web Security Service (WSS) från galleriet</span><span class="sxs-lookup"><span data-stu-id="974ec-121">Adding Symantec Web Security Service (WSS) from the gallery</span></span>
2. <span data-ttu-id="974ec-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="974ec-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-symantec-web-security-service-wss-from-the-gallery"></a><span data-ttu-id="974ec-123">Att lägga till Symantec Web Security Service (WSS) från galleriet</span><span class="sxs-lookup"><span data-stu-id="974ec-123">Adding Symantec Web Security Service (WSS) from the gallery</span></span>
<span data-ttu-id="974ec-124">Du måste lägga till Symantec Web Security Service (WSS) från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Symantec Web Security Service (WSS) i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="974ec-124">To configure the integration of Symantec Web Security Service (WSS) into Azure AD, you need to add Symantec Web Security Service (WSS) from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="974ec-125">**Utför följande steg för att lägga till Symantec Web Security Service (WSS) från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="974ec-125">**To add Symantec Web Security Service (WSS) from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="974ec-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="974ec-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="974ec-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="974ec-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="974ec-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="974ec-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="974ec-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="974ec-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="974ec-133">I sökrutan skriver **Symantec Web Security Service (WSS)**.</span><span class="sxs-lookup"><span data-stu-id="974ec-133">In the search box, type **Symantec Web Security Service (WSS)**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_search.png)

5. <span data-ttu-id="974ec-135">Välj i resultatpanelen **Symantec Web Security Service (WSS)**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="974ec-135">In the results panel, select **Symantec Web Security Service (WSS)**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="974ec-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="974ec-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="974ec-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Symantec Web Security Service (WSS) baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="974ec-138">In this section, you configure and test Azure AD single sign-on with Symantec Web Security Service (WSS) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="974ec-139">Azure AD måste du känna till motsvarande användaren i Symantec Web Security Service (WSS) till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="974ec-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Symantec Web Security Service (WSS) is to a user in Azure AD.</span></span> <span data-ttu-id="974ec-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i Symantec Web Security Service (WSS) upprättas.</span><span class="sxs-lookup"><span data-stu-id="974ec-140">In other words, a link relationship between an Azure AD user and the related user in Symantec Web Security Service (WSS) needs to be established.</span></span>

<span data-ttu-id="974ec-141">I Symantec Web Security Service (WSS), tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="974ec-141">In Symantec Web Security Service (WSS), assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="974ec-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Symantec Web Security Service (WSS), måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="974ec-142">To configure and test Azure AD single sign-on with Symantec Web Security Service (WSS), you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="974ec-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="974ec-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="974ec-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="974ec-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="974ec-145">**[Skapa en testanvändare Symantec Web Security Service (WSS)](#creating-a-symantec-web-security-service-wss-test-user)**  – du har en motsvarighet för Britta Simon i Symantec Web Security Service (WSS) som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="974ec-145">**[Creating a Symantec Web Security Service (WSS) test user](#creating-a-symantec-web-security-service-wss-test-user)** - to have a counterpart of Britta Simon in Symantec Web Security Service (WSS) that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="974ec-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="974ec-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="974ec-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="974ec-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="974ec-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="974ec-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="974ec-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Symantec Web Security Service (WSS)-program.</span><span class="sxs-lookup"><span data-stu-id="974ec-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Symantec Web Security Service (WSS) application.</span></span>

<span data-ttu-id="974ec-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Symantec Web Security Service (WSS):**</span><span class="sxs-lookup"><span data-stu-id="974ec-150">**To configure Azure AD single sign-on with Symantec Web Security Service (WSS), perform the following steps:**</span></span>

1. <span data-ttu-id="974ec-151">I Azure-portalen på den **Symantec Web Security Service (WSS)** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="974ec-151">In the Azure portal, on the **Symantec Web Security Service (WSS)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="974ec-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="974ec-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

3. <span data-ttu-id="974ec-155">På den **URL: er och Symantec Web Service (WSS) säkerhetsdomän** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="974ec-155">On the **Symantec Web Security Service (WSS) Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    <span data-ttu-id="974ec-157">a.</span><span class="sxs-lookup"><span data-stu-id="974ec-157">a.</span></span> <span data-ttu-id="974ec-158">I den **inloggnings-URL** textruta nämnt url, vilket du vill blockera enligt blockerande princip för Symantec Web Security Service (WSS).</span><span class="sxs-lookup"><span data-stu-id="974ec-158">In the **Sign-on URL** textbox, mention the url, which you want to block according to the blocking policy of the Symantec Web Security Service (WSS).</span></span>  
    
    <span data-ttu-id="974ec-159">b.</span><span class="sxs-lookup"><span data-stu-id="974ec-159">b.</span></span> <span data-ttu-id="974ec-160">I den **identifierare** textruta anger du URL:`https://saml.threatpulse.net:8443/saml/saml_realm`</span><span class="sxs-lookup"><span data-stu-id="974ec-160">In the **Identifier** textbox, type the URL: `https://saml.threatpulse.net:8443/saml/saml_realm`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="974ec-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="974ec-161">These values are not real.</span></span> <span data-ttu-id="974ec-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="974ec-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="974ec-163">Kontakta [Symantec Web Security Service (WSS) klienten supportteamet](https://www.symantec.com/contact-us) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="974ec-163">Contact [Symantec Web Security Service (WSS) Client support team](https://www.symantec.com/contact-us) to get these values.</span></span> 

4. <span data-ttu-id="974ec-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="974ec-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

5. <span data-ttu-id="974ec-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="974ec-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="974ec-168">Konfigurera enkel inloggning på **Symantec Web Security Service (WSS)** sida, måste du skicka den hämtade **XML-Metadata för** till [Symantec Web Security Service (WSS) supportteam](https://www.symantec.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="974ec-168">To configure single sign-on on **Symantec Web Security Service (WSS)** side, you need to send the downloaded **Metadata XML** to [Symantec Web Security Service (WSS) support team](https://www.symantec.com/contact-us).</span></span>

> [!TIP]
> <span data-ttu-id="974ec-169">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="974ec-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="974ec-170">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="974ec-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="974ec-171">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="974ec-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="974ec-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="974ec-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="974ec-173">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="974ec-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="974ec-175">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="974ec-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="974ec-176">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="974ec-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="974ec-178">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="974ec-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="974ec-180">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="974ec-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="974ec-182">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="974ec-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="974ec-184">a.</span><span class="sxs-lookup"><span data-stu-id="974ec-184">a.</span></span> <span data-ttu-id="974ec-185">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="974ec-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="974ec-186">b.</span><span class="sxs-lookup"><span data-stu-id="974ec-186">b.</span></span> <span data-ttu-id="974ec-187">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="974ec-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="974ec-188">c.</span><span class="sxs-lookup"><span data-stu-id="974ec-188">c.</span></span> <span data-ttu-id="974ec-189">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="974ec-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="974ec-190">d.</span><span class="sxs-lookup"><span data-stu-id="974ec-190">d.</span></span> <span data-ttu-id="974ec-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="974ec-191">Click **Create**.</span></span>
 
### <a name="creating-a-symantec-web-security-service-wss-test-user"></a><span data-ttu-id="974ec-192">Skapa en testanvändare Symantec Web Security Service (WSS)</span><span class="sxs-lookup"><span data-stu-id="974ec-192">Creating a Symantec Web Security Service (WSS) test user</span></span>

<span data-ttu-id="974ec-193">I det här avsnittet skapar du en användare som kallas Britta Simon i Symantec Web Security Service (WSS).</span><span class="sxs-lookup"><span data-stu-id="974ec-193">In this section, you create a user called Britta Simon in Symantec Web Security Service (WSS).</span></span> <span data-ttu-id="974ec-194">Arbeta med [Symantec Web Security Service (WSS) supportteam](https://www.symantec.com/contact-us) att lägga till användare i Symantec Web Security Service (WSS)-plattformen.</span><span class="sxs-lookup"><span data-stu-id="974ec-194">Work with [Symantec Web Security Service (WSS) support team](https://www.symantec.com/contact-us) to add the users in the Symantec Web Security Service (WSS) platform.</span></span> <span data-ttu-id="974ec-195">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="974ec-195">Users must be created and activated before you use single sign-on.</span></span> <span data-ttu-id="974ec-196">Också tillsammans med information om användaren måste skicka den offentliga IP-adress för datorn som du försöker komma åt programmet.</span><span class="sxs-lookup"><span data-stu-id="974ec-196">Also along with the user details you need to send the public IPaddress of the machine from which you are trying to access the application.</span></span>

> [!NOTE]
> <span data-ttu-id="974ec-197">Kontrollera [Klicka här](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) att hämta din dator är offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="974ec-197">Please [click here](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) to get your machine's public IPaddress.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="974ec-198">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="974ec-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="974ec-199">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Symantec Web Security Service (WSS).</span><span class="sxs-lookup"><span data-stu-id="974ec-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Symantec Web Security Service (WSS).</span></span>

![Tilldela användare][200] 

<span data-ttu-id="974ec-201">**Om du vill tilldela Britta Simon till Symantec Web Security Service (WSS), utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="974ec-201">**To assign Britta Simon to Symantec Web Security Service (WSS), perform the following steps:**</span></span>

1. <span data-ttu-id="974ec-202">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="974ec-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="974ec-204">Välj i listan med program **Symantec Web Security Service (WSS)**.</span><span class="sxs-lookup"><span data-stu-id="974ec-204">In the applications list, select **Symantec Web Security Service (WSS)**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_app.png) 

3. <span data-ttu-id="974ec-206">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="974ec-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="974ec-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="974ec-208">Click **Add** button.</span></span> <span data-ttu-id="974ec-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="974ec-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="974ec-211">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="974ec-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="974ec-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="974ec-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="974ec-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="974ec-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="974ec-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="974ec-214">Testing single sign-on</span></span>

<span data-ttu-id="974ec-215">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="974ec-215">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="974ec-216">När du klickar på panelen Symantec Web Security Service (WSS) på åtkomstpanelen omdirigeras du till sidan blockerande som blockerande princip har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="974ec-216">When you click the Symantec Web Security Service (WSS) tile in the Access Panel, you get redirected to the blocking page for which the blocking policy has been configured.</span></span>
<span data-ttu-id="974ec-217">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="974ec-217">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="974ec-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="974ec-218">Additional resources</span></span>

* [<span data-ttu-id="974ec-219">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="974ec-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="974ec-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="974ec-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_203.png

