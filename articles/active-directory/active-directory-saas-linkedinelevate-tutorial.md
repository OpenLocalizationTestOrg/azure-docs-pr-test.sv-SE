---
title: "Självstudier: Azure Active Directory-integrering med LinkedIn höjer | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och LinkedIn höjer."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2ad9941b-c574-42c3-bd0f-5d6ec68537ef
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeedes
ms.openlocfilehash: 189bd72c230be7dc0c0b934f94ea01e84af9ad23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-elevate"></a><span data-ttu-id="e4a9c-103">Självstudier: Azure Active Directory-integrering med LinkedIn höjer</span><span class="sxs-lookup"><span data-stu-id="e4a9c-103">Tutorial: Azure Active Directory integration with LinkedIn Elevate</span></span>

<span data-ttu-id="e4a9c-104">I kursen får du lära dig hur toointegrate LinkedIn höjer med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e4a9c-104">In this tutorial, you learn how toointegrate LinkedIn Elevate with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e4a9c-105">Integrera LinkedIn höjer med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e4a9c-105">Integrating LinkedIn Elevate with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e4a9c-106">Du kan styra i Azure AD som har åtkomst tooLinkedIn utöka</span><span class="sxs-lookup"><span data-stu-id="e4a9c-106">You can control in Azure AD who has access tooLinkedIn Elevate</span></span>
- <span data-ttu-id="e4a9c-107">Du kan aktivera din användare tooautomatically get inloggade tooLinkedIn utöka (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e4a9c-107">You can enable your users tooautomatically get signed-on tooLinkedIn Elevate (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e4a9c-108">Du kan hantera dina konton i en central plats - hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="e4a9c-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="e4a9c-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e4a9c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4a9c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e4a9c-110">Prerequisites</span></span>

<span data-ttu-id="e4a9c-111">tooconfigure Azure AD-integrering med LinkedIn höjer du behöver hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="e4a9c-111">tooconfigure Azure AD integration with LinkedIn Elevate, you need hello following items:</span></span>

- <span data-ttu-id="e4a9c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e4a9c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e4a9c-113">En LinkedIn höjer enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="e4a9c-113">A LinkedIn Elevate single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e4a9c-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e4a9c-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e4a9c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e4a9c-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="e4a9c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e4a9c-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e4a9c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e4a9c-118">Scenario description</span></span>
<span data-ttu-id="e4a9c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e4a9c-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e4a9c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e4a9c-121">Lägga till LinkedIn höjer från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e4a9c-121">Adding LinkedIn Elevate from hello gallery</span></span>
2. <span data-ttu-id="e4a9c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e4a9c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-linkedin-elevate-from-hello-gallery"></a><span data-ttu-id="e4a9c-123">Lägga till LinkedIn höjer från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e4a9c-123">Adding LinkedIn Elevate from hello gallery</span></span>
<span data-ttu-id="e4a9c-124">tooconfigure hello integrering av LinkedIn höjer i Azure AD, behöver du tooadd LinkedIn höjer hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-124">tooconfigure hello integration of LinkedIn Elevate into Azure AD, you need tooadd LinkedIn Elevate from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e4a9c-125">**tooadd LinkedIn höjer från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e4a9c-125">**tooadd LinkedIn Elevate from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e4a9c-126">I hello ** [Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e4a9c-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e4a9c-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="e4a9c-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="e4a9c-133">Skriv i sökrutan hello **LinkedIn höjer**.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-133">In hello search box, type **LinkedIn Elevate**.</span></span> <span data-ttu-id="e4a9c-134">I resultatrutan, klickar du på **LinkedIn höjer** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-134">From results panel, click **LinkedIn Elevate** tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e4a9c-136">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e4a9c-136">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e4a9c-137">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LinkedIn höjer baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-137">In this section, you configure and test Azure AD single sign-on with LinkedIn Elevate based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e4a9c-138">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i LinkedIn höjer är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-138">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LinkedIn Elevate is tooa user in Azure AD.</span></span> <span data-ttu-id="e4a9c-139">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i LinkedIn höjer toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-139">In other words, a link relationship between an Azure AD user and hello related user in LinkedIn Elevate needs toobe established.</span></span>

<span data-ttu-id="e4a9c-140">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i LinkedIn höjer.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-140">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in LinkedIn Elevate.</span></span>

<span data-ttu-id="e4a9c-141">tooconfigure och testa Azure AD enkel inloggning med LinkedIn höjer du behöver toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e4a9c-141">tooconfigure and test Azure AD single sign-on with LinkedIn Elevate, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e4a9c-142">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on) ** -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-142">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e4a9c-143">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user) ** -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-143">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e4a9c-144">**[Skapa en testanvändare LinkedIn höjer](#creating-a-linkedin-elevate-test-user) ** -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-144">**[Creating a LinkedIn Elevate test user](#creating-a-linkedin-elevate-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="e4a9c-145">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user) ** -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-145">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e4a9c-146">**[Testa enkel inloggning](#testing-single-sign-on) ** -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-146">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e4a9c-147">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e4a9c-147">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e4a9c-148">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt LinkedIn höjer program.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-148">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your LinkedIn Elevate application.</span></span>

<span data-ttu-id="e4a9c-149">**Utför följande hello tooconfigure Azure AD enkel inloggning med LinkedIn höjer:**</span><span class="sxs-lookup"><span data-stu-id="e4a9c-149">**tooconfigure Azure AD single sign-on with LinkedIn Elevate, perform hello following steps:**</span></span>

1. <span data-ttu-id="e4a9c-150">I hello Azure Management portal på hello **LinkedIn höjer** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-150">In hello Azure Management portal, on hello **LinkedIn Elevate** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="e4a9c-152">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-152">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedin_01.png)

3. <span data-ttu-id="e4a9c-154">I en annan webbläsarfönstret, inloggning tooyour LinkedIn höjer klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-154">In a different web browser window, sign-on tooyour LinkedIn Elevate tenant as an administrator.</span></span>

4. <span data-ttu-id="e4a9c-155">I **Kontocenter**, klickar du på **globala inställningar** under **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-155">In **Account Center**, click **Global Settings** under **Settings**.</span></span> <span data-ttu-id="e4a9c-156">Markera också **utöka - höjer AAD Test** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-156">Also, select **Elevate - Elevate AAD Test** from hello dropdown list.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_01.png)

5. <span data-ttu-id="e4a9c-158">Klicka på **eller klicka här tooload och kopiera enskilda fält från hello form** och kopiera **enhets-Id** och **Assertion konsumenten Access (ACS) Url**</span><span class="sxs-lookup"><span data-stu-id="e4a9c-158">Click on **OR Click Here tooload and copy individual fields from hello form** and copy **Entity Id** and **Assertion Consumer Access (ACS) Url**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_03.png)

6. <span data-ttu-id="e4a9c-160">På Azure Portal under **LinkedIn höja domän och URL: er**, utför följande steg om du vill tooconfigure SSO hello i **IdP initierade** läge</span><span class="sxs-lookup"><span data-stu-id="e4a9c-160">On Azure Portal, under **LinkedIn Elevate Domain and URLs**, perform hello following steps if you want tooconfigure SSO in **IdP Initiated** mode</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_01.png)

    <span data-ttu-id="e4a9c-162">a.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-162">a.</span></span> <span data-ttu-id="e4a9c-163">I hello **identifierare** textruta ange hello **enhets-ID** kopieras från LinkedIn-portalen</span><span class="sxs-lookup"><span data-stu-id="e4a9c-163">In hello **Identifier** textbox, enter hello **Entity ID** copied from LinkedIn Portal</span></span> 

    <span data-ttu-id="e4a9c-164">b.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-164">b.</span></span> <span data-ttu-id="e4a9c-165">I hello **Reply URL** textruta ange hello **Assertion konsumenten Access (ACS) Url** kopieras från LinkedIn-portalen</span><span class="sxs-lookup"><span data-stu-id="e4a9c-165">In hello **Reply URL** textbox, enter hello **Assertion Consumer Access (ACS) Url** copied from LinkedIn Portal</span></span>

7. <span data-ttu-id="e4a9c-166">Om du vill tooconfigure SSO i **SP-initierad**, klickar du på Visa avancerade URL inställningen alternativ i konfigurationsavsnittet hello och konfigurera hello logga på URL: en med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="e4a9c-166">If you want tooconfigure SSO in **SP Initiated**, then click Show Advanced URL setting option in hello configuration section and configure hello sign on URL with hello following pattern:</span></span>

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=elevate&applicationInstanceId=<InstanceId>` 
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_signon_02.png) 
    
8. <span data-ttu-id="e4a9c-168">Tillämpningsprogrammet LinkedIn höjer förväntar hello SAML intyg i ett specifikt format, vilket kräver att du tooadd attributet mappningar tooyour SAML-token attribut-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-168">Your LinkedIn Elevate application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="e4a9c-169">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-169">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="e4a9c-170">Hej standardvärdet **användar-ID** är **user.userprincipalname** men LinkedIn höjer förväntar denna toobe som mappats till hello användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-170">hello default value of **User Identifier** is **user.userprincipalname** but LinkedIn Elevate expects this toobe mapped with hello user's email address.</span></span> <span data-ttu-id="e4a9c-171">Som du kan använda **user.mail** attribut hello listan eller använda hello rätt attribut-värde baserat på konfigurationen för din organisation.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-171">For that you can use **user.mail** attribute from hello list or use hello appropriate attribute value based on your organization configuration.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/updateusermail.png)

9. <span data-ttu-id="e4a9c-173">I **användarattribut** klickar du på **visa och redigera andra användarattribut** och ange hello-attribut.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-173">In **User Attributes** section, click **View and edit all other user attributes** and set hello attributes.</span></span> <span data-ttu-id="e4a9c-174">Du behöver tooadd en annan anspråk med namnet **avdelning** och hello-värdet måste mappas för toobe**user.department**.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-174">You need tooadd another claim named **department** and hello value needs toobe mapped too**user.department**.</span></span>

    | <span data-ttu-id="e4a9c-175">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="e4a9c-175">Attribute Name</span></span> | <span data-ttu-id="e4a9c-176">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="e4a9c-176">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="e4a9c-177">Avdelning</span><span class="sxs-lookup"><span data-stu-id="e4a9c-177">department</span></span>| <span data-ttu-id="e4a9c-178">User.Department</span><span class="sxs-lookup"><span data-stu-id="e4a9c-178">user.department</span></span> |

      ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/userattribute.png)

      <span data-ttu-id="e4a9c-180">a.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-180">a.</span></span> <span data-ttu-id="e4a9c-181">Klicka på Lägg till attributet tooopen hello attributet informationssidan Lägg till hello avdelning attribut som visas nedan-</span><span class="sxs-lookup"><span data-stu-id="e4a9c-181">Click on Add attribute tooopen hello attribute details page add hello department attribute as shown below-</span></span>

      ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/adduserattribute.png)

      <span data-ttu-id="e4a9c-183">b.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-183">b.</span></span> <span data-ttu-id="e4a9c-184">Klicka på **Ok** toosave hello attribut.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-184">Click on **Ok** toosave hello attribute.</span></span>

      <span data-ttu-id="e4a9c-185">c.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-185">c.</span></span> <span data-ttu-id="e4a9c-186">Ändra hello namn för hello attributet **emailaddress** för**e-post**.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-186">Change hello name of hello attribute **emailaddress** too**email**.</span></span>


10. <span data-ttu-id="e4a9c-187">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-187">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_certificate.png) 

11. <span data-ttu-id="e4a9c-189">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-189">Click **Save**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_400.png)

12. <span data-ttu-id="e4a9c-191">Gå för**LinkedIn admininställningar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-191">Go too**LinkedIn Admin Settings** section.</span></span> <span data-ttu-id="e4a9c-192">Överför hello XML-fil som du precis har laddat ned från hello Azure-portalen genom att klicka på hello överför XML-filalternativet.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-192">Upload hello XML file you just downloaded from hello Azure portal by clicking on hello Upload XML file option.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_metadata_03.png)

13. <span data-ttu-id="e4a9c-194">Klicka på **på** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-194">Click **On** tooenable SSO.</span></span> <span data-ttu-id="e4a9c-195">SSO status kommer att ändras från **inte ansluten** för**ansluten**</span><span class="sxs-lookup"><span data-stu-id="e4a9c-195">SSO status will change from **Not Connected** too**Connected**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e4a9c-197">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e4a9c-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="e4a9c-198">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-198">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="e4a9c-200">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e4a9c-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e4a9c-201">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-201">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e4a9c-203">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-203">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e4a9c-205">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-205">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e4a9c-207">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e4a9c-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e4a9c-209">a.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-209">a.</span></span> <span data-ttu-id="e4a9c-210">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e4a9c-211">b.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-211">b.</span></span> <span data-ttu-id="e4a9c-212">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e4a9c-213">c.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-213">c.</span></span> <span data-ttu-id="e4a9c-214">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e4a9c-215">d.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-215">d.</span></span> <span data-ttu-id="e4a9c-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-216">Click **Create**.</span></span> 

### <a name="creating-a-linkedin-elevate-test-user"></a><span data-ttu-id="e4a9c-217">Skapa en LinkedIn höjer testanvändare</span><span class="sxs-lookup"><span data-stu-id="e4a9c-217">Creating a LinkedIn Elevate test user</span></span>

<span data-ttu-id="e4a9c-218">Länkade höjer programmet stöder bara i tid användaretablering och authentication-användare kommer att skapas i hello program automatiskt.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-218">Linked Elevate Application supports Just in time user provisioning and after authentication users will be created in hello application automatically.</span></span> <span data-ttu-id="e4a9c-219">Hej administratör på sidan Inställningar på hello LinkedIn höjer portal Vänd hello växeln **automatiskt tilldela licenser** tooactive tooenable precis tid etablering och detta kommer också tilldela en licens toohello användare.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-219">On hello admin settings page on hello LinkedIn Elevate portal flip hello switch **Automatically Assign licenses** tooactive tooenable Just in time provisioning and this will also assign a license toohello user.</span></span>

   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-linkedinElevate-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e4a9c-221">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="e4a9c-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e4a9c-222">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att ge sina access tooLinkedIn utöka.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooLinkedIn Elevate.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="e4a9c-224">**tooassign Britta Simon tooLinkedIn utöka, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="e4a9c-224">**tooassign Britta Simon tooLinkedIn Elevate, perform hello following steps:**</span></span>

1. <span data-ttu-id="e4a9c-225">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-225">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e4a9c-227">Välj i listan med program hello **LinkedIn höjer**.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-227">In hello applications list, select **LinkedIn Elevate**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-linkedinElevate-tutorial/tutorial-linkedinElevate_0001.png) 

3. <span data-ttu-id="e4a9c-229">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="e4a9c-231">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-231">Click **Add** button.</span></span> <span data-ttu-id="e4a9c-232">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="e4a9c-234">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e4a9c-235">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e4a9c-236">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e4a9c-237">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e4a9c-237">Testing single sign-on</span></span>

<span data-ttu-id="e4a9c-238">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-238">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e4a9c-239">När du klickar på hello LinkedIn höjer panelen i hello åtkomstpanelen du bör få hello Azure inloggning sidan och på efter lyckad inloggning kan du bör få till LinkedIn höjer programmet.</span><span class="sxs-lookup"><span data-stu-id="e4a9c-239">When you click hello LinkedIn Elevate tile in hello Access Panel, you should get hello Azure Sign-on page and on after successful sign-on, you should get into your LinkedIn Elevate application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e4a9c-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e4a9c-240">Additional resources</span></span>

* [<span data-ttu-id="e4a9c-241">Självstudier: Konfigurera LinkedIn höjer för automatisk användaretablering med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e4a9c-241">Tutorial: Configuring LinkedIn Elevate for automatic user provisioning with Azure Active Directory</span></span>](active-directory-saas-linkedinelevate-provisioning-tutorial.md)
* [<span data-ttu-id="e4a9c-242">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e4a9c-242">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e4a9c-243">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e4a9c-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinElevate-tutorial/tutorial_general_203.png
