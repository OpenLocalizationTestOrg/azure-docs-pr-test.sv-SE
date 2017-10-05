---
title: "Självstudier: Azure Active Directory-integrering med etouches | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och etouches."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 76cccaa8-859c-4c16-9d1d-8a6496fc7520
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 3cd9e9d6aae924369065ca492b1f6380c0ddc5fe
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-etouches"></a><span data-ttu-id="b5aa8-103">Självstudier: Azure Active Directory-integrering med etouches</span><span class="sxs-lookup"><span data-stu-id="b5aa8-103">Tutorial: Azure Active Directory integration with etouches</span></span>

<span data-ttu-id="b5aa8-104">I kursen får lära du att integrera etouches med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b5aa8-104">In this tutorial, you learn how to integrate etouches with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b5aa8-105">Integrera etouches med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b5aa8-105">Integrating etouches with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b5aa8-106">Du kan styra i Azure AD som har åtkomst till etouches</span><span class="sxs-lookup"><span data-stu-id="b5aa8-106">You can control in Azure AD who has access to etouches</span></span>
- <span data-ttu-id="b5aa8-107">Du kan aktivera användarna att automatiskt hämta loggat in på etouches (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b5aa8-107">You can enable your users to automatically get signed-on to etouches (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b5aa8-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b5aa8-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b5aa8-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b5aa8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b5aa8-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b5aa8-110">Prerequisites</span></span>

<span data-ttu-id="b5aa8-111">För att konfigurera Azure AD-integrering med etouches, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="b5aa8-111">To configure Azure AD integration with etouches, you need the following items:</span></span>

- <span data-ttu-id="b5aa8-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b5aa8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b5aa8-113">En etouches enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="b5aa8-113">An etouches single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b5aa8-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b5aa8-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b5aa8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b5aa8-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b5aa8-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b5aa8-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b5aa8-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b5aa8-118">Scenario description</span></span>
<span data-ttu-id="b5aa8-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b5aa8-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b5aa8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b5aa8-121">Att lägga till etouches från galleriet</span><span class="sxs-lookup"><span data-stu-id="b5aa8-121">Adding etouches from the gallery</span></span>
2. <span data-ttu-id="b5aa8-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b5aa8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-etouches-from-the-gallery"></a><span data-ttu-id="b5aa8-123">Att lägga till etouches från galleriet</span><span class="sxs-lookup"><span data-stu-id="b5aa8-123">Adding etouches from the gallery</span></span>
<span data-ttu-id="b5aa8-124">Du måste lägga till etouches från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av etouches i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-124">To configure the integration of etouches into Azure AD, you need to add etouches from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b5aa8-125">**Utför följande steg för att lägga till etouches från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="b5aa8-125">**To add etouches from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b5aa8-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="b5aa8-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b5aa8-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="b5aa8-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="b5aa8-133">I sökrutan skriver **etouches**väljer **etouches** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-133">In the search box, type **etouches**, select **etouches** from result panel then click **Add** button to add the application.</span></span>

    ![etouches i resultatlistan](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="b5aa8-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b5aa8-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="b5aa8-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med etouches baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-136">In this section, you configure and test Azure AD single sign-on with etouches based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b5aa8-137">Azure AD måste du känna till användaren i etouches motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-137">For single sign-on to work, Azure AD needs to know what the counterpart user in etouches is to a user in Azure AD.</span></span> <span data-ttu-id="b5aa8-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i etouches upprättas.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-138">In other words, a link relationship between an Azure AD user and the related user in etouches needs to be established.</span></span>

<span data-ttu-id="b5aa8-139">I etouches, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-139">In etouches, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="b5aa8-140">Om du vill konfigurera och testa Azure AD enkel inloggning med etouches, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b5aa8-140">To configure and test Azure AD single sign-on with etouches, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b5aa8-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b5aa8-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b5aa8-143">**[Skapa en testanvändare etouches](#create-an-etouches-test-user)**  – du har en motsvarighet för Britta Simon i etouches som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-143">**[Create an etouches test user](#create-an-etouches-test-user)** - to have a counterpart of Britta Simon in etouches that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b5aa8-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b5aa8-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="b5aa8-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b5aa8-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="b5aa8-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt etouches program.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your etouches application.</span></span>

<span data-ttu-id="b5aa8-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med etouches:**</span><span class="sxs-lookup"><span data-stu-id="b5aa8-148">**To configure Azure AD single sign-on with etouches, perform the following steps:**</span></span>

1. <span data-ttu-id="b5aa8-149">I Azure-portalen på den **etouches** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-149">In the Azure portal, on the **etouches** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="b5aa8-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_samlbase.png)

3. <span data-ttu-id="b5aa8-153">På den **etouches domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b5aa8-153">On the **etouches Domain and URLs** section, perform the following steps:</span></span>

    ![etouches domän och URL: er med enkel inloggning information](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_url.png)

    <span data-ttu-id="b5aa8-155">a.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-155">a.</span></span> <span data-ttu-id="b5aa8-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>`</span><span class="sxs-lookup"><span data-stu-id="b5aa8-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>`</span></span>

    <span data-ttu-id="b5aa8-157">b.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-157">b.</span></span> <span data-ttu-id="b5aa8-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://www.eiseverywhere.com/<instance name>`</span><span class="sxs-lookup"><span data-stu-id="b5aa8-158">In the **Identifier** textbox, type a URL using the following pattern: `https://www.eiseverywhere.com/<instance name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b5aa8-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-159">These values are not real.</span></span> <span data-ttu-id="b5aa8-160">Du uppdaterar värdet med det faktiska inloggning URL och identifierare som beskrivs senare i självstudierna.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-160">You update the value with the actual Sign on URL and Identifier, which is explained later in the tutorial.</span></span>
    > 

4. <span data-ttu-id="b5aa8-161">etouches program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-161">etouches application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="b5aa8-162">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-162">Configure the following claims for this application.</span></span> <span data-ttu-id="b5aa8-163">Du kan hantera värden för attributen från den **användarattribut** av programmet.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-163">You can manage the values of these attributes from the **User Attribute** of the application.</span></span> <span data-ttu-id="b5aa8-164">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-164">The following screenshot shows an example for this.</span></span> 

    ![Användarattribut](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_attribute.png) 

5. <span data-ttu-id="b5aa8-166">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b5aa8-166">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="b5aa8-167">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="b5aa8-167">Attribute Name</span></span> | <span data-ttu-id="b5aa8-168">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="b5aa8-168">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="b5aa8-169">E-post</span><span class="sxs-lookup"><span data-stu-id="b5aa8-169">Email</span></span> | <span data-ttu-id="b5aa8-170">User.Mail</span><span class="sxs-lookup"><span data-stu-id="b5aa8-170">user.mail</span></span> |    
    
    <span data-ttu-id="b5aa8-171">a.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-171">a.</span></span> <span data-ttu-id="b5aa8-172">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-172">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Lägg till attribut](./media/active-directory-saas-etouches-tutorial/tutorial_attribute_04.png)

    ![Attributet dialogrutan Lägg till](./media/active-directory-saas-etouches-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="b5aa8-175">b.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-175">b.</span></span> <span data-ttu-id="b5aa8-176">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-176">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="b5aa8-177">c.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-177">c.</span></span> <span data-ttu-id="b5aa8-178">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-178">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="b5aa8-179">d.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-179">d.</span></span> <span data-ttu-id="b5aa8-180">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-180">Click **Ok**.</span></span> 

6. <span data-ttu-id="b5aa8-181">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-181">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_certificate.png) 

7. <span data-ttu-id="b5aa8-183">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-183">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-etouches-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="b5aa8-185">Utför följande steg för att få SSO konfigurerats för ditt program i etouches-program:</span><span class="sxs-lookup"><span data-stu-id="b5aa8-185">To get SSO configured for your application, perform the following steps in the etouches application:</span></span> 

    ![etouches konfiguration](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_06.png) 

    <span data-ttu-id="b5aa8-187">a.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-187">a.</span></span> <span data-ttu-id="b5aa8-188">Logga in på **etouches** program med hjälp av administratörsrättigheter.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-188">Login to **etouches** application using the Admin rights.</span></span>
   
    <span data-ttu-id="b5aa8-189">b.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-189">b.</span></span> <span data-ttu-id="b5aa8-190">Gå till den **SAML** konfiguration.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-190">Go to the **SAML** Configuration.</span></span>

    <span data-ttu-id="b5aa8-191">c.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-191">c.</span></span> <span data-ttu-id="b5aa8-192">I den **allmänna inställningar** avsnittet, öppna hämtade certifikatet från Azure-portalen i anteckningar, kopiera innehållet och klistra in den i textrutan IDP metadata.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-192">In the **General Settings** section, open your downloaded certificate from Azure portal in notepad, copy the content, and then paste it into the IDP metadata textbox.</span></span> 

    <span data-ttu-id="b5aa8-193">d.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-193">d.</span></span> <span data-ttu-id="b5aa8-194">Klicka på den **Spara & förblir** knappen.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-194">Click on the **Save & Stay** button.</span></span>
  
    <span data-ttu-id="b5aa8-195">e.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-195">e.</span></span> <span data-ttu-id="b5aa8-196">Klicka på den **uppdateringsmetadata** i avsnittet SAML-Metadata.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-196">Click on the **Update Metadata** button in the SAML Metadata section.</span></span> 

    <span data-ttu-id="b5aa8-197">f.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-197">f.</span></span> <span data-ttu-id="b5aa8-198">Detta öppnar sidan och utföra en enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-198">This opens the page and perform SSO.</span></span> <span data-ttu-id="b5aa8-199">När SSO fungerar kan du ställa in användarnamnet.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-199">Once the SSO is working then you can set up the username.</span></span>

    <span data-ttu-id="b5aa8-200">g.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-200">g.</span></span> <span data-ttu-id="b5aa8-201">I fältet för användarnamn, väljer du den **e-postadress** som visas i bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-201">In the Username field, select the **emailaddress** as shown in the image below.</span></span> 

    <span data-ttu-id="b5aa8-202">h.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-202">h.</span></span> <span data-ttu-id="b5aa8-203">Kopiera den **SP enhets-ID** värdet och klistrar in det i den **identifierare** textruta som finns i **etouches domän och URL: er** avsnitt på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-203">Copy the **SP entity ID** value and paste it into the **Identifier**  textbox, which is in **etouches Domain and URLs** section on Azure portal.</span></span>

    <span data-ttu-id="b5aa8-204">Jag.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-204">i.</span></span> <span data-ttu-id="b5aa8-205">Kopiera den **SSO URL / ACS** värdet och klistrar in det i den **inloggning URL** textruta som finns i **etouches domän och URL: er** avsnitt på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-205">Copy the **SSO URL / ACS** value and paste it into the **Sign on URL** textbox, which is in **etouches Domain and URLs** section on Azure portal.</span></span>
   
> [!TIP]
> <span data-ttu-id="b5aa8-206">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="b5aa8-206">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b5aa8-207">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-207">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b5aa8-208">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b5aa8-208">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="b5aa8-209">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b5aa8-209">Create an Azure AD test user</span></span>
<span data-ttu-id="b5aa8-210">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-210">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="b5aa8-212">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="b5aa8-212">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b5aa8-213">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-213">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-etouches-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b5aa8-215">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-215">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-etouches-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b5aa8-217">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-217">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Knappen Lägg till](./media/active-directory-saas-etouches-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b5aa8-219">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b5aa8-219">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Dialogrutan användare](./media/active-directory-saas-etouches-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b5aa8-221">a.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-221">a.</span></span> <span data-ttu-id="b5aa8-222">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-222">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b5aa8-223">b.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-223">b.</span></span> <span data-ttu-id="b5aa8-224">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-224">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b5aa8-225">c.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-225">c.</span></span> <span data-ttu-id="b5aa8-226">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-226">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b5aa8-227">d.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-227">d.</span></span> <span data-ttu-id="b5aa8-228">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-228">Click **Create**.</span></span>
 
### <a name="create-an-etouches-test-user"></a><span data-ttu-id="b5aa8-229">Skapa en testanvändare etouches</span><span class="sxs-lookup"><span data-stu-id="b5aa8-229">Create an etouches test user</span></span>

<span data-ttu-id="b5aa8-230">I det här avsnittet skapar du en användare som kallas Britta Simon i etouches.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-230">In this section, you create a user called Britta Simon in etouches.</span></span> <span data-ttu-id="b5aa8-231">Arbeta med [etouches klienten supportteam](https://www.etouches.com/event-software/support/customer-support/) att lägga till användare i etouches-plattformen.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-231">Work with [etouches Client support team](https://www.etouches.com/event-software/support/customer-support/) to add the users in the etouches platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="b5aa8-232">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="b5aa8-232">Assign the Azure AD test user</span></span>

<span data-ttu-id="b5aa8-233">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till etouches.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to etouches.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="b5aa8-235">**Om du vill tilldela etouches Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b5aa8-235">**To assign Britta Simon to etouches, perform the following steps:**</span></span>

1. <span data-ttu-id="b5aa8-236">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b5aa8-238">Välj i listan med program **etouches**.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-238">In the applications list, select **etouches**.</span></span>

    ![Länken etouches i listan med program](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_app.png) 

3. <span data-ttu-id="b5aa8-240">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202] 

4. <span data-ttu-id="b5aa8-242">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-242">Click **Add** button.</span></span> <span data-ttu-id="b5aa8-243">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="b5aa8-245">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b5aa8-246">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b5aa8-247">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="b5aa8-248">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b5aa8-248">Test single sign-on</span></span>


<span data-ttu-id="b5aa8-249">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-249">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b5aa8-250">När du klickar på panelen etouches på åtkomstpanelen du bör få automatiskt loggat in på ditt etouches program.</span><span class="sxs-lookup"><span data-stu-id="b5aa8-250">When you click the etouches tile in the Access Panel, you should get automatically signed-on to your etouches application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5aa8-251">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b5aa8-251">Additional resources</span></span>

* [<span data-ttu-id="b5aa8-252">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b5aa8-252">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b5aa8-253">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b5aa8-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_203.png

