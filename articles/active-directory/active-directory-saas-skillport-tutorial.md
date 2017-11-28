---
title: "Självstudier: Azure Active Directory-integrering med Skillport | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Skillport."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4df349b2-a73f-4b88-a077-ec0fbfc26527
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jeedes
ms.openlocfilehash: ba504c3cae5f92767eb90d8453887904690fe0c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-skillport"></a><span data-ttu-id="a8dd5-103">Självstudier: Azure Active Directory-integrering med Skillport</span><span class="sxs-lookup"><span data-stu-id="a8dd5-103">Tutorial: Azure Active Directory integration with Skillport</span></span>

<span data-ttu-id="a8dd5-104">I kursen får du lära dig hur toointegrate Skillport med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a8dd5-104">In this tutorial, you learn how toointegrate Skillport with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a8dd5-105">Integrera Skillport med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a8dd5-105">Integrating Skillport with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a8dd5-106">Du kan styra i Azure AD som har åtkomst till tooSkillport</span><span class="sxs-lookup"><span data-stu-id="a8dd5-106">You can control in Azure AD who has access tooSkillport</span></span>
- <span data-ttu-id="a8dd5-107">Du kan aktivera din användare tooautomatically get inloggade tooSkillport (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a8dd5-107">You can enable your users tooautomatically get signed-on tooSkillport (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a8dd5-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a8dd5-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a8dd5-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a8dd5-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8dd5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a8dd5-110">Prerequisites</span></span>

<span data-ttu-id="a8dd5-111">tooconfigure Azure AD-integrering med Skillport, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a8dd5-111">tooconfigure Azure AD integration with Skillport, you need hello following items:</span></span>

- <span data-ttu-id="a8dd5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a8dd5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a8dd5-113">En Skillport enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="a8dd5-113">A Skillport single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a8dd5-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a8dd5-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a8dd5-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a8dd5-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a8dd5-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a8dd5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a8dd5-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a8dd5-118">Scenario description</span></span>
<span data-ttu-id="a8dd5-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a8dd5-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a8dd5-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a8dd5-121">Att lägga till Skillport från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a8dd5-121">Adding Skillport from hello gallery</span></span>
2. <span data-ttu-id="a8dd5-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a8dd5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-skillport-from-hello-gallery"></a><span data-ttu-id="a8dd5-123">Att lägga till Skillport från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a8dd5-123">Adding Skillport from hello gallery</span></span>
<span data-ttu-id="a8dd5-124">tooconfigure hello integrering av Skillport i Azure AD, behöver du tooadd Skillport hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-124">tooconfigure hello integration of Skillport into Azure AD, you need tooadd Skillport from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a8dd5-125">**tooadd Skillport från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a8dd5-125">**tooadd Skillport from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8dd5-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a8dd5-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a8dd5-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a8dd5-131">Klicka på **nytt program** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-131">Click **New Application** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a8dd5-133">Skriv i sökrutan hello **Skillport**.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-133">In hello search box, type **Skillport**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_search.png)

5. <span data-ttu-id="a8dd5-135">Markera hello resultat på panelen **Skillport**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-135">In hello results panel, select **Skillport**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a8dd5-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a8dd5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a8dd5-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Skillport baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-138">In this section, you configure and test Azure AD single sign-on with Skillport based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a8dd5-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Skillport är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Skillport is tooa user in Azure AD.</span></span> <span data-ttu-id="a8dd5-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Skillport toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-140">In other words, a link relationship between an Azure AD user and hello related user in Skillport needs toobe established.</span></span>

<span data-ttu-id="a8dd5-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Skillport.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Skillport.</span></span>

<span data-ttu-id="a8dd5-142">tooconfigure och testa Azure AD enkel inloggning med Skillport, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a8dd5-142">tooconfigure and test Azure AD single sign-on with Skillport, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a8dd5-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a8dd5-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a8dd5-145">**[Skapa en testanvändare Skillport](#creating-a-skillport-test-user)**  -toohave en motsvarighet för Britta Simon i Skillport som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-145">**[Creating a Skillport test user](#creating-a-skillport-test-user)** - toohave a counterpart of Britta Simon in Skillport that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a8dd5-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a8dd5-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a8dd5-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a8dd5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a8dd5-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Skillport program.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Skillport application.</span></span>

<span data-ttu-id="a8dd5-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Skillport:**</span><span class="sxs-lookup"><span data-stu-id="a8dd5-150">**tooconfigure Azure AD single sign-on with Skillport, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8dd5-151">I hello Azure-portalen på hello **Skillport** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-151">In hello Azure  portal, on hello **Skillport** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a8dd5-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_samlbase.png)

3. <span data-ttu-id="a8dd5-155">På hello **Skillport domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a8dd5-155">On hello **Skillport Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_url.png)

    <span data-ttu-id="a8dd5-157">a.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-157">a.</span></span> <span data-ttu-id="a8dd5-158">I hello **inloggnings-URL** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="a8dd5-158">In hello **Sign-on URL** textbox, type a URL using hello following patterns:</span></span>
      
      <span data-ttu-id="a8dd5-159">Europa Datacenter:`https://<subdomain>.skillport.eu`</span><span class="sxs-lookup"><span data-stu-id="a8dd5-159">EU Datacenter: `https://<subdomain>.skillport.eu`</span></span>
   
      <span data-ttu-id="a8dd5-160">USA Datacenter:`https://<subdomain>.skillport.com`</span><span class="sxs-lookup"><span data-stu-id="a8dd5-160">US Datacenter: `https://<subdomain>.skillport.com`</span></span>
   
    <span data-ttu-id="a8dd5-161">b.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-161">b.</span></span> <span data-ttu-id="a8dd5-162">I hello **Reply URL** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="a8dd5-162">In hello **Reply URL** textbox, type a URL using hello following patterns:</span></span>
    
      <span data-ttu-id="a8dd5-163">Europa Datacenter:`https://<subdomain>.skillport.eu/adfs/ls/`</span><span class="sxs-lookup"><span data-stu-id="a8dd5-163">EU Datacenter: `https://<subdomain>.skillport.eu/adfs/ls/`</span></span>
    
      <span data-ttu-id="a8dd5-164">USA Datacenter:`https://<subdomain>.skillport.com/sp/ACS.saml2`</span><span class="sxs-lookup"><span data-stu-id="a8dd5-164">US Datacenter: `https://<subdomain>.skillport.com/sp/ACS.saml2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a8dd5-165">Dessa värden är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-165">These values are not hello real.</span></span> <span data-ttu-id="a8dd5-166">Uppdatera dessa värden med hello faktiska Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-166">Update these values with hello actual Reply URL and Sign-On URL.</span></span> <span data-ttu-id="a8dd5-167">Kontakta [Skillport klienten supportteamet](https://www.skillsoft.com/contact.asp) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-167">Contact [Skillport Client support team](https://www.skillsoft.com/contact.asp) tooget these values.</span></span>
 
4. <span data-ttu-id="a8dd5-168">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-168">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_certificate.png) 

5. <span data-ttu-id="a8dd5-170">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-170">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skillport-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a8dd5-172">tooconfigure enkel inloggning på **Skillport** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Skillport supportteamet](https://www.skillsoft.com/contact.asp).</span><span class="sxs-lookup"><span data-stu-id="a8dd5-172">tooconfigure single sign-on on **Skillport** side, you need toosend hello downloaded **Metadata XML** too[Skillport support team](https://www.skillsoft.com/contact.asp).</span></span> <span data-ttu-id="a8dd5-173">De ska ställa in den toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-173">They will set it up toohave hello SAML SSO connection set properly on both sides.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a8dd5-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8dd5-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="a8dd5-175">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a8dd5-177">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a8dd5-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8dd5-178">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-178">In hello **Azure  portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skillport-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a8dd5-180">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-180">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skillport-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a8dd5-182">Hello överkant hello dialogrutan, klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-182">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skillport-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a8dd5-184">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a8dd5-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skillport-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a8dd5-186">a.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-186">a.</span></span> <span data-ttu-id="a8dd5-187">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a8dd5-188">b.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-188">b.</span></span> <span data-ttu-id="a8dd5-189">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a8dd5-190">c.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-190">c.</span></span> <span data-ttu-id="a8dd5-191">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a8dd5-192">d.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-192">d.</span></span> <span data-ttu-id="a8dd5-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-193">Click **Create**.</span></span>
 
### <a name="creating-a-skillport-test-user"></a><span data-ttu-id="a8dd5-194">Skapa en testanvändare Skillport</span><span class="sxs-lookup"><span data-stu-id="a8dd5-194">Creating a Skillport test user</span></span>

<span data-ttu-id="a8dd5-195">I ordning toocreate Skillport användare, behöver du toocontact [Skillport supportteamet](https://www.skillsoft.com/contact.asp) eftersom de har flera affärsscenarier enligt toohello behovet av slutanvändaren.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-195">In order toocreate Skillport test user, you need toocontact [Skillport support team](https://www.skillsoft.com/contact.asp) as they have multiple business scenarios according toohello requirement of end user.</span></span> <span data-ttu-id="a8dd5-196">Det konfigureras efter diskussioner med hello användare.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-196">They will configure it after discussion with hello users.</span></span>  

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a8dd5-197">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="a8dd5-197">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a8dd5-198">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSkillport.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSkillport.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a8dd5-200">**tooassign Britta Simon tooSkillport utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a8dd5-200">**tooassign Britta Simon tooSkillport, perform hello following steps:**</span></span>

1. <span data-ttu-id="a8dd5-201">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a8dd5-203">Välj i listan med program hello **Skillport**.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-203">In hello applications list, select **Skillport**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skillport-tutorial/tutorial_skillport_app.png) 

3. <span data-ttu-id="a8dd5-205">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a8dd5-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-207">Click **Add** button.</span></span> <span data-ttu-id="a8dd5-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a8dd5-210">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a8dd5-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a8dd5-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a8dd5-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a8dd5-213">Testing single sign-on</span></span>

<span data-ttu-id="a8dd5-214">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a8dd5-215">Du bör få automatiskt inloggade tooyour Skillport programmet när du klickar på hello Skillport panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a8dd5-215">When you click hello Skillport tile in hello Access Panel, you should get automatically signed-on tooyour Skillport application.</span></span>
<span data-ttu-id="a8dd5-216">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="a8dd5-216">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a8dd5-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a8dd5-217">Additional resources</span></span>

* [<span data-ttu-id="a8dd5-218">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a8dd5-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a8dd5-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a8dd5-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-skillport-tutorial/tutorial_general_203.png

