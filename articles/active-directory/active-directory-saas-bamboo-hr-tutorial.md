---
title: "Självstudier: Azure Active Directory-integrering med BambooHR | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och BambooHR."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f826b5d2-9c64-47df-bbbf-0adf9eb0fa71
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: jeedes
ms.openlocfilehash: 06bf91b0e598fd3d8e644378efdb753611ee1ebc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bamboohr"></a><span data-ttu-id="c8c2a-103">Självstudier: Azure Active Directory-integrering med BambooHR</span><span class="sxs-lookup"><span data-stu-id="c8c2a-103">Tutorial: Azure Active Directory integration with BambooHR</span></span>

<span data-ttu-id="c8c2a-104">I kursen får lära du att integrera BambooHR med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c8c2a-104">In this tutorial, you learn how to integrate BambooHR with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c8c2a-105">Integrera BambooHR med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c8c2a-105">Integrating BambooHR with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c8c2a-106">Du kan styra i Azure AD som har åtkomst till BambooHR</span><span class="sxs-lookup"><span data-stu-id="c8c2a-106">You can control in Azure AD who has access to BambooHR</span></span>
- <span data-ttu-id="c8c2a-107">Du kan aktivera användarna att automatiskt hämta loggat in på BambooHR (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c8c2a-107">You can enable your users to automatically get signed-on to BambooHR (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c8c2a-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c8c2a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c8c2a-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c8c2a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c8c2a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c8c2a-110">Prerequisites</span></span>

<span data-ttu-id="c8c2a-111">För att konfigurera Azure AD-integrering med BambooHR, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="c8c2a-111">To configure Azure AD integration with BambooHR, you need the following items:</span></span>

- <span data-ttu-id="c8c2a-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c8c2a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c8c2a-113">En BambooHR enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="c8c2a-113">A BambooHR single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c8c2a-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c8c2a-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c8c2a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c8c2a-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c8c2a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c8c2a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c8c2a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c8c2a-118">Scenario description</span></span>
<span data-ttu-id="c8c2a-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c8c2a-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c8c2a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c8c2a-121">Att lägga till BambooHR från galleriet</span><span class="sxs-lookup"><span data-stu-id="c8c2a-121">Adding BambooHR from the gallery</span></span>
2. <span data-ttu-id="c8c2a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c8c2a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bamboohr-from-the-gallery"></a><span data-ttu-id="c8c2a-123">Att lägga till BambooHR från galleriet</span><span class="sxs-lookup"><span data-stu-id="c8c2a-123">Adding BambooHR from the gallery</span></span>
<span data-ttu-id="c8c2a-124">Du måste lägga till BambooHR från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av BambooHR i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-124">To configure the integration of BambooHR into Azure AD, you need to add BambooHR from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c8c2a-125">**Utför följande steg för att lägga till BambooHR från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="c8c2a-125">**To add BambooHR from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c8c2a-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c8c2a-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c8c2a-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="c8c2a-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="c8c2a-133">I sökrutan skriver **BambooHR**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-133">In the search box, type **BambooHR**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_search.png)

5. <span data-ttu-id="c8c2a-135">Välj i resultatpanelen **BambooHR**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-135">In the results panel, select **BambooHR**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c8c2a-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c8c2a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c8c2a-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med BambooHR baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-138">In this section, you configure and test Azure AD single sign-on with BambooHR based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c8c2a-139">Azure AD måste du känna till användaren i BambooHR motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BambooHR is to a user in Azure AD.</span></span> <span data-ttu-id="c8c2a-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i BambooHR upprättas.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-140">In other words, a link relationship between an Azure AD user and the related user in BambooHR needs to be established.</span></span>

<span data-ttu-id="c8c2a-141">I BambooHR, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-141">In BambooHR, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c8c2a-142">Om du vill konfigurera och testa Azure AD enkel inloggning med BambooHR, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c8c2a-142">To configure and test Azure AD single sign-on with BambooHR, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c8c2a-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c8c2a-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c8c2a-145">**[Skapa en testanvändare BambooHR](#creating-a-bamboohr-test-user)**  – du har en motsvarighet för Britta Simon i BambooHR som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-145">**[Creating a BambooHR test user](#creating-a-bamboohr-test-user)** - to have a counterpart of Britta Simon in BambooHR that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c8c2a-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c8c2a-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c8c2a-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c8c2a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c8c2a-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt BambooHR program.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BambooHR application.</span></span>

<span data-ttu-id="c8c2a-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med BambooHR:**</span><span class="sxs-lookup"><span data-stu-id="c8c2a-150">**To configure Azure AD single sign-on with BambooHR, perform the following steps:**</span></span>

1. <span data-ttu-id="c8c2a-151">I Azure-portalen på den **BambooHR** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-151">In the Azure portal, on the **BambooHR** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c8c2a-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_samlbase.png)

3. <span data-ttu-id="c8c2a-155">På den **BambooHR domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c8c2a-155">On the **BambooHR Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_url.png)

    <span data-ttu-id="c8c2a-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<company>.bamboohr.com`</span><span class="sxs-lookup"><span data-stu-id="c8c2a-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company>.bamboohr.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="c8c2a-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-158">This value is not real.</span></span> <span data-ttu-id="c8c2a-159">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="c8c2a-160">Kontakta [BambooHR klienten supportteamet](https://www.bamboohr.com/contact.php) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-160">Contact [BambooHR Client support team](https://www.bamboohr.com/contact.php) to get this value.</span></span> 
 
4. <span data-ttu-id="c8c2a-161">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_certificate.png) 

5. <span data-ttu-id="c8c2a-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c8c2a-165">På den **BambooHR Configuration** klickar du på **konfigurera BambooHR** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-165">On the **BambooHR Configuration** section, click **Configure BambooHR** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c8c2a-166">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="c8c2a-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_configure.png) 

6. <span data-ttu-id="c8c2a-168">Logga in på webbplatsen BambooHR företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-168">In a different web browser window, log into your BambooHR company site as an administrator.</span></span>

7. <span data-ttu-id="c8c2a-169">På startsidan, utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="c8c2a-169">On the homepage, perform the following steps:</span></span>
   
    <span data-ttu-id="c8c2a-170">![Enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/ic796691.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="c8c2a-170">![Single Sign-On](./media/active-directory-saas-bamboo-hr-tutorial/ic796691.png "Single Sign-On")</span></span>   

    <span data-ttu-id="c8c2a-171">a.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-171">a.</span></span> <span data-ttu-id="c8c2a-172">Klicka på **appar**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-172">Click **Apps**.</span></span>
   
    <span data-ttu-id="c8c2a-173">b.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-173">b.</span></span> <span data-ttu-id="c8c2a-174">Klicka på menyn appar till vänster **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-174">In the apps menu on the left, click **Single Sign-On**.</span></span>
   
    <span data-ttu-id="c8c2a-175">c.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-175">c.</span></span> <span data-ttu-id="c8c2a-176">Klicka på **SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-176">Click **SAML Single Sign-On**.</span></span>

8. <span data-ttu-id="c8c2a-177">I den **SAML enkel inloggning** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c8c2a-177">In the **SAML Single Sign-On** section, perform the following steps:</span></span>
   
    <span data-ttu-id="c8c2a-178">![SAML enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/ic796692.png "SAML enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="c8c2a-178">![SAML Single Sign-On](./media/active-directory-saas-bamboo-hr-tutorial/ic796692.png "SAML Single Sign-On")</span></span>
   
    <span data-ttu-id="c8c2a-179">a.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-179">a.</span></span> <span data-ttu-id="c8c2a-180">Klistra in den **SAML inloggning tjänst-URL för enkel** värde i den **inloggnings-Url för SSO** textruta.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-180">Paste the **SAML Single Sign-On Service URL** value into the **SSO Login Url** textbox.</span></span>
      
    <span data-ttu-id="c8c2a-181">b.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-181">b.</span></span> <span data-ttu-id="c8c2a-182">Öppna Base64-kodade certifikat hämtas från Azure-portalen i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **X.509-certifikat** textruta</span><span class="sxs-lookup"><span data-stu-id="c8c2a-182">Open base-64 encoded certificate downloaded from Azure portal in notepad, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox</span></span>
   
    <span data-ttu-id="c8c2a-183">c.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-183">c.</span></span> <span data-ttu-id="c8c2a-184">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-184">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="c8c2a-185">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="c8c2a-185">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c8c2a-186">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-186">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c8c2a-187">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c8c2a-187">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c8c2a-188">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c8c2a-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="c8c2a-189">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-189">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="c8c2a-191">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="c8c2a-191">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c8c2a-192">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-192">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c8c2a-194">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-194">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c8c2a-196">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-196">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c8c2a-198">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c8c2a-198">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c8c2a-200">a.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-200">a.</span></span> <span data-ttu-id="c8c2a-201">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-201">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c8c2a-202">b.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-202">b.</span></span> <span data-ttu-id="c8c2a-203">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-203">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c8c2a-204">c.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-204">c.</span></span> <span data-ttu-id="c8c2a-205">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-205">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c8c2a-206">d.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-206">d.</span></span> <span data-ttu-id="c8c2a-207">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-207">Click **Create**.</span></span>
 
### <a name="creating-a-bamboohr-test-user"></a><span data-ttu-id="c8c2a-208">Skapa en testanvändare BambooHR</span><span class="sxs-lookup"><span data-stu-id="c8c2a-208">Creating a BambooHR test user</span></span>

<span data-ttu-id="c8c2a-209">Om du vill aktivera Azure AD-användare kan logga in på BambooHR etableras de i BambooHR.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-209">To enable Azure AD users to log in to BambooHR, they must be provisioned into BambooHR.</span></span>  

<span data-ttu-id="c8c2a-210">När det gäller BambooHR är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-210">In the case of BambooHR, provisioning is a manual task.</span></span>

<span data-ttu-id="c8c2a-211">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="c8c2a-211">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="c8c2a-212">Logga in på ditt **BambooHR** plats som administratör.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-212">Log in to your **BambooHR** site as administrator.</span></span>

2. <span data-ttu-id="c8c2a-213">Klicka på i verktygsfältet högst upp **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-213">In the toolbar on the top, click **Settings**.</span></span>
   
    <span data-ttu-id="c8c2a-214">![Ange](./media/active-directory-saas-bamboo-hr-tutorial/ic796694.png "inställning")</span><span class="sxs-lookup"><span data-stu-id="c8c2a-214">![Setting](./media/active-directory-saas-bamboo-hr-tutorial/ic796694.png "Setting")</span></span>

3. <span data-ttu-id="c8c2a-215">Klicka på **översikt**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-215">Click **Overview**.</span></span>

4. <span data-ttu-id="c8c2a-216">I det vänstra navigeringsfönstret, gå till **säkerhet \> användare**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-216">In the left navigation pane, go to **Security \> Users**.</span></span>

5. <span data-ttu-id="c8c2a-217">Ange användarnamn, lösenord och e-postadressen till en giltig AAD-konto som du vill etablera i relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-217">Type the user name, password, and email address of a valid AAD account you want to provision into the related textboxes.</span></span>

6. <span data-ttu-id="c8c2a-218">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-218">Click **Save**.</span></span>
        
>[!NOTE]
><span data-ttu-id="c8c2a-219">Du kan använda något annat BambooHR användarens konto skapas verktyg eller API: er som tillhandahålls av BambooHR att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-219">You can use any other BambooHR user account creation tools or APIs provided by BambooHR to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c8c2a-220">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="c8c2a-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c8c2a-221">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till BambooHR.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BambooHR.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="c8c2a-223">**Om du vill tilldela BambooHR Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c8c2a-223">**To assign Britta Simon to BambooHR, perform the following steps:**</span></span>

1. <span data-ttu-id="c8c2a-224">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c8c2a-226">Välj i listan med program **BambooHR**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-226">In the applications list, select **BambooHR**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_app.png) 

3. <span data-ttu-id="c8c2a-228">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="c8c2a-230">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-230">Click **Add** button.</span></span> <span data-ttu-id="c8c2a-231">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="c8c2a-233">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c8c2a-234">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c8c2a-235">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c8c2a-236">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c8c2a-236">Testing single sign-on</span></span>

<span data-ttu-id="c8c2a-237">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-237">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c8c2a-238">När du klickar på panelen BambooHR på åtkomstpanelen du bör få automatiskt loggat in på ditt BambooHR program.</span><span class="sxs-lookup"><span data-stu-id="c8c2a-238">When you click the BambooHR tile in the Access Panel, you should get automatically signed-on to your BambooHR application.</span></span>
<span data-ttu-id="c8c2a-239">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c8c2a-239">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c8c2a-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c8c2a-240">Additional resources</span></span>

* [<span data-ttu-id="c8c2a-241">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c8c2a-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c8c2a-242">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c8c2a-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_203.png

