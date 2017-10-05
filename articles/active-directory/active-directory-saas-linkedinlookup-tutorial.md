---
title: "Självstudier: Azure Active Directory-integrering med LinkedIn sökning | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och LinkedIn-sökning."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a2757a39-1ead-4a3e-91e4-270be3055683
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: e296431866a8611b30e72f286884890adf0f7e50
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-lookup"></a><span data-ttu-id="2004c-103">Självstudier: Azure Active Directory-integrering med LinkedIn-sökning</span><span class="sxs-lookup"><span data-stu-id="2004c-103">Tutorial: Azure Active Directory integration with LinkedIn Lookup</span></span>

<span data-ttu-id="2004c-104">I kursen får lära du att integrera LinkedIn sökning med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="2004c-104">In this tutorial, you learn how to integrate LinkedIn Lookup with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2004c-105">Integrera LinkedIn sökning med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="2004c-105">Integrating LinkedIn Lookup with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2004c-106">Du kan styra i Azure AD som har åtkomst till LinkedIn-sökning</span><span class="sxs-lookup"><span data-stu-id="2004c-106">You can control in Azure AD who has access to LinkedIn Lookup</span></span>
- <span data-ttu-id="2004c-107">Du kan aktivera användarna att automatiskt hämta loggat in på LinkedIn-sökning (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="2004c-107">You can enable your users to automatically get signed-on to LinkedIn Lookup (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2004c-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2004c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="2004c-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2004c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2004c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="2004c-110">Prerequisites</span></span>

<span data-ttu-id="2004c-111">För att konfigurera Azure AD-integrering med LinkedIn-sökning, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="2004c-111">To configure Azure AD integration with LinkedIn Lookup, you need the following items:</span></span>

- <span data-ttu-id="2004c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="2004c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2004c-113">En sökning LinkedIn enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="2004c-113">An LinkedIn Lookup single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2004c-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="2004c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2004c-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="2004c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2004c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="2004c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2004c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2004c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2004c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="2004c-118">Scenario description</span></span>
<span data-ttu-id="2004c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="2004c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2004c-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="2004c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2004c-121">Lägga till LinkedIn sökning från galleriet</span><span class="sxs-lookup"><span data-stu-id="2004c-121">Adding LinkedIn Lookup from the gallery</span></span>
2. <span data-ttu-id="2004c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2004c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-lookup-from-the-gallery"></a><span data-ttu-id="2004c-123">Lägga till LinkedIn sökning från galleriet</span><span class="sxs-lookup"><span data-stu-id="2004c-123">Adding LinkedIn Lookup from the gallery</span></span>
<span data-ttu-id="2004c-124">Du måste lägga till LinkedIn sökning från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av LinkedIn sökning i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2004c-124">To configure the integration of LinkedIn Lookup into Azure AD, you need to add LinkedIn Lookup from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2004c-125">**Utför följande steg för att lägga till LinkedIn sökning från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="2004c-125">**To add LinkedIn Lookup from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2004c-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2004c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2004c-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="2004c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="2004c-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2004c-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="2004c-131">Klicka på **nytt program** knappen överst för att lägga till nya program.</span><span class="sxs-lookup"><span data-stu-id="2004c-131">Click **New application** button on the top of the dialog to add new application.</span></span>

    ![Program][3]

4. <span data-ttu-id="2004c-133">I sökrutan skriver **LinkedIn sökning**.</span><span class="sxs-lookup"><span data-stu-id="2004c-133">In the search box, type **LinkedIn Lookup**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_search.png)

5. <span data-ttu-id="2004c-135">Välj i resultatpanelen **LinkedIn sökning**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="2004c-135">In the results panel, select **LinkedIn Lookup**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2004c-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2004c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2004c-138">Du konfigurera och testa Azure AD enkel inloggning med LinkedIn sökning baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="2004c-138">In this section, you configure and test Azure AD single sign-on with LinkedIn Lookup based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2004c-139">Azure AD måste du känna till motsvarande användaren i LinkedIn sökning till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="2004c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LinkedIn Lookup is to a user in Azure AD.</span></span> <span data-ttu-id="2004c-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i LinkedIn sökning upprättas.</span><span class="sxs-lookup"><span data-stu-id="2004c-140">In other words, a link relationship between an Azure AD user and the related user in LinkedIn Lookup needs to be established.</span></span>

<span data-ttu-id="2004c-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i LinkedIn-sökning.</span><span class="sxs-lookup"><span data-stu-id="2004c-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in LinkedIn Lookup.</span></span>

<span data-ttu-id="2004c-142">Om du vill konfigurera och testa Azure AD enkel inloggning med LinkedIn-sökning, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="2004c-142">To configure and test Azure AD single sign-on with LinkedIn Lookup, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2004c-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="2004c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2004c-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2004c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2004c-145">**[Skapa en testanvändare LinkedIn sökning](#creating-an-linkedin-lookup-test-user)**  – har en motsvarighet för Britta Simon LinkedIn-sökning som är kopplad till Azure AD-representation.</span><span class="sxs-lookup"><span data-stu-id="2004c-145">**[Creating an LinkedIn Lookup test user](#creating-an-linkedin-lookup-test-user)** - to have a counterpart of Britta Simon in LinkedIn Lookup that is linked to the Azure AD representation.</span></span>
4. <span data-ttu-id="2004c-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2004c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2004c-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="2004c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2004c-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2004c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2004c-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet LinkedIn-sökning.</span><span class="sxs-lookup"><span data-stu-id="2004c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LinkedIn Lookup application.</span></span>

<span data-ttu-id="2004c-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med LinkedIn sökning:**</span><span class="sxs-lookup"><span data-stu-id="2004c-150">**To configure Azure AD single sign-on with LinkedIn Lookup, perform the following steps:**</span></span>

1. <span data-ttu-id="2004c-151">I Azure-portalen på den **LinkedIn sökning** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2004c-151">In the Azure portal, on the **LinkedIn Lookup** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="2004c-153">På den **enkel inloggning** dialogrutan i **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2004c-153">On the **Single sign-on** dialog, in **Mode** select **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_samlbase.png)

3. <span data-ttu-id="2004c-155">I en annan webbläsarfönster inloggning till din **LinkedIn sökning** webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="2004c-155">In a different web browser window, sign-on to your **LinkedIn Lookup** website as an administrator.</span></span>

4. <span data-ttu-id="2004c-156">I **Kontocenter**, klickar du på **globala inställningar** under **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="2004c-156">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="2004c-157">Markera också **sökning** från den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="2004c-157">Also, select **Lookup** from the dropdown list.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_011.png)

5. <span data-ttu-id="2004c-159">Klicka på **eller klicka här om du vill läsa in och kopiera enskilda fält i formuläret** och kopiera **enhets-Id** och **Assertion konsumenten Access (ACS) Url**</span><span class="sxs-lookup"><span data-stu-id="2004c-159">Click **OR Click Here to load and copy individual fields from the form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_032.png)

6. <span data-ttu-id="2004c-161">På Azure-portalen under **LinkedIn sökning domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="2004c-161">On Azure portal, under **LinkedIn Lookup Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url.png)

    <span data-ttu-id="2004c-163">a.</span><span class="sxs-lookup"><span data-stu-id="2004c-163">a.</span></span> <span data-ttu-id="2004c-164">I den **identifierare** textruta, ange den **enhets-ID** kopieras från LinkedIn-portalen</span><span class="sxs-lookup"><span data-stu-id="2004c-164">In the **Identifier** textbox, enter the **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="2004c-165">b.</span><span class="sxs-lookup"><span data-stu-id="2004c-165">b.</span></span> <span data-ttu-id="2004c-166">I den **Reply URL** textruta ange den **Assertion konsumenten Access (ACS) Url** kopieras från LinkedIn-portalen</span><span class="sxs-lookup"><span data-stu-id="2004c-166">In the **Reply URL** textbox, enter the **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="2004c-167">Kontrollera **visa avancerade inställningar för URL: en**, om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="2004c-167">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url1.png)

    <span data-ttu-id="2004c-169">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://www.linkedIn.com/checkpoint/enterprise/login/<AccountId>?application=lookup`</span><span class="sxs-lookup"><span data-stu-id="2004c-169">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://www.linkedIn.com/checkpoint/enterprise/login/<AccountId>?application=lookup`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="2004c-170">Detta är inte verkliga värdet.</span><span class="sxs-lookup"><span data-stu-id="2004c-170">This is not real value.</span></span> <span data-ttu-id="2004c-171">Användaren måste uppdatera dessa värden med den faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="2004c-171">The user has to update these values with the actual Sign-On URL.</span></span> <span data-ttu-id="2004c-172">Kontakta [LinkedIn sökning klienten supportteamet](https://business.LinkedIn.com/lookup) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="2004c-172">Contact [LinkedIn Lookup Client support team](https://business.LinkedIn.com/lookup) to get this value.</span></span>

8. <span data-ttu-id="2004c-173">Din **LinkedIn sökning** program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="2004c-173">Your **LinkedIn Lookup** application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="2004c-174">Användaren har att lägga till anpassade attributmappning i konfigurationen för SAML-token attribut.</span><span class="sxs-lookup"><span data-stu-id="2004c-174">The user has to add custom attribute mappings to the SAML token attributes configuration.</span></span> <span data-ttu-id="2004c-175">Följande skärmbild visar ett exempel.</span><span class="sxs-lookup"><span data-stu-id="2004c-175">The following screenshot shows an example.</span></span> <span data-ttu-id="2004c-176">Standardvärdet för **användar-ID** är **user.userprincipalname** men LinkedIn sökning förväntar detta mappas med användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="2004c-176">The default value of **User Identifier** is **user.userprincipalname** but LinkedIn Lookup expects this to be mapped with the user's email address.</span></span> <span data-ttu-id="2004c-177">Du kan använda **user.mail** attribut i listan eller använda rätt attribut-värde baserat på konfigurationen för din organisation.</span><span class="sxs-lookup"><span data-stu-id="2004c-177">You can use **user.mail** attribute from the list or use the appropriate attribute value based on your organization configuration.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/updateusermail.png)
    
9. <span data-ttu-id="2004c-179">I **användarattribut** klickar du på **visa och redigera andra användarattribut** och ange attribut.</span><span class="sxs-lookup"><span data-stu-id="2004c-179">In **User Attributes** section, click **View and edit all other user attributes** and set the attributes.</span></span> <span data-ttu-id="2004c-180">Användaren behöver lägga till fyra anspråk med namnet **e-post**, **avdelning**, **Förnamn**, och **efternamn** och värdet är för att mappa med **user.mail**, **user.department**, **user.givenname**, och **user.surname** respektive</span><span class="sxs-lookup"><span data-stu-id="2004c-180">The user needs to add four claims named **email**,  **department**, **firstname**, and **lastname** and the value is to be mapped with **user.mail**, **user.department**, **user.givenname**, and **user.surname** respectively</span></span>

    | <span data-ttu-id="2004c-181">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="2004c-181">Attribute Name</span></span> | <span data-ttu-id="2004c-182">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="2004c-182">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="2004c-183">E-post</span><span class="sxs-lookup"><span data-stu-id="2004c-183">email</span></span>| <span data-ttu-id="2004c-184">User.Mail</span><span class="sxs-lookup"><span data-stu-id="2004c-184">user.mail</span></span> |    
    | <span data-ttu-id="2004c-185">Avdelning</span><span class="sxs-lookup"><span data-stu-id="2004c-185">department</span></span>| <span data-ttu-id="2004c-186">User.Department</span><span class="sxs-lookup"><span data-stu-id="2004c-186">user.department</span></span> |
    | <span data-ttu-id="2004c-187">Förnamn</span><span class="sxs-lookup"><span data-stu-id="2004c-187">firstname</span></span>| <span data-ttu-id="2004c-188">User.givenName</span><span class="sxs-lookup"><span data-stu-id="2004c-188">user.givenname</span></span> |
    | <span data-ttu-id="2004c-189">Efternamn</span><span class="sxs-lookup"><span data-stu-id="2004c-189">lastname</span></span>| <span data-ttu-id="2004c-190">User.surname</span><span class="sxs-lookup"><span data-stu-id="2004c-190">user.surname</span></span> |

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/userattribute.png)

    <span data-ttu-id="2004c-192">a.</span><span class="sxs-lookup"><span data-stu-id="2004c-192">a.</span></span> <span data-ttu-id="2004c-193">Klicka på **lägga till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2004c-193">Click **Add Attribute** to open the **Add Attribute** dialog.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/4.png)
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/5.png)
   
    <span data-ttu-id="2004c-196">b.</span><span class="sxs-lookup"><span data-stu-id="2004c-196">b.</span></span> <span data-ttu-id="2004c-197">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="2004c-197">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="2004c-198">c.</span><span class="sxs-lookup"><span data-stu-id="2004c-198">c.</span></span> <span data-ttu-id="2004c-199">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="2004c-199">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="2004c-200">d.</span><span class="sxs-lookup"><span data-stu-id="2004c-200">d.</span></span> <span data-ttu-id="2004c-201">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="2004c-201">Click **Ok**</span></span>

10. <span data-ttu-id="2004c-202">Utför följande steg på den **namn** attribut -</span><span class="sxs-lookup"><span data-stu-id="2004c-202">Perform the following steps on the **name** attribute-</span></span>

    <span data-ttu-id="2004c-203">a.</span><span class="sxs-lookup"><span data-stu-id="2004c-203">a.</span></span> <span data-ttu-id="2004c-204">Klicka på attributet för att öppna den **Redigera attribut** fönster.</span><span class="sxs-lookup"><span data-stu-id="2004c-204">Click on the attribute to open the **Edit Attribute** window.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/url_update.png)

    <span data-ttu-id="2004c-206">b.</span><span class="sxs-lookup"><span data-stu-id="2004c-206">b.</span></span> <span data-ttu-id="2004c-207">Ta bort URL-värdet från den **namnområde**.</span><span class="sxs-lookup"><span data-stu-id="2004c-207">Delete the URL value from the **namespace**.</span></span>
    
    <span data-ttu-id="2004c-208">c.</span><span class="sxs-lookup"><span data-stu-id="2004c-208">c.</span></span> <span data-ttu-id="2004c-209">Klicka på **Ok** spara inställningen.</span><span class="sxs-lookup"><span data-stu-id="2004c-209">Click **Ok** to save the setting.</span></span>

10. <span data-ttu-id="2004c-210">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="2004c-210">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_certificate.png) 

11. <span data-ttu-id="2004c-212">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="2004c-212">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_400.png)

12. <span data-ttu-id="2004c-214">Gå till **LinkedIn admininställningar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2004c-214">Go to **LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="2004c-215">Överför den XML-fil som du hämtade från Azure portal genom att klicka på den **överför XML-filen** alternativet.</span><span class="sxs-lookup"><span data-stu-id="2004c-215">Upload the XML file you downloaded from the Azure portal by clicking the **Upload XML file** option.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_metadata_03.png)

13. <span data-ttu-id="2004c-217">Klicka på **på** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2004c-217">Click **On** to enable SSO.</span></span> <span data-ttu-id="2004c-218">SSO status ändras från **inte ansluten** till **ansluten**</span><span class="sxs-lookup"><span data-stu-id="2004c-218">SSO status changes from **Not Connected** to **Connected**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_admin_05.png)

> [!TIP]
> <span data-ttu-id="2004c-220">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="2004c-220">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="2004c-221">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="2004c-221">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="2004c-222">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2004c-222">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2004c-223">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="2004c-223">Creating an Azure AD test user</span></span>
<span data-ttu-id="2004c-224">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2004c-224">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="2004c-226">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="2004c-226">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2004c-227">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2004c-227">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2004c-229">Gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="2004c-229">Go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2004c-231">Klicka på **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2004c-231">Click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2004c-233">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2004c-233">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2004c-235">a.</span><span class="sxs-lookup"><span data-stu-id="2004c-235">a.</span></span> <span data-ttu-id="2004c-236">I den **namn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="2004c-236">In the **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="2004c-237">b.</span><span class="sxs-lookup"><span data-stu-id="2004c-237">b.</span></span> <span data-ttu-id="2004c-238">I den **användarnamn** textruta typ av **e-postadress** av Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2004c-238">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="2004c-239">c.</span><span class="sxs-lookup"><span data-stu-id="2004c-239">c.</span></span> <span data-ttu-id="2004c-240">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="2004c-240">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="2004c-241">d.</span><span class="sxs-lookup"><span data-stu-id="2004c-241">d.</span></span> <span data-ttu-id="2004c-242">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2004c-242">Click **Create**.</span></span>
 
### <a name="creating-an-linkedin-lookup-test-user"></a><span data-ttu-id="2004c-243">Skapa en testanvändare LinkedIn-sökning</span><span class="sxs-lookup"><span data-stu-id="2004c-243">Creating an LinkedIn Lookup test user</span></span>

<span data-ttu-id="2004c-244">Länkade sökning programmet stöder bara i tid JIT-användaretablering och authentication-användare skapas automatiskt i programmet.</span><span class="sxs-lookup"><span data-stu-id="2004c-244">Linked Lookup Application supports Just in Time (JIT) user provisioning and after authentication users are created in the application automatically.</span></span> <span data-ttu-id="2004c-245">Aktivera **automatiskt tilldela licenser** tilldela en licens till användaren.</span><span class="sxs-lookup"><span data-stu-id="2004c-245">Activate **Automatically assign licenses** to assign a license to the user.</span></span>
   
   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedin_admin_license.png)

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="2004c-247">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="2004c-247">Assigning the Azure AD test user</span></span>

<span data-ttu-id="2004c-248">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till LinkedIn-sökning.</span><span class="sxs-lookup"><span data-stu-id="2004c-248">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LinkedIn Lookup.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="2004c-250">**Om du vill tilldela LinkedIn sökning Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2004c-250">**To assign Britta Simon to LinkedIn Lookup, perform the following steps:**</span></span>

1. <span data-ttu-id="2004c-251">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2004c-251">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="2004c-253">Välj i listan med program **LinkedIn sökning**.</span><span class="sxs-lookup"><span data-stu-id="2004c-253">In the applications list, select **LinkedIn Lookup**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_app.png) 

3. <span data-ttu-id="2004c-255">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="2004c-255">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="2004c-257">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="2004c-257">Click **Add** button.</span></span> <span data-ttu-id="2004c-258">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2004c-258">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="2004c-260">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="2004c-260">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="2004c-261">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2004c-261">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2004c-262">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2004c-262">Click **Assign** button on **Add Assignment** dialog.</span></span>

    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2004c-263">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2004c-263">Testing single sign-on</span></span>

<span data-ttu-id="2004c-264">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="2004c-264">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="2004c-265">När du klickar på panelen LinkedIn-sökning på åtkomstpanelen bör du omdirigeras till organisationens sida där du måste ange ditt personliga LinkedIn-kontoinformation.</span><span class="sxs-lookup"><span data-stu-id="2004c-265">When you click the LinkedIn Lookup tile in the Access Panel, you should be redirected to Organizational page where you have to provide your personal LinkedIn account details.</span></span> <span data-ttu-id="2004c-266">Den länkar ditt eget konto med ditt företag LinkedIn-konto.</span><span class="sxs-lookup"><span data-stu-id="2004c-266">It links your personal account with your LinkedIn business account.</span></span> 

<span data-ttu-id="2004c-267">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2004c-267">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2004c-268">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2004c-268">Additional resources</span></span>

* [<span data-ttu-id="2004c-269">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2004c-269">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2004c-270">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2004c-270">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_203.png

