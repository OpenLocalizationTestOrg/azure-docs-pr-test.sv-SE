---
title: "Självstudier: Azure Active Directory-integrering med IQNavigator VMS | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och IQNavigator VMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a8a09b25-dfa5-4c31-aea2-53bf1853b365
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 1164723a171843541098f6adbda0e65f7e82a0cb
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-iqnavigator-vms"></a><span data-ttu-id="940b0-103">Självstudier: Azure Active Directory-integrering med IQNavigator VMS</span><span class="sxs-lookup"><span data-stu-id="940b0-103">Tutorial: Azure Active Directory integration with IQNavigator VMS</span></span>

<span data-ttu-id="940b0-104">I kursen får lära du att integrera IQNavigator VMS med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="940b0-104">In this tutorial, you learn how to integrate IQNavigator VMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="940b0-105">Integrera IQNavigator VMS med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="940b0-105">Integrating IQNavigator VMS with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="940b0-106">Du kan styra i Azure AD som har åtkomst till IQNavigator VMS</span><span class="sxs-lookup"><span data-stu-id="940b0-106">You can control in Azure AD who has access to IQNavigator VMS</span></span>
- <span data-ttu-id="940b0-107">Du kan aktivera användarna att automatiskt hämta loggat in på IQNavigator VMS (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="940b0-107">You can enable your users to automatically get signed-on to IQNavigator VMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="940b0-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="940b0-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="940b0-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="940b0-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="940b0-110">Krav</span><span class="sxs-lookup"><span data-stu-id="940b0-110">Prerequisites</span></span>

<span data-ttu-id="940b0-111">För att konfigurera Azure AD-integrering med IQNavigator VMS, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="940b0-111">To configure Azure AD integration with IQNavigator VMS, you need the following items:</span></span>

- <span data-ttu-id="940b0-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="940b0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="940b0-113">En IQNavigator VMS enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="940b0-113">A IQNavigator VMS single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="940b0-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="940b0-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="940b0-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="940b0-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="940b0-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="940b0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="940b0-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="940b0-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="940b0-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="940b0-118">Scenario description</span></span>
<span data-ttu-id="940b0-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="940b0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="940b0-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="940b0-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="940b0-121">Att lägga till IQNavigator VMS från galleriet</span><span class="sxs-lookup"><span data-stu-id="940b0-121">Adding IQNavigator VMS from the gallery</span></span>
2. <span data-ttu-id="940b0-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="940b0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-iqnavigator-vms-from-the-gallery"></a><span data-ttu-id="940b0-123">Att lägga till IQNavigator VMS från galleriet</span><span class="sxs-lookup"><span data-stu-id="940b0-123">Adding IQNavigator VMS from the gallery</span></span>
<span data-ttu-id="940b0-124">Du måste lägga till IQNavigator VMS från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av IQNavigator VMS i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="940b0-124">To configure the integration of IQNavigator VMS into Azure AD, you need to add IQNavigator VMS from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="940b0-125">**Utför följande steg för att lägga till IQNavigator VMS från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="940b0-125">**To add IQNavigator VMS from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="940b0-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="940b0-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="940b0-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="940b0-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="940b0-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="940b0-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="940b0-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="940b0-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="940b0-133">I sökrutan skriver **IQNavigator VMS**.</span><span class="sxs-lookup"><span data-stu-id="940b0-133">In the search box, type **IQNavigator VMS**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_search.png)

5. <span data-ttu-id="940b0-135">Välj i resultatpanelen **IQNavigator VMS**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="940b0-135">In the results panel, select **IQNavigator VMS**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="940b0-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="940b0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="940b0-138">Du konfigurera och testa Azure AD enkel inloggning med IQNavigator VMS baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="940b0-138">In this section, you configure and test Azure AD single sign-on with IQNavigator VMS based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="940b0-139">Azure AD måste du känna till användaren i IQNavigator VMS motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="940b0-139">For single sign-on to work, Azure AD needs to know what the counterpart user in IQNavigator VMS is to a user in Azure AD.</span></span> <span data-ttu-id="940b0-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i IQNavigator VMS upprättas.</span><span class="sxs-lookup"><span data-stu-id="940b0-140">In other words, a link relationship between an Azure AD user and the related user in IQNavigator VMS needs to be established.</span></span>

<span data-ttu-id="940b0-141">I IQNavigator VMS, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="940b0-141">In IQNavigator VMS, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="940b0-142">Om du vill konfigurera och testa Azure AD enkel inloggning med IQNavigator VMS, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="940b0-142">To configure and test Azure AD single sign-on with IQNavigator VMS, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="940b0-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="940b0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="940b0-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="940b0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="940b0-145">**[Skapa en testanvändare IQNavigator VMS](#creating-a-iqnavigator-vms-test-user)**  – har en motsvarighet för Britta Simon IQNavigator VMS som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="940b0-145">**[Creating a IQNavigator VMS test user](#creating-a-iqnavigator-vms-test-user)** - to have a counterpart of Britta Simon in IQNavigator VMS that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="940b0-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="940b0-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="940b0-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="940b0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="940b0-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="940b0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="940b0-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt IQNavigator VMS-program.</span><span class="sxs-lookup"><span data-stu-id="940b0-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your IQNavigator VMS application.</span></span>

<span data-ttu-id="940b0-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med IQNavigator VMS:**</span><span class="sxs-lookup"><span data-stu-id="940b0-150">**To configure Azure AD single sign-on with IQNavigator VMS, perform the following steps:**</span></span>

1. <span data-ttu-id="940b0-151">I Azure-portalen på den **IQNavigator VMS** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="940b0-151">In the Azure portal, on the **IQNavigator VMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="940b0-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="940b0-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_samlbase.png)

3. <span data-ttu-id="940b0-155">På den **IQNavigator VMS domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="940b0-155">On the **IQNavigator VMS Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url.png)

    <span data-ttu-id="940b0-157">a.</span><span class="sxs-lookup"><span data-stu-id="940b0-157">a.</span></span> <span data-ttu-id="940b0-158">I den **identifierare** textruta anger du URL:`iqn.com`</span><span class="sxs-lookup"><span data-stu-id="940b0-158">In the **Identifier** textbox, type the URL:`iqn.com`</span></span>

    <span data-ttu-id="940b0-159">b.</span><span class="sxs-lookup"><span data-stu-id="940b0-159">b.</span></span> <span data-ttu-id="940b0-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.iqnavigator.com/security/login?client_name=https://sts.window.net/<instance name>`</span><span class="sxs-lookup"><span data-stu-id="940b0-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.iqnavigator.com/security/login?client_name=https://sts.window.net/<instance name>`</span></span>

4. <span data-ttu-id="940b0-161">Kontrollera **visa avancerade inställningar för URL: en**, utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="940b0-161">Check **Show advanced URL settings**, perform the following step:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url1.png)

    <span data-ttu-id="940b0-163">I den **Relay tillstånd** textruta Skriv en URL med följande mönster:`https://<subdomain>.iqnavigator.com`</span><span class="sxs-lookup"><span data-stu-id="940b0-163">In the **Relay state** textbox, type a URL using the following pattern:`https://<subdomain>.iqnavigator.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="940b0-164">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="940b0-164">These values are not real.</span></span> <span data-ttu-id="940b0-165">Uppdatera dessa värden med bibliotekets Reply URL och Relay tillstånd.</span><span class="sxs-lookup"><span data-stu-id="940b0-165">Update these values with the actual Reply URL and Relay state.</span></span> <span data-ttu-id="940b0-166">Kontakta [IQNavigator VMS klienten supportteamet](https://www.beeline.com/iqn-product-support/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="940b0-166">Contact [IQNavigator VMS Client support team](https://www.beeline.com/iqn-product-support/) to get these values.</span></span> 

5. <span data-ttu-id="940b0-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="940b0-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="940b0-169">Att generera den **Metadata** url, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="940b0-169">To generate the **Metadata** url, perform the following steps:</span></span>

    <span data-ttu-id="940b0-170">a.</span><span class="sxs-lookup"><span data-stu-id="940b0-170">a.</span></span> <span data-ttu-id="940b0-171">Klicka på **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="940b0-171">Click **App registrations**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_appregistrations.png)
   
    <span data-ttu-id="940b0-173">b.</span><span class="sxs-lookup"><span data-stu-id="940b0-173">b.</span></span> <span data-ttu-id="940b0-174">Klicka på **slutpunkter** att öppna **slutpunkter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="940b0-174">Click **Endpoints** to open **Endpoints** dialog box.</span></span>  
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_endpointicon.png)

    <span data-ttu-id="940b0-176">c.</span><span class="sxs-lookup"><span data-stu-id="940b0-176">c.</span></span> <span data-ttu-id="940b0-177">Klicka på kopieringsknappen för att kopiera **FEDERATION METADATADOKUMENTET** url och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="940b0-177">Click the copy button to copy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_endpoint.png)
     
    <span data-ttu-id="940b0-179">d.</span><span class="sxs-lookup"><span data-stu-id="940b0-179">d.</span></span> <span data-ttu-id="940b0-180">Gå till egenskapssidan för **IQNavigator VMS** och kopiera den **program-Id** med **kopiera** knappen och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="940b0-180">Now go to the property page of **IQNavigator VMS** and copy the **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_appid.png)

    <span data-ttu-id="940b0-182">e.</span><span class="sxs-lookup"><span data-stu-id="940b0-182">e.</span></span> <span data-ttu-id="940b0-183">Generera den **URL för tjänstmetadata** med hjälp av följande mönster:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="940b0-183">Generate the **Metadata URL** using the following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>

7. <span data-ttu-id="940b0-184">IQNavigator program förväntar dig identifierarvärde unik användare i namnidentifierare anspråket.</span><span class="sxs-lookup"><span data-stu-id="940b0-184">IQNavigator application expect the unique user identifier value in the Name Identifier claim.</span></span> <span data-ttu-id="940b0-185">Kunden kan mappa rätt värde för namn-ID-anspråk.</span><span class="sxs-lookup"><span data-stu-id="940b0-185">Customer can map the correct value for the Name Identifier claim.</span></span> <span data-ttu-id="940b0-186">I det här fallet har vi mappat användaren. UserPrincipalName för demo-ändamål.</span><span class="sxs-lookup"><span data-stu-id="940b0-186">In this case we have mapped the user.UserPrincipalName for the demo purpose.</span></span> <span data-ttu-id="940b0-187">Men enligt Organisationsinställningarna för din ska du mappa rätt värde för den.</span><span class="sxs-lookup"><span data-stu-id="940b0-187">But according to your organization settings you should map the correct value for it.</span></span>   

    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_attribute.png)

8. <span data-ttu-id="940b0-189">På den **IQNavigator VMS Configuration** klickar du på **konfigurera IQNavigator VMS** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="940b0-189">On the **IQNavigator VMS Configuration** section, click **Configure IQNavigator VMS** to open **Configure sign-on** window.</span></span> <span data-ttu-id="940b0-190">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="940b0-190">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_configure.png) 

9. <span data-ttu-id="940b0-192">Konfigurera enkel inloggning på **IQNavigator VMS** sida, måste du skicka den **URL för tjänstmetadata**, **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel** till [ IQNavigator VMS supportteamet](https://www.beeline.com/iqn-product-support/).</span><span class="sxs-lookup"><span data-stu-id="940b0-192">To configure single sign-on on **IQNavigator VMS** side, you need to send the **Metadata URL**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [IQNavigator VMS support team](https://www.beeline.com/iqn-product-support/).</span></span> <span data-ttu-id="940b0-193">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="940b0-193">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="940b0-194">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="940b0-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="940b0-195">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="940b0-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="940b0-196">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="940b0-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="940b0-197">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="940b0-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="940b0-198">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="940b0-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="940b0-200">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="940b0-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="940b0-201">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="940b0-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="940b0-203">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="940b0-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="940b0-205">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="940b0-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="940b0-207">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="940b0-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="940b0-209">a.</span><span class="sxs-lookup"><span data-stu-id="940b0-209">a.</span></span> <span data-ttu-id="940b0-210">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="940b0-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="940b0-211">b.</span><span class="sxs-lookup"><span data-stu-id="940b0-211">b.</span></span> <span data-ttu-id="940b0-212">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="940b0-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="940b0-213">c.</span><span class="sxs-lookup"><span data-stu-id="940b0-213">c.</span></span> <span data-ttu-id="940b0-214">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="940b0-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="940b0-215">d.</span><span class="sxs-lookup"><span data-stu-id="940b0-215">d.</span></span> <span data-ttu-id="940b0-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="940b0-216">Click **Create**.</span></span>
 
### <a name="creating-a-iqnavigator-vms-test-user"></a><span data-ttu-id="940b0-217">Skapa en IQNavigator VMS testanvändare</span><span class="sxs-lookup"><span data-stu-id="940b0-217">Creating a IQNavigator VMS test user</span></span>

<span data-ttu-id="940b0-218">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i IQNavigator VMS.</span><span class="sxs-lookup"><span data-stu-id="940b0-218">The objective of this section is to create a user called Britta Simon in IQNavigator VMS.</span></span> <span data-ttu-id="940b0-219">Arbeta med [IQNavigator VMS supportteamet](https://www.beeline.com/iqn-product-support/) att lägga till användare i IQNavigator VMS-konto.</span><span class="sxs-lookup"><span data-stu-id="940b0-219">Work with  [IQNavigator VMS support team](https://www.beeline.com/iqn-product-support/) to add the users in the IQNavigator VMS account.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="940b0-220">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="940b0-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="940b0-221">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till IQNavigator VMS.</span><span class="sxs-lookup"><span data-stu-id="940b0-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to IQNavigator VMS.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="940b0-223">**Om du vill tilldela IQNavigator VMS Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="940b0-223">**To assign Britta Simon to IQNavigator VMS, perform the following steps:**</span></span>

1. <span data-ttu-id="940b0-224">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="940b0-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="940b0-226">Välj i listan med program **IQNavigator VMS**.</span><span class="sxs-lookup"><span data-stu-id="940b0-226">In the applications list, select **IQNavigator VMS**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_app.png) 

3. <span data-ttu-id="940b0-228">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="940b0-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="940b0-230">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="940b0-230">Click **Add** button.</span></span> <span data-ttu-id="940b0-231">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="940b0-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="940b0-233">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="940b0-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="940b0-234">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="940b0-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="940b0-235">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="940b0-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="940b0-236">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="940b0-236">Testing single sign-on</span></span>

<span data-ttu-id="940b0-237">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="940b0-237">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="940b0-238">När du klickar på panelen IQNavigator VMS på åtkomstpanelen du bör få automatiskt loggat in på ditt IQNavigator VMS-program.</span><span class="sxs-lookup"><span data-stu-id="940b0-238">When you click the IQNavigator VMS tile in the Access Panel, you should get automatically signed-on to your IQNavigator VMS application.</span></span>
<span data-ttu-id="940b0-239">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="940b0-239">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="940b0-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="940b0-240">Additional resources</span></span>

* [<span data-ttu-id="940b0-241">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="940b0-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="940b0-242">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="940b0-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_203.png

