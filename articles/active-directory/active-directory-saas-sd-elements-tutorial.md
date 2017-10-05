---
title: "Självstudier: Azure Active Directory-integrering med SD element | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och SD-element."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 624eff0a0da8f548877e4a4346b21df89cd37b67
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sd-elements"></a><span data-ttu-id="2fc37-103">Självstudier: Azure Active Directory-integrering med SD-element</span><span class="sxs-lookup"><span data-stu-id="2fc37-103">Tutorial: Azure Active Directory integration with SD Elements</span></span>

<span data-ttu-id="2fc37-104">I kursen får lära du att integrera SD-element med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="2fc37-104">In this tutorial, you learn how to integrate SD Elements with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2fc37-105">Integrera SD-element med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="2fc37-105">Integrating SD Elements with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2fc37-106">Du kan styra i Azure AD som har åtkomst till SD-element</span><span class="sxs-lookup"><span data-stu-id="2fc37-106">You can control in Azure AD who has access to SD Elements</span></span>
- <span data-ttu-id="2fc37-107">Du kan aktivera användarna att automatiskt hämta loggat in på SD-element (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="2fc37-107">You can enable your users to automatically get signed-on to SD Elements (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2fc37-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2fc37-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="2fc37-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2fc37-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2fc37-110">Krav</span><span class="sxs-lookup"><span data-stu-id="2fc37-110">Prerequisites</span></span>

<span data-ttu-id="2fc37-111">Om du vill konfigurera Azure AD-integrering med SD-element behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="2fc37-111">To configure Azure AD integration with SD Elements, you need the following items:</span></span>

- <span data-ttu-id="2fc37-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="2fc37-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2fc37-113">Ett SD-element enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="2fc37-113">A SD Elements single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2fc37-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="2fc37-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2fc37-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="2fc37-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2fc37-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="2fc37-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2fc37-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2fc37-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2fc37-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="2fc37-118">Scenario description</span></span>
<span data-ttu-id="2fc37-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="2fc37-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2fc37-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="2fc37-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2fc37-121">Att lägga till SD-element från galleriet</span><span class="sxs-lookup"><span data-stu-id="2fc37-121">Adding SD Elements from the gallery</span></span>
2. <span data-ttu-id="2fc37-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2fc37-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sd-elements-from-the-gallery"></a><span data-ttu-id="2fc37-123">Att lägga till SD-element från galleriet</span><span class="sxs-lookup"><span data-stu-id="2fc37-123">Adding SD Elements from the gallery</span></span>
<span data-ttu-id="2fc37-124">Du måste lägga till SD-element från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av SD-element i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2fc37-124">To configure the integration of SD Elements into Azure AD, you need to add SD Elements from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2fc37-125">**Utför följande steg för att lägga till SD-element från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="2fc37-125">**To add SD Elements from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2fc37-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2fc37-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2fc37-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="2fc37-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="2fc37-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2fc37-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="2fc37-133">I sökrutan skriver **SD element**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-133">In the search box, type **SD Elements**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_search.png)

5. <span data-ttu-id="2fc37-135">Välj i resultatpanelen **SD element**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="2fc37-135">In the results panel, select **SD Elements**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2fc37-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2fc37-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2fc37-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med SD-element som baseras på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="2fc37-138">In this section, you configure and test Azure AD single sign-on with SD Elements based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2fc37-139">Azure AD måste du känna till motsvarande användaren i SD-element för en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="2fc37-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SD Elements is to a user in Azure AD.</span></span> <span data-ttu-id="2fc37-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i SD-element upprättas.</span><span class="sxs-lookup"><span data-stu-id="2fc37-140">In other words, a link relationship between an Azure AD user and the related user in SD Elements needs to be established.</span></span>

<span data-ttu-id="2fc37-141">I SD-element, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="2fc37-141">In SD Elements, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="2fc37-142">Om du vill konfigurera och testa Azure AD enkel inloggning med SD-element, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="2fc37-142">To configure and test Azure AD single sign-on with SD Elements, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2fc37-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="2fc37-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2fc37-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2fc37-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2fc37-145">**[Skapa en testanvändare SD element](#creating-a-sd-elements-test-user)**  – har en motsvarighet för Britta Simon SD-element som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="2fc37-145">**[Creating a SD Elements test user](#creating-a-sd-elements-test-user)** - to have a counterpart of Britta Simon in SD Elements that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="2fc37-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2fc37-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2fc37-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="2fc37-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2fc37-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2fc37-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2fc37-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt program för SD-element.</span><span class="sxs-lookup"><span data-stu-id="2fc37-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SD Elements application.</span></span>

<span data-ttu-id="2fc37-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med SD-element:**</span><span class="sxs-lookup"><span data-stu-id="2fc37-150">**To configure Azure AD single sign-on with SD Elements, perform the following steps:**</span></span>

1. <span data-ttu-id="2fc37-151">I Azure-portalen på den **SD element** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-151">In the Azure portal, on the **SD Elements** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="2fc37-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2fc37-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_samlbase.png)

3. <span data-ttu-id="2fc37-155">På den **SD element domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2fc37-155">On the **SD Elements Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_url.png)

    <span data-ttu-id="2fc37-157">a.</span><span class="sxs-lookup"><span data-stu-id="2fc37-157">a.</span></span> <span data-ttu-id="2fc37-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<tenantname>.sdelements.com/sso/saml2/metadata`</span><span class="sxs-lookup"><span data-stu-id="2fc37-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenantname>.sdelements.com/sso/saml2/metadata`</span></span>

    <span data-ttu-id="2fc37-159">b.</span><span class="sxs-lookup"><span data-stu-id="2fc37-159">b.</span></span> <span data-ttu-id="2fc37-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<tenantname>.sdelements.com/sso/saml2/acs/`</span><span class="sxs-lookup"><span data-stu-id="2fc37-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<tenantname>.sdelements.com/sso/saml2/acs/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2fc37-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="2fc37-161">These values are not real.</span></span> <span data-ttu-id="2fc37-162">Uppdatera dessa värden med den faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="2fc37-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="2fc37-163">Kontakta [SD element supportteam](mailto:support@sdelements.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="2fc37-163">Contact [SD Elements support team](mailto:support@sdelements.com) to get these values.</span></span>

4. <span data-ttu-id="2fc37-164">Programmet SD-element förväntas SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="2fc37-164">SD Elements application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="2fc37-165">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="2fc37-165">Configure the following claims for this application.</span></span> <span data-ttu-id="2fc37-166">Du kan hantera värden för attributen från den **”användarattribut”** för programmet.</span><span class="sxs-lookup"><span data-stu-id="2fc37-166">You can manage the values of these attributes from the **"User Attribute"** tab of the application.</span></span> <span data-ttu-id="2fc37-167">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="2fc37-167">The following screenshot shows an example for this.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_attribute.png)

5. <span data-ttu-id="2fc37-169">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2fc37-169">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span> 

    | <span data-ttu-id="2fc37-170">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="2fc37-170">Attribute Name</span></span> | <span data-ttu-id="2fc37-171">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="2fc37-171">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="2fc37-172">E-post</span><span class="sxs-lookup"><span data-stu-id="2fc37-172">email</span></span> |<span data-ttu-id="2fc37-173">User.Mail</span><span class="sxs-lookup"><span data-stu-id="2fc37-173">user.mail</span></span> |
    | <span data-ttu-id="2fc37-174">Förnamn</span><span class="sxs-lookup"><span data-stu-id="2fc37-174">firstname</span></span> |<span data-ttu-id="2fc37-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="2fc37-175">user.givenname</span></span> |
    | <span data-ttu-id="2fc37-176">Efternamn</span><span class="sxs-lookup"><span data-stu-id="2fc37-176">lastname</span></span> |<span data-ttu-id="2fc37-177">User.surname</span><span class="sxs-lookup"><span data-stu-id="2fc37-177">user.surname</span></span> |

    <span data-ttu-id="2fc37-178">a.</span><span class="sxs-lookup"><span data-stu-id="2fc37-178">a.</span></span> <span data-ttu-id="2fc37-179">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2fc37-179">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_05.png)

    <span data-ttu-id="2fc37-182">b.</span><span class="sxs-lookup"><span data-stu-id="2fc37-182">b.</span></span> <span data-ttu-id="2fc37-183">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="2fc37-183">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="2fc37-184">c.</span><span class="sxs-lookup"><span data-stu-id="2fc37-184">c.</span></span> <span data-ttu-id="2fc37-185">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="2fc37-185">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="2fc37-186">d.</span><span class="sxs-lookup"><span data-stu-id="2fc37-186">d.</span></span> <span data-ttu-id="2fc37-187">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-187">Click **Ok**.</span></span>
 
6. <span data-ttu-id="2fc37-188">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="2fc37-188">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_certificate.png) 

7. <span data-ttu-id="2fc37-190">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="2fc37-190">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="2fc37-192">På den **SD element Configuration** klickar du på **konfigurera SD-element** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="2fc37-192">On the **SD Elements Configuration** section, click **Configure SD Elements** to open **Configure sign-on** window.</span></span> <span data-ttu-id="2fc37-193">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="2fc37-193">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_configure.png)

9. <span data-ttu-id="2fc37-195">För att få enkel inloggning aktiverad, kontakta din [SD element supportteam](mailto:support@sdelements.com) och ge dem hämtade certifikatfilen.</span><span class="sxs-lookup"><span data-stu-id="2fc37-195">To get single sign-on enabled, contact your [SD Elements support team](mailto:support@sdelements.com) and provide them with the downloaded certificate file.</span></span> 

10. <span data-ttu-id="2fc37-196">I ett annat webbläsarfönster inloggning till SD-element-klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="2fc37-196">In a different browser window, sign-on to your SD Elements tenant as an administrator.</span></span>

11. <span data-ttu-id="2fc37-197">Klicka på menyn högst upp **System**, och sedan **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-197">In the menu on the top, click **System**, and then **Single Sign-on**.</span></span> 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_09.png) 

12. <span data-ttu-id="2fc37-199">På den **inställningar för enkel inloggning** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2fc37-199">On the **Single Sign-On Settings** dialog, perform the following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_10.png) 
   
    <span data-ttu-id="2fc37-201">a.</span><span class="sxs-lookup"><span data-stu-id="2fc37-201">a.</span></span> <span data-ttu-id="2fc37-202">Som **SSO typen**väljer **SAML**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-202">As **SSO Type**, select **SAML**.</span></span>
   
    <span data-ttu-id="2fc37-203">b.</span><span class="sxs-lookup"><span data-stu-id="2fc37-203">b.</span></span> <span data-ttu-id="2fc37-204">I den **identitet providern enhets-ID** textruta klistra in värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2fc37-204">In the **Identity Provider Entity ID** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 
   
    <span data-ttu-id="2fc37-205">c.</span><span class="sxs-lookup"><span data-stu-id="2fc37-205">c.</span></span> <span data-ttu-id="2fc37-206">I den **identitet providern enkel inloggning** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2fc37-206">In the **Identity Provider Single Sign-On Service** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span> 
   
    <span data-ttu-id="2fc37-207">d.</span><span class="sxs-lookup"><span data-stu-id="2fc37-207">d.</span></span> <span data-ttu-id="2fc37-208">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-208">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="2fc37-209">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="2fc37-209">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="2fc37-210">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="2fc37-210">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="2fc37-211">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2fc37-211">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2fc37-212">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="2fc37-212">Creating an Azure AD test user</span></span>
<span data-ttu-id="2fc37-213">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2fc37-213">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="2fc37-215">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="2fc37-215">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2fc37-216">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2fc37-216">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2fc37-218">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-218">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2fc37-220">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2fc37-220">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2fc37-222">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2fc37-222">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2fc37-224">a.</span><span class="sxs-lookup"><span data-stu-id="2fc37-224">a.</span></span> <span data-ttu-id="2fc37-225">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-225">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2fc37-226">b.</span><span class="sxs-lookup"><span data-stu-id="2fc37-226">b.</span></span> <span data-ttu-id="2fc37-227">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2fc37-227">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2fc37-228">c.</span><span class="sxs-lookup"><span data-stu-id="2fc37-228">c.</span></span> <span data-ttu-id="2fc37-229">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-229">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="2fc37-230">d.</span><span class="sxs-lookup"><span data-stu-id="2fc37-230">d.</span></span> <span data-ttu-id="2fc37-231">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-231">Click **Create**.</span></span>
 
### <a name="creating-a-sd-elements-test-user"></a><span data-ttu-id="2fc37-232">Skapa en testanvändare SD-element</span><span class="sxs-lookup"><span data-stu-id="2fc37-232">Creating a SD Elements test user</span></span>

<span data-ttu-id="2fc37-233">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i SD-element.</span><span class="sxs-lookup"><span data-stu-id="2fc37-233">The objective of this section is to create a user called Britta Simon in SD Elements.</span></span> <span data-ttu-id="2fc37-234">SD-element är skapa SD element användare en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="2fc37-234">In the case of SD Elements, creating SD Elements users is a manual task.</span></span>

<span data-ttu-id="2fc37-235">**Utför följande steg för att skapa Britta Simon i SD-element:**</span><span class="sxs-lookup"><span data-stu-id="2fc37-235">**To create Britta Simon in SD Elements, perform the following steps:**</span></span>

1. <span data-ttu-id="2fc37-236">I ett webbläsarfönster inloggning till webbplatsen för företagets SD-element som administratör.</span><span class="sxs-lookup"><span data-stu-id="2fc37-236">In a web browser window, sign-on to your SD Elements company site as an administrator.</span></span>

2. <span data-ttu-id="2fc37-237">Klicka på menyn högst upp **Användarhantering**, och sedan **användare**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-237">In the menu on the top, click **User Management**, and then **Users**.</span></span>
   
    ![Skapa en testanvändare SD-element](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_11.png) 

3. <span data-ttu-id="2fc37-239">Klicka på **lägga till nya användare**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-239">Click **Add New User**.</span></span>
   
    ![Skapa en testanvändare SD-element](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_12.png)
 
4. <span data-ttu-id="2fc37-241">På den **Lägg till nya användare** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2fc37-241">On the **Add New User** dialog, perform the following steps:</span></span>
   
    ![Skapa en testanvändare SD-element](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_13.png) 
   
    <span data-ttu-id="2fc37-243">a.</span><span class="sxs-lookup"><span data-stu-id="2fc37-243">a.</span></span> <span data-ttu-id="2fc37-244">I den **e-post** textruta ange e-postadress för användaren som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="2fc37-244">In the **E-mail** textbox, enter the email of user like **brittasimon@contoso.com**.</span></span>
   
    <span data-ttu-id="2fc37-245">b.</span><span class="sxs-lookup"><span data-stu-id="2fc37-245">b.</span></span> <span data-ttu-id="2fc37-246">I den **Förnamn** textruta Ange först namnet på användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-246">In the **First Name** textbox, enter the first name of user like **Britta**.</span></span>
   
    <span data-ttu-id="2fc37-247">c.</span><span class="sxs-lookup"><span data-stu-id="2fc37-247">c.</span></span> <span data-ttu-id="2fc37-248">I den **efternamn** textruta Ange efternamn för användaren som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-248">In the **Last Name** textbox, enter the last name of user like **Simon**.</span></span>
   
    <span data-ttu-id="2fc37-249">d.</span><span class="sxs-lookup"><span data-stu-id="2fc37-249">d.</span></span> <span data-ttu-id="2fc37-250">Som **rollen**väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-250">As **Role**, select **User**.</span></span> 
   
    <span data-ttu-id="2fc37-251">e.</span><span class="sxs-lookup"><span data-stu-id="2fc37-251">e.</span></span> <span data-ttu-id="2fc37-252">Klicka på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-252">Click **Create User**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="2fc37-253">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="2fc37-253">Assigning the Azure AD test user</span></span>

<span data-ttu-id="2fc37-254">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till SD-element.</span><span class="sxs-lookup"><span data-stu-id="2fc37-254">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SD Elements.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="2fc37-256">**Utför följande steg om du vill tilldela Britta Simon till SD-element:**</span><span class="sxs-lookup"><span data-stu-id="2fc37-256">**To assign Britta Simon to SD Elements, perform the following steps:**</span></span>

1. <span data-ttu-id="2fc37-257">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-257">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="2fc37-259">Välj i listan med program **SD element**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-259">In the applications list, select **SD Elements**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_app.png) 

3. <span data-ttu-id="2fc37-261">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="2fc37-261">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="2fc37-263">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="2fc37-263">Click **Add** button.</span></span> <span data-ttu-id="2fc37-264">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2fc37-264">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="2fc37-266">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="2fc37-266">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="2fc37-267">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2fc37-267">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2fc37-268">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2fc37-268">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2fc37-269">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2fc37-269">Testing single sign-on</span></span>

<span data-ttu-id="2fc37-270">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="2fc37-270">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>
  
<span data-ttu-id="2fc37-271">När du klickar på panelen SD-element i åtkomstpanelen du bör få automatiskt loggat in på ditt SD-element-program.</span><span class="sxs-lookup"><span data-stu-id="2fc37-271">When you click the SD Elements tile in the Access Panel, you should get automatically signed-on to your SD Elements application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2fc37-272">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2fc37-272">Additional resources</span></span>

* [<span data-ttu-id="2fc37-273">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2fc37-273">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2fc37-274">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2fc37-274">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_203.png

