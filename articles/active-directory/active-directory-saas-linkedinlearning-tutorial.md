---
title: "Självstudier: Azure Active Directory-integrering med LinkedIn Learning | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och LinkedIn Learning."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: jeedes
ms.openlocfilehash: 6ad28cb3adaa63ddc3d3769a650d26ca6a7e2695
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a><span data-ttu-id="b7041-103">Självstudier: Azure Active Directory-integrering med LinkedIn Learning</span><span class="sxs-lookup"><span data-stu-id="b7041-103">Tutorial: Azure Active Directory integration with LinkedIn Learning</span></span>

<span data-ttu-id="b7041-104">I kursen får lära du att integrera LinkedIn Learning med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b7041-104">In this tutorial, you learn how to integrate LinkedIn Learning with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b7041-105">Integrera LinkedIn Learning med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b7041-105">Integrating LinkedIn Learning with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b7041-106">Du kan styra i Azure AD som har åtkomst till LinkedIn-utbildning</span><span class="sxs-lookup"><span data-stu-id="b7041-106">You can control in Azure AD who has access to LinkedIn Learning</span></span>
- <span data-ttu-id="b7041-107">Du kan aktivera användarna att automatiskt hämta loggat in på LinkedIn Learning (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b7041-107">You can enable your users to automatically get signed-on to LinkedIn Learning (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b7041-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b7041-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b7041-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b7041-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b7041-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b7041-110">Prerequisites</span></span>

<span data-ttu-id="b7041-111">Om du vill konfigurera Azure AD-integrering med LinkedIn Learning behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="b7041-111">To configure Azure AD integration with LinkedIn Learning, you need the following items:</span></span>

- <span data-ttu-id="b7041-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b7041-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b7041-113">En LinkedIn Learning enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="b7041-113">A LinkedIn Learning single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b7041-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b7041-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b7041-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b7041-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b7041-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b7041-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b7041-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b7041-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b7041-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b7041-118">Scenario description</span></span>
<span data-ttu-id="b7041-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b7041-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b7041-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b7041-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b7041-121">Lägga till LinkedIn Learning från galleriet</span><span class="sxs-lookup"><span data-stu-id="b7041-121">Adding LinkedIn Learning from the gallery</span></span>
2. <span data-ttu-id="b7041-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7041-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-learning-from-the-gallery"></a><span data-ttu-id="b7041-123">Lägga till LinkedIn Learning från galleriet</span><span class="sxs-lookup"><span data-stu-id="b7041-123">Adding LinkedIn Learning from the gallery</span></span>
<span data-ttu-id="b7041-124">Du måste lägga till LinkedIn Learning från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av LinkedIn Learning i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b7041-124">To configure the integration of LinkedIn Learning into Azure AD, you need to add LinkedIn Learning from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b7041-125">**Utför följande steg för att lägga till LinkedIn Learning från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="b7041-125">**To add LinkedIn Learning from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b7041-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b7041-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b7041-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="b7041-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b7041-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b7041-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="b7041-131">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7041-131">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="b7041-133">I sökrutan skriver **LinkedIn Learning**.</span><span class="sxs-lookup"><span data-stu-id="b7041-133">In the search box, type **LinkedIn Learning**.</span></span> <span data-ttu-id="b7041-134">I resultatrutan, klickar du på **LinkedIn Learning** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="b7041-134">From results panel, click **LinkedIn Learning** to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b7041-136">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7041-136">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b7041-137">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LinkedIn Learning baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b7041-137">In this section, you configure and test Azure AD single sign-on with LinkedIn Learning based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b7041-138">Azure AD måste du känna till användaren i LinkedIn Learning motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="b7041-138">For single sign-on to work, Azure AD needs to know what the counterpart user in LinkedIn Learning is to a user in Azure AD.</span></span> <span data-ttu-id="b7041-139">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i LinkedIn Learning upprättas.</span><span class="sxs-lookup"><span data-stu-id="b7041-139">In other words, a link relationship between an Azure AD user and the related user in LinkedIn Learning needs to be established.</span></span>

<span data-ttu-id="b7041-140">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i LinkedIn utbildning.</span><span class="sxs-lookup"><span data-stu-id="b7041-140">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in LinkedIn Learning.</span></span>

<span data-ttu-id="b7041-141">Om du vill konfigurera och testa Azure AD enkel inloggning med LinkedIn Learning, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b7041-141">To configure and test Azure AD single sign-on with LinkedIn Learning, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b7041-142">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b7041-142">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b7041-143">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b7041-143">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b7041-144">**[Skapa en testanvändare LinkedIn Learning](#creating-a-linkedin-learning-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b7041-144">**[Creating a LinkedIn Learning test user](#creating-a-linkedin-learning-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="b7041-145">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b7041-145">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b7041-146">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b7041-146">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b7041-147">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7041-147">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b7041-148">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt LinkedIn Learning-program.</span><span class="sxs-lookup"><span data-stu-id="b7041-148">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LinkedIn Learning application.</span></span>

<span data-ttu-id="b7041-149">**Utför följande steg för att konfigurera Azure AD enkel inloggning med LinkedIn Learning:**</span><span class="sxs-lookup"><span data-stu-id="b7041-149">**To configure Azure AD single sign-on with LinkedIn Learning, perform the following steps:**</span></span>

1. <span data-ttu-id="b7041-150">I Azure-portalen på den **LinkedIn Learning** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b7041-150">In the Azure portal, on the **LinkedIn Learning** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="b7041-152">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b7041-152">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedin_01.png)

3. <span data-ttu-id="b7041-154">I en annan webbläsarfönster inloggning till LinkedIn Learning-klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="b7041-154">In a different web browser window, sign-on to your LinkedIn Learning tenant as an administrator.</span></span>

4. <span data-ttu-id="b7041-155">I **Kontocenter**, klickar du på **globala inställningar** under **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="b7041-155">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="b7041-156">Markera också **utbildning - standard** från den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="b7041-156">Also, select **Learning - Default** from the dropdown list.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

5. <span data-ttu-id="b7041-158">Klicka på **eller klicka här om du vill läsa in och kopiera enskilda fält i formuläret** och kopiera **enhets-Id** och **Assertion konsumenten Access (ACS) Url**</span><span class="sxs-lookup"><span data-stu-id="b7041-158">Click **OR Click Here to load and copy individual fields from the form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

6. <span data-ttu-id="b7041-160">På Azure-portalen under **LinkedIn Learning domän och URL: er**, utför följande steg om du vill konfigurera enkel inloggning i **IdP initierade** läge</span><span class="sxs-lookup"><span data-stu-id="b7041-160">On Azure portal, under **LinkedIn Learning Domain and URLs**, perform the following steps if you want to configure SSO in **IdP Initiated** mode</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_01.png)

    <span data-ttu-id="b7041-162">a.</span><span class="sxs-lookup"><span data-stu-id="b7041-162">a.</span></span> <span data-ttu-id="b7041-163">I den **identifierare** textruta, ange den **enhets-ID** kopieras från LinkedIn-portalen</span><span class="sxs-lookup"><span data-stu-id="b7041-163">In the **Identifier** textbox, enter the **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="b7041-164">b.</span><span class="sxs-lookup"><span data-stu-id="b7041-164">b.</span></span> <span data-ttu-id="b7041-165">I den **Reply URL** textruta ange den **Assertion konsumenten Access (ACS) Url** kopieras från LinkedIn-portalen</span><span class="sxs-lookup"><span data-stu-id="b7041-165">In the **Reply URL** textbox, enter the **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="b7041-166">Om du vill konfigurera enkel inloggning i **SP-initierad**, klickar du på Visa avancerade URL inställningen alternativ i konfigurationsavsnittet och konfigurera URL inloggning med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="b7041-166">If you want to configure SSO in **SP Initiated**, then click Show Advanced URL setting option in the configuration section and configure the sign-on URL with the following pattern:</span></span>

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_02.png)   
    
8. <span data-ttu-id="b7041-168">Tillämpningsprogrammet LinkedIn Learning förväntar SAML-intyg i ett specifikt format, vilket kräver att du kan lägga till anpassade attributmappning konfigurationen för SAML-token attribut.</span><span class="sxs-lookup"><span data-stu-id="b7041-168">Your LinkedIn Learning application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="b7041-169">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="b7041-169">The following screenshot shows an example for this.</span></span> <span data-ttu-id="b7041-170">Standardvärdet för **användar-ID** är **user.userprincipalname** men LinkedIn Learning förväntar detta mappas med användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="b7041-170">The default value of **User Identifier** is **user.userprincipalname** but LinkedIn Learning expects this to be mapped with the user's email address.</span></span> <span data-ttu-id="b7041-171">Som du kan använda **user.mail** attribut i listan eller använda rätt attribut-värde baserat på konfigurationen för din organisation.</span><span class="sxs-lookup"><span data-stu-id="b7041-171">For that you can use **user.mail** attribute from the list or use the appropriate attribute value based on your organization configuration.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/updateusermail.png)
    
9. <span data-ttu-id="b7041-173">I **användarattribut** klickar du på **visa och redigera andra användarattribut** och ange attribut.</span><span class="sxs-lookup"><span data-stu-id="b7041-173">In **User Attributes** section, click **View and edit all other user attributes** and set the attributes.</span></span> <span data-ttu-id="b7041-174">Användaren behöver lägga till fyra anspråk med namnet **e-post**, **avdelning**, **Förnamn**, och **efternamn** och värdet är för att mappa med **user.mail**, **user.department**, **user.givenname**, och **user.surname** respektive</span><span class="sxs-lookup"><span data-stu-id="b7041-174">The user needs to add four claims named **email**, **department**, **firstname**, and **lastname** and the value is to be mapped with **user.mail**, **user.department**, **user.givenname**, and **user.surname** respectively</span></span>

    | <span data-ttu-id="b7041-175">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="b7041-175">Attribute Name</span></span> | <span data-ttu-id="b7041-176">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="b7041-176">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="b7041-177">E-post</span><span class="sxs-lookup"><span data-stu-id="b7041-177">email</span></span>| <span data-ttu-id="b7041-178">User.Mail</span><span class="sxs-lookup"><span data-stu-id="b7041-178">user.mail</span></span> |    
    | <span data-ttu-id="b7041-179">Avdelning</span><span class="sxs-lookup"><span data-stu-id="b7041-179">department</span></span>| <span data-ttu-id="b7041-180">User.Department</span><span class="sxs-lookup"><span data-stu-id="b7041-180">user.department</span></span> |
    | <span data-ttu-id="b7041-181">Förnamn</span><span class="sxs-lookup"><span data-stu-id="b7041-181">firstname</span></span>| <span data-ttu-id="b7041-182">User.givenName</span><span class="sxs-lookup"><span data-stu-id="b7041-182">user.givenname</span></span> |
    | <span data-ttu-id="b7041-183">Efternamn</span><span class="sxs-lookup"><span data-stu-id="b7041-183">lastname</span></span>| <span data-ttu-id="b7041-184">User.surname</span><span class="sxs-lookup"><span data-stu-id="b7041-184">user.surname</span></span> |
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/userattribute.png)
    
    <span data-ttu-id="b7041-186">a.</span><span class="sxs-lookup"><span data-stu-id="b7041-186">a.</span></span> <span data-ttu-id="b7041-187">Klicka på **lägga till attributet** att öppna dialogrutan attribut.</span><span class="sxs-lookup"><span data-stu-id="b7041-187">Click **Add Attribute** to open the attribute dialog.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_04.png)

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="b7041-190">b.</span><span class="sxs-lookup"><span data-stu-id="b7041-190">b.</span></span> <span data-ttu-id="b7041-191">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="b7041-191">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="b7041-192">c.</span><span class="sxs-lookup"><span data-stu-id="b7041-192">c.</span></span> <span data-ttu-id="b7041-193">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="b7041-193">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="b7041-194">d.</span><span class="sxs-lookup"><span data-stu-id="b7041-194">d.</span></span> <span data-ttu-id="b7041-195">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="b7041-195">Click **Ok**</span></span>

10. <span data-ttu-id="b7041-196">Utför följande steg på den **namn** attribut -</span><span class="sxs-lookup"><span data-stu-id="b7041-196">Perform the following steps on the **name** attribute-</span></span>

    <span data-ttu-id="b7041-197">a.</span><span class="sxs-lookup"><span data-stu-id="b7041-197">a.</span></span> <span data-ttu-id="b7041-198">Klicka på attributet för att öppna den **Redigera attribut** fönster.</span><span class="sxs-lookup"><span data-stu-id="b7041-198">Click on the attribute to open the **Edit Attribute** window.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinLearning-tutorial/url_update.png)

    <span data-ttu-id="b7041-200">b.</span><span class="sxs-lookup"><span data-stu-id="b7041-200">b.</span></span> <span data-ttu-id="b7041-201">Ta bort URL-värdet från den **namnområde**.</span><span class="sxs-lookup"><span data-stu-id="b7041-201">Delete the URL value from the **namespace**.</span></span>
    
    <span data-ttu-id="b7041-202">c.</span><span class="sxs-lookup"><span data-stu-id="b7041-202">c.</span></span> <span data-ttu-id="b7041-203">Klicka på **Ok** spara inställningen.</span><span class="sxs-lookup"><span data-stu-id="b7041-203">Click **Ok** to save the setting.</span></span>

11. <span data-ttu-id="b7041-204">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="b7041-204">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_certificate.png) 

12. <span data-ttu-id="b7041-206">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b7041-206">Click **Save**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_400.png)

13. <span data-ttu-id="b7041-208">Gå till **LinkedIn admininställningar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b7041-208">Go to **LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="b7041-209">Överför den XML-fil som du hämtade från Azure portal genom att klicka på alternativet överför XML-fil.</span><span class="sxs-lookup"><span data-stu-id="b7041-209">Upload the XML file you downloaded from the Azure portal by clicking the Upload XML file option.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

14. <span data-ttu-id="b7041-211">Klicka på **på** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b7041-211">Click **On** to enable SSO.</span></span> <span data-ttu-id="b7041-212">SSO status ändras från **inte ansluten** till **ansluten**</span><span class="sxs-lookup"><span data-stu-id="b7041-212">SSO status changes from **Not Connected** to **Connected**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b7041-214">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b7041-214">Creating an Azure AD test user</span></span>
<span data-ttu-id="b7041-215">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b7041-215">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="b7041-217">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="b7041-217">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b7041-218">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b7041-218">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b7041-220">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b7041-220">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b7041-222">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7041-222">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b7041-224">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b7041-224">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b7041-226">a.</span><span class="sxs-lookup"><span data-stu-id="b7041-226">a.</span></span> <span data-ttu-id="b7041-227">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b7041-227">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b7041-228">b.</span><span class="sxs-lookup"><span data-stu-id="b7041-228">b.</span></span> <span data-ttu-id="b7041-229">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b7041-229">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b7041-230">c.</span><span class="sxs-lookup"><span data-stu-id="b7041-230">c.</span></span> <span data-ttu-id="b7041-231">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b7041-231">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b7041-232">d.</span><span class="sxs-lookup"><span data-stu-id="b7041-232">d.</span></span> <span data-ttu-id="b7041-233">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b7041-233">Click **Create**.</span></span> 

### <a name="creating-a-linkedin-learning-test-user"></a><span data-ttu-id="b7041-234">Skapa en testanvändare LinkedIn-utbildning</span><span class="sxs-lookup"><span data-stu-id="b7041-234">Creating a LinkedIn Learning test user</span></span>

<span data-ttu-id="b7041-235">Länkade Learning programmet stöder.</span><span class="sxs-lookup"><span data-stu-id="b7041-235">Linked Learning Application supports.</span></span> <span data-ttu-id="b7041-236">Precis i tid användaretablering och efter autentisering skapas användare i programmet automatiskt.</span><span class="sxs-lookup"><span data-stu-id="b7041-236">Just in time user provisioning and after authentication users are created in the application automatically.</span></span> <span data-ttu-id="b7041-237">Om administratören sidan Inställningar på LinkedIn Learning portal Vänd växeln **automatiskt tilldela licenser** som aktiv för att aktivera precis i tid etablering och detta kan också tilldela en licens till användaren.</span><span class="sxs-lookup"><span data-stu-id="b7041-237">On the admin settings page on the LinkedIn Learning portal flip the switch **Automatically Assign licenses** to active to enable Just in time provisioning and this will also assign a license to the user.</span></span>
   
   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinLearning-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b7041-239">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="b7041-239">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b7041-240">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till LinkedIn Learning.</span><span class="sxs-lookup"><span data-stu-id="b7041-240">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LinkedIn Learning.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="b7041-242">**Om du vill tilldela LinkedIn Learning Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b7041-242">**To assign Britta Simon to LinkedIn Learning, perform the following steps:**</span></span>

1. <span data-ttu-id="b7041-243">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b7041-243">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b7041-245">Välj i listan med program **LinkedIn Learning**.</span><span class="sxs-lookup"><span data-stu-id="b7041-245">In the applications list, select **LinkedIn Learning**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_0001.png) 

3. <span data-ttu-id="b7041-247">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b7041-247">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="b7041-249">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="b7041-249">Click **Add** button.</span></span> <span data-ttu-id="b7041-250">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7041-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="b7041-252">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="b7041-252">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b7041-253">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7041-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b7041-254">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7041-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b7041-255">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7041-255">Testing single sign-on</span></span>

<span data-ttu-id="b7041-256">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="b7041-256">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b7041-257">När du klickar på panelen LinkedIn Learning på åtkomstpanelen du bör få sidan Azure inloggning och på efter lyckad inloggning kan du bör få till LinkedIn Learning programmet.</span><span class="sxs-lookup"><span data-stu-id="b7041-257">When you click the LinkedIn Learning tile in the Access Panel, you should get the Azure Sign-on page and on after successful sign-on, you should get into your LinkedIn Learning application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b7041-258">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b7041-258">Additional resources</span></span>

* [<span data-ttu-id="b7041-259">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b7041-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b7041-260">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b7041-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_203.png