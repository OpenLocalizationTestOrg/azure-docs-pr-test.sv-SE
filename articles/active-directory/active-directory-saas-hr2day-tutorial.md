---
title: "Självstudier: Azure Active Directory-integrering med HR2day av Merces | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och HR2day av Merces."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 853d08c9-27b1-48d4-b8e7-3705140eb67f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: 77e2fcf9c306259286b15767f3a992510d6d79d8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a><span data-ttu-id="0ab3d-103">Självstudier: Azure Active Directory-integrering med HR2day av Merces</span><span class="sxs-lookup"><span data-stu-id="0ab3d-103">Tutorial: Azure Active Directory integration with HR2day by Merces</span></span>

<span data-ttu-id="0ab3d-104">I kursen får lära du att integrera HR2day av Merces med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="0ab3d-104">In this tutorial, you learn how to integrate HR2day by Merces with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0ab3d-105">Integrera HR2day av Merces med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="0ab3d-105">Integrating HR2day by Merces with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0ab3d-106">Du kan styra i Azure AD som har åtkomst till HR2day av Merces.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-106">You can control in Azure AD who has access to HR2day by Merces.</span></span>
- <span data-ttu-id="0ab3d-107">Du kan aktivera användarna att få loggas automatiskt HR2day av Merces med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-107">You can enable your users to automatically get signed in to HR2day by Merces with their Azure AD accounts.</span></span>
- <span data-ttu-id="0ab3d-108">Du kan hantera dina konton i en central plats--Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-108">You can manage your accounts in one central location--the Azure portal.</span></span>

<span data-ttu-id="0ab3d-109">Mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0ab3d-109">For more information about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0ab3d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="0ab3d-110">Prerequisites</span></span>

<span data-ttu-id="0ab3d-111">För att konfigurera Azure AD-integrering med HR2day av Merces, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="0ab3d-111">To configure Azure AD integration with HR2day by Merces, you need the following items:</span></span>

- <span data-ttu-id="0ab3d-112">En Azure AD-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-112">An Azure AD subscription.</span></span>
- <span data-ttu-id="0ab3d-113">En HR2day av Merces enkel inloggning aktiverad prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-113">An HR2day by Merces single sign-on enabled subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="0ab3d-114">Vi rekommenderar inte använda en produktionsmiljö för att testa stegen i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-114">We don't recommend using a production environment to test the steps in this tutorial.</span></span>

<span data-ttu-id="0ab3d-115">Följ dessa rekommendationer för att testa stegen i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="0ab3d-115">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="0ab3d-116">Använd inte din produktionsmiljö om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-116">Don't use your production environment unless it's necessary.</span></span>
- <span data-ttu-id="0ab3d-117">Hämta en [en månaders kostnadsfri utvärderingsversion av Azure AD](https://azure.microsoft.com/pricing/free-trial/) om det inte redan har.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-117">Get a [one-month free trial of Azure AD](https://azure.microsoft.com/pricing/free-trial/) if you don't already have it.</span></span>  

## <a name="scenario-description"></a><span data-ttu-id="0ab3d-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="0ab3d-118">Scenario description</span></span>
<span data-ttu-id="0ab3d-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0ab3d-120">Det scenario som beskrivs här består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="0ab3d-120">The scenario that's outlined here consists of two main building blocks:</span></span>

1. <span data-ttu-id="0ab3d-121">Lägger till HR2day av Merces från galleriet.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-121">Adding HR2day by Merces from the gallery.</span></span>
2. <span data-ttu-id="0ab3d-122">Konfigurera och testa Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-122">Configuring and testing Azure AD single sign-on.</span></span>

## <a name="add-hr2day-by-merces-from-the-gallery"></a><span data-ttu-id="0ab3d-123">Lägg till HR2day av Merces från galleriet</span><span class="sxs-lookup"><span data-stu-id="0ab3d-123">Add HR2day by Merces from the gallery</span></span>
<span data-ttu-id="0ab3d-124">För att konfigurera integrering av HR2day av Merces i Azure AD, lägga till HR2day av Merces från galleriet i listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-124">To configure the integration of HR2day by Merces into Azure AD, add HR2day by Merces from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0ab3d-125">**Om du vill lägga till HR2day av Merces från galleriet, gör du följande:**</span><span class="sxs-lookup"><span data-stu-id="0ab3d-125">**To add HR2day by Merces from the gallery, take the following steps:**</span></span>

1. <span data-ttu-id="0ab3d-126">I den [Azure-portalen](https://portal.azure.com), på det vänstra navigeringsfönstret väljer du den **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-126">In the [Azure portal](https://portal.azure.com), on the left navigation pane, select the **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0ab3d-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-128">Go to **Enterprise applications**.</span></span> <span data-ttu-id="0ab3d-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="0ab3d-131">Om du vill lägga till ett nytt program, Välj den **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-131">To add a new application, select the **New application** button on the top of the dialog box.</span></span>

    ![Program][3]

4. <span data-ttu-id="0ab3d-133">I sökrutan skriver **HR2day av Merces**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-133">In the search box, type **HR2day by Merces**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_search.png)

5. <span data-ttu-id="0ab3d-135">Välj i resultatpanelen **HR2day av Merces**, och välj sedan den **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-135">In the results panel, select **HR2day by Merces**, and then select the **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="0ab3d-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0ab3d-137">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="0ab3d-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med HR2day av Merces baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-138">In this section, you configure and test Azure AD single sign-on with HR2day by Merces based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0ab3d-139">För enkel inloggning ska fungera, måste Azure AD att veta vilka motsvarande användaren i HR2day av Merces är att en användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-139">For single sign-on to work, Azure AD needs to know who the counterpart user in HR2day by Merces is to a user in Azure AD.</span></span> <span data-ttu-id="0ab3d-140">Med andra ord måste du skapa en länk mellan en Azure AD-användare och relaterade användaren i HR2day av Merces.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-140">In other words, you need to establish a link between an Azure AD user and the related user in HR2day by Merces.</span></span>

<span data-ttu-id="0ab3d-141">Tilldela i HR2day av Merces den **användarnamn** i Azure AD **användarnamn** att upprätta en relation.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-141">In HR2day by Merces, assign the **user name** in Azure AD to  **Username** to establish the relationship.</span></span>

<span data-ttu-id="0ab3d-142">Om du vill konfigurera och testa Azure AD enkel inloggning med HR2day av Merces, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="0ab3d-142">To configure and test Azure AD single sign-on with HR2day by Merces, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0ab3d-143">[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on): att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-143">[Configure Azure AD single sign-on](#configuring-azure-ad-single-sign-on): Enable your users to use this feature.</span></span>
2. <span data-ttu-id="0ab3d-144">[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user): testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-144">[Create an Azure AD test user](#creating-an-azure-ad-test-user): Test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0ab3d-145">[Skapa en HR2day av Merces testanvändare](#creating-an-hr2day-by-merces-test-user): skapa en motsvarighet för Britta Simon i HR2day av Merces som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-145">[Create an HR2day by Merces test user](#creating-an-hr2day-by-merces-test-user): Create a counterpart of Britta Simon in HR2day by Merces that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0ab3d-146">[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user): aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-146">[Assign the Azure AD test user](#assigning-the-azure-ad-test-user): Enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0ab3d-147">[Testa enkel inloggning](#testing-single-sign-on): Kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-147">[Test single sign-on](#testing-single-sign-on): Verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="0ab3d-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0ab3d-148">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="0ab3d-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din HR2day av Merces program.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your HR2day by Merces application.</span></span>

<span data-ttu-id="0ab3d-150">**För att konfigurera Azure AD enkel inloggning med HR2day av Merces, gör du följande:**</span><span class="sxs-lookup"><span data-stu-id="0ab3d-150">**To configure Azure AD single sign-on with HR2day by Merces, take the following steps:**</span></span>

1. <span data-ttu-id="0ab3d-151">I Azure-portalen på den **HR2day av Merces** programmet integration anger **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-151">In the Azure portal, on the **HR2day by Merces** application integration page, select **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="0ab3d-153">Aktivera enkel inloggning, i den **enkel inloggning** dialogrutan **läge** som **SAML-baserade inloggning**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-153">To enable single sign-on, in the **Single sign-on** dialog box, select **Mode** as **SAML-based Sign-on**.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_samlbase.png)

3. <span data-ttu-id="0ab3d-155">I den **HR2day Merces domänen och URL: er** avsnittet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="0ab3d-155">In the **HR2day by Merces Domain and URLs** section, take the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_url.png)

    <span data-ttu-id="0ab3d-157">a.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-157">a.</span></span> <span data-ttu-id="0ab3d-158">I den **inloggnings-URL** Skriv en URL med hjälp av följande mönster: `https://<tenantname>.force.com/<instancename>`.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-158">In the **Sign-on URL** box, type a URL by using the following pattern: `https://<tenantname>.force.com/<instancename>`.</span></span>

    <span data-ttu-id="0ab3d-159">b.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-159">b.</span></span> <span data-ttu-id="0ab3d-160">I den **identifierare** Skriv en URL med hjälp av följande mönster: `https://hr2day.force.com/<companyname>`.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-160">In the **Identifier** box, type a URL by using the following pattern: `https://hr2day.force.com/<companyname>`.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0ab3d-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-161">These values are not real.</span></span> <span data-ttu-id="0ab3d-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-162">Update these values with the actual sign-on URL and identifier.</span></span> <span data-ttu-id="0ab3d-163">Kontakta den [HR2day av Merces klienten supportteamet](mailto:servicedesk@merces.nl) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-163">Contact the [HR2day by Merces client support team](mailto:servicedesk@merces.nl) to get these values.</span></span> 
 


4. <span data-ttu-id="0ab3d-164">På den **SAML-signeringscertifikat** väljer **Certificate(Base64)**, och sedan spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-164">On the **SAML Signing Certificate** section, select **Certificate(Base64)**, and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_certificate.png) 

5. <span data-ttu-id="0ab3d-166">Det här avsnittet beskrivs hur användarna att autentisera till HR2day av Merces med sitt konto i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-166">This section describes how to enable users to authenticate to HR2day by Merces with their account in Azure AD.</span></span> <span data-ttu-id="0ab3d-167">De kan göra detta med hjälp av federation som baseras på SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-167">They do this by using federation that's based on the SAML protocol.</span></span>

    <span data-ttu-id="0ab3d-168">Din HR2day av Merces program förväntar SAML-intyg i ett specifikt format, vilket kräver att du kan lägga till anpassade attributmappning SAML-token.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-168">Your HR2day by Merces application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token.</span></span> <span data-ttu-id="0ab3d-169">Följande skärmbild visar ett exempel på detta.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-169">The following screenshot shows an example of this.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_00.png)
    
    > [!NOTE] 
    <span data-ttu-id="0ab3d-171">Innan du kan konfigurera SAML-kontroll måste du kontakta den [HR2day av Merces klienten supportteamet](mailto:servicedesk@merces.nl) och begära värdet för attributet för unik identifierare för din klient.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-171">Before you can configure the SAML assertion, you must contact the [HR2day by Merces Client support team](mailto:servicedesk@merces.nl) and request the value of the unique identifier attribute for your tenant.</span></span> <span data-ttu-id="0ab3d-172">Du behöver det här värdet för att slutföra stegen i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-172">You need this value to complete the steps in the next section.</span></span> 

6. <span data-ttu-id="0ab3d-173">I den **enkel inloggning** i dialogrutan den **användarattribut** avsnittet, konfigurera attributet SAML-token som visas i följande bild.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-173">In the **Single sign-on** dialog box, in the **User Attributes** section, configure the SAML token attribute as shown in the following image.</span></span> <span data-ttu-id="0ab3d-174">Sedan gör du följande.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-174">Then take the following steps.</span></span>
    
      | <span data-ttu-id="0ab3d-175">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="0ab3d-175">Attribute name</span></span>    |   <span data-ttu-id="0ab3d-176">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="0ab3d-176">Attribute value</span></span> |  
    | ------------------- | -------------------- |    
    | <span data-ttu-id="0ab3d-177">ATTR_LOGINCLAIM</span><span class="sxs-lookup"><span data-stu-id="0ab3d-177">ATTR_LOGINCLAIM</span></span> | <span data-ttu-id="0ab3d-178">koppling ([e] ”102938475Z” ”, @”</span><span class="sxs-lookup"><span data-stu-id="0ab3d-178">join([mail],"102938475Z","@"</span></span> |
    
      <span data-ttu-id="0ab3d-179">a.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-179">a.</span></span> <span data-ttu-id="0ab3d-180">Öppna den **lägga till attributet** markerar **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-180">To open the **Add Attribute** dialog, select **Add attribute**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="0ab3d-183">b.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-183">b.</span></span> <span data-ttu-id="0ab3d-184">I den **namn** skriver **ATTR_LOGINCLAIM**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-184">In the **Name** box, type **ATTR_LOGINCLAIM**.</span></span>

    <span data-ttu-id="0ab3d-185">c.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-185">c.</span></span> <span data-ttu-id="0ab3d-186">Från den **värdet** väljer **Join()**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-186">From the **Value** list, select **Join()**.</span></span>

    <span data-ttu-id="0ab3d-187">d.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-187">d.</span></span> <span data-ttu-id="0ab3d-188">Från den **sträng1** väljer **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-188">From the **String1** list, select **user.mail**.</span></span>

    <span data-ttu-id="0ab3d-189">e.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-189">e.</span></span> <span data-ttu-id="0ab3d-190">För **sträng2**, ange den unika identifieraren som tillhandahålls av din HR2day-teamet.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-190">For **String2**, type the unique identifier that's provided by your HR2day team.</span></span>

    <span data-ttu-id="0ab3d-191">f.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-191">f.</span></span> <span data-ttu-id="0ab3d-192">I den **avgränsare** skriver  **@** .</span><span class="sxs-lookup"><span data-stu-id="0ab3d-192">In the **Separator** box, type **@**.</span></span>
    
    <span data-ttu-id="0ab3d-193">g.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-193">g.</span></span> <span data-ttu-id="0ab3d-194">Välj **Ok**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-194">Select **Ok**.</span></span>

7. <span data-ttu-id="0ab3d-195">Välj knappen **Spara**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-195">Select the **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="0ab3d-197">I den **HR2day Merces konfigurationen** väljer **konfigurera HR2day av Merces** att öppna den **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-197">In the **HR2day by Merces Configuration** section, select **Configure HR2day by Merces** to open the **Configure sign-on** window.</span></span> <span data-ttu-id="0ab3d-198">Kopiera den **Sign-Out URL**, **SAML enhets-ID**, och **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-198">Copy the **Sign-Out URL**, **SAML Entity ID**, and **SAML Single Sign-On Service URL** from the **Quick Reference** section.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_configure.png) 

9. <span data-ttu-id="0ab3d-200">Om du vill konfigurera enkel inloggning för ditt program ska kontakta den [HR2day av Merces klienten supportteamet](mailTo:servicedesk@merces.nl).</span><span class="sxs-lookup"><span data-stu-id="0ab3d-200">To configure SSO  for your application, contact the [HR2day by Merces client support team](mailTo:servicedesk@merces.nl).</span></span> <span data-ttu-id="0ab3d-201">Bifoga den hämtade **Certificate(Base64)** filen till din e-post.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-201">Attach the downloaded **Certificate(Base64)** file to your email.</span></span> <span data-ttu-id="0ab3d-202">Ger också den **Sign-Out URL**, **SAML enhets-ID**, och **SAML inloggning tjänst-URL för enkel** så att de kan konfigureras för SSO-integrering.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-202">Also provide the **Sign-Out URL**, **SAML Entity ID**, and **SAML Single Sign-On Service URL** so that they can be configured for SSO integration.</span></span>

    > [!NOTE]
    ><span data-ttu-id="0ab3d-203">Anges till Merces-teamet att den här integreringen behöver enhets-ID anges med mönstret **https://hr2day.force.com/INSTANCENAME**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-203">Mention to the Merces team that this integration needs the Entity ID to be set with the pattern **https://hr2day.force.com/INSTANCENAME**.</span></span>

    > [!TIP]
    ><span data-ttu-id="0ab3d-204">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="0ab3d-204">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0ab3d-205">När du lägger till den här appen från den **Active Directory** > **företagsprogram** väljer den **enkel inloggning** fliken.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-205">After you add this app from the **Active Directory** > **Enterprise Applications** section, select the **Single Sign-On** tab.</span></span> <span data-ttu-id="0ab3d-206">Komma åt inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-206">Then access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0ab3d-207">Du kan läsa mer om funktionen inbäddade dokumentationen i den [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="0ab3d-207">You can read more about the embedded documentation feature in the [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="0ab3d-208">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="0ab3d-208">Create an Azure AD test user</span></span>
<span data-ttu-id="0ab3d-209">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-209">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="0ab3d-211">**Om du vill skapa en testanvändare i Azure AD, gör du följande:**</span><span class="sxs-lookup"><span data-stu-id="0ab3d-211">**To create a test user in Azure AD, take the following steps:**</span></span>

1. <span data-ttu-id="0ab3d-212">I den **Azure-portalen**, på det vänstra navigeringsfönstret väljer du den **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-212">In the **Azure portal**, on the left navigation pane, select the **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0ab3d-214">Om du vill visa en lista över användare, gå till **användare och grupper**, och välj sedan **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-214">To display the list of users, go to **Users and groups**, and then select **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0ab3d-216">Öppna den **användare** dialogrutan **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-216">To open the **User** dialog box, select **Add** on the top of the dialog box.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0ab3d-218">I den **användaren** dialogrutan rutan, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="0ab3d-218">In the **User** dialog box, take the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0ab3d-220">a.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-220">a.</span></span> <span data-ttu-id="0ab3d-221">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-221">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0ab3d-222">b.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-222">b.</span></span> <span data-ttu-id="0ab3d-223">I den **användarnamn** skriver den **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-223">In the **User name** box, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0ab3d-224">c.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-224">c.</span></span> <span data-ttu-id="0ab3d-225">Välj **visa lösenordet**, och sedan skriva ned lösenordet.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-225">Select **Show Password**, and then write down the password.</span></span>

    <span data-ttu-id="0ab3d-226">d.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-226">d.</span></span> <span data-ttu-id="0ab3d-227">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-227">Select **Create**.</span></span>
 
### <a name="create-an-hr2day-by-merces-test-user"></a><span data-ttu-id="0ab3d-228">Skapa en HR2day av Merces testanvändare</span><span class="sxs-lookup"><span data-stu-id="0ab3d-228">Create an HR2day by Merces test user</span></span>

<span data-ttu-id="0ab3d-229">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i HR2day av Merces.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-229">The objective of this section is to create a user called Britta Simon in HR2day by Merces.</span></span> <span data-ttu-id="0ab3d-230">Om du vill lägga till användare i kontot HR2day arbeta med den [HR2day av Merces klienten supportteamet](mailto:servicedesk@merces.nl).</span><span class="sxs-lookup"><span data-stu-id="0ab3d-230">To add the users in the HR2day account, work with the [HR2day by Merces client support team](mailto:servicedesk@merces.nl).</span></span> 

> [!NOTE]
> <span data-ttu-id="0ab3d-231">Om du behöver skapa en användare manuellt kan kontakta den [HR2day av Merces klienten supportteamet](mailto:servicedesk@merces.nl).</span><span class="sxs-lookup"><span data-stu-id="0ab3d-231">If you need to create a user manually, contact the [HR2day by Merces client support team](mailto:servicedesk@merces.nl).</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="0ab3d-232">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="0ab3d-232">Assign the Azure AD test user</span></span>

<span data-ttu-id="0ab3d-233">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till HR2day av Merces.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-233">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to HR2day by Merces.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="0ab3d-235">**Om du vill tilldela HR2day av Merces Britta Simon gör du följande:**</span><span class="sxs-lookup"><span data-stu-id="0ab3d-235">**To assign Britta Simon to HR2day by Merces, take the following steps:**</span></span>

1. <span data-ttu-id="0ab3d-236">Öppna vyn program i Azure-portalen, gå till vyn katalog och gå sedan till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-236">In the Azure portal, open the applications view, go to the directory view, and then go to **Enterprise applications**.</span></span> <span data-ttu-id="0ab3d-237">Välj därefter **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-237">Next, select **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="0ab3d-239">Välj i listan med program **HR2day av Merces**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-239">In the applications list, select **HR2day by Merces**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_app.png) 

3. <span data-ttu-id="0ab3d-241">Välj på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-241">In the menu on the left, select **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="0ab3d-243">Välj den **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-243">Select the **Add** button.</span></span> <span data-ttu-id="0ab3d-244">I den **Lägg uppdrag** dialogrutan **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-244">Then, in the **Add Assignment** dialog box, select **Users and groups**.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="0ab3d-246">I den **användare och grupper** i dialogrutan den **användare** väljer **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-246">In the **Users and groups** dialog box, in the **Users** list, select **Britta Simon**.</span></span>

6. <span data-ttu-id="0ab3d-247">Klicka på den **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-247">Click the **Select** button.</span></span>

7. <span data-ttu-id="0ab3d-248">I den **Lägg uppdrag** dialogrutan **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-248">In the **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="0ab3d-249">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0ab3d-249">Test single sign-on</span></span>

<span data-ttu-id="0ab3d-250">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-250">The objective of this section is to test your Azure AD single sign-on configuration by using the Access Panel.</span></span>  

<span data-ttu-id="0ab3d-251">När du markerar HR2day genom Merces panelen på åtkomstpanelen loggas du automatiskt komma till din HR2day av Merces program.</span><span class="sxs-lookup"><span data-stu-id="0ab3d-251">When you select the HR2day by Merces tile in the Access Panel, you automatically get signed in  to your HR2day by Merces application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0ab3d-252">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="0ab3d-252">Additional resources</span></span>

* [<span data-ttu-id="0ab3d-253">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0ab3d-253">List of tutorials about how to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0ab3d-254">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0ab3d-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_203.png

