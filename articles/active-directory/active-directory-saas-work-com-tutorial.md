---
title: "Självstudier: Azure Active Directory-integrering med Work.com | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Work.com."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 7cfec8e9ac12d43095483696a15c0580776d3114
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a><span data-ttu-id="bc208-103">Självstudier: Azure Active Directory-integrering med Work.com</span><span class="sxs-lookup"><span data-stu-id="bc208-103">Tutorial: Azure Active Directory integration with Work.com</span></span>

<span data-ttu-id="bc208-104">I kursen får lära du att integrera Work.com med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="bc208-104">In this tutorial, you learn how to integrate Work.com with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bc208-105">Integrera Work.com med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="bc208-105">Integrating Work.com with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="bc208-106">Du kan styra i Azure AD som har åtkomst till Work.com</span><span class="sxs-lookup"><span data-stu-id="bc208-106">You can control in Azure AD who has access to Work.com</span></span>
- <span data-ttu-id="bc208-107">Du kan aktivera användarna att automatiskt hämta loggat in på Work.com (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="bc208-107">You can enable your users to automatically get signed-on to Work.com (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bc208-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bc208-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="bc208-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bc208-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bc208-110">Krav</span><span class="sxs-lookup"><span data-stu-id="bc208-110">Prerequisites</span></span>

<span data-ttu-id="bc208-111">För att konfigurera Azure AD-integrering med Work.com, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="bc208-111">To configure Azure AD integration with Work.com, you need the following items:</span></span>

- <span data-ttu-id="bc208-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="bc208-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bc208-113">En Work.com enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="bc208-113">A Work.com single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bc208-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="bc208-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bc208-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="bc208-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bc208-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="bc208-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bc208-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bc208-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bc208-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="bc208-118">Scenario description</span></span>
<span data-ttu-id="bc208-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="bc208-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bc208-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="bc208-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bc208-121">Lägg till Work.com från galleriet</span><span class="sxs-lookup"><span data-stu-id="bc208-121">Add Work.com from the gallery</span></span>
2. <span data-ttu-id="bc208-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bc208-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-workcom-from-the-gallery"></a><span data-ttu-id="bc208-123">Lägg till Work.com från galleriet</span><span class="sxs-lookup"><span data-stu-id="bc208-123">Add Work.com from the gallery</span></span>
<span data-ttu-id="bc208-124">Du måste lägga till Work.com från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Work.com i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bc208-124">To configure the integration of Work.com into Azure AD, you need to add Work.com from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="bc208-125">**Utför följande steg för att lägga till Work.com från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="bc208-125">**To add Work.com from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="bc208-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="bc208-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bc208-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="bc208-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="bc208-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="bc208-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="bc208-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bc208-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="bc208-133">I sökrutan skriver **Work.com**väljer **Work.com** från resultatrutan Klicka **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="bc208-133">In the search box, type **Work.com**, select **Work.com** from results panel then click **Add** button to add the application.</span></span>

    ![Lägg till från galleriet](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="bc208-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bc208-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="bc208-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Work.com baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="bc208-136">In this section, you configure and test Azure AD single sign-on with Work.com based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="bc208-137">Azure AD måste du känna till användaren i Work.com motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="bc208-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Work.com is to a user in Azure AD.</span></span> <span data-ttu-id="bc208-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Work.com upprättas.</span><span class="sxs-lookup"><span data-stu-id="bc208-138">In other words, a link relationship between an Azure AD user and the related user in Work.com needs to be established.</span></span>

<span data-ttu-id="bc208-139">I Work.com, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="bc208-139">In Work.com, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="bc208-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Work.com, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="bc208-140">To configure and test Azure AD single sign-on with Work.com, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="bc208-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="bc208-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="bc208-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bc208-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bc208-143">**[Skapa en testanvändare Work.com](#create-a-workcom-test-user)**  – du har en motsvarighet för Britta Simon i Work.com som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="bc208-143">**[Create a Work.com test user](#create-a-workcom-test-user)** - to have a counterpart of Britta Simon in Work.com that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="bc208-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bc208-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bc208-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="bc208-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="bc208-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bc208-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="bc208-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Work.com program.</span><span class="sxs-lookup"><span data-stu-id="bc208-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Work.com application.</span></span>

>[!NOTE]
><span data-ttu-id="bc208-148">Om du vill konfigurera enkel inloggning måste du konfigurera ett anpassat domännamn Work.com ännu.</span><span class="sxs-lookup"><span data-stu-id="bc208-148">To configure single sign-on, you need to setup a custom Work.com domain name yet.</span></span> <span data-ttu-id="bc208-149">Du måste ange minst ett domännamn, testa ditt domännamn och distribuera den till hela organisationen.</span><span class="sxs-lookup"><span data-stu-id="bc208-149">You need to define at least a domain name, test your domain name, and deploy it to your entire organization.</span></span>

<span data-ttu-id="bc208-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Work.com:**</span><span class="sxs-lookup"><span data-stu-id="bc208-150">**To configure Azure AD single sign-on with Work.com, perform the following steps:**</span></span>

1. <span data-ttu-id="bc208-151">I Azure-portalen på den **Work.com** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="bc208-151">In the Azure portal, on the **Work.com** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="bc208-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bc208-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![SAML-baserade inloggning](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_samlbase.png)

3. <span data-ttu-id="bc208-155">På den **Work.com domän och URL: er** avsnittet, utför följande:</span><span class="sxs-lookup"><span data-stu-id="bc208-155">On the **Work.com Domain and URLs** section, perform the following:</span></span>

    ![Avsnittet Work.com domän och URL: er](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_url.png)

    <span data-ttu-id="bc208-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`http://<companyname>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="bc208-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `http://<companyname>.my.salesforce.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="bc208-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="bc208-158">This value is not real.</span></span> <span data-ttu-id="bc208-159">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="bc208-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="bc208-160">Kontakta [Work.com klienten supportteamet](https://help.salesforce.com/articleView?id=000159855&type=3) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="bc208-160">Contact [Work.com Client support team](https://help.salesforce.com/articleView?id=000159855&type=3) to get this value.</span></span> 

4. <span data-ttu-id="bc208-161">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="bc208-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Signeringscertifikat för SAML-avsnitt](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_certificate.png) 

5. <span data-ttu-id="bc208-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="bc208-163">Click **Save** button.</span></span>

    ![Knappen Spara](./media/active-directory-saas-work-com-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bc208-165">På den **Work.com Configuration** klickar du på **konfigurera Work.com** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="bc208-165">On the **Work.com Configuration** section, click **Configure Work.com** to open **Configure sign-on** window.</span></span> <span data-ttu-id="bc208-166">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="bc208-166">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Work.com konfigurationsavsnitt](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_configure.png) 
7. <span data-ttu-id="bc208-168">Logga in på din Work.com klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="bc208-168">Log in to your Work.com tenant as administrator.</span></span>

8. <span data-ttu-id="bc208-169">Gå till **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="bc208-169">Go to **Setup**.</span></span>
   
    <span data-ttu-id="bc208-170">![Installationsprogrammet](./media/active-directory-saas-work-com-tutorial/ic794108.png "installationen")</span><span class="sxs-lookup"><span data-stu-id="bc208-170">![Setup](./media/active-directory-saas-work-com-tutorial/ic794108.png "Setup")</span></span>

9. <span data-ttu-id="bc208-171">I det vänstra navigeringsfönstret i den **administrera** klickar du på **domänhantering** Expandera avsnittet relaterade och klicka sedan på **min domän** att öppna den **min domän** sidan.</span><span class="sxs-lookup"><span data-stu-id="bc208-171">On the left navigation pane, in the **Administer** section, click **Domain Management** to expand the related section, and then click **My Domain** to open the **My Domain** page.</span></span> 
   
    <span data-ttu-id="bc208-172">![Min domän](./media/active-directory-saas-work-com-tutorial/ic767825.png "min domän")</span><span class="sxs-lookup"><span data-stu-id="bc208-172">![My Domain](./media/active-directory-saas-work-com-tutorial/ic767825.png "My Domain")</span></span>

10. <span data-ttu-id="bc208-173">Om du vill verifiera att din domän har ställts in korrekt, se till att den är i ”**steg 4 distribueras till användarna**” och granska din ”**Mina Domäninställningar**”.</span><span class="sxs-lookup"><span data-stu-id="bc208-173">To verify that your domain has been set up correctly, make sure that it is in “**Step 4 Deployed to Users**” and review your “**My Domain Settings**”.</span></span>
   
    <span data-ttu-id="bc208-174">![Domänen som distribuerats till användaren](./media/active-directory-saas-work-com-tutorial/ic784377.png "domän som har distribuerats till användaren")</span><span class="sxs-lookup"><span data-stu-id="bc208-174">![Domain Deployed to User](./media/active-directory-saas-work-com-tutorial/ic784377.png "Domain Deployed to User")</span></span>

11. <span data-ttu-id="bc208-175">Logga in på din Work.com-klient.</span><span class="sxs-lookup"><span data-stu-id="bc208-175">Log in to your Work.com tenant.</span></span>

12. <span data-ttu-id="bc208-176">Gå till **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="bc208-176">Go to **Setup**.</span></span>
    
    <span data-ttu-id="bc208-177">![Installationsprogrammet](./media/active-directory-saas-work-com-tutorial/ic794108.png "installationen")</span><span class="sxs-lookup"><span data-stu-id="bc208-177">![Setup](./media/active-directory-saas-work-com-tutorial/ic794108.png "Setup")</span></span>

13. <span data-ttu-id="bc208-178">Expandera den **säkerhetsåtgärder** -menyn och klicka sedan på **inställningar för enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="bc208-178">Expand the **Security Controls** menu, and then click **Single Sign-On Settings**.</span></span>
    
    <span data-ttu-id="bc208-179">![Enkel inloggning inställningar](./media/active-directory-saas-work-com-tutorial/ic794113.png "enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="bc208-179">![Single Sign-On Settings](./media/active-directory-saas-work-com-tutorial/ic794113.png "Single Sign-On Settings")</span></span>

14. <span data-ttu-id="bc208-180">På den **inställningar för enkel inloggning** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="bc208-180">On the **Single Sign-On Settings** dialog page, perform the following steps:</span></span>
    
    <span data-ttu-id="bc208-181">![SAML aktiverat](./media/active-directory-saas-work-com-tutorial/ic781026.png "SAML aktiverad")</span><span class="sxs-lookup"><span data-stu-id="bc208-181">![SAML Enabled](./media/active-directory-saas-work-com-tutorial/ic781026.png "SAML Enabled")</span></span>
    
    <span data-ttu-id="bc208-182">a.</span><span class="sxs-lookup"><span data-stu-id="bc208-182">a.</span></span> <span data-ttu-id="bc208-183">Välj **SAML aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="bc208-183">Select **SAML Enabled**.</span></span>
    
    <span data-ttu-id="bc208-184">b.</span><span class="sxs-lookup"><span data-stu-id="bc208-184">b.</span></span> <span data-ttu-id="bc208-185">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="bc208-185">Click **New**.</span></span>

15. <span data-ttu-id="bc208-186">I den **SAML enkel inloggning inställningar** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="bc208-186">In the **SAML Single Sign-On Settings** section, perform the following steps:</span></span>
    
    <span data-ttu-id="bc208-187">![SAML enkel inloggning inställningen](./media/active-directory-saas-work-com-tutorial/ic794114.png "SAML enkel inloggning inställningen")</span><span class="sxs-lookup"><span data-stu-id="bc208-187">![SAML Single Sign-On Setting](./media/active-directory-saas-work-com-tutorial/ic794114.png "SAML Single Sign-On Setting")</span></span>
    
    <span data-ttu-id="bc208-188">a.</span><span class="sxs-lookup"><span data-stu-id="bc208-188">a.</span></span> <span data-ttu-id="bc208-189">I den **namn** textruta, ange ett namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="bc208-189">In the **Name** textbox, type a name for your configuration.</span></span>  
       
    > [!NOTE]
    > <span data-ttu-id="bc208-190">Att tillhandahålla ett värde för **namn** automatiskt fylla i **API-namnet** textruta.</span><span class="sxs-lookup"><span data-stu-id="bc208-190">Providing a value for **Name** does automatically populate the **API Name** textbox.</span></span>
    
    <span data-ttu-id="bc208-191">b.</span><span class="sxs-lookup"><span data-stu-id="bc208-191">b.</span></span> <span data-ttu-id="bc208-192">I **utfärdaren** textruta klistra in värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bc208-192">In **Issuer** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="bc208-193">c.</span><span class="sxs-lookup"><span data-stu-id="bc208-193">c.</span></span> <span data-ttu-id="bc208-194">Om du vill överföra hämtat certifikat från Azure-portalen klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="bc208-194">To upload the downloaded certificate from Azure portal, click **Browse**.</span></span>
    
    <span data-ttu-id="bc208-195">d.</span><span class="sxs-lookup"><span data-stu-id="bc208-195">d.</span></span> <span data-ttu-id="bc208-196">I den **enhets-Id** textruta typen `https://salesforce-work.com`.</span><span class="sxs-lookup"><span data-stu-id="bc208-196">In the **Entity Id** textbox, type `https://salesforce-work.com`.</span></span>
    
    <span data-ttu-id="bc208-197">e.</span><span class="sxs-lookup"><span data-stu-id="bc208-197">e.</span></span> <span data-ttu-id="bc208-198">Som **SAML identitetstyp**väljer **Assertion innehåller Federation-ID från användarobjektet**.</span><span class="sxs-lookup"><span data-stu-id="bc208-198">As **SAML Identity Type**, select **Assertion contains the Federation ID from the User object**.</span></span>
    
    <span data-ttu-id="bc208-199">f.</span><span class="sxs-lookup"><span data-stu-id="bc208-199">f.</span></span> <span data-ttu-id="bc208-200">Som **SAML identitet plats**väljer **identitet är i elementet NameIdentfier i instruktionen ämne**.</span><span class="sxs-lookup"><span data-stu-id="bc208-200">As **SAML Identity Location**, select **Identity is in the NameIdentfier element of the Subject statement**.</span></span>
    
    <span data-ttu-id="bc208-201">g.</span><span class="sxs-lookup"><span data-stu-id="bc208-201">g.</span></span> <span data-ttu-id="bc208-202">I **identitet providern inloggnings-URL** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bc208-202">In **Identity Provider Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="bc208-203">h.</span><span class="sxs-lookup"><span data-stu-id="bc208-203">h.</span></span> <span data-ttu-id="bc208-204">I **identitet providern logga ut URL** textruta klistra in värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bc208-204">In **Identity Provider Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="bc208-205">Jag.</span><span class="sxs-lookup"><span data-stu-id="bc208-205">i.</span></span> <span data-ttu-id="bc208-206">Som **providern initierade begära Tjänstbindning**väljer **HTTP Post**.</span><span class="sxs-lookup"><span data-stu-id="bc208-206">As **Service Provider Initiated Request Binding**, select **HTTP Post**.</span></span>
    
    <span data-ttu-id="bc208-207">j.</span><span class="sxs-lookup"><span data-stu-id="bc208-207">j.</span></span> <span data-ttu-id="bc208-208">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="bc208-208">Click **Save**.</span></span>

16. <span data-ttu-id="bc208-209">I din Work.com klassiska portalen i det vänstra navigeringsfönstret klickar du på **domänhantering** Expandera avsnittet relaterade och klicka sedan på **min domän** att öppna den **min domän** sidan.</span><span class="sxs-lookup"><span data-stu-id="bc208-209">In your Work.com classic portal, on the left navigation pane, click **Domain Management** to expand the related section, and then click **My Domain** to open the **My Domain** page.</span></span> 
    
    <span data-ttu-id="bc208-210">![Min domän](./media/active-directory-saas-work-com-tutorial/ic794115.png "min domän")</span><span class="sxs-lookup"><span data-stu-id="bc208-210">![My Domain](./media/active-directory-saas-work-com-tutorial/ic794115.png "My Domain")</span></span>

17. <span data-ttu-id="bc208-211">På den **min domän** sidan den **inloggningen sidan anpassning** klickar du på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="bc208-211">On the **My Domain** page, in the **Login Page Branding** section, click **Edit**.</span></span>
    
    <span data-ttu-id="bc208-212">![Inloggningssidan anpassning](./media/active-directory-saas-work-com-tutorial/ic767826.png "inloggningssidan anpassning")</span><span class="sxs-lookup"><span data-stu-id="bc208-212">![Login Page Branding](./media/active-directory-saas-work-com-tutorial/ic767826.png "Login Page Branding")</span></span>

14. <span data-ttu-id="bc208-213">På den **inloggningen sidan anpassning** sidan den **Autentiseringstjänsten** avsnitt, namnet på din **SAML SSO inställningar** visas.</span><span class="sxs-lookup"><span data-stu-id="bc208-213">On the **Login Page Branding** page, in the **Authentication Service** section, the name of your **SAML SSO Settings** is displayed.</span></span> <span data-ttu-id="bc208-214">Markera den och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="bc208-214">Select it, and then click **Save**.</span></span>
    
    <span data-ttu-id="bc208-215">![Inloggningssidan anpassning](./media/active-directory-saas-work-com-tutorial/ic784366.png "inloggningssidan anpassning")</span><span class="sxs-lookup"><span data-stu-id="bc208-215">![Login Page Branding](./media/active-directory-saas-work-com-tutorial/ic784366.png "Login Page Branding")</span></span>

> [!TIP]
> <span data-ttu-id="bc208-216">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="bc208-216">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="bc208-217">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="bc208-217">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="bc208-218">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bc208-218">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="bc208-219">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="bc208-219">Create an Azure AD test user</span></span>
<span data-ttu-id="bc208-220">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bc208-220">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="bc208-222">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="bc208-222">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="bc208-223">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="bc208-223">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-work-com-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bc208-225">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="bc208-225">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Användare och grupper -> alla användare](./media/active-directory-saas-work-com-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bc208-227">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bc208-227">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Lägg till](./media/active-directory-saas-work-com-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bc208-229">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="bc208-229">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Dialogrutan Användarsida](./media/active-directory-saas-work-com-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bc208-231">a.</span><span class="sxs-lookup"><span data-stu-id="bc208-231">a.</span></span> <span data-ttu-id="bc208-232">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bc208-232">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bc208-233">b.</span><span class="sxs-lookup"><span data-stu-id="bc208-233">b.</span></span> <span data-ttu-id="bc208-234">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bc208-234">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bc208-235">c.</span><span class="sxs-lookup"><span data-stu-id="bc208-235">c.</span></span> <span data-ttu-id="bc208-236">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="bc208-236">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="bc208-237">d.</span><span class="sxs-lookup"><span data-stu-id="bc208-237">d.</span></span> <span data-ttu-id="bc208-238">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="bc208-238">Click **Create**.</span></span>
 
### <a name="create-a-workcom-test-user"></a><span data-ttu-id="bc208-239">Skapa en testanvändare Work.com</span><span class="sxs-lookup"><span data-stu-id="bc208-239">Create a Work.com test user</span></span>
<span data-ttu-id="bc208-240">För Azure Active Directory-användare för att kunna logga in, måste de etableras till Work.com.</span><span class="sxs-lookup"><span data-stu-id="bc208-240">For Azure Active Directory users to be able to sign in, they must be provisioned to Work.com.</span></span> <span data-ttu-id="bc208-241">När det gäller Work.com är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="bc208-241">In the case of Work.com, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="bc208-242">Utför följande steg för att konfigurera användaretablering:</span><span class="sxs-lookup"><span data-stu-id="bc208-242">To configure user provisioning, perform the following steps:</span></span>
1. <span data-ttu-id="bc208-243">Logga in på webbplatsen Work.com företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="bc208-243">Sign on to your Work.com company site as an administrator.</span></span>

2. <span data-ttu-id="bc208-244">Gå till **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="bc208-244">Go to **Setup**.</span></span>
   
    <span data-ttu-id="bc208-245">![Installationsprogrammet](./media/active-directory-saas-work-com-tutorial/IC794108.png "installationen")</span><span class="sxs-lookup"><span data-stu-id="bc208-245">![Setup](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")</span></span>
3. <span data-ttu-id="bc208-246">Gå till **hantera användare \> användare**.</span><span class="sxs-lookup"><span data-stu-id="bc208-246">Go to **Manage Users \> Users**.</span></span>
   
    <span data-ttu-id="bc208-247">![Hantera användare](./media/active-directory-saas-work-com-tutorial/IC784369.png "hantera användare")</span><span class="sxs-lookup"><span data-stu-id="bc208-247">![Manage Users](./media/active-directory-saas-work-com-tutorial/IC784369.png "Manage Users")</span></span>

4. <span data-ttu-id="bc208-248">Klicka på **ny användare**.</span><span class="sxs-lookup"><span data-stu-id="bc208-248">Click **New User**.</span></span>
   
    <span data-ttu-id="bc208-249">![Alla användare](./media/active-directory-saas-work-com-tutorial/IC794117.png "alla användare")</span><span class="sxs-lookup"><span data-stu-id="bc208-249">![All Users](./media/active-directory-saas-work-com-tutorial/IC794117.png "All Users")</span></span>

5. <span data-ttu-id="bc208-250">I avsnittet användare redigera utför följande steg i attribut för ett giltigt Azure AD-kontot som du vill etablera i relaterade textrutor:</span><span class="sxs-lookup"><span data-stu-id="bc208-250">In the User Edit section, perform the following steps, in attributes of a valid Azure AD account you want to provision into the related textboxes:</span></span>
   
    <span data-ttu-id="bc208-251">![Redigera användare](./media/active-directory-saas-work-com-tutorial/ic794118.png "Redigera användare")</span><span class="sxs-lookup"><span data-stu-id="bc208-251">![User Edit](./media/active-directory-saas-work-com-tutorial/ic794118.png "User Edit")</span></span>
   
    <span data-ttu-id="bc208-252">a.</span><span class="sxs-lookup"><span data-stu-id="bc208-252">a.</span></span> <span data-ttu-id="bc208-253">I den **Förnamn** textruta typ av **Förnamn** användarens **Britta**.</span><span class="sxs-lookup"><span data-stu-id="bc208-253">In the **First Name** textbox, type the **first name** of the user **Britta**.</span></span>
    
    <span data-ttu-id="bc208-254">b.</span><span class="sxs-lookup"><span data-stu-id="bc208-254">b.</span></span> <span data-ttu-id="bc208-255">I den **efternamn** textruta typ av **efternamn** användarens **Simon**.</span><span class="sxs-lookup"><span data-stu-id="bc208-255">In the **Last Name** textbox, type the **last name** of the user **Simon**.</span></span>
    
    <span data-ttu-id="bc208-256">c.</span><span class="sxs-lookup"><span data-stu-id="bc208-256">c.</span></span> <span data-ttu-id="bc208-257">I den **Alias** textruta typ av **namn** användarens **BrittaS**.</span><span class="sxs-lookup"><span data-stu-id="bc208-257">In the **Alias** textbox, type the **name** of the user **BrittaS**.</span></span>
    
    <span data-ttu-id="bc208-258">d.</span><span class="sxs-lookup"><span data-stu-id="bc208-258">d.</span></span> <span data-ttu-id="bc208-259">I den **e-post** textruta typ av **e-postadress** för användare  **Brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="bc208-259">In the **Email** textbox, type the **email address** of user **Brittasimon@contoso.com**.</span></span>
    
    <span data-ttu-id="bc208-260">e.</span><span class="sxs-lookup"><span data-stu-id="bc208-260">e.</span></span> <span data-ttu-id="bc208-261">I den **användarnamn** textruta, Skriv ett användarnamn för användaren som  **Brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="bc208-261">In the **User Name** textbox, type a user name of user like **Brittasimon@contoso.com**.</span></span>
    
    <span data-ttu-id="bc208-262">f.</span><span class="sxs-lookup"><span data-stu-id="bc208-262">f.</span></span> <span data-ttu-id="bc208-263">I den **smeknamn** textruta typ a **smeknamn** för användare **Simon**.</span><span class="sxs-lookup"><span data-stu-id="bc208-263">In the **Nick Name** textbox, type a **nick name** of user **Simon**.</span></span>
    
    <span data-ttu-id="bc208-264">g.</span><span class="sxs-lookup"><span data-stu-id="bc208-264">g.</span></span> <span data-ttu-id="bc208-265">Välj **rollen**, **användarlicens**, och **profil**.</span><span class="sxs-lookup"><span data-stu-id="bc208-265">Select **Role**, **User License**, and **Profile**.</span></span>
    
    <span data-ttu-id="bc208-266">h.</span><span class="sxs-lookup"><span data-stu-id="bc208-266">h.</span></span> <span data-ttu-id="bc208-267">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="bc208-267">Click **Save**.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="bc208-268">Azure AD-kontoinnehavaren får ett e-postmeddelande med en länk för att bekräfta kontot innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="bc208-268">The Azure AD account holder will get an email including a link to confirm the account before it becomes active.</span></span>
    > 
    > 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="bc208-269">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="bc208-269">Assign the Azure AD test user</span></span>

<span data-ttu-id="bc208-270">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Work.com.</span><span class="sxs-lookup"><span data-stu-id="bc208-270">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Work.com.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="bc208-272">**Om du vill tilldela Work.com Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="bc208-272">**To assign Britta Simon to Work.com, perform the following steps:**</span></span>

1. <span data-ttu-id="bc208-273">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="bc208-273">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="bc208-275">Välj i listan med program **Work.com**.</span><span class="sxs-lookup"><span data-stu-id="bc208-275">In the applications list, select **Work.com**.</span></span>

    ![Work.com i appens lista](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_app.png) 

3. <span data-ttu-id="bc208-277">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="bc208-277">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="bc208-279">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="bc208-279">Click **Add** button.</span></span> <span data-ttu-id="bc208-280">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bc208-280">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="bc208-282">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="bc208-282">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="bc208-283">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bc208-283">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bc208-284">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bc208-284">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="bc208-285">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bc208-285">Test single sign-on</span></span>

<span data-ttu-id="bc208-286">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="bc208-286">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="bc208-287">När du klickar på panelen Work.com på åtkomstpanelen du bör få automatiskt loggat in på ditt Work.com program.</span><span class="sxs-lookup"><span data-stu-id="bc208-287">When you click the Work.com tile in the Access Panel, you should get automatically signed-on to your Work.com application.</span></span>
<span data-ttu-id="bc208-288">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bc208-288">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bc208-289">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="bc208-289">Additional resources</span></span>

* [<span data-ttu-id="bc208-290">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bc208-290">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bc208-291">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bc208-291">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_203.png

