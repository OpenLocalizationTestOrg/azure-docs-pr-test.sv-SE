---
title: "Självstudier: Azure Active Directory-integrering med LinkedIn sökning | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och LinkedIn-sökning."
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
ms.openlocfilehash: d79c34baa676391699e4b49806f16422fcfe73e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-lookup"></a><span data-ttu-id="98b45-103">Självstudier: Azure Active Directory-integrering med LinkedIn-sökning</span><span class="sxs-lookup"><span data-stu-id="98b45-103">Tutorial: Azure Active Directory integration with LinkedIn Lookup</span></span>

<span data-ttu-id="98b45-104">I kursen får du lära dig hur toointegrate LinkedIn sökning med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="98b45-104">In this tutorial, you learn how toointegrate LinkedIn Lookup with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="98b45-105">Integrera LinkedIn sökning med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="98b45-105">Integrating LinkedIn Lookup with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="98b45-106">Du kan styra i Azure AD som har åtkomst tooLinkedIn sökning</span><span class="sxs-lookup"><span data-stu-id="98b45-106">You can control in Azure AD who has access tooLinkedIn Lookup</span></span>
- <span data-ttu-id="98b45-107">Du kan aktivera din användare tooautomatically get inloggade tooLinkedIn sökning (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="98b45-107">You can enable your users tooautomatically get signed-on tooLinkedIn Lookup (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="98b45-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="98b45-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="98b45-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="98b45-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="98b45-110">Krav</span><span class="sxs-lookup"><span data-stu-id="98b45-110">Prerequisites</span></span>

<span data-ttu-id="98b45-111">tooconfigure Azure AD-integrering med LinkedIn-sökning, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="98b45-111">tooconfigure Azure AD integration with LinkedIn Lookup, you need hello following items:</span></span>

- <span data-ttu-id="98b45-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="98b45-112">An Azure AD subscription</span></span>
- <span data-ttu-id="98b45-113">En sökning LinkedIn enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="98b45-113">An LinkedIn Lookup single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="98b45-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="98b45-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="98b45-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="98b45-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="98b45-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="98b45-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="98b45-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="98b45-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="98b45-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="98b45-118">Scenario description</span></span>
<span data-ttu-id="98b45-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="98b45-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="98b45-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="98b45-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="98b45-121">Lägga till LinkedIn sökning från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="98b45-121">Adding LinkedIn Lookup from hello gallery</span></span>
2. <span data-ttu-id="98b45-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="98b45-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-lookup-from-hello-gallery"></a><span data-ttu-id="98b45-123">Lägga till LinkedIn sökning från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="98b45-123">Adding LinkedIn Lookup from hello gallery</span></span>
<span data-ttu-id="98b45-124">tooconfigure hello integrering av LinkedIn sökning i Azure AD, behöver du tooadd LinkedIn sökning hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="98b45-124">tooconfigure hello integration of LinkedIn Lookup into Azure AD, you need tooadd LinkedIn Lookup from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="98b45-125">**tooadd LinkedIn sökning från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="98b45-125">**tooadd LinkedIn Lookup from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="98b45-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="98b45-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="98b45-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="98b45-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="98b45-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="98b45-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="98b45-131">Klicka på **nytt program** hello längst upp i hello dialogrutan tooadd nytt program.</span><span class="sxs-lookup"><span data-stu-id="98b45-131">Click **New application** button on hello top of hello dialog tooadd new application.</span></span>

    ![Program][3]

4. <span data-ttu-id="98b45-133">Skriv i sökrutan hello **LinkedIn sökning**.</span><span class="sxs-lookup"><span data-stu-id="98b45-133">In hello search box, type **LinkedIn Lookup**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_search.png)

5. <span data-ttu-id="98b45-135">Markera hello resultat på panelen **LinkedIn sökning**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="98b45-135">In hello results panel, select **LinkedIn Lookup**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="98b45-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="98b45-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="98b45-138">Du konfigurera och testa Azure AD enkel inloggning med LinkedIn sökning baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="98b45-138">In this section, you configure and test Azure AD single sign-on with LinkedIn Lookup based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="98b45-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i LinkedIn sökning är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="98b45-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LinkedIn Lookup is tooa user in Azure AD.</span></span> <span data-ttu-id="98b45-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i LinkedIn sökning toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="98b45-140">In other words, a link relationship between an Azure AD user and hello related user in LinkedIn Lookup needs toobe established.</span></span>

<span data-ttu-id="98b45-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i LinkedIn-sökning.</span><span class="sxs-lookup"><span data-stu-id="98b45-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in LinkedIn Lookup.</span></span>

<span data-ttu-id="98b45-142">tooconfigure och testa Azure AD enkel inloggning med LinkedIn-sökning, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="98b45-142">tooconfigure and test Azure AD single sign-on with LinkedIn Lookup, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="98b45-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="98b45-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="98b45-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="98b45-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="98b45-145">**[Skapa en testanvändare LinkedIn sökning](#creating-an-linkedin-lookup-test-user)**  -toohave en motsvarighet för Britta Simon i LinkedIn uppslag som är länkade toohello Azure AD-representation.</span><span class="sxs-lookup"><span data-stu-id="98b45-145">**[Creating an LinkedIn Lookup test user](#creating-an-linkedin-lookup-test-user)** - toohave a counterpart of Britta Simon in LinkedIn Lookup that is linked toohello Azure AD representation.</span></span>
4. <span data-ttu-id="98b45-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="98b45-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="98b45-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="98b45-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="98b45-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="98b45-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="98b45-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet LinkedIn-sökning.</span><span class="sxs-lookup"><span data-stu-id="98b45-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LinkedIn Lookup application.</span></span>

<span data-ttu-id="98b45-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med LinkedIn sökning:**</span><span class="sxs-lookup"><span data-stu-id="98b45-150">**tooconfigure Azure AD single sign-on with LinkedIn Lookup, perform hello following steps:**</span></span>

1. <span data-ttu-id="98b45-151">I hello Azure-portalen på hello **LinkedIn sökning** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="98b45-151">In hello Azure portal, on hello **LinkedIn Lookup** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="98b45-153">På hello **enkel inloggning** dialogrutan i **läge** Välj **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="98b45-153">On hello **Single sign-on** dialog, in **Mode** select **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_samlbase.png)

3. <span data-ttu-id="98b45-155">I en annan webbläsarfönster inloggning tooyour **LinkedIn sökning** webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="98b45-155">In a different web browser window, sign-on tooyour **LinkedIn Lookup** website as an administrator.</span></span>

4. <span data-ttu-id="98b45-156">I **Kontocenter**, klickar du på **globala inställningar** under **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="98b45-156">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="98b45-157">Markera också **sökning** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="98b45-157">Also, select **Lookup** from hello dropdown list.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_011.png)

5. <span data-ttu-id="98b45-159">Klicka på **eller klicka här tooload och kopiera enskilda fält från hello form** och kopiera **enhets-Id** och **Assertion konsumenten Access (ACS) Url**</span><span class="sxs-lookup"><span data-stu-id="98b45-159">Click **OR Click Here tooload and copy individual fields from hello form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_032.png)

6. <span data-ttu-id="98b45-161">På Azure-portalen under **LinkedIn sökning domän och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="98b45-161">On Azure portal, under **LinkedIn Lookup Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url.png)

    <span data-ttu-id="98b45-163">a.</span><span class="sxs-lookup"><span data-stu-id="98b45-163">a.</span></span> <span data-ttu-id="98b45-164">I hello **identifierare** textruta ange hello **enhets-ID** kopieras från LinkedIn-portalen</span><span class="sxs-lookup"><span data-stu-id="98b45-164">In hello **Identifier** textbox, enter hello **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="98b45-165">b.</span><span class="sxs-lookup"><span data-stu-id="98b45-165">b.</span></span> <span data-ttu-id="98b45-166">I hello **Reply URL** textruta ange hello **Assertion konsumenten Access (ACS) Url** kopieras från LinkedIn-portalen</span><span class="sxs-lookup"><span data-stu-id="98b45-166">In hello **Reply URL** textbox, enter hello **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="98b45-167">Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="98b45-167">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url1.png)

    <span data-ttu-id="98b45-169">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.linkedIn.com/checkpoint/enterprise/login/<AccountId>?application=lookup`</span><span class="sxs-lookup"><span data-stu-id="98b45-169">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://www.linkedIn.com/checkpoint/enterprise/login/<AccountId>?application=lookup`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="98b45-170">Detta är inte verkliga värdet.</span><span class="sxs-lookup"><span data-stu-id="98b45-170">This is not real value.</span></span> <span data-ttu-id="98b45-171">hello användaren har tooupdate dessa värden med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="98b45-171">hello user has tooupdate these values with hello actual Sign-On URL.</span></span> <span data-ttu-id="98b45-172">Kontakta [LinkedIn sökning klienten supportteamet](https://business.LinkedIn.com/lookup) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="98b45-172">Contact [LinkedIn Lookup Client support team](https://business.LinkedIn.com/lookup) tooget this value.</span></span>

8. <span data-ttu-id="98b45-173">Din **LinkedIn sökning** program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="98b45-173">Your **LinkedIn Lookup** application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="98b45-174">hello användare har tooadd attributet mappningar toohello SAML-token attribut konfiguration.</span><span class="sxs-lookup"><span data-stu-id="98b45-174">hello user has tooadd custom attribute mappings toohello SAML token attributes configuration.</span></span> <span data-ttu-id="98b45-175">hello följande skärmbild visar ett exempel.</span><span class="sxs-lookup"><span data-stu-id="98b45-175">hello following screenshot shows an example.</span></span> <span data-ttu-id="98b45-176">Hej standardvärdet **användar-ID** är **user.userprincipalname** men LinkedIn sökning förväntar denna toobe som mappats till hello användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="98b45-176">hello default value of **User Identifier** is **user.userprincipalname** but LinkedIn Lookup expects this toobe mapped with hello user's email address.</span></span> <span data-ttu-id="98b45-177">Du kan använda **user.mail** attribut hello listan eller använda hello rätt attribut-värde baserat på konfigurationen för din organisation.</span><span class="sxs-lookup"><span data-stu-id="98b45-177">You can use **user.mail** attribute from hello list or use hello appropriate attribute value based on your organization configuration.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/updateusermail.png)
    
9. <span data-ttu-id="98b45-179">I **användarattribut** klickar du på **visa och redigera andra användarattribut** och ange hello-attribut.</span><span class="sxs-lookup"><span data-stu-id="98b45-179">In **User Attributes** section, click **View and edit all other user attributes** and set hello attributes.</span></span> <span data-ttu-id="98b45-180">hello användare behöver tooadd fyra anspråk med namnet **e-post**, **avdelning**, **Förnamn**, och **efternamn** och hello-värdet är toobe som mappats till **user.mail**, **user.department**, **user.givenname**, och **user.surname** respektive</span><span class="sxs-lookup"><span data-stu-id="98b45-180">hello user needs tooadd four claims named **email**,  **department**, **firstname**, and **lastname** and hello value is toobe mapped with **user.mail**, **user.department**, **user.givenname**, and **user.surname** respectively</span></span>

    | <span data-ttu-id="98b45-181">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="98b45-181">Attribute Name</span></span> | <span data-ttu-id="98b45-182">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="98b45-182">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="98b45-183">E-post</span><span class="sxs-lookup"><span data-stu-id="98b45-183">email</span></span>| <span data-ttu-id="98b45-184">User.Mail</span><span class="sxs-lookup"><span data-stu-id="98b45-184">user.mail</span></span> |    
    | <span data-ttu-id="98b45-185">Avdelning</span><span class="sxs-lookup"><span data-stu-id="98b45-185">department</span></span>| <span data-ttu-id="98b45-186">User.Department</span><span class="sxs-lookup"><span data-stu-id="98b45-186">user.department</span></span> |
    | <span data-ttu-id="98b45-187">Förnamn</span><span class="sxs-lookup"><span data-stu-id="98b45-187">firstname</span></span>| <span data-ttu-id="98b45-188">User.givenName</span><span class="sxs-lookup"><span data-stu-id="98b45-188">user.givenname</span></span> |
    | <span data-ttu-id="98b45-189">Efternamn</span><span class="sxs-lookup"><span data-stu-id="98b45-189">lastname</span></span>| <span data-ttu-id="98b45-190">User.surname</span><span class="sxs-lookup"><span data-stu-id="98b45-190">user.surname</span></span> |

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/userattribute.png)

    <span data-ttu-id="98b45-192">a.</span><span class="sxs-lookup"><span data-stu-id="98b45-192">a.</span></span> <span data-ttu-id="98b45-193">Klicka på **lägga till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="98b45-193">Click **Add Attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/4.png)
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/5.png)
   
    <span data-ttu-id="98b45-196">b.</span><span class="sxs-lookup"><span data-stu-id="98b45-196">b.</span></span> <span data-ttu-id="98b45-197">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="98b45-197">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="98b45-198">c.</span><span class="sxs-lookup"><span data-stu-id="98b45-198">c.</span></span> <span data-ttu-id="98b45-199">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="98b45-199">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="98b45-200">d.</span><span class="sxs-lookup"><span data-stu-id="98b45-200">d.</span></span> <span data-ttu-id="98b45-201">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="98b45-201">Click **Ok**</span></span>

10. <span data-ttu-id="98b45-202">Utför följande steg på hello hello **namn** attribut -</span><span class="sxs-lookup"><span data-stu-id="98b45-202">Perform hello following steps on hello **name** attribute-</span></span>

    <span data-ttu-id="98b45-203">a.</span><span class="sxs-lookup"><span data-stu-id="98b45-203">a.</span></span> <span data-ttu-id="98b45-204">Klicka på hello attributet tooopen hello **Redigera attribut** fönster.</span><span class="sxs-lookup"><span data-stu-id="98b45-204">Click on hello attribute tooopen hello **Edit Attribute** window.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/url_update.png)

    <span data-ttu-id="98b45-206">b.</span><span class="sxs-lookup"><span data-stu-id="98b45-206">b.</span></span> <span data-ttu-id="98b45-207">Ta bort hello URL-värdet från hello **namnområde**.</span><span class="sxs-lookup"><span data-stu-id="98b45-207">Delete hello URL value from hello **namespace**.</span></span>
    
    <span data-ttu-id="98b45-208">c.</span><span class="sxs-lookup"><span data-stu-id="98b45-208">c.</span></span> <span data-ttu-id="98b45-209">Klicka på **Ok** toosave hello inställningen.</span><span class="sxs-lookup"><span data-stu-id="98b45-209">Click **Ok** toosave hello setting.</span></span>

10. <span data-ttu-id="98b45-210">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="98b45-210">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_certificate.png) 

11. <span data-ttu-id="98b45-212">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="98b45-212">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_400.png)

12. <span data-ttu-id="98b45-214">Gå för**LinkedIn admininställningar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="98b45-214">Go too**LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="98b45-215">Överför hello XML-fil som du hämtade från hello Azure-portalen genom att klicka på hello **överför XML-filen** alternativet.</span><span class="sxs-lookup"><span data-stu-id="98b45-215">Upload hello XML file you downloaded from hello Azure portal by clicking hello **Upload XML file** option.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_metadata_03.png)

13. <span data-ttu-id="98b45-217">Klicka på **på** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="98b45-217">Click **On** tooenable SSO.</span></span> <span data-ttu-id="98b45-218">SSO status ändras från **inte ansluten** för**ansluten**</span><span class="sxs-lookup"><span data-stu-id="98b45-218">SSO status changes from **Not Connected** too**Connected**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_admin_05.png)

> [!TIP]
> <span data-ttu-id="98b45-220">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="98b45-220">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="98b45-221">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="98b45-221">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="98b45-222">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="98b45-222">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="98b45-223">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="98b45-223">Creating an Azure AD test user</span></span>
<span data-ttu-id="98b45-224">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="98b45-224">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="98b45-226">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="98b45-226">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="98b45-227">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="98b45-227">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="98b45-229">Gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="98b45-229">Go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="98b45-231">Klicka på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="98b45-231">Click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="98b45-233">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="98b45-233">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="98b45-235">a.</span><span class="sxs-lookup"><span data-stu-id="98b45-235">a.</span></span> <span data-ttu-id="98b45-236">I hello **namn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="98b45-236">In hello **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="98b45-237">b.</span><span class="sxs-lookup"><span data-stu-id="98b45-237">b.</span></span> <span data-ttu-id="98b45-238">I hello **användarnamn** textruta typen hello **e-postadress** av Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="98b45-238">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="98b45-239">c.</span><span class="sxs-lookup"><span data-stu-id="98b45-239">c.</span></span> <span data-ttu-id="98b45-240">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="98b45-240">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="98b45-241">d.</span><span class="sxs-lookup"><span data-stu-id="98b45-241">d.</span></span> <span data-ttu-id="98b45-242">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="98b45-242">Click **Create**.</span></span>
 
### <a name="creating-an-linkedin-lookup-test-user"></a><span data-ttu-id="98b45-243">Skapa en testanvändare LinkedIn-sökning</span><span class="sxs-lookup"><span data-stu-id="98b45-243">Creating an LinkedIn Lookup test user</span></span>

<span data-ttu-id="98b45-244">Länkade sökning programmet stöder bara i tid JIT-användaretablering och authentication-användare skapas i hello program automatiskt.</span><span class="sxs-lookup"><span data-stu-id="98b45-244">Linked Lookup Application supports Just in Time (JIT) user provisioning and after authentication users are created in hello application automatically.</span></span> <span data-ttu-id="98b45-245">Aktivera **automatiskt tilldela licenser** tooassign en licens toohello användare.</span><span class="sxs-lookup"><span data-stu-id="98b45-245">Activate **Automatically assign licenses** tooassign a license toohello user.</span></span>
   
   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedin_admin_license.png)

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="98b45-247">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="98b45-247">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="98b45-248">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLinkedIn sökning.</span><span class="sxs-lookup"><span data-stu-id="98b45-248">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLinkedIn Lookup.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="98b45-250">**tooassign Britta Simon tooLinkedIn sökning, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="98b45-250">**tooassign Britta Simon tooLinkedIn Lookup, perform hello following steps:**</span></span>

1. <span data-ttu-id="98b45-251">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="98b45-251">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="98b45-253">Välj i listan med program hello **LinkedIn sökning**.</span><span class="sxs-lookup"><span data-stu-id="98b45-253">In hello applications list, select **LinkedIn Lookup**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_app.png) 

3. <span data-ttu-id="98b45-255">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="98b45-255">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="98b45-257">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="98b45-257">Click **Add** button.</span></span> <span data-ttu-id="98b45-258">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="98b45-258">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="98b45-260">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="98b45-260">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="98b45-261">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="98b45-261">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="98b45-262">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="98b45-262">Click **Assign** button on **Add Assignment** dialog.</span></span>

    
### <a name="testing-single-sign-on"></a><span data-ttu-id="98b45-263">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="98b45-263">Testing single sign-on</span></span>

<span data-ttu-id="98b45-264">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="98b45-264">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="98b45-265">När du klickar på hello LinkedIn sökning panelen i hello åtkomstpanelen ska omdirigerade tooOrganizational sidan där du har tooprovide din personliga LinkedIn-kontoinformation.</span><span class="sxs-lookup"><span data-stu-id="98b45-265">When you click hello LinkedIn Lookup tile in hello Access Panel, you should be redirected tooOrganizational page where you have tooprovide your personal LinkedIn account details.</span></span> <span data-ttu-id="98b45-266">Den länkar ditt eget konto med ditt företag LinkedIn-konto.</span><span class="sxs-lookup"><span data-stu-id="98b45-266">It links your personal account with your LinkedIn business account.</span></span> 

<span data-ttu-id="98b45-267">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="98b45-267">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="98b45-268">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="98b45-268">Additional resources</span></span>

* [<span data-ttu-id="98b45-269">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="98b45-269">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="98b45-270">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="98b45-270">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

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

