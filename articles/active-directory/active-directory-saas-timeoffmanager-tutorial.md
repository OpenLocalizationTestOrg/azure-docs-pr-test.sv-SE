---
title: "Självstudier: Azure Active Directory-integrering med TimeOffManager | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och TimeOffManager."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3685912f-d5aa-4730-ab58-35a088fc1cc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 3f944ffbf704694b293b4b1e5bdb4f2c93ae35a1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a><span data-ttu-id="d5629-103">Självstudier: Azure Active Directory-integrering med TimeOffManager</span><span class="sxs-lookup"><span data-stu-id="d5629-103">Tutorial: Azure Active Directory integration with TimeOffManager</span></span>

<span data-ttu-id="d5629-104">I kursen får lära du att integrera TimeOffManager med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d5629-104">In this tutorial, you learn how to integrate TimeOffManager with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d5629-105">Integrera TimeOffManager med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d5629-105">Integrating TimeOffManager with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d5629-106">Du kan styra i Azure AD som har åtkomst till TimeOffManager</span><span class="sxs-lookup"><span data-stu-id="d5629-106">You can control in Azure AD who has access to TimeOffManager</span></span>
- <span data-ttu-id="d5629-107">Du kan aktivera användarna att automatiskt hämta loggat in på TimeOffManager (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d5629-107">You can enable your users to automatically get signed-on to TimeOffManager (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d5629-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d5629-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d5629-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d5629-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d5629-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d5629-110">Prerequisites</span></span>

<span data-ttu-id="d5629-111">För att konfigurera Azure AD-integrering med TimeOffManager, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="d5629-111">To configure Azure AD integration with TimeOffManager, you need the following items:</span></span>

- <span data-ttu-id="d5629-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d5629-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d5629-113">En TimeOffManager enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="d5629-113">A TimeOffManager single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d5629-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d5629-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d5629-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d5629-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d5629-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d5629-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d5629-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d5629-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d5629-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d5629-118">Scenario description</span></span>
<span data-ttu-id="d5629-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d5629-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d5629-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d5629-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d5629-121">Lägg till TimeOffManager från galleriet</span><span class="sxs-lookup"><span data-stu-id="d5629-121">Add TimeOffManager from the gallery</span></span>
2. <span data-ttu-id="d5629-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d5629-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-timeoffmanager-from-the-gallery"></a><span data-ttu-id="d5629-123">Lägg till TimeOffManager från galleriet</span><span class="sxs-lookup"><span data-stu-id="d5629-123">Add TimeOffManager from the gallery</span></span>
<span data-ttu-id="d5629-124">Du måste lägga till TimeOffManager från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av TimeOffManager i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d5629-124">To configure the integration of TimeOffManager into Azure AD, you need to add TimeOffManager from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d5629-125">**Utför följande steg för att lägga till TimeOffManager från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="d5629-125">**To add TimeOffManager from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d5629-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d5629-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d5629-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d5629-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d5629-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d5629-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d5629-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d5629-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d5629-133">I sökrutan skriver **TimeOffManager**väljer **TimeOffManager** från resultatet Kontrollpanelen och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="d5629-133">In the search box, type **TimeOffManager**, select **TimeOffManager** from result panel and then click **Add** button to add the application.</span></span>

    ![Lägg till från galleriet](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="d5629-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d5629-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="d5629-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med TimeOffManager baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d5629-136">In this section, you configure and test Azure AD single sign-on with TimeOffManager based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d5629-137">Azure AD måste du känna till användaren i TimeOffManager motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="d5629-137">For single sign-on to work, Azure AD needs to know what the counterpart user in TimeOffManager is to a user in Azure AD.</span></span> <span data-ttu-id="d5629-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i TimeOffManager upprättas.</span><span class="sxs-lookup"><span data-stu-id="d5629-138">In other words, a link relationship between an Azure AD user and the related user in TimeOffManager needs to be established.</span></span>

<span data-ttu-id="d5629-139">I TimeOffManager, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="d5629-139">In TimeOffManager, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d5629-140">Om du vill konfigurera och testa Azure AD enkel inloggning med TimeOffManager, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d5629-140">To configure and test Azure AD single sign-on with TimeOffManager, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d5629-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d5629-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d5629-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d5629-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d5629-143">**[Skapa en testanvändare TimeOffManager](#create-a-timeoffmanager-test-user)**  – du har en motsvarighet för Britta Simon i TimeOffManager som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d5629-143">**[Create a TimeOffManager test user](#create-a-timeoffmanager-test-user)** - to have a counterpart of Britta Simon in TimeOffManager that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d5629-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d5629-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d5629-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d5629-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="d5629-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d5629-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="d5629-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt TimeOffManager program.</span><span class="sxs-lookup"><span data-stu-id="d5629-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TimeOffManager application.</span></span>

<span data-ttu-id="d5629-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med TimeOffManager:**</span><span class="sxs-lookup"><span data-stu-id="d5629-148">**To configure Azure AD single sign-on with TimeOffManager, perform the following steps:**</span></span>

1. <span data-ttu-id="d5629-149">I Azure-portalen på den **TimeOffManager** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d5629-149">In the Azure portal, on the **TimeOffManager** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d5629-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d5629-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![SAML-baserade inloggning](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_samlbase.png)

3. <span data-ttu-id="d5629-153">På den **TimeOffManager domän och URL: er** avsnittet, utför följande:</span><span class="sxs-lookup"><span data-stu-id="d5629-153">On the **TimeOffManager Domain and URLs** section, perform the following:</span></span>

     ![Avsnittet TimeOffManager domän och URL: er](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_url.png)

    <span data-ttu-id="d5629-155">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`</span><span class="sxs-lookup"><span data-stu-id="d5629-155">In the **Reply URL** textbox, type a URL using the following pattern: `https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d5629-156">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="d5629-156">This value is not real.</span></span> <span data-ttu-id="d5629-157">Uppdatera det här värdet med det faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="d5629-157">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="d5629-158">Du kan hämta värdet från **inställningssidan för enkelinloggning** som beskrivs senare i självstudiekursen eller kontakta [TimeOffManager supportteamet](http://www.timeoffmanager.com/contact-us.aspx).</span><span class="sxs-lookup"><span data-stu-id="d5629-158">You can get this value from **Single Sign on settings page** which is explained later in the tutorial or Contact [TimeOffManager support team](http://www.timeoffmanager.com/contact-us.aspx).</span></span>
 
4. <span data-ttu-id="d5629-159">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="d5629-159">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Signeringscertifikat för SAML-avsnitt](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_certificate.png) 

5. <span data-ttu-id="d5629-161">Syftet med det här avsnittet är att beskriva hur användarna att autentisera till TimeOffManger med sitt konto i Azure AD med hjälp av federation baserat på SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="d5629-161">The objective of this section is to outline how to enable users to authenticate to TimeOffManger with their account in Azure AD using federation based on the SAML protocol.</span></span>
    
    <span data-ttu-id="d5629-162">Tillämpningsprogrammet TimeOffManger förväntar SAML-intyg i ett specifikt format, vilket kräver att du kan lägga till anpassade attributmappning konfigurationen för SAML-token attribut.</span><span class="sxs-lookup"><span data-stu-id="d5629-162">Your TimeOffManger application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="d5629-163">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="d5629-163">The following screenshot shows an example for this.</span></span>

    <span data-ttu-id="d5629-164">![SAML-token attribut](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "attribut för saml-token")</span><span class="sxs-lookup"><span data-stu-id="d5629-164">![saml token attributes](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "saml token attributes")</span></span>
    
    | <span data-ttu-id="d5629-165">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="d5629-165">Attribute Name</span></span> | <span data-ttu-id="d5629-166">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="d5629-166">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="d5629-167">Förnamn</span><span class="sxs-lookup"><span data-stu-id="d5629-167">Firstname</span></span> |<span data-ttu-id="d5629-168">User.givenName</span><span class="sxs-lookup"><span data-stu-id="d5629-168">User.givenname</span></span> |
    | <span data-ttu-id="d5629-169">Efternamn</span><span class="sxs-lookup"><span data-stu-id="d5629-169">Lastname</span></span> |<span data-ttu-id="d5629-170">User.surname</span><span class="sxs-lookup"><span data-stu-id="d5629-170">User.surname</span></span> |
    | <span data-ttu-id="d5629-171">E-post</span><span class="sxs-lookup"><span data-stu-id="d5629-171">Email</span></span> |<span data-ttu-id="d5629-172">User.Mail</span><span class="sxs-lookup"><span data-stu-id="d5629-172">User.mail</span></span> |
    
    <span data-ttu-id="d5629-173">a.</span><span class="sxs-lookup"><span data-stu-id="d5629-173">a.</span></span>  <span data-ttu-id="d5629-174">För varje datarad i tabellen ovan, klickar du på **lägga till användarattribut**.</span><span class="sxs-lookup"><span data-stu-id="d5629-174">For each data row in the table above, click **add user attribute**.</span></span>
    
    <span data-ttu-id="d5629-175">![SAML-token attribut](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "attribut för saml-token")</span><span class="sxs-lookup"><span data-stu-id="d5629-175">![saml token attributes](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "saml token attributes")</span></span>
    
    <span data-ttu-id="d5629-176">![SAML-token attribut](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "attribut för saml-token")</span><span class="sxs-lookup"><span data-stu-id="d5629-176">![saml token attributes](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "saml token attributes")</span></span>
    
    <span data-ttu-id="d5629-177">b.</span><span class="sxs-lookup"><span data-stu-id="d5629-177">b.</span></span>  <span data-ttu-id="d5629-178">I den **attributnamn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="d5629-178">In the **Attribute Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="d5629-179">c.</span><span class="sxs-lookup"><span data-stu-id="d5629-179">c.</span></span>  <span data-ttu-id="d5629-180">I den **attributvärdet** textruta Välj attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="d5629-180">In the **Attribute Value** textbox, select the attribute  value shown for that row.</span></span>
    
    <span data-ttu-id="d5629-181">d.</span><span class="sxs-lookup"><span data-stu-id="d5629-181">d.</span></span>  <span data-ttu-id="d5629-182">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d5629-182">Click **Ok**.</span></span>
    
6. <span data-ttu-id="d5629-183">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d5629-183">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="d5629-185">På den **TimeOffManager Configuration** klickar du på **konfigurera TimeOffManager** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="d5629-185">On the **TimeOffManager Configuration** section, click **Configure TimeOffManager** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d5629-186">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="d5629-186">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![TimeOffManager konfigurationsavsnitt](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_configure.png) 

8. <span data-ttu-id="d5629-188">Logga in på webbplatsen TimeOffManager företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="d5629-188">In a different web browser window, log into your TimeOffManager company site as an administrator.</span></span>

9. <span data-ttu-id="d5629-189">Gå till **konto \> konto alternativ \> enkel inloggning inställningar**.</span><span class="sxs-lookup"><span data-stu-id="d5629-189">Go to **Account \> Account Options \> Single Sign-On Settings**.</span></span>
   
   <span data-ttu-id="d5629-190">![Enkel inloggning inställningar](./media/active-directory-saas-timeoffmanager-tutorial/ic795917.png "enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="d5629-190">![Single Sign-On Settings](./media/active-directory-saas-timeoffmanager-tutorial/ic795917.png "Single Sign-On Settings")</span></span>
7. <span data-ttu-id="d5629-191">I den **inställningar för enkel inloggning** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d5629-191">In the **Single Sign-On Settings** section, perform the following steps:</span></span>
   
   <span data-ttu-id="d5629-192">![Enkel inloggning inställningar](./media/active-directory-saas-timeoffmanager-tutorial/ic795918.png "enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="d5629-192">![Single Sign-On Settings](./media/active-directory-saas-timeoffmanager-tutorial/ic795918.png "Single Sign-On Settings")</span></span>
   
   <span data-ttu-id="d5629-193">a.</span><span class="sxs-lookup"><span data-stu-id="d5629-193">a.</span></span> <span data-ttu-id="d5629-194">Öppna din Base64-kodade certifikatet i anteckningar, kopiera innehållet i den till Urklipp och klistra in hela certifikatet till **X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="d5629-194">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste the entire Certificate into **X.509 Certificate** textbox.</span></span>
   
   <span data-ttu-id="d5629-195">b.</span><span class="sxs-lookup"><span data-stu-id="d5629-195">b.</span></span> <span data-ttu-id="d5629-196">I **Idp utfärdaren** textruta klistra in värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d5629-196">In **Idp Issuer** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="d5629-197">c.</span><span class="sxs-lookup"><span data-stu-id="d5629-197">c.</span></span> <span data-ttu-id="d5629-198">I **IdP slutpunkts-URL** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d5629-198">In **IdP Endpoint URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="d5629-199">d.</span><span class="sxs-lookup"><span data-stu-id="d5629-199">d.</span></span> <span data-ttu-id="d5629-200">Som **genomdriva SAML**väljer **nr**.</span><span class="sxs-lookup"><span data-stu-id="d5629-200">As **Enforce SAML**, select **No**.</span></span>
   
   <span data-ttu-id="d5629-201">e.</span><span class="sxs-lookup"><span data-stu-id="d5629-201">e.</span></span> <span data-ttu-id="d5629-202">Som **skapa automatiskt användare**väljer **Ja**.</span><span class="sxs-lookup"><span data-stu-id="d5629-202">As **Auto-Create Users**, select **Yes**.</span></span>
   
   <span data-ttu-id="d5629-203">f.</span><span class="sxs-lookup"><span data-stu-id="d5629-203">f.</span></span> <span data-ttu-id="d5629-204">I **logga ut URL** textruta klistra in värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d5629-204">In **Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="d5629-205">g.</span><span class="sxs-lookup"><span data-stu-id="d5629-205">g.</span></span> <span data-ttu-id="d5629-206">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="d5629-206">click **Save Changes**.</span></span>

11. <span data-ttu-id="d5629-207">I **inställningar för enkelinloggning** sidan, kopierar du värdet för **Assertion konsument-tjänstens URL** och klistra in den i den **Reply URL** textrutan under **TimeOffManager Domänen och URL: er** avsnitt i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d5629-207">In **Single Sign on settings** page, copy the value of **Assertion Consumer Service URL** and paste it in the **Reply URL** text box under **TimeOffManager Domain and URLs** section in Azure portal.</span></span> 

      <span data-ttu-id="d5629-208">![Enkel inloggning inställningar](./media/active-directory-saas-timeoffmanager-tutorial/ic795915.png "enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="d5629-208">![Single Sign-On Settings](./media/active-directory-saas-timeoffmanager-tutorial/ic795915.png "Single Sign-On Settings")</span></span>

> [!TIP]
> <span data-ttu-id="d5629-209">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="d5629-209">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d5629-210">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="d5629-210">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d5629-211">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d5629-211">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="d5629-212">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d5629-212">Create an Azure AD test user</span></span>
<span data-ttu-id="d5629-213">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d5629-213">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d5629-215">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="d5629-215">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d5629-216">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d5629-216">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d5629-218">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d5629-218">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Användare och grupper--> alla användare](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d5629-220">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d5629-220">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Knappen Lägg till](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d5629-222">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d5629-222">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Dialogrutan Användarsida](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d5629-224">a.</span><span class="sxs-lookup"><span data-stu-id="d5629-224">a.</span></span> <span data-ttu-id="d5629-225">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d5629-225">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d5629-226">b.</span><span class="sxs-lookup"><span data-stu-id="d5629-226">b.</span></span> <span data-ttu-id="d5629-227">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d5629-227">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d5629-228">c.</span><span class="sxs-lookup"><span data-stu-id="d5629-228">c.</span></span> <span data-ttu-id="d5629-229">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d5629-229">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d5629-230">d.</span><span class="sxs-lookup"><span data-stu-id="d5629-230">d.</span></span> <span data-ttu-id="d5629-231">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d5629-231">Click **Create**.</span></span>
 
### <a name="create-a-timeoffmanager-test-user"></a><span data-ttu-id="d5629-232">Skapa en testanvändare TimeOffManager</span><span class="sxs-lookup"><span data-stu-id="d5629-232">Create a TimeOffManager test user</span></span>

<span data-ttu-id="d5629-233">För att aktivera Azure AD-användare att logga in på TimeOffManager etableras de för TimeOffManager.</span><span class="sxs-lookup"><span data-stu-id="d5629-233">In order to enable Azure AD users to log into TimeOffManager, they must be provisioned to TimeOffManager.</span></span>  

<span data-ttu-id="d5629-234">TimeOffManager stöder bara i tid användaretablering.</span><span class="sxs-lookup"><span data-stu-id="d5629-234">TimeOffManager supports just in time user provisioning.</span></span> <span data-ttu-id="d5629-235">Det finns ingen åtgärd-objekt.</span><span class="sxs-lookup"><span data-stu-id="d5629-235">There is no action item for you.</span></span>  

<span data-ttu-id="d5629-236">Användare läggs till automatiskt under den första inloggningen med enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="d5629-236">The users are added automatically during the first login using single sign on.</span></span>

>[!NOTE]
><span data-ttu-id="d5629-237">Du kan använda något annat TimeOffManager användarens konto skapas verktyg eller API: er som tillhandahålls av TimeOffManager att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="d5629-237">You can use any other TimeOffManager user account creation tools or APIs provided by TimeOffManager to provision Azure AD user accounts.</span></span>
> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="d5629-238">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="d5629-238">Assign the Azure AD test user</span></span>

<span data-ttu-id="d5629-239">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till TimeOffManager.</span><span class="sxs-lookup"><span data-stu-id="d5629-239">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TimeOffManager.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d5629-241">**Om du vill tilldela TimeOffManager Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d5629-241">**To assign Britta Simon to TimeOffManager, perform the following steps:**</span></span>

1. <span data-ttu-id="d5629-242">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d5629-242">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d5629-244">Välj i listan med program **TimeOffManager**.</span><span class="sxs-lookup"><span data-stu-id="d5629-244">In the applications list, select **TimeOffManager**.</span></span>

    ![TimeOffManager i listan över appar](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_app.png) 

3. <span data-ttu-id="d5629-246">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d5629-246">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d5629-248">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d5629-248">Click **Add** button.</span></span> <span data-ttu-id="d5629-249">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d5629-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d5629-251">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="d5629-251">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d5629-252">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d5629-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d5629-253">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d5629-253">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="d5629-254">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d5629-254">Test single sign-on</span></span>

<span data-ttu-id="d5629-255">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="d5629-255">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d5629-256">När du klickar på panelen TimeOffManager på åtkomstpanelen du bör få automatiskt loggat in på ditt TimeOffManager program.</span><span class="sxs-lookup"><span data-stu-id="d5629-256">When you click the TimeOffManager tile in the Access Panel, you should get automatically signed-on to your TimeOffManager application.</span></span> <span data-ttu-id="d5629-257">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d5629-257">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d5629-258">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d5629-258">Additional resources</span></span>

* [<span data-ttu-id="d5629-259">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d5629-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d5629-260">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d5629-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_203.png

