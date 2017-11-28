---
title: "Självstudier: Azure Active Directory-integrering med LinkedInSalesNavigator | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och LinkedInSalesNavigator."
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
ms.openlocfilehash: 443d302d40d7af16aba5114e00963f23ea8d12d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-sales-navigator"></a><span data-ttu-id="f5dc9-103">Självstudier: Azure Active Directory-integrering med LinkedIn försäljning Navigator</span><span class="sxs-lookup"><span data-stu-id="f5dc9-103">Tutorial: Azure Active Directory integration with LinkedIn Sales Navigator</span></span>

<span data-ttu-id="f5dc9-104">I kursen får du lära dig hur toointegrate LinkedIn försäljning Navigator med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="f5dc9-104">In this tutorial, you learn how toointegrate LinkedIn Sales Navigator with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f5dc9-105">Integrera LinkedIn försäljning Navigator med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f5dc9-105">Integrating LinkedIn Sales Navigator with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f5dc9-106">Du kan styra i Azure AD som har åtkomst tooLinkedIn försäljning Navigator</span><span class="sxs-lookup"><span data-stu-id="f5dc9-106">You can control in Azure AD who has access tooLinkedIn Sales Navigator</span></span>
- <span data-ttu-id="f5dc9-107">Du kan aktivera din användare tooautomatically get inloggade tooLinkedIn försäljning Navigator (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="f5dc9-107">You can enable your users tooautomatically get signed-on tooLinkedIn Sales Navigator (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f5dc9-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f5dc9-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f5dc9-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, Bläddra [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f5dc9-109">If you want tooknow more details about SaaS app integration with Azure AD, browse [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f5dc9-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f5dc9-110">Prerequisites</span></span>

<span data-ttu-id="f5dc9-111">tooconfigure Azure AD-integrering med LinkedIn försäljning Navigator måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="f5dc9-111">tooconfigure Azure AD integration with LinkedIn Sales Navigator, you need hello following items:</span></span>

- <span data-ttu-id="f5dc9-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f5dc9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f5dc9-113">En LinkedIn försäljning Navigator enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="f5dc9-113">A LinkedIn Sales Navigator single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f5dc9-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f5dc9-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f5dc9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f5dc9-116">Undvik att använda i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-116">Avoid using your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f5dc9-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f5dc9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f5dc9-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="f5dc9-118">Scenario description</span></span>
<span data-ttu-id="f5dc9-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f5dc9-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="f5dc9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f5dc9-121">Lägga till LinkedIn försäljning Navigator från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f5dc9-121">Adding LinkedIn Sales Navigator from hello gallery</span></span>
2. <span data-ttu-id="f5dc9-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f5dc9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-sales-navigator-from-hello-gallery"></a><span data-ttu-id="f5dc9-123">Lägga till LinkedIn försäljning Navigator från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f5dc9-123">Adding LinkedIn Sales Navigator from hello gallery</span></span>
<span data-ttu-id="f5dc9-124">tooconfigure hello integrering av LinkedIn försäljning Navigator i Azure AD, behöver du tooadd LinkedIn försäljning Navigator hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-124">tooconfigure hello integration of LinkedIn Sales Navigator into Azure AD, you need tooadd LinkedIn Sales Navigator from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f5dc9-125">**tooadd LinkedIn försäljning Navigator från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f5dc9-125">**tooadd LinkedIn Sales Navigator from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f5dc9-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f5dc9-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f5dc9-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="f5dc9-131">Klicka på **nytt program** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="f5dc9-133">Skriv i sökrutan hello **LinkedIn försäljning Navigator**.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-133">In hello search box, type **LinkedIn Sales Navigator**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_search.png)

5. <span data-ttu-id="f5dc9-135">Markera hello resultat på panelen **LinkedIn försäljning Navigator**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-135">In hello results panel, select **LinkedIn Sales Navigator**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f5dc9-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f5dc9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f5dc9-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LinkedIn försäljning Navigator baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-138">In this section, you configure and test Azure AD single sign-on with LinkedIn Sales Navigator based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f5dc9-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i LinkedIn försäljning Navigator är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LinkedIn Sales Navigator is tooa user in Azure AD.</span></span> <span data-ttu-id="f5dc9-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i LinkedIn försäljning Navigator toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-140">In other words, a link relationship between an Azure AD user and hello related user in LinkedIn Sales Navigator needs toobe established.</span></span>

<span data-ttu-id="f5dc9-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i LinkedIn försäljning Navigator.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in LinkedIn Sales Navigator.</span></span>

<span data-ttu-id="f5dc9-142">tooconfigure och testa Azure AD enkel inloggning med LinkedIn försäljning Navigator, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="f5dc9-142">tooconfigure and test Azure AD single sign-on with LinkedIn Sales Navigator, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f5dc9-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f5dc9-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f5dc9-145">**[Skapa en testanvändare LinkedIn försäljning Navigator](#creating-a-linkedin-sales-navigator-test-user)**  -toohave en motsvarighet för Britta Simon i LinkedIn försäljning Navigator som är länkade toohello Azure AD-representation av hello användare.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-145">**[Creating a LinkedIn Sales Navigator test user](#creating-a-linkedin-sales-navigator-test-user)** - toohave a counterpart of Britta Simon in LinkedIn Sales Navigator that is linked toohello Azure AD representation of hello user.</span></span>
4. <span data-ttu-id="f5dc9-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f5dc9-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f5dc9-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f5dc9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f5dc9-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet LinkedIn försäljning Navigator.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LinkedIn Sales Navigator application.</span></span>

<span data-ttu-id="f5dc9-150">**tooconfigure Azure AD enkel inloggning med LinkedIn försäljning Navigator utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f5dc9-150">**tooconfigure Azure AD single sign-on with LinkedIn Sales Navigator, perform hello following steps:**</span></span>

1. <span data-ttu-id="f5dc9-151">I hello Azure-portalen på hello **LinkedIn försäljning Navigator** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-151">In hello Azure portal, on hello **LinkedIn Sales Navigator** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="f5dc9-153">På hello **enkel inloggning** dialogrutan i **läge** Välj **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-153">On hello **Single sign-on** dialog, in **Mode** select **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_samlbase.png)

3. <span data-ttu-id="f5dc9-155">I en annan webbläsarfönster inloggning tooyour **LinkedIn försäljning Navigator** webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-155">In a different web browser window, sign-on tooyour **LinkedIn Sales Navigator** website as an administrator.</span></span>

4. <span data-ttu-id="f5dc9-156">I **Kontocenter**, klickar du på **globala inställningar** under **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-156">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="f5dc9-157">Markera också **försäljning Navigator** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-157">Also, select **Sales Navigator** from hello dropdown list.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

5. <span data-ttu-id="f5dc9-159">Klicka på **eller klicka här tooload och kopiera enskilda fält från hello form** och kopiera **enhets-Id** och **Assertion konsumenten Access (ACS) Url**.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-159">Click **OR Click Here tooload and copy individual fields from hello form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

6. <span data-ttu-id="f5dc9-161">På Azure-portalen under **URL: er och domänen för LinkedIn förs Navigator** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **IDP** initierade läge.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-161">On Azure portal, under **LinkedIn Sales Navigator Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url1.png)

    <span data-ttu-id="f5dc9-163">a.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-163">a.</span></span> <span data-ttu-id="f5dc9-164">I hello **identifierare** textruta ange hello **enhets-ID** kopieras från LinkedIn-portalen</span><span class="sxs-lookup"><span data-stu-id="f5dc9-164">In hello **Identifier** textbox, enter hello **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="f5dc9-165">b.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-165">b.</span></span> <span data-ttu-id="f5dc9-166">I hello **Reply URL** textruta ange hello **Assertion konsumenten Access (ACS) Url** kopieras från LinkedIn-portalen</span><span class="sxs-lookup"><span data-stu-id="f5dc9-166">In hello **Reply URL** textbox, enter hello **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="f5dc9-167">Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-167">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url2.png)

    <span data-ttu-id="f5dc9-169">I hello **inloggnings-URL** textruta hello TYPVÄRDE med hello följande mönster:`https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`</span><span class="sxs-lookup"><span data-stu-id="f5dc9-169">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`</span></span>

8. <span data-ttu-id="f5dc9-170">Din **LinkedIn försäljning Navigator** program förväntar hello SAML intyg i ett specifikt format, vilket kräver tooadd attributet mappningar tooyour SAML-token attribut konfiguration.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-170">Your **LinkedIn Sales Navigator** application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="f5dc9-171">hello följande skärmbild visar ett exempel.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-171">hello following screenshot shows an example.</span></span> <span data-ttu-id="f5dc9-172">Hej standardvärdet **användar-ID** är **user.userprincipalname** men LinkedIn försäljning Navigator förväntar att det toobe som mappats till hello användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-172">hello default value of **User Identifier** is **user.userprincipalname** but LinkedIn Sales Navigator expects it toobe mapped with hello user's email address.</span></span> <span data-ttu-id="f5dc9-173">Du kan använda **user.mail** attribut hello listan eller använda hello rätt attribut-värde baserat på konfigurationen för din organisation.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-173">You can use **user.mail** attribute from hello list or use hello appropriate attribute value based on your organization configuration.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/updateusermail.png)
    
9. <span data-ttu-id="f5dc9-175">I **användarattribut** klickar du på **visa och redigera andra användarattribut** och ange hello-attribut.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-175">In **User Attributes** section, click **View and edit all other user attributes** and set hello attributes.</span></span> <span data-ttu-id="f5dc9-176">hello användare behöver tooadd fyra anspråk med namnet **e-post**, **avdelning**, **Förnamn**, och **efternamn** och hello-värdet är toobe som mappats till **user.mail**, **user.department**, **user.givenname**, och **user.surname** respektive</span><span class="sxs-lookup"><span data-stu-id="f5dc9-176">hello user needs tooadd four claims named **email**, **department**, **firstname**, and **lastname** and hello value is toobe mapped with **user.mail**, **user.department**, **user.givenname**, and **user.surname** respectively</span></span>

    | <span data-ttu-id="f5dc9-177">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="f5dc9-177">Attribute Name</span></span> | <span data-ttu-id="f5dc9-178">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="f5dc9-178">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="f5dc9-179">E-post</span><span class="sxs-lookup"><span data-stu-id="f5dc9-179">email</span></span>| <span data-ttu-id="f5dc9-180">User.Mail</span><span class="sxs-lookup"><span data-stu-id="f5dc9-180">user.mail</span></span> |
    | <span data-ttu-id="f5dc9-181">Avdelning</span><span class="sxs-lookup"><span data-stu-id="f5dc9-181">department</span></span>| <span data-ttu-id="f5dc9-182">User.Department</span><span class="sxs-lookup"><span data-stu-id="f5dc9-182">user.department</span></span> |
    | <span data-ttu-id="f5dc9-183">Förnamn</span><span class="sxs-lookup"><span data-stu-id="f5dc9-183">firstname</span></span>| <span data-ttu-id="f5dc9-184">User.givenName</span><span class="sxs-lookup"><span data-stu-id="f5dc9-184">user.givenname</span></span> |
    | <span data-ttu-id="f5dc9-185">Efternamn</span><span class="sxs-lookup"><span data-stu-id="f5dc9-185">lastname</span></span>| <span data-ttu-id="f5dc9-186">User.surname</span><span class="sxs-lookup"><span data-stu-id="f5dc9-186">user.surname</span></span> |
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/userattribute.png)
    
    <span data-ttu-id="f5dc9-188">a.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-188">a.</span></span> <span data-ttu-id="f5dc9-189">Klicka på **lägga till attributet** tooopen hello attributet dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-189">Click on **Add Attribute** tooopen hello attribute dialog.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_04.png)
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_attribute_05.png)
   
    <span data-ttu-id="f5dc9-192">b.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-192">b.</span></span> <span data-ttu-id="f5dc9-193">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-193">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="f5dc9-194">c.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-194">c.</span></span> <span data-ttu-id="f5dc9-195">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-195">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="f5dc9-196">d.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-196">d.</span></span> <span data-ttu-id="f5dc9-197">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="f5dc9-197">Click **Ok**</span></span>

10. <span data-ttu-id="f5dc9-198">Utför följande steg på hello hello **namn** attribut -</span><span class="sxs-lookup"><span data-stu-id="f5dc9-198">Perform hello following steps on hello **name** attribute-</span></span>

    <span data-ttu-id="f5dc9-199">a.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-199">a.</span></span> <span data-ttu-id="f5dc9-200">Klicka på hello attributet tooopen hello **Redigera attribut** fönster.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-200">Click on hello attribute tooopen hello **Edit Attribute** window.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/url_update.png)

    <span data-ttu-id="f5dc9-202">b.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-202">b.</span></span> <span data-ttu-id="f5dc9-203">Ta bort hello URL-värdet från hello **namnområde**.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-203">Delete hello URL value from hello **namespace**.</span></span>
    
    <span data-ttu-id="f5dc9-204">c.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-204">c.</span></span> <span data-ttu-id="f5dc9-205">Klicka på **Ok** toosave hello inställningen.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-205">Click **Ok** toosave hello setting.</span></span>

11. <span data-ttu-id="f5dc9-206">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-206">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_certificate.png) 

12. <span data-ttu-id="f5dc9-208">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-208">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_400.png)

13. <span data-ttu-id="f5dc9-210">Gå för**LinkedIn admininställningar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-210">Go too**LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="f5dc9-211">Klicka på **överför XML-filen** tooupload hello Metadata XML-fil som du har hämtat från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-211">Click **Upload XML file** tooupload hello Metadata XML file that you have downloaded from hello Azure portal.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

14. <span data-ttu-id="f5dc9-213">Klicka på **på** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-213">Click **On** tooenable SSO.</span></span> <span data-ttu-id="f5dc9-214">SSO status ändras från **inte ansluten** för**ansluten**</span><span class="sxs-lookup"><span data-stu-id="f5dc9-214">SSO status changes from **Not Connected** too**Connected**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)


> [!TIP]
> <span data-ttu-id="f5dc9-216">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="f5dc9-216">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f5dc9-217">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-217">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f5dc9-218">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f5dc9-218">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f5dc9-219">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f5dc9-219">Creating an Azure AD test user</span></span>
<span data-ttu-id="f5dc9-220">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-220">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="f5dc9-222">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f5dc9-222">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f5dc9-223">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-223">In hello **Azure  portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f5dc9-225">Gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-225">Go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f5dc9-227">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-227">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f5dc9-229">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f5dc9-229">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f5dc9-231">a.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-231">a.</span></span> <span data-ttu-id="f5dc9-232">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-232">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f5dc9-233">b.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-233">b.</span></span> <span data-ttu-id="f5dc9-234">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-234">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f5dc9-235">c.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-235">c.</span></span> <span data-ttu-id="f5dc9-236">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-236">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f5dc9-237">d.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-237">d.</span></span> <span data-ttu-id="f5dc9-238">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-238">Click **Create**.</span></span>
 
### <a name="creating-a-linkedin-sales-navigator-test-user"></a><span data-ttu-id="f5dc9-239">Skapa en testanvändare LinkedIn försäljning Navigator</span><span class="sxs-lookup"><span data-stu-id="f5dc9-239">Creating a LinkedIn Sales Navigator test user</span></span>

<span data-ttu-id="f5dc9-240">Länkade försäljning Navigator programmet stöder bara i tid JIT-användaretablering och authentication-användare skapas i hello program automatiskt.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-240">Linked Sales Navigator Application supports Just in Time (JIT) user provisioning and after authentication users are created in hello application automatically.</span></span> <span data-ttu-id="f5dc9-241">Aktivera **automatiskt tilldela licenser** tooassign en licens toohello användare.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-241">Activate **Automatically assign licenses** tooassign a license toohello user.</span></span>
   
   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f5dc9-243">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="f5dc9-243">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f5dc9-244">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLinkedIn försäljning Navigator.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-244">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLinkedIn Sales Navigator.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="f5dc9-246">**tooassign Britta Simon tooLinkedIn försäljning Navigator utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f5dc9-246">**tooassign Britta Simon tooLinkedIn Sales Navigator, perform hello following steps:**</span></span>

1. <span data-ttu-id="f5dc9-247">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-247">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="f5dc9-249">Välj i listan med program hello **LinkedIn försäljning Navigator**.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-249">In hello applications list, select **LinkedIn Sales Navigator**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_app.png) 

3. <span data-ttu-id="f5dc9-251">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-251">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="f5dc9-253">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-253">Click **Add** button.</span></span> <span data-ttu-id="f5dc9-254">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="f5dc9-256">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-256">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f5dc9-257">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f5dc9-258">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f5dc9-259">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f5dc9-259">Testing single sign-on</span></span>

<span data-ttu-id="f5dc9-260">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-260">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f5dc9-261">När du klickar på hello LinkedIn försäljning Navigator panelen i hello åtkomstpanelen ska omdirigerade tooOrganizational sidan där du har tooprovide din personliga LinkedIn-kontoinformation.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-261">When you click hello LinkedIn Sales Navigator tile in hello Access Panel, you should be redirected tooOrganizational page where you have tooprovide your personal LinkedIn account details.</span></span> <span data-ttu-id="f5dc9-262">Den länkar ditt eget konto med ditt företag LinkedIn-konto.</span><span class="sxs-lookup"><span data-stu-id="f5dc9-262">It links your personal account with your LinkedIn business account.</span></span> <span data-ttu-id="f5dc9-263">Läs mer om hello åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="f5dc9-263">For more information about hello Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f5dc9-264">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f5dc9-264">Additional resources</span></span>

* [<span data-ttu-id="f5dc9-265">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f5dc9-265">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f5dc9-266">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f5dc9-266">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



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

