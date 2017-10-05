---
title: "Självstudier: Azure Active Directory-integrering med Igloo programvara | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Igloo programvaran och Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2eb625c1-d3fc-4ae1-a304-6a6733a10e6e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: ab3891e11eb33b4d233e4fc967a40c7df06e4f4e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-igloo-software"></a><span data-ttu-id="07469-103">Självstudier: Azure Active Directory-integrering med Igloo programvara</span><span class="sxs-lookup"><span data-stu-id="07469-103">Tutorial: Azure Active Directory integration with Igloo Software</span></span>

<span data-ttu-id="07469-104">I kursen får lära du att integrera Igloo programvara med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="07469-104">In this tutorial, you learn how to integrate Igloo Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="07469-105">Integrera Igloo programvara med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="07469-105">Integrating Igloo Software with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="07469-106">Du kan styra i Azure AD som har åtkomst till Igloo programvara</span><span class="sxs-lookup"><span data-stu-id="07469-106">You can control in Azure AD who has access to Igloo Software</span></span>
- <span data-ttu-id="07469-107">Du kan aktivera användarna att automatiskt hämta loggat in på Igloo programvara (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="07469-107">You can enable your users to automatically get signed-on to Igloo Software (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="07469-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="07469-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="07469-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="07469-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="07469-110">Krav</span><span class="sxs-lookup"><span data-stu-id="07469-110">Prerequisites</span></span>

<span data-ttu-id="07469-111">För att konfigurera Azure AD-integrering med Igloo programvara, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="07469-111">To configure Azure AD integration with Igloo Software, you need the following items:</span></span>

- <span data-ttu-id="07469-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="07469-112">An Azure AD subscription</span></span>
- <span data-ttu-id="07469-113">En Igloo programvara enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="07469-113">An Igloo Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="07469-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="07469-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="07469-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="07469-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="07469-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="07469-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="07469-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="07469-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="07469-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="07469-118">Scenario description</span></span>
<span data-ttu-id="07469-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="07469-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="07469-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="07469-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="07469-121">Lägga till Igloo programvara från galleriet</span><span class="sxs-lookup"><span data-stu-id="07469-121">Adding Igloo Software from the gallery</span></span>
2. <span data-ttu-id="07469-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="07469-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-igloo-software-from-the-gallery"></a><span data-ttu-id="07469-123">Lägga till Igloo programvara från galleriet</span><span class="sxs-lookup"><span data-stu-id="07469-123">Adding Igloo Software from the gallery</span></span>
<span data-ttu-id="07469-124">Du måste lägga till Igloo programvara från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Igloo programvara i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="07469-124">To configure the integration of Igloo Software into Azure AD, you need to add Igloo Software from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="07469-125">**Utför följande steg för att lägga till Igloo programvara från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="07469-125">**To add Igloo Software from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="07469-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="07469-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="07469-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="07469-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="07469-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="07469-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="07469-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="07469-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="07469-133">I sökrutan skriver **Igloo programvara**.</span><span class="sxs-lookup"><span data-stu-id="07469-133">In the search box, type **Igloo Software**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_search.png)

5. <span data-ttu-id="07469-135">Välj i resultatpanelen **Igloo programvara**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="07469-135">In the results panel, select **Igloo Software**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="07469-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="07469-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="07469-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Igloo programvara baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="07469-138">In this section, you configure and test Azure AD single sign-on with Igloo Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="07469-139">Azure AD måste du känna till motsvarande användaren i Igloo programvara till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="07469-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Igloo Software is to a user in Azure AD.</span></span> <span data-ttu-id="07469-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Igloo programvara upprättas.</span><span class="sxs-lookup"><span data-stu-id="07469-140">In other words, a link relationship between an Azure AD user and the related user in Igloo Software needs to be established.</span></span>

<span data-ttu-id="07469-141">I Igloo programvara, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="07469-141">In Igloo Software, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="07469-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Igloo programvara, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="07469-142">To configure and test Azure AD single sign-on with Igloo Software, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="07469-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="07469-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="07469-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="07469-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="07469-145">**[Skapa en testanvändare Igloo programvara](#creating-an-igloo-software-test-user)**  – du har en motsvarighet för Britta Simon Igloo programvara som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="07469-145">**[Creating an Igloo Software test user](#creating-an-igloo-software-test-user)** - to have a counterpart of Britta Simon in Igloo Software that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="07469-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="07469-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="07469-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="07469-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="07469-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="07469-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="07469-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i Igloo programmet.</span><span class="sxs-lookup"><span data-stu-id="07469-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Igloo Software application.</span></span>

<span data-ttu-id="07469-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Igloo programvara:**</span><span class="sxs-lookup"><span data-stu-id="07469-150">**To configure Azure AD single sign-on with Igloo Software, perform the following steps:**</span></span>

1. <span data-ttu-id="07469-151">I Azure-portalen på den **Igloo programvara** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="07469-151">In the Azure portal, on the **Igloo Software** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="07469-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="07469-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_samlbase.png)

3. <span data-ttu-id="07469-155">På den **Igloo programvara domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="07469-155">On the **Igloo Software Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_url.png)
    
    <span data-ttu-id="07469-157">a.</span><span class="sxs-lookup"><span data-stu-id="07469-157">a.</span></span> <span data-ttu-id="07469-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<company name>.igloocommmunities.com`</span><span class="sxs-lookup"><span data-stu-id="07469-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.igloocommmunities.com`</span></span>

    <span data-ttu-id="07469-159">b.</span><span class="sxs-lookup"><span data-stu-id="07469-159">b.</span></span> <span data-ttu-id="07469-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<company name>.igloocommmunities.com/saml.digest`</span><span class="sxs-lookup"><span data-stu-id="07469-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.igloocommmunities.com/saml.digest`</span></span>

    <span data-ttu-id="07469-161">c.</span><span class="sxs-lookup"><span data-stu-id="07469-161">c.</span></span> <span data-ttu-id="07469-162">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<company name>.igloocommmunities.com/saml.digest`</span><span class="sxs-lookup"><span data-stu-id="07469-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.igloocommmunities.com/saml.digest`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="07469-163">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="07469-163">These values are not real.</span></span> <span data-ttu-id="07469-164">Uppdatera dessa värden med den faktiska identifierare Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="07469-164">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="07469-165">Kontakta [Igloo Programvaruklienten supportteamet](https://www.igloosoftware.com/services/support) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="07469-165">Contact [Igloo Software Client support team](https://www.igloosoftware.com/services/support) to get these values.</span></span> 

4. <span data-ttu-id="07469-166">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="07469-166">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_certificate.png) 

5. <span data-ttu-id="07469-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="07469-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-igloo-software-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="07469-170">På den **Igloo programvarukonfiguration** klickar du på **konfigurera Igloo programvara** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="07469-170">On the **Igloo Software Configuration** section, click **Configure Igloo Software** to open **Configure sign-on** window.</span></span> <span data-ttu-id="07469-171">Kopiera den **Sign-Out Webbadressen och SAML enkel inloggning Service** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="07469-171">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_configure.png) 

7. <span data-ttu-id="07469-173">I en annan webbläsarfönster loggar du in på webbplatsen för företagets Igloo programvara som en administratör.</span><span class="sxs-lookup"><span data-stu-id="07469-173">In a different web browser window, log in to your Igloo Software company site as an administrator.</span></span>

8. <span data-ttu-id="07469-174">Gå till den **Kontrollpanelen**.</span><span class="sxs-lookup"><span data-stu-id="07469-174">Go to the **Control Panel**.</span></span>
   
     <span data-ttu-id="07469-175">![Kontrollpanelen](./media/active-directory-saas-igloo-software-tutorial/ic799949.png "Kontrollpanelen")</span><span class="sxs-lookup"><span data-stu-id="07469-175">![Control Panel](./media/active-directory-saas-igloo-software-tutorial/ic799949.png "Control Panel")</span></span>

9. <span data-ttu-id="07469-176">I den **medlemskap** klickar du på **logga inställningarna**.</span><span class="sxs-lookup"><span data-stu-id="07469-176">In the **Membership** tab, click **Sign In Settings**.</span></span>
   
    <span data-ttu-id="07469-177">![Logga in inställningar](./media/active-directory-saas-igloo-software-tutorial/ic783968.png "logga in inställningarna")</span><span class="sxs-lookup"><span data-stu-id="07469-177">![Sign in Settings](./media/active-directory-saas-igloo-software-tutorial/ic783968.png "Sign in Settings")</span></span>

10. <span data-ttu-id="07469-178">Klicka på i konfigurationsavsnittet SAML **konfigurera SAML-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="07469-178">In the SAML Configuration section, click **Configure SAML Authentication**.</span></span>
   
    <span data-ttu-id="07469-179">![SAML-konfiguration](./media/active-directory-saas-igloo-software-tutorial/ic783969.png "SAML-konfiguration")</span><span class="sxs-lookup"><span data-stu-id="07469-179">![SAML Configuration](./media/active-directory-saas-igloo-software-tutorial/ic783969.png "SAML Configuration")</span></span>
   
11. <span data-ttu-id="07469-180">I den **Allmän konfiguration** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="07469-180">In the **General Configuration** section, perform the following steps:</span></span>
   
    <span data-ttu-id="07469-181">![Allmän konfiguration](./media/active-directory-saas-igloo-software-tutorial/ic783970.png "Allmän konfiguration")</span><span class="sxs-lookup"><span data-stu-id="07469-181">![General Configuration](./media/active-directory-saas-igloo-software-tutorial/ic783970.png "General Configuration")</span></span>

    <span data-ttu-id="07469-182">a.</span><span class="sxs-lookup"><span data-stu-id="07469-182">a.</span></span> <span data-ttu-id="07469-183">I den **anslutningsnamn** textruta skriver du ett eget namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="07469-183">In the **Connection Name** textbox, type a custom name for your configuration.</span></span>
   
    <span data-ttu-id="07469-184">b.</span><span class="sxs-lookup"><span data-stu-id="07469-184">b.</span></span> <span data-ttu-id="07469-185">I den **IdP inloggnings-URL** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="07469-185">In the **IdP Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="07469-186">c.</span><span class="sxs-lookup"><span data-stu-id="07469-186">c.</span></span> <span data-ttu-id="07469-187">I den **IdP logga ut URL** textruta klistra in värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="07469-187">In the **IdP Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="07469-188">d.</span><span class="sxs-lookup"><span data-stu-id="07469-188">d.</span></span> <span data-ttu-id="07469-189">Välj **logga ut svar och typ av begäran HTTP** som **efter**.</span><span class="sxs-lookup"><span data-stu-id="07469-189">Select **Logout Response and Request HTTP Type** as **POST**.</span></span>
   
    <span data-ttu-id="07469-190">e.</span><span class="sxs-lookup"><span data-stu-id="07469-190">e.</span></span> <span data-ttu-id="07469-191">Öppna din **Base64-** kodade certifikatet i anteckningar hämtas från Azure-portalen, kopiera innehållet i den till Urklipp och klistra in den till den **certifikat med offentlig** textruta.</span><span class="sxs-lookup"><span data-stu-id="07469-191">Open your **base-64** encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **Public Certificate** textbox.</span></span>
    
12. <span data-ttu-id="07469-192">I den **svar och Autentiseringskonfiguration**, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="07469-192">In the **Response and Authentication Configuration**, perform the following steps:</span></span>
    
    <span data-ttu-id="07469-193">![Svar och Autentiseringskonfiguration](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "svar och Autentiseringskonfiguration")</span><span class="sxs-lookup"><span data-stu-id="07469-193">![Response and Authentication Configuration](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Response and Authentication Configuration")</span></span>
  
      <span data-ttu-id="07469-194">a.</span><span class="sxs-lookup"><span data-stu-id="07469-194">a.</span></span> <span data-ttu-id="07469-195">Som **identitetsleverantör**väljer **Microsoft ADFS**.</span><span class="sxs-lookup"><span data-stu-id="07469-195">As **Identity Provider**, select **Microsoft ADFS**.</span></span>
      
      <span data-ttu-id="07469-196">b.</span><span class="sxs-lookup"><span data-stu-id="07469-196">b.</span></span> <span data-ttu-id="07469-197">Som **Ämnesidentifierarens typ**väljer **e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="07469-197">As **Identifier Type**, select **Email Address**.</span></span> 

      <span data-ttu-id="07469-198">c.</span><span class="sxs-lookup"><span data-stu-id="07469-198">c.</span></span> <span data-ttu-id="07469-199">I den **e-attributet** textruta typen **e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="07469-199">In the **Email Attribute** textbox, type **emailaddress**.</span></span>

      <span data-ttu-id="07469-200">d.</span><span class="sxs-lookup"><span data-stu-id="07469-200">d.</span></span> <span data-ttu-id="07469-201">I den **förnamn attributet** textruta typen **givenname**.</span><span class="sxs-lookup"><span data-stu-id="07469-201">In the **First Name Attribute** textbox, type **givenname**.</span></span>

      <span data-ttu-id="07469-202">e.</span><span class="sxs-lookup"><span data-stu-id="07469-202">e.</span></span> <span data-ttu-id="07469-203">I den **senaste namnattributet** textruta typen **efternamn**.</span><span class="sxs-lookup"><span data-stu-id="07469-203">In the **Last Name Attribute** textbox, type **surname**.</span></span>

13. <span data-ttu-id="07469-204">Utför följande steg för att slutföra konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="07469-204">Perform the following steps to complete the configuration:</span></span>
    
    <span data-ttu-id="07469-205">![Skapa användare på inloggning](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "skapa användare på Logga in")</span><span class="sxs-lookup"><span data-stu-id="07469-205">![User creation on Sign in](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "User creation on Sign in")</span></span> 

     <span data-ttu-id="07469-206">a.</span><span class="sxs-lookup"><span data-stu-id="07469-206">a.</span></span> <span data-ttu-id="07469-207">Som **skapa användare på inloggning**väljer **skapa en ny användare på din plats när de loggar in**.</span><span class="sxs-lookup"><span data-stu-id="07469-207">As **User creation on Sign in**, select **Create a new user in your site when they sign in**.</span></span>

     <span data-ttu-id="07469-208">b.</span><span class="sxs-lookup"><span data-stu-id="07469-208">b.</span></span> <span data-ttu-id="07469-209">Som **inloggning inställningar**väljer **Använd SAML-knappen ”Logga in” skärmen**.</span><span class="sxs-lookup"><span data-stu-id="07469-209">As **Sign in Settings**, select **Use SAML button on “Sign in” screen**.</span></span>

     <span data-ttu-id="07469-210">c.</span><span class="sxs-lookup"><span data-stu-id="07469-210">c.</span></span> <span data-ttu-id="07469-211">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="07469-211">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="07469-212">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="07469-212">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="07469-213">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="07469-213">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="07469-214">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="07469-214">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="07469-215">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="07469-215">Creating an Azure AD test user</span></span>
<span data-ttu-id="07469-216">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="07469-216">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="07469-218">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="07469-218">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="07469-219">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="07469-219">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="07469-221">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="07469-221">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="07469-223">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="07469-223">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="07469-225">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="07469-225">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="07469-227">a.</span><span class="sxs-lookup"><span data-stu-id="07469-227">a.</span></span> <span data-ttu-id="07469-228">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="07469-228">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="07469-229">b.</span><span class="sxs-lookup"><span data-stu-id="07469-229">b.</span></span> <span data-ttu-id="07469-230">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="07469-230">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="07469-231">c.</span><span class="sxs-lookup"><span data-stu-id="07469-231">c.</span></span> <span data-ttu-id="07469-232">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="07469-232">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="07469-233">d.</span><span class="sxs-lookup"><span data-stu-id="07469-233">d.</span></span> <span data-ttu-id="07469-234">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="07469-234">Click **Create**.</span></span>
 
### <a name="creating-an-igloo-software-test-user"></a><span data-ttu-id="07469-235">Skapa en testanvändare Igloo programvara</span><span class="sxs-lookup"><span data-stu-id="07469-235">Creating an Igloo Software test user</span></span>

<span data-ttu-id="07469-236">Det finns inga uppgiften att konfigurera användaretablering Igloo programvara.</span><span class="sxs-lookup"><span data-stu-id="07469-236">There is no action item for you to configure user provisioning to Igloo Software.</span></span>  

<span data-ttu-id="07469-237">När en tilldelad användare försöker logga in på Igloo programvara med hjälp av åtkomstpanelen kontrollerar Igloo programvara om användaren finns.</span><span class="sxs-lookup"><span data-stu-id="07469-237">When an assigned user tries to log in to Igloo Software using the access panel, Igloo Software checks whether the user exists.</span></span>  <span data-ttu-id="07469-238">Om det finns inget användarkonto ännu, skapas den automatiskt med Igloo program.</span><span class="sxs-lookup"><span data-stu-id="07469-238">If there is no user account available yet, it is automatically created by Igloo Software.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="07469-239">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="07469-239">Assigning the Azure AD test user</span></span>

<span data-ttu-id="07469-240">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Igloo programvara.</span><span class="sxs-lookup"><span data-stu-id="07469-240">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Igloo Software.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="07469-242">**Om du vill tilldela Igloo programvara Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="07469-242">**To assign Britta Simon to Igloo Software, perform the following steps:**</span></span>

1. <span data-ttu-id="07469-243">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="07469-243">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="07469-245">Välj i listan med program **Igloo programvara**.</span><span class="sxs-lookup"><span data-stu-id="07469-245">In the applications list, select **Igloo Software**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_app.png) 

3. <span data-ttu-id="07469-247">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="07469-247">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="07469-249">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="07469-249">Click **Add** button.</span></span> <span data-ttu-id="07469-250">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="07469-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="07469-252">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="07469-252">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="07469-253">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="07469-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="07469-254">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="07469-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="07469-255">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="07469-255">Testing single sign-on</span></span>

<span data-ttu-id="07469-256">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="07469-256">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="07469-257">När du klickar på panelen Igloo programvara på åtkomstpanelen du bör få automatiskt loggat in på Igloo programmet.</span><span class="sxs-lookup"><span data-stu-id="07469-257">When you click the Igloo Software tile in the Access Panel, you should get automatically signed-on to your Igloo Software application.</span></span>
<span data-ttu-id="07469-258">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="07469-258">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="07469-259">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="07469-259">Additional resources</span></span>

* [<span data-ttu-id="07469-260">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="07469-260">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="07469-261">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="07469-261">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_203.png

