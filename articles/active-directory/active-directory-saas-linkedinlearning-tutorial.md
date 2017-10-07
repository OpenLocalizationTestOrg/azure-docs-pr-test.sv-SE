---
title: "Självstudier: Azure Active Directory-integrering med LinkedIn Learning | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och LinkedIn Learning."
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
ms.openlocfilehash: 14610a25132ed0ccf5892cad6ccc4e1ef03ff280
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a><span data-ttu-id="c2811-103">Självstudier: Azure Active Directory-integrering med LinkedIn Learning</span><span class="sxs-lookup"><span data-stu-id="c2811-103">Tutorial: Azure Active Directory integration with LinkedIn Learning</span></span>

<span data-ttu-id="c2811-104">I kursen får du lära dig hur toointegrate LinkedIn Learning med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c2811-104">In this tutorial, you learn how toointegrate LinkedIn Learning with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c2811-105">Integrera LinkedIn Learning med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c2811-105">Integrating LinkedIn Learning with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c2811-106">Du kan styra i Azure AD som har åtkomst tooLinkedIn utbildning</span><span class="sxs-lookup"><span data-stu-id="c2811-106">You can control in Azure AD who has access tooLinkedIn Learning</span></span>
- <span data-ttu-id="c2811-107">Du kan aktivera din användare tooautomatically get inloggade tooLinkedIn Learning (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c2811-107">You can enable your users tooautomatically get signed-on tooLinkedIn Learning (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c2811-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c2811-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c2811-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c2811-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c2811-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c2811-110">Prerequisites</span></span>

<span data-ttu-id="c2811-111">tooconfigure Azure AD-integrering med LinkedIn Learning måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="c2811-111">tooconfigure Azure AD integration with LinkedIn Learning, you need hello following items:</span></span>

- <span data-ttu-id="c2811-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c2811-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c2811-113">En LinkedIn Learning enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="c2811-113">A LinkedIn Learning single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c2811-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c2811-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c2811-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c2811-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c2811-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c2811-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c2811-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c2811-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c2811-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c2811-118">Scenario description</span></span>
<span data-ttu-id="c2811-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c2811-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c2811-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c2811-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c2811-121">Lägga till LinkedIn Learning från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c2811-121">Adding LinkedIn Learning from hello gallery</span></span>
2. <span data-ttu-id="c2811-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c2811-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-learning-from-hello-gallery"></a><span data-ttu-id="c2811-123">Lägga till LinkedIn Learning från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c2811-123">Adding LinkedIn Learning from hello gallery</span></span>
<span data-ttu-id="c2811-124">tooconfigure hello integrering av LinkedIn Learning i Azure AD, behöver du tooadd LinkedIn Learning hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="c2811-124">tooconfigure hello integration of LinkedIn Learning into Azure AD, you need tooadd LinkedIn Learning from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c2811-125">**tooadd LinkedIn Learning från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c2811-125">**tooadd LinkedIn Learning from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c2811-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c2811-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c2811-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c2811-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c2811-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="c2811-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="c2811-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2811-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="c2811-133">Skriv i sökrutan hello **LinkedIn Learning**.</span><span class="sxs-lookup"><span data-stu-id="c2811-133">In hello search box, type **LinkedIn Learning**.</span></span> <span data-ttu-id="c2811-134">I resultatrutan, klickar du på **LinkedIn Learning** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="c2811-134">From results panel, click **LinkedIn Learning** tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c2811-136">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c2811-136">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c2811-137">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LinkedIn Learning baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c2811-137">In this section, you configure and test Azure AD single sign-on with LinkedIn Learning based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c2811-138">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i LinkedIn Learning är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c2811-138">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LinkedIn Learning is tooa user in Azure AD.</span></span> <span data-ttu-id="c2811-139">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användaren i LinkedIn Learning toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="c2811-139">In other words, a link relationship between an Azure AD user and hello related user in LinkedIn Learning needs toobe established.</span></span>

<span data-ttu-id="c2811-140">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i LinkedIn utbildning.</span><span class="sxs-lookup"><span data-stu-id="c2811-140">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in LinkedIn Learning.</span></span>

<span data-ttu-id="c2811-141">tooconfigure och testa Azure AD enkel inloggning med LinkedIn Learning, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c2811-141">tooconfigure and test Azure AD single sign-on with LinkedIn Learning, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c2811-142">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c2811-142">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c2811-143">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c2811-143">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c2811-144">**[Skapa en testanvändare LinkedIn Learning](#creating-a-linkedin-learning-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c2811-144">**[Creating a LinkedIn Learning test user](#creating-a-linkedin-learning-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="c2811-145">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c2811-145">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c2811-146">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c2811-146">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c2811-147">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c2811-147">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c2811-148">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt LinkedIn Learning-program.</span><span class="sxs-lookup"><span data-stu-id="c2811-148">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LinkedIn Learning application.</span></span>

<span data-ttu-id="c2811-149">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med LinkedIn Learning:**</span><span class="sxs-lookup"><span data-stu-id="c2811-149">**tooconfigure Azure AD single sign-on with LinkedIn Learning, perform hello following steps:**</span></span>

1. <span data-ttu-id="c2811-150">I hello Azure-portalen på hello **LinkedIn Learning** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c2811-150">In hello Azure portal, on hello **LinkedIn Learning** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c2811-152">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c2811-152">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedin_01.png)

3. <span data-ttu-id="c2811-154">I en annan webbläsarfönstret, inloggning tooyour LinkedIn Learning klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="c2811-154">In a different web browser window, sign-on tooyour LinkedIn Learning tenant as an administrator.</span></span>

4. <span data-ttu-id="c2811-155">I **Kontocenter**, klickar du på **globala inställningar** under **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="c2811-155">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="c2811-156">Markera också **utbildning - standard** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="c2811-156">Also, select **Learning - Default** from hello dropdown list.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

5. <span data-ttu-id="c2811-158">Klicka på **eller klicka här tooload och kopiera enskilda fält från hello form** och kopiera **enhets-Id** och **Assertion konsumenten Access (ACS) Url**</span><span class="sxs-lookup"><span data-stu-id="c2811-158">Click **OR Click Here tooload and copy individual fields from hello form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

6. <span data-ttu-id="c2811-160">På Azure-portalen under **LinkedIn Learning domän och URL: er**, utför följande steg om du vill tooconfigure SSO hello i **IdP initierade** läge</span><span class="sxs-lookup"><span data-stu-id="c2811-160">On Azure portal, under **LinkedIn Learning Domain and URLs**, perform hello following steps if you want tooconfigure SSO in **IdP Initiated** mode</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_01.png)

    <span data-ttu-id="c2811-162">a.</span><span class="sxs-lookup"><span data-stu-id="c2811-162">a.</span></span> <span data-ttu-id="c2811-163">I hello **identifierare** textruta ange hello **enhets-ID** kopieras från LinkedIn-portalen</span><span class="sxs-lookup"><span data-stu-id="c2811-163">In hello **Identifier** textbox, enter hello **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="c2811-164">b.</span><span class="sxs-lookup"><span data-stu-id="c2811-164">b.</span></span> <span data-ttu-id="c2811-165">I hello **Reply URL** textruta ange hello **Assertion konsumenten Access (ACS) Url** kopieras från LinkedIn-portalen</span><span class="sxs-lookup"><span data-stu-id="c2811-165">In hello **Reply URL** textbox, enter hello **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="c2811-166">Om du vill tooconfigure SSO i **SP-initierad**, klicka på Visa avancerade URL inställningen alternativ i konfigurationsavsnittet hello och konfigurera hello inloggnings-URL med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="c2811-166">If you want tooconfigure SSO in **SP Initiated**, then click Show Advanced URL setting option in hello configuration section and configure hello sign-on URL with hello following pattern:</span></span>

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_02.png)   
    
8. <span data-ttu-id="c2811-168">Tillämpningsprogrammet LinkedIn Learning förväntar hello SAML intyg i ett specifikt format, vilket kräver att du tooadd attributet mappningar tooyour SAML-token attribut-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c2811-168">Your LinkedIn Learning application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="c2811-169">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="c2811-169">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="c2811-170">Hej standardvärdet **användar-ID** är **user.userprincipalname** men LinkedIn Learning förväntar denna toobe som mappats till hello användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="c2811-170">hello default value of **User Identifier** is **user.userprincipalname** but LinkedIn Learning expects this toobe mapped with hello user's email address.</span></span> <span data-ttu-id="c2811-171">Som du kan använda **user.mail** attribut hello listan eller använda hello rätt attribut-värde baserat på konfigurationen för din organisation.</span><span class="sxs-lookup"><span data-stu-id="c2811-171">For that you can use **user.mail** attribute from hello list or use hello appropriate attribute value based on your organization configuration.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/updateusermail.png)
    
9. <span data-ttu-id="c2811-173">I **användarattribut** klickar du på **visa och redigera andra användarattribut** och ange hello-attribut.</span><span class="sxs-lookup"><span data-stu-id="c2811-173">In **User Attributes** section, click **View and edit all other user attributes** and set hello attributes.</span></span> <span data-ttu-id="c2811-174">hello användare behöver tooadd fyra anspråk med namnet **e-post**, **avdelning**, **Förnamn**, och **efternamn** och hello-värdet är toobe som mappats till **user.mail**, **user.department**, **user.givenname**, och **user.surname** respektive</span><span class="sxs-lookup"><span data-stu-id="c2811-174">hello user needs tooadd four claims named **email**, **department**, **firstname**, and **lastname** and hello value is toobe mapped with **user.mail**, **user.department**, **user.givenname**, and **user.surname** respectively</span></span>

    | <span data-ttu-id="c2811-175">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="c2811-175">Attribute Name</span></span> | <span data-ttu-id="c2811-176">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="c2811-176">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="c2811-177">E-post</span><span class="sxs-lookup"><span data-stu-id="c2811-177">email</span></span>| <span data-ttu-id="c2811-178">User.Mail</span><span class="sxs-lookup"><span data-stu-id="c2811-178">user.mail</span></span> |    
    | <span data-ttu-id="c2811-179">Avdelning</span><span class="sxs-lookup"><span data-stu-id="c2811-179">department</span></span>| <span data-ttu-id="c2811-180">User.Department</span><span class="sxs-lookup"><span data-stu-id="c2811-180">user.department</span></span> |
    | <span data-ttu-id="c2811-181">Förnamn</span><span class="sxs-lookup"><span data-stu-id="c2811-181">firstname</span></span>| <span data-ttu-id="c2811-182">User.givenName</span><span class="sxs-lookup"><span data-stu-id="c2811-182">user.givenname</span></span> |
    | <span data-ttu-id="c2811-183">Efternamn</span><span class="sxs-lookup"><span data-stu-id="c2811-183">lastname</span></span>| <span data-ttu-id="c2811-184">User.surname</span><span class="sxs-lookup"><span data-stu-id="c2811-184">user.surname</span></span> |
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/userattribute.png)
    
    <span data-ttu-id="c2811-186">a.</span><span class="sxs-lookup"><span data-stu-id="c2811-186">a.</span></span> <span data-ttu-id="c2811-187">Klicka på **lägga till attributet** tooopen hello attributet dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2811-187">Click **Add Attribute** tooopen hello attribute dialog.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_04.png)

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="c2811-190">b.</span><span class="sxs-lookup"><span data-stu-id="c2811-190">b.</span></span> <span data-ttu-id="c2811-191">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="c2811-191">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="c2811-192">c.</span><span class="sxs-lookup"><span data-stu-id="c2811-192">c.</span></span> <span data-ttu-id="c2811-193">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="c2811-193">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="c2811-194">d.</span><span class="sxs-lookup"><span data-stu-id="c2811-194">d.</span></span> <span data-ttu-id="c2811-195">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="c2811-195">Click **Ok**</span></span>

10. <span data-ttu-id="c2811-196">Utför följande steg på hello hello **namn** attribut -</span><span class="sxs-lookup"><span data-stu-id="c2811-196">Perform hello following steps on hello **name** attribute-</span></span>

    <span data-ttu-id="c2811-197">a.</span><span class="sxs-lookup"><span data-stu-id="c2811-197">a.</span></span> <span data-ttu-id="c2811-198">Klicka på hello attributet tooopen hello **Redigera attribut** fönster.</span><span class="sxs-lookup"><span data-stu-id="c2811-198">Click on hello attribute tooopen hello **Edit Attribute** window.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinLearning-tutorial/url_update.png)

    <span data-ttu-id="c2811-200">b.</span><span class="sxs-lookup"><span data-stu-id="c2811-200">b.</span></span> <span data-ttu-id="c2811-201">Ta bort hello URL-värdet från hello **namnområde**.</span><span class="sxs-lookup"><span data-stu-id="c2811-201">Delete hello URL value from hello **namespace**.</span></span>
    
    <span data-ttu-id="c2811-202">c.</span><span class="sxs-lookup"><span data-stu-id="c2811-202">c.</span></span> <span data-ttu-id="c2811-203">Klicka på **Ok** toosave hello inställningen.</span><span class="sxs-lookup"><span data-stu-id="c2811-203">Click **Ok** toosave hello setting.</span></span>

11. <span data-ttu-id="c2811-204">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="c2811-204">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_certificate.png) 

12. <span data-ttu-id="c2811-206">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="c2811-206">Click **Save**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_400.png)

13. <span data-ttu-id="c2811-208">Gå för**LinkedIn admininställningar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c2811-208">Go too**LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="c2811-209">Överför hello XML-fil som du hämtade från hello Azure-portalen genom att klicka på alternativet för hello överför XML-filen.</span><span class="sxs-lookup"><span data-stu-id="c2811-209">Upload hello XML file you downloaded from hello Azure portal by clicking hello Upload XML file option.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

14. <span data-ttu-id="c2811-211">Klicka på **på** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c2811-211">Click **On** tooenable SSO.</span></span> <span data-ttu-id="c2811-212">SSO status ändras från **inte ansluten** för**ansluten**</span><span class="sxs-lookup"><span data-stu-id="c2811-212">SSO status changes from **Not Connected** too**Connected**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c2811-214">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c2811-214">Creating an Azure AD test user</span></span>
<span data-ttu-id="c2811-215">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c2811-215">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="c2811-217">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c2811-217">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c2811-218">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c2811-218">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c2811-220">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c2811-220">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c2811-222">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2811-222">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c2811-224">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c2811-224">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c2811-226">a.</span><span class="sxs-lookup"><span data-stu-id="c2811-226">a.</span></span> <span data-ttu-id="c2811-227">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c2811-227">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c2811-228">b.</span><span class="sxs-lookup"><span data-stu-id="c2811-228">b.</span></span> <span data-ttu-id="c2811-229">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c2811-229">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c2811-230">c.</span><span class="sxs-lookup"><span data-stu-id="c2811-230">c.</span></span> <span data-ttu-id="c2811-231">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c2811-231">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c2811-232">d.</span><span class="sxs-lookup"><span data-stu-id="c2811-232">d.</span></span> <span data-ttu-id="c2811-233">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c2811-233">Click **Create**.</span></span> 

### <a name="creating-a-linkedin-learning-test-user"></a><span data-ttu-id="c2811-234">Skapa en testanvändare LinkedIn-utbildning</span><span class="sxs-lookup"><span data-stu-id="c2811-234">Creating a LinkedIn Learning test user</span></span>

<span data-ttu-id="c2811-235">Länkade Learning programmet stöder.</span><span class="sxs-lookup"><span data-stu-id="c2811-235">Linked Learning Application supports.</span></span> <span data-ttu-id="c2811-236">Precis i tid användaretablering och efter autentisering skapas användare i hello program automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c2811-236">Just in time user provisioning and after authentication users are created in hello application automatically.</span></span> <span data-ttu-id="c2811-237">Hej administratör på sidan Inställningar på hello LinkedIn Learning portal Vänd hello växeln **automatiskt tilldela licenser** tooactive tooenable precis tid etablering och detta kommer också tilldela en licens toohello användare.</span><span class="sxs-lookup"><span data-stu-id="c2811-237">On hello admin settings page on hello LinkedIn Learning portal flip hello switch **Automatically Assign licenses** tooactive tooenable Just in time provisioning and this will also assign a license toohello user.</span></span>
   
   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinLearning-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c2811-239">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="c2811-239">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c2811-240">I det här avsnittet kan du aktivera Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLinkedIn Learning.</span><span class="sxs-lookup"><span data-stu-id="c2811-240">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLinkedIn Learning.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="c2811-242">**tooassign Britta Simon tooLinkedIn Learning, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c2811-242">**tooassign Britta Simon tooLinkedIn Learning, perform hello following steps:**</span></span>

1. <span data-ttu-id="c2811-243">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c2811-243">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c2811-245">Välj i listan med program hello **LinkedIn Learning**.</span><span class="sxs-lookup"><span data-stu-id="c2811-245">In hello applications list, select **LinkedIn Learning**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_0001.png) 

3. <span data-ttu-id="c2811-247">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c2811-247">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="c2811-249">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c2811-249">Click **Add** button.</span></span> <span data-ttu-id="c2811-250">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2811-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="c2811-252">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="c2811-252">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c2811-253">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2811-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c2811-254">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2811-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c2811-255">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c2811-255">Testing single sign-on</span></span>

<span data-ttu-id="c2811-256">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c2811-256">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c2811-257">När du klickar på hello LinkedIn Learning panelen i hello åtkomstpanelen du bör få hello Azure inloggning sidan och på efter lyckad inloggning kan du bör få till LinkedIn Learning programmet.</span><span class="sxs-lookup"><span data-stu-id="c2811-257">When you click hello LinkedIn Learning tile in hello Access Panel, you should get hello Azure Sign-on page and on after successful sign-on, you should get into your LinkedIn Learning application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c2811-258">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c2811-258">Additional resources</span></span>

* [<span data-ttu-id="c2811-259">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c2811-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c2811-260">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c2811-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

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