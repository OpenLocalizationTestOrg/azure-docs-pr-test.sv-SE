---
title: "Självstudier: Azure Active Directory-integrering med Clever | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Clever."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 069ff13a-310e-4366-a147-d6ec5cca12a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 84082ff567e37d7fff80be9e089c67cfab911861
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clever"></a><span data-ttu-id="161f9-103">Självstudier: Azure Active Directory-integrering med Clever</span><span class="sxs-lookup"><span data-stu-id="161f9-103">Tutorial: Azure Active Directory integration with Clever</span></span>

<span data-ttu-id="161f9-104">I kursen får lära du att integrera Clever med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="161f9-104">In this tutorial, you learn how to integrate Clever with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="161f9-105">Integrera Clever med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="161f9-105">Integrating Clever with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="161f9-106">Du kan styra i Azure AD som har åtkomst till Clever.</span><span class="sxs-lookup"><span data-stu-id="161f9-106">You can control in Azure AD who has access to Clever.</span></span>
- <span data-ttu-id="161f9-107">Du kan aktivera användarna att automatiskt hämta loggat in på Clever (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="161f9-107">You can enable your users to automatically get signed-on to Clever (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="161f9-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="161f9-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="161f9-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="161f9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="161f9-110">Krav</span><span class="sxs-lookup"><span data-stu-id="161f9-110">Prerequisites</span></span>

<span data-ttu-id="161f9-111">För att konfigurera Azure AD-integrering med Clever, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="161f9-111">To configure Azure AD integration with Clever, you need the following items:</span></span>

- <span data-ttu-id="161f9-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="161f9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="161f9-113">En smarta enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="161f9-113">A Clever single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="161f9-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="161f9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="161f9-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="161f9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="161f9-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="161f9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="161f9-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="161f9-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="161f9-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="161f9-118">Scenario description</span></span>
<span data-ttu-id="161f9-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="161f9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="161f9-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="161f9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="161f9-121">Att lägga till Clever från galleriet</span><span class="sxs-lookup"><span data-stu-id="161f9-121">Adding Clever from the gallery</span></span>
2. <span data-ttu-id="161f9-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="161f9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-clever-from-the-gallery"></a><span data-ttu-id="161f9-123">Att lägga till Clever från galleriet</span><span class="sxs-lookup"><span data-stu-id="161f9-123">Adding Clever from the gallery</span></span>
<span data-ttu-id="161f9-124">Du måste lägga till Clever från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Clever i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="161f9-124">To configure the integration of Clever into Azure AD, you need to add Clever from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="161f9-125">**Utför följande steg för att lägga till Clever från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="161f9-125">**To add Clever from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="161f9-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="161f9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="161f9-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="161f9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="161f9-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="161f9-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="161f9-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="161f9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="161f9-133">I sökrutan skriver **Clever**väljer **Clever** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="161f9-133">In the search box, type **Clever**, select **Clever** from result panel then click **Add** button to add the application.</span></span>

    ![Smarta i resultatlistan](./media/active-directory-saas-clever-tutorial/tutorial_clever_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="161f9-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="161f9-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="161f9-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Clever baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="161f9-136">In this section, you configure and test Azure AD single sign-on with Clever based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="161f9-137">Azure AD måste du känna till användaren i Clever motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="161f9-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Clever is to a user in Azure AD.</span></span> <span data-ttu-id="161f9-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Clever upprättas.</span><span class="sxs-lookup"><span data-stu-id="161f9-138">In other words, a link relationship between an Azure AD user and the related user in Clever needs to be established.</span></span>

<span data-ttu-id="161f9-139">I Clever, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="161f9-139">In Clever, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="161f9-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Clever, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="161f9-140">To configure and test Azure AD single sign-on with Clever, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="161f9-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="161f9-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="161f9-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="161f9-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="161f9-143">**[Skapa en smarta testanvändare](#create-a-clever-test-user)**  – du har en motsvarighet för Britta Simon i Clever som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="161f9-143">**[Create a Clever test user](#create-a-clever-test-user)** - to have a counterpart of Britta Simon in Clever that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="161f9-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="161f9-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="161f9-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="161f9-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="161f9-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="161f9-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="161f9-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i smarta programmet.</span><span class="sxs-lookup"><span data-stu-id="161f9-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Clever application.</span></span>

<span data-ttu-id="161f9-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Clever:**</span><span class="sxs-lookup"><span data-stu-id="161f9-148">**To configure Azure AD single sign-on with Clever, perform the following steps:**</span></span>

1. <span data-ttu-id="161f9-149">I Azure-portalen på den **Clever** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="161f9-149">In the Azure portal, on the **Clever** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="161f9-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="161f9-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-clever-tutorial/tutorial_clever_samlbase.png)

3. <span data-ttu-id="161f9-153">På den **smarta domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="161f9-153">On the **Clever Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och smarta domän med enkel inloggning information](./media/active-directory-saas-clever-tutorial/tutorial_clever_url.png)

    <span data-ttu-id="161f9-155">a.</span><span class="sxs-lookup"><span data-stu-id="161f9-155">a.</span></span> <span data-ttu-id="161f9-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://clever.com/in/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="161f9-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://clever.com/in/<companyname>`</span></span>

    <span data-ttu-id="161f9-157">b.</span><span class="sxs-lookup"><span data-stu-id="161f9-157">b.</span></span> <span data-ttu-id="161f9-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://clever.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="161f9-158">In the **Identifier** textbox, type a URL using the following pattern: `https://clever.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="161f9-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="161f9-159">These values are not real.</span></span> <span data-ttu-id="161f9-160">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="161f9-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="161f9-161">Kontakta [smarta klienten supportteamet](https://clever.com/about/contact/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="161f9-161">Contact [Clever Client support team](https://clever.com/about/contact/) to get these values.</span></span>

4. <span data-ttu-id="161f9-162">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="161f9-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-clever-tutorial/tutorial_clever_certificate.png)

5. <span data-ttu-id="161f9-164">Smarta programmet förväntar SAML-intyg i ett specifikt format, vilket kräver att du kan lägga till attributmappningar till din **SAML-Token attribut** konfiguration.</span><span class="sxs-lookup"><span data-stu-id="161f9-164">The Clever application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your **SAML Token Attributes** configuration.</span></span>

    <span data-ttu-id="161f9-165">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="161f9-165">The following screenshot shows an example for this.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-clever-tutorial/tutorial_clever_07.png) 

6. <span data-ttu-id="161f9-167">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden ovan och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="161f9-167">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="161f9-168">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="161f9-168">Attribute Name</span></span>  | <span data-ttu-id="161f9-169">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="161f9-169">Attribute Value</span></span> |
    | --------------- | -------------------- |    
    | <span data-ttu-id="161f9-170">clever.student.Credentials.District\_användarnamn</span><span class="sxs-lookup"><span data-stu-id="161f9-170">clever.student.credentials.district\_username</span></span>  | <span data-ttu-id="161f9-171">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="161f9-171">user.userprincipalname</span></span> |
    | <span data-ttu-id="161f9-172">Förnamn</span><span class="sxs-lookup"><span data-stu-id="161f9-172">Firstname</span></span>  | <span data-ttu-id="161f9-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="161f9-173">user.givenname</span></span> |
    | <span data-ttu-id="161f9-174">Efternamn</span><span class="sxs-lookup"><span data-stu-id="161f9-174">Lastname</span></span>  | <span data-ttu-id="161f9-175">User.surname</span><span class="sxs-lookup"><span data-stu-id="161f9-175">user.surname</span></span> |    

    <span data-ttu-id="161f9-176">a.</span><span class="sxs-lookup"><span data-stu-id="161f9-176">a.</span></span> <span data-ttu-id="161f9-177">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="161f9-177">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-clever-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-clever-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="161f9-180">b.</span><span class="sxs-lookup"><span data-stu-id="161f9-180">b.</span></span> <span data-ttu-id="161f9-181">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="161f9-181">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="161f9-182">c.</span><span class="sxs-lookup"><span data-stu-id="161f9-182">c.</span></span> <span data-ttu-id="161f9-183">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="161f9-183">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="161f9-184">d.</span><span class="sxs-lookup"><span data-stu-id="161f9-184">d.</span></span> <span data-ttu-id="161f9-185">Lämna den **Namespace** textruta tomt.</span><span class="sxs-lookup"><span data-stu-id="161f9-185">Leave the **Namespace** textbox blank.</span></span>
    
    <span data-ttu-id="161f9-186">d.</span><span class="sxs-lookup"><span data-stu-id="161f9-186">d.</span></span> <span data-ttu-id="161f9-187">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="161f9-187">Click **Ok**.</span></span>     

5. <span data-ttu-id="161f9-188">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="161f9-188">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-clever-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="161f9-190">Att generera den **Metadata** url, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="161f9-190">To generate the **Metadata** url, perform the following steps:</span></span>

    <span data-ttu-id="161f9-191">a.</span><span class="sxs-lookup"><span data-stu-id="161f9-191">a.</span></span> <span data-ttu-id="161f9-192">Klicka på **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="161f9-192">Click **App registrations**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-clever-tutorial/tutorial_clever_appregistrations.png)
   
    <span data-ttu-id="161f9-194">b.</span><span class="sxs-lookup"><span data-stu-id="161f9-194">b.</span></span> <span data-ttu-id="161f9-195">Klicka på **slutpunkter** att öppna **slutpunkter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="161f9-195">Click **Endpoints** to open **Endpoints** dialog box.</span></span>  
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-clever-tutorial/tutorial_clever_endpointicon.png)

    <span data-ttu-id="161f9-197">c.</span><span class="sxs-lookup"><span data-stu-id="161f9-197">c.</span></span> <span data-ttu-id="161f9-198">Klicka på kopieringsknappen för att kopiera **FEDERATION METADATADOKUMENTET** url och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="161f9-198">Click the copy button to copy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-clever-tutorial/tutorial_clever_endpoint.png)
     
    <span data-ttu-id="161f9-200">d.</span><span class="sxs-lookup"><span data-stu-id="161f9-200">d.</span></span> <span data-ttu-id="161f9-201">Gå till egenskapssidan för **Clever** och kopiera den **program-Id** med **kopiera** knappen och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="161f9-201">Now go to the property page of **Clever** and copy the **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-clever-tutorial/tutorial_clever_appid.png)

    <span data-ttu-id="161f9-203">e.</span><span class="sxs-lookup"><span data-stu-id="161f9-203">e.</span></span> <span data-ttu-id="161f9-204">Generera den **URL för tjänstmetadata** med hjälp av följande mönster:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="161f9-204">Generate the **Metadata URL** using the following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>   

9. <span data-ttu-id="161f9-205">I en annan webbläsarfönster loggar du in på webbplatsen smarta företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="161f9-205">In a different web browser window, log in to your Clever company site as an administrator.</span></span>

10. <span data-ttu-id="161f9-206">I verktygsfältet klickar du på **Instant inloggningen**.</span><span class="sxs-lookup"><span data-stu-id="161f9-206">In the toolbar, click **Instant Login**.</span></span>

    <span data-ttu-id="161f9-207">![Direkt inloggning](./media/active-directory-saas-clever-tutorial/ic798984.png "Instant inloggning")</span><span class="sxs-lookup"><span data-stu-id="161f9-207">![Instant Login](./media/active-directory-saas-clever-tutorial/ic798984.png "Instant Login")</span></span>

11. <span data-ttu-id="161f9-208">På den **Instant inloggningen** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="161f9-208">On the **Instant Login** page, perform the following steps:</span></span>
      
      <span data-ttu-id="161f9-209">![Direkt inloggning](./media/active-directory-saas-clever-tutorial/ic798985.png "Instant inloggning")</span><span class="sxs-lookup"><span data-stu-id="161f9-209">![Instant Login](./media/active-directory-saas-clever-tutorial/ic798985.png "Instant Login")</span></span>
      
      <span data-ttu-id="161f9-210">a.</span><span class="sxs-lookup"><span data-stu-id="161f9-210">a.</span></span> <span data-ttu-id="161f9-211">Typ av **Inloggningswebbadressen**.</span><span class="sxs-lookup"><span data-stu-id="161f9-211">Type the **Login URL**.</span></span>
      
      >[!NOTE]
      ><span data-ttu-id="161f9-212">Den **Inloggningswebbadressen** är ett anpassat värde.</span><span class="sxs-lookup"><span data-stu-id="161f9-212">The **Login URL** is a custom value.</span></span> <span data-ttu-id="161f9-213">Kontakta [smarta klienten supportteamet](https://clever.com/about/contact/) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="161f9-213">Contact [Clever Client support team](https://clever.com/about/contact/) to get this value.</span></span>
      
      <span data-ttu-id="161f9-214">b.</span><span class="sxs-lookup"><span data-stu-id="161f9-214">b.</span></span> <span data-ttu-id="161f9-215">Som **identitetssystem**väljer **ADFS**.</span><span class="sxs-lookup"><span data-stu-id="161f9-215">As **Identity System**, select **ADFS**.</span></span>

      <span data-ttu-id="161f9-216">c.</span><span class="sxs-lookup"><span data-stu-id="161f9-216">c.</span></span> <span data-ttu-id="161f9-217">Typ av **Metadata-URL** i den **URL för tjänstmetadata** textruta.</span><span class="sxs-lookup"><span data-stu-id="161f9-217">Type the **Metadata URL** in the **Metadata URL** textbox.</span></span>
      
      <span data-ttu-id="161f9-218">d.</span><span class="sxs-lookup"><span data-stu-id="161f9-218">d.</span></span> <span data-ttu-id="161f9-219">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="161f9-219">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="161f9-220">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="161f9-220">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="161f9-221">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="161f9-221">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="161f9-222">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="161f9-222">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="161f9-223">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="161f9-223">Create an Azure AD test user</span></span>

<span data-ttu-id="161f9-224">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="161f9-224">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="161f9-226">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="161f9-226">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="161f9-227">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="161f9-227">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-clever-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="161f9-229">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="161f9-229">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-clever-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="161f9-231">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="161f9-231">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-clever-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="161f9-233">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="161f9-233">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-clever-tutorial/create_aaduser_04.png)

    <span data-ttu-id="161f9-235">a.</span><span class="sxs-lookup"><span data-stu-id="161f9-235">a.</span></span> <span data-ttu-id="161f9-236">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="161f9-236">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="161f9-237">b.</span><span class="sxs-lookup"><span data-stu-id="161f9-237">b.</span></span> <span data-ttu-id="161f9-238">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="161f9-238">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="161f9-239">c.</span><span class="sxs-lookup"><span data-stu-id="161f9-239">c.</span></span> <span data-ttu-id="161f9-240">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="161f9-240">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="161f9-241">d.</span><span class="sxs-lookup"><span data-stu-id="161f9-241">d.</span></span> <span data-ttu-id="161f9-242">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="161f9-242">Click **Create**.</span></span>
 
### <a name="create-a-clever-test-user"></a><span data-ttu-id="161f9-243">Skapa en smarta testanvändare</span><span class="sxs-lookup"><span data-stu-id="161f9-243">Create a Clever test user</span></span>

<span data-ttu-id="161f9-244">Om du vill aktivera Azure AD-användare kan logga in på Clever etableras de i Clever.</span><span class="sxs-lookup"><span data-stu-id="161f9-244">To enable Azure AD users to log in to Clever, they must be provisioned into Clever.</span></span>

<span data-ttu-id="161f9-245">Arbeta med vid Clever, [smarta klienten supportteamet](https://clever.com/about/contact/) att lägga till användare i smarta plattformen.</span><span class="sxs-lookup"><span data-stu-id="161f9-245">In case of Clever, Work with [Clever Client support team](https://clever.com/about/contact/) to add the users in the Clever platform.</span></span> <span data-ttu-id="161f9-246">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="161f9-246">Users must be created and activated before you use single sign-on.</span></span> 

>[!NOTE]
><span data-ttu-id="161f9-247">Du kan använda något annat smarta användarens konto skapas verktyg eller API: er som tillhandahålls av Clever att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="161f9-247">You can use any other Clever user account creation tools or APIs provided by Clever to provision Azure AD user accounts.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="161f9-248">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="161f9-248">Assign the Azure AD test user</span></span>

<span data-ttu-id="161f9-249">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Clever.</span><span class="sxs-lookup"><span data-stu-id="161f9-249">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Clever.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="161f9-251">**Om du vill tilldela Clever Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="161f9-251">**To assign Britta Simon to Clever, perform the following steps:**</span></span>

1. <span data-ttu-id="161f9-252">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="161f9-252">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="161f9-254">Välj i listan med program **Clever**.</span><span class="sxs-lookup"><span data-stu-id="161f9-254">In the applications list, select **Clever**.</span></span>

    ![Clever länken i listan med program](./media/active-directory-saas-clever-tutorial/tutorial_clever_app.png)  

3. <span data-ttu-id="161f9-256">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="161f9-256">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="161f9-258">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="161f9-258">Click **Add** button.</span></span> <span data-ttu-id="161f9-259">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="161f9-259">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="161f9-261">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="161f9-261">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="161f9-262">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="161f9-262">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="161f9-263">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="161f9-263">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="161f9-264">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="161f9-264">Test single sign-on</span></span>

<span data-ttu-id="161f9-265">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="161f9-265">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="161f9-266">När du klickar på panelen smarta åtkomst på panelen du ska hämta automatiskt loggat in på smarta programmet.</span><span class="sxs-lookup"><span data-stu-id="161f9-266">When you click the Clever tile in the Access Panel, you should get automatically signed-on to your Clever application.</span></span>
<span data-ttu-id="161f9-267">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="161f9-267">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="161f9-268">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="161f9-268">Additional resources</span></span>

* [<span data-ttu-id="161f9-269">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="161f9-269">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="161f9-270">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="161f9-270">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-clever-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clever-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clever-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clever-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clever-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clever-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clever-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clever-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clever-tutorial/tutorial_general_203.png

