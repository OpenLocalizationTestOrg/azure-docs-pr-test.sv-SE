---
title: "Självstudier: Azure Active Directory-integrering med Gigya | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Gigya."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2c7d200b-9242-44a5-ac8a-ab3214a78e41
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: b65a33989a045a3e0b57fda522a9bc3b0770c7f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-gigya"></a><span data-ttu-id="e7838-103">Självstudier: Azure Active Directory-integrering med Gigya</span><span class="sxs-lookup"><span data-stu-id="e7838-103">Tutorial: Azure Active Directory integration with Gigya</span></span>

<span data-ttu-id="e7838-104">I kursen får lära du att integrera Gigya med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e7838-104">In this tutorial, you learn how to integrate Gigya with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e7838-105">Integrera Gigya med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e7838-105">Integrating Gigya with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e7838-106">Du kan styra i Azure AD som har åtkomst till Gigya</span><span class="sxs-lookup"><span data-stu-id="e7838-106">You can control in Azure AD who has access to Gigya</span></span>
- <span data-ttu-id="e7838-107">Du kan aktivera användarna att automatiskt hämta loggat in på Gigya (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e7838-107">You can enable your users to automatically get signed-on to Gigya (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e7838-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e7838-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e7838-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e7838-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e7838-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e7838-110">Prerequisites</span></span>

<span data-ttu-id="e7838-111">För att konfigurera Azure AD-integrering med Gigya, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="e7838-111">To configure Azure AD integration with Gigya, you need the following items:</span></span>

- <span data-ttu-id="e7838-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e7838-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e7838-113">En Gigya enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="e7838-113">A Gigya single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e7838-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e7838-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e7838-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e7838-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e7838-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e7838-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e7838-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e7838-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e7838-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e7838-118">Scenario description</span></span>
<span data-ttu-id="e7838-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e7838-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e7838-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e7838-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e7838-121">Att lägga till Gigya från galleriet</span><span class="sxs-lookup"><span data-stu-id="e7838-121">Adding Gigya from the gallery</span></span>
2. <span data-ttu-id="e7838-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e7838-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-gigya-from-the-gallery"></a><span data-ttu-id="e7838-123">Att lägga till Gigya från galleriet</span><span class="sxs-lookup"><span data-stu-id="e7838-123">Adding Gigya from the gallery</span></span>
<span data-ttu-id="e7838-124">Du måste lägga till Gigya från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Gigya i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e7838-124">To configure the integration of Gigya into Azure AD, you need to add Gigya from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e7838-125">**Utför följande steg för att lägga till Gigya från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="e7838-125">**To add Gigya from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e7838-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e7838-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e7838-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e7838-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e7838-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e7838-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="e7838-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e7838-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="e7838-133">I sökrutan skriver **Gigya**.</span><span class="sxs-lookup"><span data-stu-id="e7838-133">In the search box, type **Gigya**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_search.png)

5. <span data-ttu-id="e7838-135">Välj i resultatpanelen **Gigya**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="e7838-135">In the results panel, select **Gigya**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e7838-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e7838-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e7838-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Gigya baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e7838-138">In this section, you configure and test Azure AD single sign-on with Gigya based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e7838-139">Azure AD måste du känna till användaren i Gigya motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="e7838-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Gigya is to a user in Azure AD.</span></span> <span data-ttu-id="e7838-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Gigya upprättas.</span><span class="sxs-lookup"><span data-stu-id="e7838-140">In other words, a link relationship between an Azure AD user and the related user in Gigya needs to be established.</span></span>

<span data-ttu-id="e7838-141">I Gigya, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="e7838-141">In Gigya, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e7838-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Gigya, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e7838-142">To configure and test Azure AD single sign-on with Gigya, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e7838-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e7838-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e7838-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e7838-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e7838-145">**[Skapa en testanvändare Gigya](#creating-a-gigya-test-user)**  – du har en motsvarighet för Britta Simon i Gigya som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="e7838-145">**[Creating a Gigya test user](#creating-a-gigya-test-user)** - to have a counterpart of Britta Simon in Gigya that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e7838-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e7838-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e7838-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e7838-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e7838-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e7838-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e7838-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Gigya program.</span><span class="sxs-lookup"><span data-stu-id="e7838-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Gigya application.</span></span>

<span data-ttu-id="e7838-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Gigya:**</span><span class="sxs-lookup"><span data-stu-id="e7838-150">**To configure Azure AD single sign-on with Gigya, perform the following steps:**</span></span>

1. <span data-ttu-id="e7838-151">I Azure-portalen på den **Gigya** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e7838-151">In the Azure portal, on the **Gigya** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="e7838-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e7838-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_samlbase.png)

3. <span data-ttu-id="e7838-155">På den **Gigya domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e7838-155">On the **Gigya Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_url.png)

    <span data-ttu-id="e7838-157">a.</span><span class="sxs-lookup"><span data-stu-id="e7838-157">a.</span></span> <span data-ttu-id="e7838-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`http://<companyname>.gigya.com`</span><span class="sxs-lookup"><span data-stu-id="e7838-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `http://<companyname>.gigya.com`</span></span>

    <span data-ttu-id="e7838-159">b.</span><span class="sxs-lookup"><span data-stu-id="e7838-159">b.</span></span> <span data-ttu-id="e7838-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://fidm.gigya.com/saml/v2.0/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="e7838-160">In the **Identifier** textbox, type a URL using the following pattern: `https://fidm.gigya.com/saml/v2.0/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e7838-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="e7838-161">These values are not real.</span></span> <span data-ttu-id="e7838-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="e7838-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="e7838-163">Kontakta [Gigya klienten supportteamet](https://www.gigya.com/support-policy/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="e7838-163">Contact [Gigya Client support team](https://www.gigya.com/support-policy/) to get these values.</span></span> 
 
4. <span data-ttu-id="e7838-164">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="e7838-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_certificate.png) 

5. <span data-ttu-id="e7838-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e7838-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-gigya-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e7838-168">På den **Gigya Configuration** klickar du på **konfigurera Gigya** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="e7838-168">On the **Gigya Configuration** section, click **Configure Gigya** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e7838-169">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="e7838-169">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_configure.png) 

7. <span data-ttu-id="e7838-171">Logga in på webbplatsen Gigya företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="e7838-171">In a different web browser window, log into your Gigya company site as an administrator.</span></span>

8. <span data-ttu-id="e7838-172">Gå till **inställningar \> SAML inloggningen**, och klicka sedan på den **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e7838-172">Go to **Settings \> SAML Login**, and then click the **Add** button.</span></span>
   
    <span data-ttu-id="e7838-173">![SAML-inloggningen](./media/active-directory-saas-gigya-tutorial/ic789532.png "SAML-inloggning")</span><span class="sxs-lookup"><span data-stu-id="e7838-173">![SAML Login](./media/active-directory-saas-gigya-tutorial/ic789532.png "SAML Login")</span></span>

9. <span data-ttu-id="e7838-174">I den **SAML inloggningen** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e7838-174">In the **SAML Login** section, perform the following steps:</span></span>
   
    <span data-ttu-id="e7838-175">![SAML-konfiguration](./media/active-directory-saas-gigya-tutorial/ic789533.png "SAML-konfiguration")</span><span class="sxs-lookup"><span data-stu-id="e7838-175">![SAML Configuration](./media/active-directory-saas-gigya-tutorial/ic789533.png "SAML Configuration")</span></span>
   
    <span data-ttu-id="e7838-176">a.</span><span class="sxs-lookup"><span data-stu-id="e7838-176">a.</span></span> <span data-ttu-id="e7838-177">I den **namn** textruta, ange ett namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="e7838-177">In the **Name** textbox, type a name for your configuration.</span></span>
   
    <span data-ttu-id="e7838-178">b.</span><span class="sxs-lookup"><span data-stu-id="e7838-178">b.</span></span> <span data-ttu-id="e7838-179">I **utfärdaren** textruta klistra in värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e7838-179">In **Issuer** textbox, paste the value of **SAML Entity ID** which you have copied from Azure Portal.</span></span> 
   
    <span data-ttu-id="e7838-180">c.</span><span class="sxs-lookup"><span data-stu-id="e7838-180">c.</span></span> <span data-ttu-id="e7838-181">I **inloggning tjänst-URL för enkel** textruta klistra in värdet för **inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e7838-181">In **Single Sign-On Service URL** textbox, paste the value of **Single Sign-On Service URL** which you have copied from Azure Portal.</span></span>
   
    <span data-ttu-id="e7838-182">d.</span><span class="sxs-lookup"><span data-stu-id="e7838-182">d.</span></span> <span data-ttu-id="e7838-183">I **Format för namn-ID** textruta klistra in värdet för **identifierare namnformat** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e7838-183">In **Name ID Format** textbox, paste the value of **Name Identifier Format** which you have copied from Azure Portal.</span></span>
   
    <span data-ttu-id="e7838-184">e.</span><span class="sxs-lookup"><span data-stu-id="e7838-184">e.</span></span> <span data-ttu-id="e7838-185">Öppna din Base64-kodade certifikatet i anteckningar som hämtas från Azure-portalen, kopiera innehållet i den till Urklipp och klistra in den till den **X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="e7838-185">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox.</span></span>
   
    <span data-ttu-id="e7838-186">f.</span><span class="sxs-lookup"><span data-stu-id="e7838-186">f.</span></span> <span data-ttu-id="e7838-187">Klicka på **Spara inställningar**.</span><span class="sxs-lookup"><span data-stu-id="e7838-187">Click **Save Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="e7838-188">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="e7838-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e7838-189">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="e7838-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e7838-190">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e7838-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e7838-191">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e7838-191">Creating an Azure AD test user</span></span>

<span data-ttu-id="e7838-192">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e7838-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="e7838-194">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="e7838-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e7838-195">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e7838-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-gigya-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e7838-197">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e7838-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-gigya-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e7838-199">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e7838-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-gigya-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e7838-201">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e7838-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-gigya-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e7838-203">a.</span><span class="sxs-lookup"><span data-stu-id="e7838-203">a.</span></span> <span data-ttu-id="e7838-204">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e7838-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e7838-205">b.</span><span class="sxs-lookup"><span data-stu-id="e7838-205">b.</span></span> <span data-ttu-id="e7838-206">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e7838-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e7838-207">c.</span><span class="sxs-lookup"><span data-stu-id="e7838-207">c.</span></span> <span data-ttu-id="e7838-208">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e7838-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e7838-209">d.</span><span class="sxs-lookup"><span data-stu-id="e7838-209">d.</span></span> <span data-ttu-id="e7838-210">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e7838-210">Click **Create**.</span></span>
 
### <a name="creating-a-gigya-test-user"></a><span data-ttu-id="e7838-211">Skapa en testanvändare Gigya</span><span class="sxs-lookup"><span data-stu-id="e7838-211">Creating a Gigya test user</span></span>

<span data-ttu-id="e7838-212">För att aktivera Azure AD-användare att logga in på Gigya etableras de i Gigya.</span><span class="sxs-lookup"><span data-stu-id="e7838-212">In order to enable Azure AD users to log into Gigya, they must be provisioned into Gigya.</span></span>  
<span data-ttu-id="e7838-213">När det gäller Gigya är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="e7838-213">In the case of Gigya, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a><span data-ttu-id="e7838-214">Utför följande steg för att etablera en användarkonton:</span><span class="sxs-lookup"><span data-stu-id="e7838-214">To provision a user accounts, perform the following steps:</span></span>

1. <span data-ttu-id="e7838-215">Logga in på ditt **Gigya** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="e7838-215">Log in to your **Gigya** company site as an administrator.</span></span>

2. <span data-ttu-id="e7838-216">Gå till **Admin \> hantera användare**, och klicka sedan på **bjuda in användare**.</span><span class="sxs-lookup"><span data-stu-id="e7838-216">Go to **Admin \> Manage Users**, and then click **Invite Users**.</span></span>
   
    <span data-ttu-id="e7838-217">![Hantera användare](./media/active-directory-saas-gigya-tutorial/ic789535.png "hantera användare")</span><span class="sxs-lookup"><span data-stu-id="e7838-217">![Manage Users](./media/active-directory-saas-gigya-tutorial/ic789535.png "Manage Users")</span></span>

3. <span data-ttu-id="e7838-218">I dialogrutan bjuda in användare utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="e7838-218">On the Invite Users dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="e7838-219">![Bjuda in användare](./media/active-directory-saas-gigya-tutorial/ic789536.png "bjuda in användare")</span><span class="sxs-lookup"><span data-stu-id="e7838-219">![Invite Users](./media/active-directory-saas-gigya-tutorial/ic789536.png "Invite Users")</span></span>
   
    <span data-ttu-id="e7838-220">a.</span><span class="sxs-lookup"><span data-stu-id="e7838-220">a.</span></span> <span data-ttu-id="e7838-221">I den **e-post** textruta skriver du e postalias för ett giltigt Azure Active Directory-konto som du vill etablera.</span><span class="sxs-lookup"><span data-stu-id="e7838-221">In the **Email** textbox, type the email alias of a valid Azure Active Directory account you want to provision.</span></span>
    
    <span data-ttu-id="e7838-222">b.</span><span class="sxs-lookup"><span data-stu-id="e7838-222">b.</span></span> <span data-ttu-id="e7838-223">Klicka på **bjuda in användare**.</span><span class="sxs-lookup"><span data-stu-id="e7838-223">Click **Invite User**.</span></span>
      
    > [!NOTE]
    > <span data-ttu-id="e7838-224">Azure Active Directory kontoinnehavaren får ett e-postmeddelande som innehåller en länk för att bekräfta kontot innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="e7838-224">The Azure Active Directory account holder will receive an email that includes a link to confirm the account before it becomes active.</span></span>
    > 
    

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e7838-225">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="e7838-225">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e7838-226">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Gigya.</span><span class="sxs-lookup"><span data-stu-id="e7838-226">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Gigya.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="e7838-228">**Om du vill tilldela Gigya Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e7838-228">**To assign Britta Simon to Gigya, perform the following steps:**</span></span>

1. <span data-ttu-id="e7838-229">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e7838-229">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e7838-231">Välj i listan med program **Gigya**.</span><span class="sxs-lookup"><span data-stu-id="e7838-231">In the applications list, select **Gigya**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-gigya-tutorial/tutorial_gigya_app.png) 

3. <span data-ttu-id="e7838-233">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e7838-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="e7838-235">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e7838-235">Click **Add** button.</span></span> <span data-ttu-id="e7838-236">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e7838-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="e7838-238">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="e7838-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e7838-239">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e7838-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e7838-240">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e7838-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e7838-241">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e7838-241">Testing single sign-on</span></span>

<span data-ttu-id="e7838-242">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e7838-242">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="e7838-243">När du klickar på panelen Gigya på åtkomstpanelen du bör få automatiskt loggat in på ditt Gigya program.</span><span class="sxs-lookup"><span data-stu-id="e7838-243">When you click the Gigya tile in the Access Panel, you should get automatically signed-on to your Gigya application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e7838-244">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e7838-244">Additional resources</span></span>

* [<span data-ttu-id="e7838-245">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e7838-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e7838-246">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e7838-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-gigya-tutorial/tutorial_general_203.png

