---
title: "Självstudier: Azure Active Directory-integrering med Onit | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Onit."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bc479a28-8fcd-493f-ac53-681975a5149c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.openlocfilehash: 47c0055b89dbcf6a30a7f9ac5a33913e7bf463fa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-onit"></a><span data-ttu-id="304e7-103">Självstudier: Azure Active Directory-integrering med Onit</span><span class="sxs-lookup"><span data-stu-id="304e7-103">Tutorial: Azure Active Directory integration with Onit</span></span>

<span data-ttu-id="304e7-104">I kursen får lära du att integrera Onit med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="304e7-104">In this tutorial, you learn how to integrate Onit with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="304e7-105">Integrera Onit med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="304e7-105">Integrating Onit with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="304e7-106">Du kan styra i Azure AD som har åtkomst till Onit.</span><span class="sxs-lookup"><span data-stu-id="304e7-106">You can control in Azure AD who has access to Onit.</span></span>
- <span data-ttu-id="304e7-107">Du kan aktivera användarna att automatiskt hämta loggat in på Onit (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="304e7-107">You can enable your users to automatically get signed-on to Onit (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="304e7-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="304e7-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="304e7-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="304e7-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="304e7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="304e7-110">Prerequisites</span></span>

<span data-ttu-id="304e7-111">För att konfigurera Azure AD-integrering med Onit, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="304e7-111">To configure Azure AD integration with Onit, you need the following items:</span></span>

- <span data-ttu-id="304e7-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="304e7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="304e7-113">En Onit enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="304e7-113">An Onit single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="304e7-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="304e7-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="304e7-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="304e7-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="304e7-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="304e7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="304e7-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="304e7-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="304e7-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="304e7-118">Scenario description</span></span>

<span data-ttu-id="304e7-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="304e7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="304e7-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="304e7-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="304e7-121">Att lägga till Onit från galleriet</span><span class="sxs-lookup"><span data-stu-id="304e7-121">Adding Onit from the gallery</span></span>
2. <span data-ttu-id="304e7-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="304e7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-onit-from-the-gallery"></a><span data-ttu-id="304e7-123">Att lägga till Onit från galleriet</span><span class="sxs-lookup"><span data-stu-id="304e7-123">Adding Onit from the gallery</span></span>
<span data-ttu-id="304e7-124">Du måste lägga till Onit från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Onit i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="304e7-124">To configure the integration of Onit into Azure AD, you need to add Onit from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="304e7-125">**Utför följande steg för att lägga till Onit från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="304e7-125">**To add Onit from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="304e7-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="304e7-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="304e7-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="304e7-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="304e7-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="304e7-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="304e7-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="304e7-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="304e7-133">I sökrutan skriver **Onit**väljer **Onit** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="304e7-133">In the search box, type **Onit**, select **Onit** from result panel then click **Add** button to add the application.</span></span>

    ![Onit i resultatlistan](./media/active-directory-saas-onit-tutorial/tutorial_onit_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="304e7-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="304e7-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="304e7-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Onit baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="304e7-136">In this section, you configure and test Azure AD single sign-on with Onit based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="304e7-137">Azure AD måste du känna till användaren i Onit motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="304e7-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Onit is to a user in Azure AD.</span></span> <span data-ttu-id="304e7-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Onit upprättas.</span><span class="sxs-lookup"><span data-stu-id="304e7-138">In other words, a link relationship between an Azure AD user and the related user in Onit needs to be established.</span></span>

<span data-ttu-id="304e7-139">I Onit, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="304e7-139">In Onit, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="304e7-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Onit, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="304e7-140">To configure and test Azure AD single sign-on with Onit, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="304e7-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="304e7-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="304e7-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="304e7-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="304e7-143">**[Skapa en testanvändare Onit](#create-an-onit-test-user)**  – du har en motsvarighet för Britta Simon i Onit som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="304e7-143">**[Create an Onit test user](#create-an-onit-test-user)** - to have a counterpart of Britta Simon in Onit that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="304e7-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="304e7-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="304e7-145">**[Testa enkel inloggning](#test-single-sign-on)**  att kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="304e7-145">**[Test single sign-on](#test-single-sign-on)** to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="304e7-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="304e7-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="304e7-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Onit program.</span><span class="sxs-lookup"><span data-stu-id="304e7-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Onit application.</span></span>

<span data-ttu-id="304e7-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Onit:**</span><span class="sxs-lookup"><span data-stu-id="304e7-148">**To configure Azure AD single sign-on with Onit, perform the following steps:**</span></span>

1. <span data-ttu-id="304e7-149">I Azure-portalen på den **Onit** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="304e7-149">In the Azure portal, on the **Onit** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="304e7-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="304e7-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-onit-tutorial/tutorial_onit_samlbase.png)

3. <span data-ttu-id="304e7-153">På den **Onit domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="304e7-153">On the **Onit Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och Onit domän med enkel inloggning information](./media/active-directory-saas-onit-tutorial/tutorial_onit_url.png)

    <span data-ttu-id="304e7-155">a.</span><span class="sxs-lookup"><span data-stu-id="304e7-155">a.</span></span> <span data-ttu-id="304e7-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<sub-domain>.onit.com`</span><span class="sxs-lookup"><span data-stu-id="304e7-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<sub-domain>.onit.com`</span></span>

    <span data-ttu-id="304e7-157">b.</span><span class="sxs-lookup"><span data-stu-id="304e7-157">b.</span></span> <span data-ttu-id="304e7-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<sub-domain>.onit.com`</span><span class="sxs-lookup"><span data-stu-id="304e7-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<sub-domain>.onit.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="304e7-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="304e7-159">These values are not real.</span></span> <span data-ttu-id="304e7-160">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="304e7-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="304e7-161">Kontakta [Onit klienten supportteamet](https://www.onit.com/support) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="304e7-161">Contact [Onit Client support team](https://www.onit.com/support) to get these values.</span></span> 
 
4. <span data-ttu-id="304e7-162">På den **SAML-signeringscertifikat** avsnittet, kopiera den **TUMAVTRYCKET** värdet för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="304e7-162">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-onit-tutorial/tutorial_onit_certificate.png) 

5. <span data-ttu-id="304e7-164">Onit program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="304e7-164">Onit application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="304e7-165">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="304e7-165">Please configure the following claims for this application.</span></span> <span data-ttu-id="304e7-166">Du kan hantera värden för attributen från den **”Atrribute”** för programmet.</span><span class="sxs-lookup"><span data-stu-id="304e7-166">You can manage the values of these attributes from the **"Atrribute"** tab of the application.</span></span> <span data-ttu-id="304e7-167">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="304e7-167">The following screenshot shows an example for this.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-onit-tutorial/tutorial_onit_attribute.png) 

6. <span data-ttu-id="304e7-169">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="304e7-169">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="304e7-170">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="304e7-170">Attribute Name</span></span> | <span data-ttu-id="304e7-171">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="304e7-171">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="304e7-172">E-post</span><span class="sxs-lookup"><span data-stu-id="304e7-172">email</span></span> | <span data-ttu-id="304e7-173">User.Mail</span><span class="sxs-lookup"><span data-stu-id="304e7-173">user.mail</span></span> |
    
    <span data-ttu-id="304e7-174">a.</span><span class="sxs-lookup"><span data-stu-id="304e7-174">a.</span></span> <span data-ttu-id="304e7-175">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="304e7-175">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-onit-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-onit-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="304e7-178">b.</span><span class="sxs-lookup"><span data-stu-id="304e7-178">b.</span></span> <span data-ttu-id="304e7-179">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="304e7-179">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="304e7-180">c.</span><span class="sxs-lookup"><span data-stu-id="304e7-180">c.</span></span> <span data-ttu-id="304e7-181">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="304e7-181">From the **Value** list, type the attribute value shown for that row.</span></span>

    <span data-ttu-id="304e7-182">d.</span><span class="sxs-lookup"><span data-stu-id="304e7-182">d.</span></span> <span data-ttu-id="304e7-183">Lämna den **Namespace** tomt.</span><span class="sxs-lookup"><span data-stu-id="304e7-183">Leave the **Namespace** blank.</span></span>
    
    <span data-ttu-id="304e7-184">e.</span><span class="sxs-lookup"><span data-stu-id="304e7-184">e.</span></span> <span data-ttu-id="304e7-185">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="304e7-185">Click **Ok**.</span></span>

7. <span data-ttu-id="304e7-186">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="304e7-186">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-onit-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="304e7-188">På den **Onit Configuration** klickar du på **konfigurera Onit** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="304e7-188">On the **Onit Configuration** section, click **Configure Onit** to open **Configure sign-on** window.</span></span> <span data-ttu-id="304e7-189">Kopiera den **Sign-Out URL, SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="304e7-189">Copy the **Sign-Out URL, SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Onit konfiguration](./media/active-directory-saas-onit-tutorial/tutorial_onit_configure.png)

9. <span data-ttu-id="304e7-191">Logga in på webbplatsen Onit företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="304e7-191">In a different web browser window, log into your Onit company site as an administrator.</span></span>

10. <span data-ttu-id="304e7-192">Klicka på menyn högst upp **Administration**.</span><span class="sxs-lookup"><span data-stu-id="304e7-192">In the menu on the top, click **Administration**.</span></span>
   
   <span data-ttu-id="304e7-193">![Administration](./media/active-directory-saas-onit-tutorial/IC791174.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="304e7-193">![Administration](./media/active-directory-saas-onit-tutorial/IC791174.png "Administration")</span></span>
11. <span data-ttu-id="304e7-194">Klicka på **redigera Corporation**.</span><span class="sxs-lookup"><span data-stu-id="304e7-194">Click **Edit Corporation**.</span></span>
   
   <span data-ttu-id="304e7-195">![Redigera Corporation](./media/active-directory-saas-onit-tutorial/IC791175.png "redigera Corporation")</span><span class="sxs-lookup"><span data-stu-id="304e7-195">![Edit Corporation](./media/active-directory-saas-onit-tutorial/IC791175.png "Edit Corporation")</span></span>
   
12. <span data-ttu-id="304e7-196">Klicka på den **säkerhet** fliken.</span><span class="sxs-lookup"><span data-stu-id="304e7-196">Click the **Security** tab.</span></span>
    
    <span data-ttu-id="304e7-197">![Redigera företagsinformation](./media/active-directory-saas-onit-tutorial/IC791176.png "Redigera företagsinformation")</span><span class="sxs-lookup"><span data-stu-id="304e7-197">![Edit Company Information](./media/active-directory-saas-onit-tutorial/IC791176.png "Edit Company Information")</span></span>

13. <span data-ttu-id="304e7-198">På den **säkerhet** fliken, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="304e7-198">On the **Security** tab, perform the following steps:</span></span>

    <span data-ttu-id="304e7-199">![Enkel inloggning](./media/active-directory-saas-onit-tutorial/IC791177.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="304e7-199">![Single Sign-On](./media/active-directory-saas-onit-tutorial/IC791177.png "Single Sign-On")</span></span>

    <span data-ttu-id="304e7-200">a.</span><span class="sxs-lookup"><span data-stu-id="304e7-200">a.</span></span> <span data-ttu-id="304e7-201">Som **autentiseringsstrategi**väljer **enkel inloggning och lösenord**.</span><span class="sxs-lookup"><span data-stu-id="304e7-201">As **Authentication Strategy**, select **Single Sign On and Password**.</span></span>
    
    <span data-ttu-id="304e7-202">b.</span><span class="sxs-lookup"><span data-stu-id="304e7-202">b.</span></span> <span data-ttu-id="304e7-203">I **Idp mål-URL** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="304e7-203">In **Idp Target URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="304e7-204">c.</span><span class="sxs-lookup"><span data-stu-id="304e7-204">c.</span></span> <span data-ttu-id="304e7-205">I **Idp logga ut URL** textruta klistra in värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="304e7-205">In **Idp logout URL** textbox, paste the value of  **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="304e7-206">d.</span><span class="sxs-lookup"><span data-stu-id="304e7-206">d.</span></span> <span data-ttu-id="304e7-207">I **Idp Cert fingeravtryck (SHA1)** textruta klistra in den **tumavtrycket** värdet för certifikat som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="304e7-207">In **Idp Cert Fingerprint (SHA1)** textbox, paste the  **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span>

> [!TIP]
> <span data-ttu-id="304e7-208">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="304e7-208">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="304e7-209">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="304e7-209">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="304e7-210">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="304e7-210">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="304e7-211">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="304e7-211">Create an Azure AD test user</span></span>

<span data-ttu-id="304e7-212">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="304e7-212">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="304e7-214">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="304e7-214">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="304e7-215">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="304e7-215">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-onit-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="304e7-217">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="304e7-217">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-onit-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="304e7-219">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="304e7-219">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-onit-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="304e7-221">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="304e7-221">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-onit-tutorial/create_aaduser_04.png)

    <span data-ttu-id="304e7-223">a.</span><span class="sxs-lookup"><span data-stu-id="304e7-223">a.</span></span> <span data-ttu-id="304e7-224">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="304e7-224">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="304e7-225">b.</span><span class="sxs-lookup"><span data-stu-id="304e7-225">b.</span></span> <span data-ttu-id="304e7-226">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="304e7-226">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="304e7-227">c.</span><span class="sxs-lookup"><span data-stu-id="304e7-227">c.</span></span> <span data-ttu-id="304e7-228">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="304e7-228">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="304e7-229">d.</span><span class="sxs-lookup"><span data-stu-id="304e7-229">d.</span></span> <span data-ttu-id="304e7-230">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="304e7-230">Click **Create**.</span></span>
 
### <a name="create-an-onit-test-user"></a><span data-ttu-id="304e7-231">Skapa en testanvändare Onit</span><span class="sxs-lookup"><span data-stu-id="304e7-231">Create an Onit test user</span></span>

<span data-ttu-id="304e7-232">För att aktivera Azure AD-användare att logga in på Onit etableras de i Onit.</span><span class="sxs-lookup"><span data-stu-id="304e7-232">In order to enable Azure AD users to log into Onit, they must be provisioned into Onit.</span></span>  

<span data-ttu-id="304e7-233">När det gäller Onit är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="304e7-233">In the case of Onit, provisioning is a manual task.</span></span>

<span data-ttu-id="304e7-234">**Utför följande steg för att konfigurera användaretablering:**</span><span class="sxs-lookup"><span data-stu-id="304e7-234">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="304e7-235">Logga in på ditt **Onit** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="304e7-235">Sign on to your **Onit** company site as an administrator.</span></span>
2. <span data-ttu-id="304e7-236">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="304e7-236">Click **Add User**.</span></span>
   
   <span data-ttu-id="304e7-237">![Administration](./media/active-directory-saas-onit-tutorial/IC791180.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="304e7-237">![Administration](./media/active-directory-saas-onit-tutorial/IC791180.png "Administration")</span></span>
3. <span data-ttu-id="304e7-238">På den **Lägg till användare** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="304e7-238">On the **Add User** dialog page, perform the following steps:</span></span>
   
   <span data-ttu-id="304e7-239">![Lägg till användare](./media/active-directory-saas-onit-tutorial/IC791181.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="304e7-239">![Add User](./media/active-directory-saas-onit-tutorial/IC791181.png "Add User")</span></span>
   
  1. <span data-ttu-id="304e7-240">Typ av **namn** och **e-postadress** av ett giltigt Azure AD-kontot som du vill etablera i relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="304e7-240">Type the **Name** and the **Email Address** of a valid Azure AD account you want to provision into the related textboxes.</span></span>
  2. <span data-ttu-id="304e7-241">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="304e7-241">Click **Create**.</span></span>    
   
 > [!NOTE]
 > <span data-ttu-id="304e7-242">Azure Active Directory kontoinnehavaren får ett e-postmeddelande och följer en länk för att bekräfta sina konton innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="304e7-242">The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="304e7-243">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="304e7-243">Assign the Azure AD test user</span></span>

<span data-ttu-id="304e7-244">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Onit.</span><span class="sxs-lookup"><span data-stu-id="304e7-244">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Onit.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="304e7-246">**Om du vill tilldela Onit Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="304e7-246">**To assign Britta Simon to Onit, perform the following steps:**</span></span>

1. <span data-ttu-id="304e7-247">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="304e7-247">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="304e7-249">Välj i listan med program **Onit**.</span><span class="sxs-lookup"><span data-stu-id="304e7-249">In the applications list, select **Onit**.</span></span>

    ![Länken Onit i listan med program](./media/active-directory-saas-onit-tutorial/tutorial_onit_app.png)  

3. <span data-ttu-id="304e7-251">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="304e7-251">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="304e7-253">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="304e7-253">Click **Add** button.</span></span> <span data-ttu-id="304e7-254">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="304e7-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="304e7-256">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="304e7-256">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="304e7-257">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="304e7-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="304e7-258">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="304e7-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="304e7-259">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="304e7-259">Test single sign-on</span></span>

<span data-ttu-id="304e7-260">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="304e7-260">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="304e7-261">När du klickar på panelen Onit på åtkomstpanelen du bör få automatiskt loggat in på ditt Onit program.</span><span class="sxs-lookup"><span data-stu-id="304e7-261">When you click the Onit tile in the Access Panel, you should get automatically signed-on to your Onit application.</span></span>
<span data-ttu-id="304e7-262">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="304e7-262">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="304e7-263">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="304e7-263">Additional resources</span></span>

* [<span data-ttu-id="304e7-264">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="304e7-264">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="304e7-265">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="304e7-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-onit-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-onit-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-onit-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-onit-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-onit-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-onit-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-onit-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-onit-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-onit-tutorial/tutorial_general_203.png

