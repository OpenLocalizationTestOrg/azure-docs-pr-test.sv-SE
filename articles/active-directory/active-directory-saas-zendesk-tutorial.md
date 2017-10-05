---
title: "Självstudier: Azure Active Directory-integrering med Zendesk | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Zendesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 51c06d838c5ed6286dfb99ea25faaaf33bad5f3c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a><span data-ttu-id="127bd-103">Självstudier: Azure Active Directory-integrering med Zendesk</span><span class="sxs-lookup"><span data-stu-id="127bd-103">Tutorial: Azure Active Directory integration with Zendesk</span></span>

<span data-ttu-id="127bd-104">I kursen får lära du att integrera Zendesk med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="127bd-104">In this tutorial, you learn how to integrate Zendesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="127bd-105">Integrera Zendesk med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="127bd-105">Integrating Zendesk with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="127bd-106">Du kan styra i Azure AD som har åtkomst till Zendesk</span><span class="sxs-lookup"><span data-stu-id="127bd-106">You can control in Azure AD who has access to Zendesk</span></span>
- <span data-ttu-id="127bd-107">Du kan aktivera användarna att automatiskt hämta loggat in på Zendesk (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="127bd-107">You can enable your users to automatically get signed-on to Zendesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="127bd-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="127bd-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="127bd-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="127bd-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="127bd-110">Krav</span><span class="sxs-lookup"><span data-stu-id="127bd-110">Prerequisites</span></span>

<span data-ttu-id="127bd-111">För att konfigurera Azure AD-integrering med Zendesk, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="127bd-111">To configure Azure AD integration with Zendesk, you need the following items:</span></span>

- <span data-ttu-id="127bd-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="127bd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="127bd-113">En Zendesk enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="127bd-113">A Zendesk single sign-on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="127bd-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="127bd-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="127bd-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="127bd-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="127bd-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="127bd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="127bd-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="127bd-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="127bd-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="127bd-118">Scenario description</span></span>
<span data-ttu-id="127bd-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="127bd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="127bd-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="127bd-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="127bd-121">Att lägga till Zendesk från galleriet</span><span class="sxs-lookup"><span data-stu-id="127bd-121">Adding Zendesk from the gallery</span></span>
2. <span data-ttu-id="127bd-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="127bd-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-zendesk-from-the-gallery"></a><span data-ttu-id="127bd-123">Att lägga till Zendesk från galleriet</span><span class="sxs-lookup"><span data-stu-id="127bd-123">Adding Zendesk from the gallery</span></span>
<span data-ttu-id="127bd-124">Du måste lägga till Zendesk från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Zendesk i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="127bd-124">To configure the integration of Zendesk into Azure AD, you need to add Zendesk from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="127bd-125">**Utför följande steg för att lägga till Zendesk från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="127bd-125">**To add Zendesk from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="127bd-126">I den  **[Azure Portal](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="127bd-126">In the **[Azure Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="127bd-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="127bd-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="127bd-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="127bd-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="127bd-131">Klicka på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="127bd-131">Click **New application** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="127bd-133">I sökrutan skriver **Zendesk**.</span><span class="sxs-lookup"><span data-stu-id="127bd-133">In the search box, type **Zendesk**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_search.png)

5. <span data-ttu-id="127bd-135">Välj i resultatpanelen **Zendesk**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="127bd-135">In the results panel, select **Zendesk**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="127bd-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="127bd-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="127bd-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Zendesk baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="127bd-138">In this section, you configure and test Azure AD single sign-on with Zendesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="127bd-139">Azure AD måste du känna till användaren i Zendesk motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="127bd-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Zendesk is to a user in Azure AD.</span></span> <span data-ttu-id="127bd-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Zendesk upprättas.</span><span class="sxs-lookup"><span data-stu-id="127bd-140">In other words, a link relationship between an Azure AD user and the related user in Zendesk needs to be established.</span></span>

<span data-ttu-id="127bd-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Zendesk.</span><span class="sxs-lookup"><span data-stu-id="127bd-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Zendesk.</span></span>

<span data-ttu-id="127bd-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Zendesk, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="127bd-142">To configure and test Azure AD single sign-on with Zendesk, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="127bd-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="127bd-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="127bd-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="127bd-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="127bd-145">**[Skapa en testanvändare Zendesk](#creating-a-zendesk-test-user)**  – du har en motsvarighet för Britta Simon i Zendesk som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="127bd-145">**[Creating a Zendesk test user](#creating-a-zendesk-test-user)** - to have a counterpart of Britta Simon in Zendesk that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="127bd-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="127bd-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="127bd-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="127bd-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="127bd-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="127bd-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="127bd-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Zendesk-program.</span><span class="sxs-lookup"><span data-stu-id="127bd-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Zendesk application.</span></span>

<span data-ttu-id="127bd-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Zendesk:**</span><span class="sxs-lookup"><span data-stu-id="127bd-150">**To configure Azure AD single sign-on with Zendesk, perform the following steps:**</span></span>

1. <span data-ttu-id="127bd-151">I Azure-portalen på den **Zendesk** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="127bd-151">In the Azure portal, on the **Zendesk** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="127bd-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="127bd-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. <span data-ttu-id="127bd-155">På den **Zendesk domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="127bd-155">On the **Zendesk Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_url.png)

    <span data-ttu-id="127bd-157">a.</span><span class="sxs-lookup"><span data-stu-id="127bd-157">a.</span></span> <span data-ttu-id="127bd-158">I den **inloggnings-URL** textruta Skriv det värde som använder följande mönster:`https://<subdomain>.zendesk.com`</span><span class="sxs-lookup"><span data-stu-id="127bd-158">In the **Sign-on URL** textbox, type the value using the following pattern: `https://<subdomain>.zendesk.com`</span></span>

    <span data-ttu-id="127bd-159">b.</span><span class="sxs-lookup"><span data-stu-id="127bd-159">b.</span></span> <span data-ttu-id="127bd-160">I den **identifierare** textruta Skriv det värde som använder följande mönster:`https://<subdomain>.zendesk.com`</span><span class="sxs-lookup"><span data-stu-id="127bd-160">In the **Identifier** textbox, type the value using the following pattern: `https://<subdomain>.zendesk.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="127bd-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="127bd-161">These values are not real.</span></span> <span data-ttu-id="127bd-162">Uppdatera dessa värden med den faktiska inloggnings-URL och Identifierare.</span><span class="sxs-lookup"><span data-stu-id="127bd-162">Update these values with the actual Sign-on URL and Identifier URL.</span></span> <span data-ttu-id="127bd-163">Kontakta [Zendesk supportteamet](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="127bd-163">Contact [Zendesk support team](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) to get these values.</span></span> 

4. <span data-ttu-id="127bd-164">Zendesk förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="127bd-164">Zendesk expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="127bd-165">Det finns inga obligatoriska SAML-attribut men om du vill kan du lägga till ett attribut från **användarattribut** avsnittet genom att följa de nedanstående steg:</span><span class="sxs-lookup"><span data-stu-id="127bd-165">There are no mandatory SAML attributes but optionally you can add an attribute from **User Attributes** section by following the below steps:</span></span> 

     ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes1.png)

    <span data-ttu-id="127bd-167">a.</span><span class="sxs-lookup"><span data-stu-id="127bd-167">a.</span></span> <span data-ttu-id="127bd-168">Klicka på den **visa och redigera alla andra attribut** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="127bd-168">Click the **View and edit all the other attributes** check box.</span></span>
     
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes2.png)
   
    <span data-ttu-id="127bd-170">b.</span><span class="sxs-lookup"><span data-stu-id="127bd-170">b.</span></span> <span data-ttu-id="127bd-171">Klicka på den **lägga till attributet** att öppna **Lägg till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="127bd-171">Click the **Add Attribute** to open **Add attribute** dialog.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="127bd-173">c.</span><span class="sxs-lookup"><span data-stu-id="127bd-173">c.</span></span> <span data-ttu-id="127bd-174">I den **namn** textruta ange attributets namn (till exempel **e-postadress**).</span><span class="sxs-lookup"><span data-stu-id="127bd-174">In the **Name** textbox, type the attribute name (for example **emailaddress**).</span></span>
    
    <span data-ttu-id="127bd-175">d.</span><span class="sxs-lookup"><span data-stu-id="127bd-175">d.</span></span> <span data-ttu-id="127bd-176">Från den **värdet** väljer attribut-värde (som **user.mail**).</span><span class="sxs-lookup"><span data-stu-id="127bd-176">From the **Value** list, select the attribute value (as **user.mail**).</span></span>
    
    <span data-ttu-id="127bd-177">e.</span><span class="sxs-lookup"><span data-stu-id="127bd-177">e.</span></span> <span data-ttu-id="127bd-178">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="127bd-178">Click **Ok**</span></span>
 
    > [!NOTE] 
    > <span data-ttu-id="127bd-179">Du kan använda Tilläggsattribut för att lägga till attribut som inte ingår i Azure AD som standard.</span><span class="sxs-lookup"><span data-stu-id="127bd-179">You use extension attributes to add attributes that are not in Azure AD by default.</span></span> <span data-ttu-id="127bd-180">Klicka på [användarattribut som kan anges i SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) för att hämta den fullständiga listan med SAML attribut som **Zendesk** accepterar.</span><span class="sxs-lookup"><span data-stu-id="127bd-180">Click [User attributes that can be set in SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) to get the complete list of SAML attributes that **Zendesk** accepts.</span></span> 

5. <span data-ttu-id="127bd-181">På den **SAML-signeringscertifikat** avsnittet, kopiera den **TUMAVTRYCKET** värdet för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="127bd-181">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_certificate.png) 

6. <span data-ttu-id="127bd-183">På den **Zendesk Configuration** klickar du på **konfigurera Zendesk** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="127bd-183">On the **Zendesk Configuration** section, click **Configure Zendesk** to open **Configure sign-on** window.</span></span> <span data-ttu-id="127bd-184">Kopiera den **Sign-Out Webbadressen och SAML enkel inloggning Service** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="127bd-184">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_configure.png) 

7. <span data-ttu-id="127bd-186">Logga in på webbplatsen Zendesk företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="127bd-186">In a different web browser window, log into your Zendesk company site as an administrator.</span></span>

8. <span data-ttu-id="127bd-187">Klicka på **Admin**.</span><span class="sxs-lookup"><span data-stu-id="127bd-187">Click **Admin**.</span></span>

9. <span data-ttu-id="127bd-188">I det vänstra navigeringsfönstret klickar du på **inställningar**, och klicka sedan på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="127bd-188">In the left navigation pane, click **Settings**, and then click **Security**.</span></span>

10. <span data-ttu-id="127bd-189">På den **säkerhet** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="127bd-189">On the **Security** page, perform the following steps:</span></span> 
   
     <span data-ttu-id="127bd-190">![Säkerhet](./media/active-directory-saas-zendesk-tutorial/ic773089.png "säkerhet")</span><span class="sxs-lookup"><span data-stu-id="127bd-190">![Security](./media/active-directory-saas-zendesk-tutorial/ic773089.png "Security")</span></span>

    <span data-ttu-id="127bd-191">![Enkel inloggning](./media/active-directory-saas-zendesk-tutorial/ic773090.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="127bd-191">![Single sign-on](./media/active-directory-saas-zendesk-tutorial/ic773090.png "Single sign-on")</span></span>

     <span data-ttu-id="127bd-192">a.</span><span class="sxs-lookup"><span data-stu-id="127bd-192">a.</span></span> <span data-ttu-id="127bd-193">Klicka på den **Admin & agenter** fliken.</span><span class="sxs-lookup"><span data-stu-id="127bd-193">Click the **Admin & Agents** tab.</span></span>

     <span data-ttu-id="127bd-194">b.</span><span class="sxs-lookup"><span data-stu-id="127bd-194">b.</span></span> <span data-ttu-id="127bd-195">Välj **enkel inloggning (SSO) och SAML**, och välj sedan **SAML**.</span><span class="sxs-lookup"><span data-stu-id="127bd-195">Select **Single sign-on (SSO) and SAML**, and then select **SAML**.</span></span>

     <span data-ttu-id="127bd-196">c.</span><span class="sxs-lookup"><span data-stu-id="127bd-196">c.</span></span> <span data-ttu-id="127bd-197">I **SAML SSO URL** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="127bd-197">In **SAML SSO URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 

     <span data-ttu-id="127bd-198">d.</span><span class="sxs-lookup"><span data-stu-id="127bd-198">d.</span></span> <span data-ttu-id="127bd-199">I **Remote logga ut URL** textruta klistra in värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="127bd-199">In **Remote Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
        
     <span data-ttu-id="127bd-200">e.</span><span class="sxs-lookup"><span data-stu-id="127bd-200">e.</span></span> <span data-ttu-id="127bd-201">I **certifikat fingeravtryck** textruta klistra in den **tumavtrycket** värdet för certifikat som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="127bd-201">In **Certificate Fingerprint** textbox, paste the **Thumbprint** value of certificate which you have copied from Azure portal.</span></span>
     
     <span data-ttu-id="127bd-202">f.</span><span class="sxs-lookup"><span data-stu-id="127bd-202">f.</span></span> <span data-ttu-id="127bd-203">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="127bd-203">Click **Save**.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="127bd-204">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="127bd-204">Creating an Azure AD test user</span></span>
<span data-ttu-id="127bd-205">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="127bd-205">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="127bd-207">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="127bd-207">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="127bd-208">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="127bd-208">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="127bd-210">Om du vill visa en lista över användare går du till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="127bd-210">To display the list of users go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="127bd-212">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="127bd-212">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="127bd-214">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="127bd-214">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="127bd-216">a.</span><span class="sxs-lookup"><span data-stu-id="127bd-216">a.</span></span> <span data-ttu-id="127bd-217">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="127bd-217">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="127bd-218">b.</span><span class="sxs-lookup"><span data-stu-id="127bd-218">b.</span></span> <span data-ttu-id="127bd-219">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="127bd-219">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="127bd-220">c.</span><span class="sxs-lookup"><span data-stu-id="127bd-220">c.</span></span> <span data-ttu-id="127bd-221">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="127bd-221">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="127bd-222">d.</span><span class="sxs-lookup"><span data-stu-id="127bd-222">d.</span></span> <span data-ttu-id="127bd-223">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="127bd-223">Click **Create**.</span></span> 

### <a name="creating-a-zendesk-test-user"></a><span data-ttu-id="127bd-224">Skapa en testanvändare Zendesk</span><span class="sxs-lookup"><span data-stu-id="127bd-224">Creating a Zendesk test user</span></span>

<span data-ttu-id="127bd-225">Aktivera Azure AD-användare att logga in på **Zendesk**, de måste etableras i **Zendesk**.</span><span class="sxs-lookup"><span data-stu-id="127bd-225">To enable Azure AD users to log into **Zendesk**, they must be provisioned into **Zendesk**.</span></span>  
<span data-ttu-id="127bd-226">Beroende på vilken roll som tilldelats i apparna, är det förväntat beteende:</span><span class="sxs-lookup"><span data-stu-id="127bd-226">Depending on the role assigned in the apps, it's the expected behavior:</span></span>

 1. <span data-ttu-id="127bd-227">**Slutanvändarens** konton tillhandahålls automatiskt när du loggar in.</span><span class="sxs-lookup"><span data-stu-id="127bd-227">**End-user** accounts are automatically provisioned when signing in.</span></span>
 2. <span data-ttu-id="127bd-228">**Agenten** och **Admin** konton måste tillhandahållas manuellt i **Zendesk** innan du loggar in.</span><span class="sxs-lookup"><span data-stu-id="127bd-228">**Agent** and **Admin** accounts need to be manually provisioned in **Zendesk** before signing in.</span></span>
 
<span data-ttu-id="127bd-229">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="127bd-229">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="127bd-230">Logga in på ditt **Zendesk** klient.</span><span class="sxs-lookup"><span data-stu-id="127bd-230">Log in to your **Zendesk** tenant.</span></span>

2. <span data-ttu-id="127bd-231">Välj den **lista över kunder** fliken.</span><span class="sxs-lookup"><span data-stu-id="127bd-231">Select the **Customer List** tab.</span></span>

3. <span data-ttu-id="127bd-232">Välj den **användare** och på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="127bd-232">Select the **User** tab, and click **Add**.</span></span>
   
    <span data-ttu-id="127bd-233">![Lägg till användare](./media/active-directory-saas-zendesk-tutorial/ic773632.png "Lägg till användare")</span><span class="sxs-lookup"><span data-stu-id="127bd-233">![Add user](./media/active-directory-saas-zendesk-tutorial/ic773632.png "Add user")</span></span>
4. <span data-ttu-id="127bd-234">Skriv e-postadress för ett befintligt Azure AD-konto du vill etablera och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="127bd-234">Type the email address of an existing Azure AD account you want to provision, and then click **Save**.</span></span>
   
    <span data-ttu-id="127bd-235">![Ny användare](./media/active-directory-saas-zendesk-tutorial/ic773633.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="127bd-235">![New user](./media/active-directory-saas-zendesk-tutorial/ic773633.png "New user")</span></span>

> [!NOTE]
> <span data-ttu-id="127bd-236">Du kan använda något annat Zendesk användarens konto skapas verktyg eller API: er fås från Zendesk till etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="127bd-236">You can use any other Zendesk user account creation tools or APIs provided by Zendesk to provision AAD user accounts.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="127bd-237">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="127bd-237">Assigning the Azure AD test user</span></span>

<span data-ttu-id="127bd-238">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Zendesk.</span><span class="sxs-lookup"><span data-stu-id="127bd-238">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Zendesk.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="127bd-240">**Om du vill tilldela Zendesk Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="127bd-240">**To assign Britta Simon to Zendesk, perform the following steps:**</span></span>

1. <span data-ttu-id="127bd-241">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="127bd-241">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="127bd-243">Välj i listan med program **Zendesk**.</span><span class="sxs-lookup"><span data-stu-id="127bd-243">In the applications list, select **Zendesk**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_app.png) 

3. <span data-ttu-id="127bd-245">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="127bd-245">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="127bd-247">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="127bd-247">Click **Add** button.</span></span> <span data-ttu-id="127bd-248">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="127bd-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="127bd-250">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="127bd-250">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="127bd-251">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="127bd-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="127bd-252">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="127bd-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="127bd-253">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="127bd-253">Testing single sign-on</span></span>

<span data-ttu-id="127bd-254">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="127bd-254">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="127bd-255">När du klickar på panelen Zendesk på åtkomstpanelen du bör få automatiskt loggat in på ditt Zendesk-program.</span><span class="sxs-lookup"><span data-stu-id="127bd-255">When you click the Zendesk tile in the Access Panel, you should get automatically signed-on to your Zendesk application.</span></span>
<span data-ttu-id="127bd-256">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="127bd-256">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="127bd-257">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="127bd-257">Additional resources</span></span>

* [<span data-ttu-id="127bd-258">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="127bd-258">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="127bd-259">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="127bd-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_203.png
