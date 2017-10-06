---
title: "Självstudier: Azure Active Directory-integrering med FilesAnywhere | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och FilesAnywhere."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: jeedes
ms.openlocfilehash: 376364a5c75f8d069ea6390c58586acb378cd8b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-filesanywhere"></a><span data-ttu-id="e9245-103">Självstudier: Azure Active Directory-integrering med FilesAnywhere</span><span class="sxs-lookup"><span data-stu-id="e9245-103">Tutorial: Azure Active Directory integration with FilesAnywhere</span></span>

<span data-ttu-id="e9245-104">I kursen får du lära dig hur toointegrate FilesAnywhere med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e9245-104">In this tutorial, you learn how toointegrate FilesAnywhere with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e9245-105">Integrera FilesAnywhere med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e9245-105">Integrating FilesAnywhere with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e9245-106">Du kan styra i Azure AD som har åtkomst till tooFilesAnywhere</span><span class="sxs-lookup"><span data-stu-id="e9245-106">You can control in Azure AD who has access tooFilesAnywhere</span></span>
- <span data-ttu-id="e9245-107">Du kan aktivera din användare tooautomatically get inloggade tooFilesAnywhere (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e9245-107">You can enable your users tooautomatically get signed-on tooFilesAnywhere (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e9245-108">Du kan hantera dina konton i en central plats - hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="e9245-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="e9245-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e9245-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e9245-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e9245-110">Prerequisites</span></span>

<span data-ttu-id="e9245-111">tooconfigure Azure AD-integrering med FilesAnywhere, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="e9245-111">tooconfigure Azure AD integration with FilesAnywhere, you need hello following items:</span></span>

- <span data-ttu-id="e9245-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e9245-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e9245-113">En FilesAnywhere enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="e9245-113">A FilesAnywhere single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="e9245-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e9245-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="e9245-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e9245-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e9245-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e9245-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="e9245-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e9245-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="e9245-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e9245-118">Scenario description</span></span>
<span data-ttu-id="e9245-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e9245-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e9245-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e9245-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e9245-121">Att lägga till FilesAnywhere från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e9245-121">Adding FilesAnywhere from hello gallery</span></span>
2. <span data-ttu-id="e9245-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e9245-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-filesanywhere-from-hello-gallery"></a><span data-ttu-id="e9245-123">Att lägga till FilesAnywhere från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e9245-123">Adding FilesAnywhere from hello gallery</span></span>
<span data-ttu-id="e9245-124">tooconfigure hello integrering av FilesAnywhere i Azure AD, behöver du tooadd FilesAnywhere hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="e9245-124">tooconfigure hello integration of FilesAnywhere into Azure AD, you need tooadd FilesAnywhere from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e9245-125">**tooadd FilesAnywhere från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e9245-125">**tooadd FilesAnywhere from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e9245-126">I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e9245-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e9245-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e9245-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e9245-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="e9245-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="e9245-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e9245-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="e9245-133">Skriv i sökrutan hello **FilesAnywhere**.</span><span class="sxs-lookup"><span data-stu-id="e9245-133">In hello search box, type **FilesAnywhere**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_search.png)

5. <span data-ttu-id="e9245-135">Markera hello resultat på panelen **FilesAnywhere**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="e9245-135">In hello results panel, select **FilesAnywhere**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_addfromgallery.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e9245-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e9245-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e9245-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med FilesAnywhere baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e9245-138">In this section, you configure and test Azure AD single sign-on with FilesAnywhere based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e9245-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i FilesAnywhere är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e9245-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in FilesAnywhere is tooa user in Azure AD.</span></span> <span data-ttu-id="e9245-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i FilesAnywhere toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="e9245-140">In other words, a link relationship between an Azure AD user and hello related user in FilesAnywhere needs toobe established.</span></span>

<span data-ttu-id="e9245-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i FilesAnywhere.</span><span class="sxs-lookup"><span data-stu-id="e9245-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in FilesAnywhere.</span></span>

<span data-ttu-id="e9245-142">tooconfigure och testa Azure AD enkel inloggning med FilesAnywhere, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e9245-142">tooconfigure and test Azure AD single sign-on with FilesAnywhere, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e9245-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e9245-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e9245-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e9245-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e9245-145">**[Skapa en testanvändare FilesAnywhere](#creating-a-filesanywhere-test-user)**  -toohave en motsvarighet för Britta Simon i FilesAnywhere som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="e9245-145">**[Creating a FilesAnywhere test user](#creating-a-filesanywhere-test-user)** - toohave a counterpart of Britta Simon in FilesAnywhere that is linked toohello Azure AD representation of her.</span></span>
3. <span data-ttu-id="e9245-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e9245-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
4. <span data-ttu-id="e9245-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e9245-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e9245-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e9245-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e9245-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt FilesAnywhere program.</span><span class="sxs-lookup"><span data-stu-id="e9245-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your FilesAnywhere application.</span></span>

<span data-ttu-id="e9245-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med FilesAnywhere:**</span><span class="sxs-lookup"><span data-stu-id="e9245-150">**tooconfigure Azure AD single sign-on with FilesAnywhere, perform hello following steps:**</span></span>

1. <span data-ttu-id="e9245-151">I hello Azure Management portal på hello **FilesAnywhere** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e9245-151">In hello Azure Management portal, on hello **FilesAnywhere** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="e9245-153">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e9245-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_samlbase.png)

3. <span data-ttu-id="e9245-155">På hello **FilesAnywhere domän och URL: er** om du vill tooconfigure hello programmet i **IDP initierade läge**:</span><span class="sxs-lookup"><span data-stu-id="e9245-155">On hello **FilesAnywhere Domain and URLs** section, If you wish tooconfigure hello application in **IDP initiated mode**:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_url.png)
    
    <span data-ttu-id="e9245-157">a.</span><span class="sxs-lookup"><span data-stu-id="e9245-157">a.</span></span> <span data-ttu-id="e9245-158">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.filesanywhere.com/saml20.aspx?c=215`</span><span class="sxs-lookup"><span data-stu-id="e9245-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.filesanywhere.com/saml20.aspx?c=215`</span></span>
> [!NOTE]
> <span data-ttu-id="e9245-159">Observera värdet hello **215** är en **clientid** och är bara ett exempel.</span><span class="sxs-lookup"><span data-stu-id="e9245-159">Please note that hello value **215** is a **clientid** and is just an example.</span></span> <span data-ttu-id="e9245-160">Du behöver tooreplace med hello faktiska clientid värde.</span><span class="sxs-lookup"><span data-stu-id="e9245-160">You need tooreplace it with hello actual clientid value.</span></span>

4. <span data-ttu-id="e9245-161">På hello **FilesAnywhere domän och URL: er** om du vill tooconfigure hello programmet i **SP initierade läge**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e9245-161">On hello **FilesAnywhere Domain and URLs** section, If you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_url1.png)

    <span data-ttu-id="e9245-163">a.</span><span class="sxs-lookup"><span data-stu-id="e9245-163">a.</span></span> <span data-ttu-id="e9245-164">Klicka på hello **visa avancerade inställningar för URL: en** alternativet</span><span class="sxs-lookup"><span data-stu-id="e9245-164">Click on hello **Show advanced URL settings** option</span></span>

    <span data-ttu-id="e9245-165">b.</span><span class="sxs-lookup"><span data-stu-id="e9245-165">b.</span></span> <span data-ttu-id="e9245-166">I hello **logga URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<sub domain>.filesanywhere.com/`</span><span class="sxs-lookup"><span data-stu-id="e9245-166">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://<sub domain>.filesanywhere.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e9245-167">Observera att detta inte är hello verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="e9245-167">Please note that these are not hello real values.</span></span> <span data-ttu-id="e9245-168">Du har tooupdate dessa värden med hello faktiska logga URL och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="e9245-168">You have tooupdate these values with hello actual Sign On URL and Reply URL.</span></span> <span data-ttu-id="e9245-169">Kontakta [FilesAnywhere supportteamet](mailto:support@FilesAnywhere.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="e9245-169">Contact [FilesAnywhere support team](mailto:support@FilesAnywhere.com) tooget these values.</span></span> 

5. <span data-ttu-id="e9245-170">FilesAnywhere program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="e9245-170">FilesAnywhere Software application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="e9245-171">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="e9245-171">Please configure hello following claims for this application.</span></span> <span data-ttu-id="e9245-172">Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="e9245-172">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="e9245-173">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="e9245-173">hello following screenshot shows an example for this.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_filesanywhere_attribute.png)
    
    <span data-ttu-id="e9245-175">Hej när användare loggar in med FilesAnywhere de få hello värdet för **clientid** -attributet från [FilesAnywhere team](mailto:support@FilesAnywhere.com).</span><span class="sxs-lookup"><span data-stu-id="e9245-175">When hello users signs up with FilesAnywhere they get hello value of **clientid** attribute from [FilesAnywhere team](mailto:support@FilesAnywhere.com).</span></span> <span data-ttu-id="e9245-176">Du har tooadd hello ”klient-Id”-attribut med hello unikt värde som tillhandahålls av FilesAnywhere.</span><span class="sxs-lookup"><span data-stu-id="e9245-176">You have tooadd hello "Client Id" attribute with hello unique value provided by FilesAnywhere.</span></span> <span data-ttu-id="e9245-177">Dessa attribut som visas ovan krävs.</span><span class="sxs-lookup"><span data-stu-id="e9245-177">All these attributes shown above are required.</span></span>
    > [!NOTE] 
    > <span data-ttu-id="e9245-178">Observera värdet hello **2331** av **clientid** är bara ett exempel.</span><span class="sxs-lookup"><span data-stu-id="e9245-178">Please note that hello value **2331** of **clientid** is just an example.</span></span> <span data-ttu-id="e9245-179">Du måste tooprovide hello faktiskt värde.</span><span class="sxs-lookup"><span data-stu-id="e9245-179">You need tooprovide hello actual value.</span></span>


6. <span data-ttu-id="e9245-180">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i hello bilden ovan och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e9245-180">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="e9245-181">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="e9245-181">Attribute Name</span></span> | <span data-ttu-id="e9245-182">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="e9245-182">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="e9245-183">clientid</span><span class="sxs-lookup"><span data-stu-id="e9245-183">clientid</span></span> | <span data-ttu-id="e9245-184">*”uniquevalue”*</span><span class="sxs-lookup"><span data-stu-id="e9245-184">*"uniquevalue"*</span></span> |

    <span data-ttu-id="e9245-185">a.</span><span class="sxs-lookup"><span data-stu-id="e9245-185">a.</span></span> <span data-ttu-id="e9245-186">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e9245-186">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_05.png)
    
    <span data-ttu-id="e9245-189">b.</span><span class="sxs-lookup"><span data-stu-id="e9245-189">b.</span></span> <span data-ttu-id="e9245-190">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="e9245-190">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="e9245-191">c.</span><span class="sxs-lookup"><span data-stu-id="e9245-191">c.</span></span> <span data-ttu-id="e9245-192">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="e9245-192">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="e9245-193">d.</span><span class="sxs-lookup"><span data-stu-id="e9245-193">d.</span></span> <span data-ttu-id="e9245-194">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="e9245-194">Click **Ok**</span></span>

7. <span data-ttu-id="e9245-195">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e9245-195">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="e9245-197">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="e9245-197">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_certificate.png) 

9. <span data-ttu-id="e9245-199">På hello **FilesAnywhere Configuration** klickar du på **konfigurera FilesAnywhere** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="e9245-199">On hello **FilesAnywhere Configuration** section, click **Configure FilesAnywhere** tooopen **Configure sign-on** window.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_configure.png) 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_configuresignon.png)

10. <span data-ttu-id="e9245-202">tooget SSO konfigurationen har slutförts för programmet i slutet av FilesAnywhere Kontakta [FilesAnywhere supportteamet](mailto:support@FilesAnywhere.com) och ge dem hello hämtas SAML-token signera certifikat och enkel inloggning (SSO)-URL.</span><span class="sxs-lookup"><span data-stu-id="e9245-202">tooget SSO configuration complete for your application at FilesAnywhere end, contact [FilesAnywhere support team](mailto:support@FilesAnywhere.com) and provide them hello downloaded SAML token signing Certificate and Single Sign On (SSO) URL.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e9245-203">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e9245-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="e9245-204">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e9245-204">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="e9245-206">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e9245-206">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e9245-207">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e9245-207">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e9245-209">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="e9245-209">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e9245-211">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e9245-211">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e9245-213">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e9245-213">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-FilesAnywhere-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e9245-215">a.</span><span class="sxs-lookup"><span data-stu-id="e9245-215">a.</span></span> <span data-ttu-id="e9245-216">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e9245-216">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e9245-217">b.</span><span class="sxs-lookup"><span data-stu-id="e9245-217">b.</span></span> <span data-ttu-id="e9245-218">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e9245-218">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e9245-219">c.</span><span class="sxs-lookup"><span data-stu-id="e9245-219">c.</span></span> <span data-ttu-id="e9245-220">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e9245-220">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e9245-221">d.</span><span class="sxs-lookup"><span data-stu-id="e9245-221">d.</span></span> <span data-ttu-id="e9245-222">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e9245-222">Click **Create**.</span></span> 



### <a name="creating-a-filesanywhere-test-user"></a><span data-ttu-id="e9245-223">Skapa en testanvändare FilesAnywhere</span><span class="sxs-lookup"><span data-stu-id="e9245-223">Creating a FilesAnywhere test user</span></span>

<span data-ttu-id="e9245-224">Programmet stöder bara i tid användaretablering och authentication-användare kommer att skapas i hello program automatiskt.</span><span class="sxs-lookup"><span data-stu-id="e9245-224">Application supports Just in time user provisioning and after authentication users will be created in hello application automatically.</span></span> 


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e9245-225">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="e9245-225">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e9245-226">I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooFilesAnywhere sin åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e9245-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooFilesAnywhere.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="e9245-228">**tooassign Britta Simon tooFilesAnywhere utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e9245-228">**tooassign Britta Simon tooFilesAnywhere, perform hello following steps:**</span></span>

1. <span data-ttu-id="e9245-229">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e9245-229">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e9245-231">Välj i listan med program hello **FilesAnywhere**.</span><span class="sxs-lookup"><span data-stu-id="e9245-231">In hello applications list, select **FilesAnywhere**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_FilesAnywhere_app.png) 

3. <span data-ttu-id="e9245-233">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e9245-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="e9245-235">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e9245-235">Click **Add** button.</span></span> <span data-ttu-id="e9245-236">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e9245-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="e9245-238">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="e9245-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e9245-239">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e9245-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e9245-240">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e9245-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="e9245-241">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e9245-241">Testing single sign-on</span></span>

<span data-ttu-id="e9245-242">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e9245-242">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e9245-243">Du bör få automatiskt inloggade tooyour FilesAnywhere programmet när du klickar på hello FilesAnywhere panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e9245-243">When you click hello FilesAnywhere tile in hello Access Panel, you should get automatically signed-on tooyour FilesAnywhere application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="e9245-244">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e9245-244">Additional resources</span></span>

* [<span data-ttu-id="e9245-245">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e9245-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e9245-246">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e9245-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-FilesAnywhere-tutorial/tutorial_general_203.png
