---
title: "Självstudier: Azure Active Directory-integrering med LinkedInSalesNavigator | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och LinkedInSalesNavigator."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7a9fa8f3-d611-4ffe-8d50-04e9586b24da
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: ef26a16e79d9c9b0654634960b57dc59827b2c24
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-sales-navigator"></a><span data-ttu-id="99f4e-103">Självstudier: Azure Active Directory-integrering med LinkedIn försäljning Navigator</span><span class="sxs-lookup"><span data-stu-id="99f4e-103">Tutorial: Azure Active Directory integration with LinkedIn Sales Navigator</span></span>

<span data-ttu-id="99f4e-104">Lär dig hur du integrerar Navigatören för LinkedIn försäljning med Azure Active Directory (AD Azure) i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="99f4e-104">In this tutorial, you learn how to integrate LinkedIn Sales Navigator with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="99f4e-105">Integrera LinkedIn försäljning Navigator med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="99f4e-105">Integrating LinkedIn Sales Navigator with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="99f4e-106">Du kan styra i Azure AD som har åtkomst till LinkedIn försäljning Navigator</span><span class="sxs-lookup"><span data-stu-id="99f4e-106">You can control in Azure AD who has access to LinkedIn Sales Navigator</span></span>
- <span data-ttu-id="99f4e-107">Du kan aktivera användarna att automatiskt hämta loggat in på LinkedIn försäljning Navigator (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="99f4e-107">You can enable your users to automatically get signed-on to LinkedIn Sales Navigator (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="99f4e-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="99f4e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="99f4e-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD Bläddra [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="99f4e-109">If you want to know more details about SaaS app integration with Azure AD, browse [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="99f4e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="99f4e-110">Prerequisites</span></span>

<span data-ttu-id="99f4e-111">Om du vill konfigurera Azure AD-integrering med LinkedIn försäljning Navigator behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="99f4e-111">To configure Azure AD integration with LinkedIn Sales Navigator, you need the following items:</span></span>

- <span data-ttu-id="99f4e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="99f4e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="99f4e-113">En LinkedIn försäljning Navigator enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="99f4e-113">A LinkedIn Sales Navigator single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="99f4e-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="99f4e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="99f4e-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="99f4e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="99f4e-116">Undvik att använda i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="99f4e-116">Avoid using your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="99f4e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="99f4e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="99f4e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="99f4e-118">Scenario description</span></span>
<span data-ttu-id="99f4e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="99f4e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="99f4e-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="99f4e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="99f4e-121">Lägga till LinkedIn försäljning Navigator från galleriet</span><span class="sxs-lookup"><span data-stu-id="99f4e-121">Adding LinkedIn Sales Navigator from the gallery</span></span>
2. <span data-ttu-id="99f4e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="99f4e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-sales-navigator-from-the-gallery"></a><span data-ttu-id="99f4e-123">Lägga till LinkedIn försäljning Navigator från galleriet</span><span class="sxs-lookup"><span data-stu-id="99f4e-123">Adding LinkedIn Sales Navigator from the gallery</span></span>
<span data-ttu-id="99f4e-124">Du måste lägga till LinkedIn försäljning Navigator från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av LinkedIn försäljning Navigator i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="99f4e-124">To configure the integration of LinkedIn Sales Navigator into Azure AD, you need to add LinkedIn Sales Navigator from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="99f4e-125">**Utför följande steg för att lägga till LinkedIn försäljning Navigator från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="99f4e-125">**To add LinkedIn Sales Navigator from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="99f4e-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="99f4e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="99f4e-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="99f4e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="99f4e-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="99f4e-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="99f4e-131">Klicka på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99f4e-131">Click **New application** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="99f4e-133">I sökrutan skriver **LinkedIn försäljning Navigator**.</span><span class="sxs-lookup"><span data-stu-id="99f4e-133">In the search box, type **LinkedIn Sales Navigator**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_search.png)

5. <span data-ttu-id="99f4e-135">Välj i resultatpanelen **LinkedIn försäljning Navigator**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="99f4e-135">In the results panel, select **LinkedIn Sales Navigator**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="99f4e-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="99f4e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="99f4e-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LinkedIn försäljning Navigator baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="99f4e-138">In this section, you configure and test Azure AD single sign-on with LinkedIn Sales Navigator based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="99f4e-139">Azure AD måste du känna till användaren i LinkedIn försäljning Navigator motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="99f4e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LinkedIn Sales Navigator is to a user in Azure AD.</span></span> <span data-ttu-id="99f4e-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i LinkedIn försäljning Navigator upprättas.</span><span class="sxs-lookup"><span data-stu-id="99f4e-140">In other words, a link relationship between an Azure AD user and the related user in LinkedIn Sales Navigator needs to be established.</span></span>

<span data-ttu-id="99f4e-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i LinkedIn försäljning Navigator.</span><span class="sxs-lookup"><span data-stu-id="99f4e-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in LinkedIn Sales Navigator.</span></span>

<span data-ttu-id="99f4e-142">Om du vill konfigurera och testa Azure AD enkel inloggning med LinkedIn försäljning Navigator, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="99f4e-142">To configure and test Azure AD single sign-on with LinkedIn Sales Navigator, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="99f4e-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="99f4e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="99f4e-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="99f4e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="99f4e-145">**[Skapa en testanvändare LinkedIn försäljning Navigator](#creating-a-linkedin-sales-navigator-test-user)**  – du har en motsvarighet för Britta Simon i LinkedIn försäljning Navigator som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="99f4e-145">**[Creating a LinkedIn Sales Navigator test user](#creating-a-linkedin-sales-navigator-test-user)** - to have a counterpart of Britta Simon in LinkedIn Sales Navigator that is linked to the Azure AD representation of the user.</span></span>
4. <span data-ttu-id="99f4e-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="99f4e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="99f4e-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="99f4e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="99f4e-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="99f4e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="99f4e-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet LinkedIn försäljning Navigator.</span><span class="sxs-lookup"><span data-stu-id="99f4e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LinkedIn Sales Navigator application.</span></span>

<span data-ttu-id="99f4e-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med LinkedIn försäljning Navigator:**</span><span class="sxs-lookup"><span data-stu-id="99f4e-150">**To configure Azure AD single sign-on with LinkedIn Sales Navigator, perform the following steps:**</span></span>

1. <span data-ttu-id="99f4e-151">I Azure-portalen på den **LinkedIn försäljning Navigator** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="99f4e-151">In the Azure portal, on the **LinkedIn Sales Navigator** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="99f4e-153">På den **enkel inloggning** dialogrutan i **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="99f4e-153">On the **Single sign-on** dialog, in **Mode** select **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_samlbase.png)

3. <span data-ttu-id="99f4e-155">I en annan webbläsarfönster inloggning till din **LinkedIn försäljning Navigator** webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="99f4e-155">In a different web browser window, sign-on to your **LinkedIn Sales Navigator** website as an administrator.</span></span>

4. <span data-ttu-id="99f4e-156">I **Kontocenter**, klickar du på **globala inställningar** under **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="99f4e-156">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="99f4e-157">Markera också **försäljning Navigator** från den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="99f4e-157">Also, select **Sales Navigator** from the dropdown list.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

5. <span data-ttu-id="99f4e-159">Klicka på **eller klicka här om du vill läsa in och kopiera enskilda fält i formuläret** och kopiera **enhets-Id** och **Assertion konsumenten Access (ACS) Url**.</span><span class="sxs-lookup"><span data-stu-id="99f4e-159">Click **OR Click Here to load and copy individual fields from the form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

6. <span data-ttu-id="99f4e-161">På Azure-portalen under **URL: er och domänen för LinkedIn förs Navigator** avsnittet, utför följande steg om du vill konfigurera programmet i **IDP** initierade läge.</span><span class="sxs-lookup"><span data-stu-id="99f4e-161">On Azure portal, under **LinkedIn Sales Navigator Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url1.png)

    <span data-ttu-id="99f4e-163">a.</span><span class="sxs-lookup"><span data-stu-id="99f4e-163">a.</span></span> <span data-ttu-id="99f4e-164">I den **identifierare** textruta, ange den **enhets-ID** kopieras från LinkedIn-portalen</span><span class="sxs-lookup"><span data-stu-id="99f4e-164">In the **Identifier** textbox, enter the **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="99f4e-165">b.</span><span class="sxs-lookup"><span data-stu-id="99f4e-165">b.</span></span> <span data-ttu-id="99f4e-166">I den **Reply URL** textruta ange den **Assertion konsumenten Access (ACS) Url** kopieras från LinkedIn-portalen</span><span class="sxs-lookup"><span data-stu-id="99f4e-166">In the **Reply URL** textbox, enter the **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="99f4e-167">Kontrollera **visa avancerade inställningar för URL: en**, om du vill konfigurera programmet i **SP** initierade läge.</span><span class="sxs-lookup"><span data-stu-id="99f4e-167">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url2.png)

    <span data-ttu-id="99f4e-169">I den **inloggnings-URL** textruta Skriv det värde som använder följande mönster:`https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`</span><span class="sxs-lookup"><span data-stu-id="99f4e-169">In the **Sign-on URL** textbox, type the value using the following pattern: `https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`</span></span>

8. <span data-ttu-id="99f4e-170">Din **LinkedIn försäljning Navigator** program förväntar SAML-intyg i ett specifikt format, vilket kräver att du kan lägga till anpassade attributmappning konfigurationen för SAML-token attribut.</span><span class="sxs-lookup"><span data-stu-id="99f4e-170">Your **LinkedIn Sales Navigator** application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="99f4e-171">Följande skärmbild visar ett exempel.</span><span class="sxs-lookup"><span data-stu-id="99f4e-171">The following screenshot shows an example.</span></span> <span data-ttu-id="99f4e-172">Standardvärdet för **användar-ID** är **user.userprincipalname** men LinkedIn försäljning Navigator förväntar att det ska mappas med användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="99f4e-172">The default value of **User Identifier** is **user.userprincipalname** but LinkedIn Sales Navigator expects it to be mapped with the user's email address.</span></span> <span data-ttu-id="99f4e-173">Du kan använda **user.mail** attribut i listan eller använda rätt attribut-värde baserat på konfigurationen för din organisation.</span><span class="sxs-lookup"><span data-stu-id="99f4e-173">You can use **user.mail** attribute from the list or use the appropriate attribute value based on your organization configuration.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/updateusermail.png)
    
9. <span data-ttu-id="99f4e-175">I **användarattribut** klickar du på **visa och redigera andra användarattribut** och ange attribut.</span><span class="sxs-lookup"><span data-stu-id="99f4e-175">In **User Attributes** section, click **View and edit all other user attributes** and set the attributes.</span></span> <span data-ttu-id="99f4e-176">Användaren behöver lägga till fyra anspråk med namnet **e-post**, **avdelning**, **Förnamn**, och **efternamn** och värdet är för att mappa med **user.mail**, **user.department**, **user.givenname**, och **user.surname** respektive</span><span class="sxs-lookup"><span data-stu-id="99f4e-176">The user needs to add four claims named **email**, **department**, **firstname**, and **lastname** and the value is to be mapped with **user.mail**, **user.department**, **user.givenname**, and **user.surname** respectively</span></span>

    | <span data-ttu-id="99f4e-177">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="99f4e-177">Attribute Name</span></span> | <span data-ttu-id="99f4e-178">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="99f4e-178">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="99f4e-179">E-post</span><span class="sxs-lookup"><span data-stu-id="99f4e-179">email</span></span>| <span data-ttu-id="99f4e-180">User.Mail</span><span class="sxs-lookup"><span data-stu-id="99f4e-180">user.mail</span></span> |
    | <span data-ttu-id="99f4e-181">Avdelning</span><span class="sxs-lookup"><span data-stu-id="99f4e-181">department</span></span>| <span data-ttu-id="99f4e-182">User.Department</span><span class="sxs-lookup"><span data-stu-id="99f4e-182">user.department</span></span> |
    | <span data-ttu-id="99f4e-183">Förnamn</span><span class="sxs-lookup"><span data-stu-id="99f4e-183">firstname</span></span>| <span data-ttu-id="99f4e-184">User.givenName</span><span class="sxs-lookup"><span data-stu-id="99f4e-184">user.givenname</span></span> |
    | <span data-ttu-id="99f4e-185">Efternamn</span><span class="sxs-lookup"><span data-stu-id="99f4e-185">lastname</span></span>| <span data-ttu-id="99f4e-186">User.surname</span><span class="sxs-lookup"><span data-stu-id="99f4e-186">user.surname</span></span> |
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/userattribute.png)
    
    <span data-ttu-id="99f4e-188">a.</span><span class="sxs-lookup"><span data-stu-id="99f4e-188">a.</span></span> <span data-ttu-id="99f4e-189">Klicka på **lägga till attributet** att öppna dialogrutan attribut.</span><span class="sxs-lookup"><span data-stu-id="99f4e-189">Click on **Add Attribute** to open the attribute dialog.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_04.png)
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_05.png)
   
    <span data-ttu-id="99f4e-192">b.</span><span class="sxs-lookup"><span data-stu-id="99f4e-192">b.</span></span> <span data-ttu-id="99f4e-193">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="99f4e-193">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="99f4e-194">c.</span><span class="sxs-lookup"><span data-stu-id="99f4e-194">c.</span></span> <span data-ttu-id="99f4e-195">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="99f4e-195">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="99f4e-196">d.</span><span class="sxs-lookup"><span data-stu-id="99f4e-196">d.</span></span> <span data-ttu-id="99f4e-197">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="99f4e-197">Click **Ok**</span></span>

10. <span data-ttu-id="99f4e-198">Utför följande steg på den **namn** attribut -</span><span class="sxs-lookup"><span data-stu-id="99f4e-198">Perform the following steps on the **name** attribute-</span></span>

    <span data-ttu-id="99f4e-199">a.</span><span class="sxs-lookup"><span data-stu-id="99f4e-199">a.</span></span> <span data-ttu-id="99f4e-200">Klicka på attributet för att öppna den **Redigera attribut** fönster.</span><span class="sxs-lookup"><span data-stu-id="99f4e-200">Click on the attribute to open the **Edit Attribute** window.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/url_update.png)

    <span data-ttu-id="99f4e-202">b.</span><span class="sxs-lookup"><span data-stu-id="99f4e-202">b.</span></span> <span data-ttu-id="99f4e-203">Ta bort URL-värdet från den **namnområde**.</span><span class="sxs-lookup"><span data-stu-id="99f4e-203">Delete the URL value from the **namespace**.</span></span>
    
    <span data-ttu-id="99f4e-204">c.</span><span class="sxs-lookup"><span data-stu-id="99f4e-204">c.</span></span> <span data-ttu-id="99f4e-205">Klicka på **Ok** spara inställningen.</span><span class="sxs-lookup"><span data-stu-id="99f4e-205">Click **Ok** to save the setting.</span></span>

11. <span data-ttu-id="99f4e-206">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="99f4e-206">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_certificate.png) 

12. <span data-ttu-id="99f4e-208">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="99f4e-208">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_400.png)

13. <span data-ttu-id="99f4e-210">Gå till **LinkedIn admininställningar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="99f4e-210">Go to **LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="99f4e-211">Klicka på **överför XML-filen** att överföra Metadata XML-filen som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="99f4e-211">Click **Upload XML file** to upload the Metadata XML file that you have downloaded from the Azure portal.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

14. <span data-ttu-id="99f4e-213">Klicka på **på** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="99f4e-213">Click **On** to enable SSO.</span></span> <span data-ttu-id="99f4e-214">SSO status ändras från **inte ansluten** till **ansluten**</span><span class="sxs-lookup"><span data-stu-id="99f4e-214">SSO status changes from **Not Connected** to **Connected**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)


> [!TIP]
> <span data-ttu-id="99f4e-216">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="99f4e-216">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="99f4e-217">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="99f4e-217">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="99f4e-218">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="99f4e-218">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="99f4e-219">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="99f4e-219">Creating an Azure AD test user</span></span>
<span data-ttu-id="99f4e-220">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="99f4e-220">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="99f4e-222">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="99f4e-222">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="99f4e-223">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="99f4e-223">In the **Azure  portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="99f4e-225">Gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="99f4e-225">Go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="99f4e-227">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99f4e-227">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="99f4e-229">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="99f4e-229">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="99f4e-231">a.</span><span class="sxs-lookup"><span data-stu-id="99f4e-231">a.</span></span> <span data-ttu-id="99f4e-232">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="99f4e-232">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="99f4e-233">b.</span><span class="sxs-lookup"><span data-stu-id="99f4e-233">b.</span></span> <span data-ttu-id="99f4e-234">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="99f4e-234">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="99f4e-235">c.</span><span class="sxs-lookup"><span data-stu-id="99f4e-235">c.</span></span> <span data-ttu-id="99f4e-236">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="99f4e-236">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="99f4e-237">d.</span><span class="sxs-lookup"><span data-stu-id="99f4e-237">d.</span></span> <span data-ttu-id="99f4e-238">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="99f4e-238">Click **Create**.</span></span>
 
### <a name="creating-a-linkedin-sales-navigator-test-user"></a><span data-ttu-id="99f4e-239">Skapa en testanvändare LinkedIn försäljning Navigator</span><span class="sxs-lookup"><span data-stu-id="99f4e-239">Creating a LinkedIn Sales Navigator test user</span></span>

<span data-ttu-id="99f4e-240">Länkade försäljning Navigator programmet stöder bara i tid JIT-användaretablering och authentication-användare skapas automatiskt i programmet.</span><span class="sxs-lookup"><span data-stu-id="99f4e-240">Linked Sales Navigator Application supports Just in Time (JIT) user provisioning and after authentication users are created in the application automatically.</span></span> <span data-ttu-id="99f4e-241">Aktivera **automatiskt tilldela licenser** tilldela en licens till användaren.</span><span class="sxs-lookup"><span data-stu-id="99f4e-241">Activate **Automatically assign licenses** to assign a license to the user.</span></span>
   
   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="99f4e-243">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="99f4e-243">Assigning the Azure AD test user</span></span>

<span data-ttu-id="99f4e-244">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till LinkedIn försäljning Navigator.</span><span class="sxs-lookup"><span data-stu-id="99f4e-244">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LinkedIn Sales Navigator.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="99f4e-246">**Om du vill tilldela LinkedIn försäljning Navigator Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="99f4e-246">**To assign Britta Simon to LinkedIn Sales Navigator, perform the following steps:**</span></span>

1. <span data-ttu-id="99f4e-247">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="99f4e-247">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="99f4e-249">Välj i listan med program **LinkedIn försäljning Navigator**.</span><span class="sxs-lookup"><span data-stu-id="99f4e-249">In the applications list, select **LinkedIn Sales Navigator**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_app.png) 

3. <span data-ttu-id="99f4e-251">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="99f4e-251">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="99f4e-253">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="99f4e-253">Click **Add** button.</span></span> <span data-ttu-id="99f4e-254">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99f4e-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="99f4e-256">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="99f4e-256">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="99f4e-257">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99f4e-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="99f4e-258">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99f4e-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="99f4e-259">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="99f4e-259">Testing single sign-on</span></span>

<span data-ttu-id="99f4e-260">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="99f4e-260">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="99f4e-261">När du klickar på panelen LinkedIn försäljning Navigator på åtkomstpanelen bör du omdirigeras till organisationens sida där du måste ange ditt personliga LinkedIn-kontoinformation.</span><span class="sxs-lookup"><span data-stu-id="99f4e-261">When you click the LinkedIn Sales Navigator tile in the Access Panel, you should be redirected to Organizational page where you have to provide your personal LinkedIn account details.</span></span> <span data-ttu-id="99f4e-262">Den länkar ditt eget konto med ditt företag LinkedIn-konto.</span><span class="sxs-lookup"><span data-stu-id="99f4e-262">It links your personal account with your LinkedIn business account.</span></span> <span data-ttu-id="99f4e-263">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="99f4e-263">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="99f4e-264">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="99f4e-264">Additional resources</span></span>

* [<span data-ttu-id="99f4e-265">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="99f4e-265">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="99f4e-266">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="99f4e-266">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_203.png

