---
title: "Självstudier: Azure Active Directory-integrering med Klue | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Klue."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 08341008-980b-4111-adb2-97bbabbf1e47
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: c64417c136340b3ffa5d67c618c6fe037d2992b5
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-klue"></a><span data-ttu-id="c9ba2-103">Självstudier: Azure Active Directory-integrering med Klue</span><span class="sxs-lookup"><span data-stu-id="c9ba2-103">Tutorial: Azure Active Directory integration with Klue</span></span>

<span data-ttu-id="c9ba2-104">I kursen får lära du att integrera Klue med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c9ba2-104">In this tutorial, you learn how to integrate Klue with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c9ba2-105">Integrera Klue med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c9ba2-105">Integrating Klue with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c9ba2-106">Du kan styra i Azure AD som har åtkomst till Klue</span><span class="sxs-lookup"><span data-stu-id="c9ba2-106">You can control in Azure AD who has access to Klue</span></span>
- <span data-ttu-id="c9ba2-107">Du kan aktivera användarna att automatiskt hämta loggat in på Klue (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c9ba2-107">You can enable your users to automatically get signed-on to Klue (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c9ba2-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c9ba2-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c9ba2-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c9ba2-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c9ba2-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c9ba2-110">Prerequisites</span></span>

<span data-ttu-id="c9ba2-111">För att konfigurera Azure AD-integrering med Klue, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="c9ba2-111">To configure Azure AD integration with Klue, you need the following items:</span></span>

- <span data-ttu-id="c9ba2-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c9ba2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c9ba2-113">En Klue enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="c9ba2-113">A Klue single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c9ba2-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c9ba2-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c9ba2-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c9ba2-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c9ba2-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c9ba2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c9ba2-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c9ba2-118">Scenario description</span></span>
<span data-ttu-id="c9ba2-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c9ba2-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c9ba2-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c9ba2-121">Att lägga till Klue från galleriet</span><span class="sxs-lookup"><span data-stu-id="c9ba2-121">Adding Klue from the gallery</span></span>
2. <span data-ttu-id="c9ba2-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c9ba2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-klue-from-the-gallery"></a><span data-ttu-id="c9ba2-123">Att lägga till Klue från galleriet</span><span class="sxs-lookup"><span data-stu-id="c9ba2-123">Adding Klue from the gallery</span></span>
<span data-ttu-id="c9ba2-124">Du måste lägga till Klue från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Klue i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-124">To configure the integration of Klue into Azure AD, you need to add Klue from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c9ba2-125">**Utför följande steg för att lägga till Klue från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="c9ba2-125">**To add Klue from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c9ba2-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c9ba2-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c9ba2-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="c9ba2-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="c9ba2-133">I sökrutan skriver **Klue**.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-133">In the search box, type **Klue**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-klue-tutorial/tutorial_klue_search.png)

5. <span data-ttu-id="c9ba2-135">Välj i resultatpanelen **Klue**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-135">In the results panel, select **Klue**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-klue-tutorial/tutorial_klue_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c9ba2-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c9ba2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c9ba2-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Klue baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-138">In this section, you configure and test Azure AD single sign-on with Klue based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c9ba2-139">Azure AD måste du känna till användaren i Klue motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Klue is to a user in Azure AD.</span></span> <span data-ttu-id="c9ba2-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Klue upprättas.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-140">In other words, a link relationship between an Azure AD user and the related user in Klue needs to be established.</span></span>

<span data-ttu-id="c9ba2-141">I Klue, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-141">In Klue, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c9ba2-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Klue, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c9ba2-142">To configure and test Azure AD single sign-on with Klue, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c9ba2-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c9ba2-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c9ba2-145">**[Skapa en testanvändare Klue](#creating-a-klue-test-user)**  – du har en motsvarighet för Britta Simon i Klue som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-145">**[Creating a Klue test user](#creating-a-klue-test-user)** - to have a counterpart of Britta Simon in Klue that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c9ba2-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c9ba2-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c9ba2-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c9ba2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c9ba2-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Klue program.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Klue application.</span></span>

<span data-ttu-id="c9ba2-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Klue:**</span><span class="sxs-lookup"><span data-stu-id="c9ba2-150">**To configure Azure AD single sign-on with Klue, perform the following steps:**</span></span>

1. <span data-ttu-id="c9ba2-151">I Azure-portalen på den **Klue** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-151">In the Azure portal, on the **Klue** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c9ba2-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-klue-tutorial/tutorial_klue_samlbase.png)

3. <span data-ttu-id="c9ba2-155">På den **Klue domän och URL: er** om du vill konfigurera programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="c9ba2-155">On the **Klue Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-klue-tutorial/tutorial_klue_url1.png)

    <span data-ttu-id="c9ba2-157">a.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-157">a.</span></span> <span data-ttu-id="c9ba2-158">I den **identifierare** textruta Skriv en URL med följande mönster:`urn:klue:<Customer ID>`</span><span class="sxs-lookup"><span data-stu-id="c9ba2-158">In the **Identifier** textbox, type a URL using the following pattern: `urn:klue:<Customer ID>`</span></span>

    <span data-ttu-id="c9ba2-159">b.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-159">b.</span></span> <span data-ttu-id="c9ba2-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://app.klue.com/account/auth/saml/<Customer UUID>/callback`</span><span class="sxs-lookup"><span data-stu-id="c9ba2-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://app.klue.com/account/auth/saml/<Customer UUID>/callback`</span></span>

4. <span data-ttu-id="c9ba2-161">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="c9ba2-162">Om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="c9ba2-162">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-klue-tutorial/tutorial_klue_url2.png)

    <span data-ttu-id="c9ba2-164">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://app.klue.com/account/auth/saml/<Customer UUID>/`</span><span class="sxs-lookup"><span data-stu-id="c9ba2-164">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://app.klue.com/account/auth/saml/<Customer UUID>/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="c9ba2-165">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-165">These values are not real.</span></span> <span data-ttu-id="c9ba2-166">Uppdatera dessa värden med den faktiska Reply URL, identifierare och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-166">Update these values with the actual Reply URL, Identifier, and Sign-On URL.</span></span> <span data-ttu-id="c9ba2-167">Kontakta [Klue klienten supportteamet](mailto:support@klue.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-167">Contact [Klue Client support team](mailto:support@klue.com) to get these values.</span></span>

5. <span data-ttu-id="c9ba2-168">Programmet Klue förväntar SAML-intyg i ett specifikt format, vilket kräver att du kan lägga till anpassade attributmappning konfigurationen för SAML-token attribut.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-168">The Klue application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="c9ba2-169">Du kan hantera värden för attributen från den ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-169">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-klue-tutorial/attribute.png)

6. <span data-ttu-id="c9ba2-171">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i den föregående bilden och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c9ba2-171">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the preceding image and perform the following steps:</span></span>
    
    | <span data-ttu-id="c9ba2-172">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="c9ba2-172">Attribute Name</span></span>      | <span data-ttu-id="c9ba2-173">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="c9ba2-173">Attribute Value</span></span>      |
    | ------------------- | -------------------- |
    | <span data-ttu-id="c9ba2-174">Förnamn</span><span class="sxs-lookup"><span data-stu-id="c9ba2-174">first_name</span></span>          | <span data-ttu-id="c9ba2-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="c9ba2-175">user.givenname</span></span> |
    | <span data-ttu-id="c9ba2-176">Efternamn</span><span class="sxs-lookup"><span data-stu-id="c9ba2-176">last_name</span></span>           | <span data-ttu-id="c9ba2-177">User.surname</span><span class="sxs-lookup"><span data-stu-id="c9ba2-177">user.surname</span></span> |
    | <span data-ttu-id="c9ba2-178">E-post</span><span class="sxs-lookup"><span data-stu-id="c9ba2-178">email</span></span>               | <span data-ttu-id="c9ba2-179">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="c9ba2-179">user.userprincipalname</span></span>|
    
    <span data-ttu-id="c9ba2-180">a.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-180">a.</span></span> <span data-ttu-id="c9ba2-181">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-181">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-klue-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-klue-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="c9ba2-184">b.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-184">b.</span></span> <span data-ttu-id="c9ba2-185">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-185">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="c9ba2-186">c.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-186">c.</span></span> <span data-ttu-id="c9ba2-187">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-187">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="c9ba2-188">d.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-188">d.</span></span> <span data-ttu-id="c9ba2-189">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-189">Click **Ok**.</span></span>

7. <span data-ttu-id="c9ba2-190">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-190">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-klue-tutorial/tutorial_klue_certificate.png) 

8. <span data-ttu-id="c9ba2-192">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-192">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-klue-tutorial/tutorial_general_400.png)
    
9. <span data-ttu-id="c9ba2-194">På den **Klue Configuration** klickar du på **konfigurera Klue** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-194">On the **Klue Configuration** section, click **Configure Klue** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c9ba2-195">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="c9ba2-195">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-klue-tutorial/tutorial_klue_configure.png) 

10. <span data-ttu-id="c9ba2-197">Konfigurera enkel inloggning på **Klue** sida, måste du skicka den hämtade **Certificate(Base64) och SAML enkel inloggning URL för SAML enhets-ID** till [Klue supportteamet](mailto:support@klue.com).</span><span class="sxs-lookup"><span data-stu-id="c9ba2-197">To configure single sign-on on **Klue** side, you need to send the downloaded **Certificate(Base64), SAML Single Sign-On Service URL, and SAML Entity ID** to [Klue support team](mailto:support@klue.com).</span></span>

> [!TIP]
> <span data-ttu-id="c9ba2-198">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="c9ba2-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c9ba2-199">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c9ba2-200">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c9ba2-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c9ba2-201">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c9ba2-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="c9ba2-202">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="c9ba2-204">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="c9ba2-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c9ba2-205">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-klue-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c9ba2-207">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-klue-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c9ba2-209">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-klue-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c9ba2-211">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c9ba2-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-klue-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c9ba2-213">a.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-213">a.</span></span> <span data-ttu-id="c9ba2-214">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c9ba2-215">b.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-215">b.</span></span> <span data-ttu-id="c9ba2-216">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c9ba2-217">c.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-217">c.</span></span> <span data-ttu-id="c9ba2-218">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c9ba2-219">d.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-219">d.</span></span> <span data-ttu-id="c9ba2-220">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-220">Click **Create**.</span></span>
 
### <a name="creating-a-klue-test-user"></a><span data-ttu-id="c9ba2-221">Skapa en testanvändare Klue</span><span class="sxs-lookup"><span data-stu-id="c9ba2-221">Creating a Klue test user</span></span>

<span data-ttu-id="c9ba2-222">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Klue.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-222">The objective of this section is to create a user called Britta Simon in Klue.</span></span> <span data-ttu-id="c9ba2-223">Klue stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-223">Klue supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="c9ba2-224">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-224">There is no action item for you in this section.</span></span> <span data-ttu-id="c9ba2-225">En ny användare skapas under ett försök att komma åt Klue om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-225">A new user is created during an attempt to access Klue if it doesn't exist yet.</span></span>

>[!Note]
><span data-ttu-id="c9ba2-226">Om du behöver skapa en användare manuellt, kontakta [Klue supportteamet](mailto:support@klue.com).</span><span class="sxs-lookup"><span data-stu-id="c9ba2-226">If you need to create a user manually, Contact [Klue support team](mailto:support@klue.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c9ba2-227">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="c9ba2-227">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c9ba2-228">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Klue.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-228">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Klue.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="c9ba2-230">**Om du vill tilldela Klue Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c9ba2-230">**To assign Britta Simon to Klue, perform the following steps:**</span></span>

1. <span data-ttu-id="c9ba2-231">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-231">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c9ba2-233">Välj i listan med program **Klue**.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-233">In the applications list, select **Klue**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-klue-tutorial/tutorial_klue_app.png) 

3. <span data-ttu-id="c9ba2-235">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-235">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="c9ba2-237">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-237">Click **Add** button.</span></span> <span data-ttu-id="c9ba2-238">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="c9ba2-240">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-240">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c9ba2-241">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c9ba2-242">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c9ba2-243">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c9ba2-243">Testing single sign-on</span></span>

<span data-ttu-id="c9ba2-244">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-244">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c9ba2-245">När du klickar på panelen Klue på åtkomstpanelen du bör få automatiskt loggat in på ditt Klue program.</span><span class="sxs-lookup"><span data-stu-id="c9ba2-245">When you click the Klue tile in the Access Panel, you should get automatically signed-on to your Klue application.</span></span>
<span data-ttu-id="c9ba2-246">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c9ba2-246">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c9ba2-247">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c9ba2-247">Additional resources</span></span>

* [<span data-ttu-id="c9ba2-248">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c9ba2-248">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c9ba2-249">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c9ba2-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-klue-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-klue-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-klue-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-klue-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-klue-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-klue-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-klue-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-klue-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-klue-tutorial/tutorial_general_203.png

