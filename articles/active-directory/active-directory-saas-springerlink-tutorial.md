---
title: "Självstudier: Azure Active Directory-integrering med Springer länk | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Springer länk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 58cdf029-bdc0-43c4-a469-b921c2a669bd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: jeedes
ms.openlocfilehash: dabd2f72b3a195fc359826a4863a197e5019f5c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-springer-link"></a><span data-ttu-id="15a6c-103">Självstudier: Azure Active Directory-integrering med Springer länk</span><span class="sxs-lookup"><span data-stu-id="15a6c-103">Tutorial: Azure Active Directory integration with Springer Link</span></span>

<span data-ttu-id="15a6c-104">I kursen får du lära dig hur toointegrate Springer länka med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="15a6c-104">In this tutorial, you learn how toointegrate Springer Link with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="15a6c-105">Integrera Springer länken med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="15a6c-105">Integrating Springer Link with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="15a6c-106">Du kan styra i Azure AD som har åtkomst tooSpringer länk.</span><span class="sxs-lookup"><span data-stu-id="15a6c-106">You can control in Azure AD who has access tooSpringer Link.</span></span>
- <span data-ttu-id="15a6c-107">Du kan låta dina användare tooautomatically get inloggade tooSpringer länk (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="15a6c-107">You can enable your users tooautomatically get signed-on tooSpringer Link (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="15a6c-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="15a6c-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="15a6c-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="15a6c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15a6c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="15a6c-110">Prerequisites</span></span>

<span data-ttu-id="15a6c-111">tooconfigure Azure AD-integrering med Springer länk, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="15a6c-111">tooconfigure Azure AD integration with Springer Link, you need hello following items:</span></span>

- <span data-ttu-id="15a6c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="15a6c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="15a6c-113">En länk Springer enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="15a6c-113">A Springer Link single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="15a6c-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="15a6c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="15a6c-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="15a6c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="15a6c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="15a6c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="15a6c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="15a6c-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="15a6c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="15a6c-118">Scenario description</span></span>
<span data-ttu-id="15a6c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="15a6c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="15a6c-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="15a6c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="15a6c-121">Ny Springer länk från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="15a6c-121">Adding Springer Link from hello gallery</span></span>
2. <span data-ttu-id="15a6c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="15a6c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-springer-link-from-hello-gallery"></a><span data-ttu-id="15a6c-123">Ny Springer länk från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="15a6c-123">Adding Springer Link from hello gallery</span></span>
<span data-ttu-id="15a6c-124">tooconfigure hello integrering av Springer länken i Azure AD, behöver du tooadd Springer länken hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="15a6c-124">tooconfigure hello integration of Springer Link into Azure AD, you need tooadd Springer Link from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="15a6c-125">**tooadd Springer länk från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="15a6c-125">**tooadd Springer Link from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="15a6c-126">I hello ** [Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="15a6c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="15a6c-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="15a6c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="15a6c-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="15a6c-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="15a6c-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="15a6c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="15a6c-133">Skriv i sökrutan hello **Springer länken**väljer **Springer länken** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="15a6c-133">In hello search box, type **Springer Link**, select **Springer Link** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Springer länken i resultatlistan hello](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="15a6c-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="15a6c-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="15a6c-136">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Springer länk baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="15a6c-136">In this section, you configure and test Azure AD single sign-on with Springer Link based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="15a6c-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Springer länk är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="15a6c-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Springer Link is tooa user in Azure AD.</span></span> <span data-ttu-id="15a6c-138">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Springer länken toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="15a6c-138">In other words, a link relationship between an Azure AD user and hello related user in Springer Link needs toobe established.</span></span>

<span data-ttu-id="15a6c-139">Tilldela hello värdet för hello i Springer länken **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="15a6c-139">In Springer Link, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="15a6c-140">tooconfigure och testa Azure AD enkel inloggning med Springer länk, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="15a6c-140">tooconfigure and test Azure AD single sign-on with Springer Link, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="15a6c-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on) ** -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="15a6c-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="15a6c-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user) ** -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="15a6c-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="15a6c-143">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user) ** -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="15a6c-143">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
4. <span data-ttu-id="15a6c-144">**[Testa enkel inloggning](#test-single-sign-on) ** -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="15a6c-144">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="15a6c-145">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="15a6c-145">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="15a6c-146">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Springer länk.</span><span class="sxs-lookup"><span data-stu-id="15a6c-146">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Springer Link application.</span></span>

<span data-ttu-id="15a6c-147">**Utför följande hello tooconfigure Azure AD enkel inloggning med Springer länk:**</span><span class="sxs-lookup"><span data-stu-id="15a6c-147">**tooconfigure Azure AD single sign-on with Springer Link, perform hello following steps:**</span></span>

1. <span data-ttu-id="15a6c-148">I hello Azure-portalen på hello **Springer länken** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="15a6c-148">In hello Azure portal, on hello **Springer Link** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="15a6c-150">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="15a6c-150">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_samlbase.png)

3. <span data-ttu-id="15a6c-152">På hello **Springer länken domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="15a6c-152">On hello **Springer Link Domain and URLs** section,  If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![URL: er och springer länken domän med enkel inloggning information](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_url1.png)

    <span data-ttu-id="15a6c-154">a.</span><span class="sxs-lookup"><span data-stu-id="15a6c-154">a.</span></span> <span data-ttu-id="15a6c-155">I hello **identifierare** textruta typen hello URL:`https://fsso.springer.com`</span><span class="sxs-lookup"><span data-stu-id="15a6c-155">In hello **Identifier** textbox, type hello URL: `https://fsso.springer.com`</span></span>

    <span data-ttu-id="15a6c-156">b.</span><span class="sxs-lookup"><span data-stu-id="15a6c-156">b.</span></span> <span data-ttu-id="15a6c-157">I hello **Reply URL** textruta typen hello URL:`https://fsso-qa1.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`</span><span class="sxs-lookup"><span data-stu-id="15a6c-157">In hello **Reply URL** textbox, type hello URL: `https://fsso-qa1.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`</span></span>    

4. <span data-ttu-id="15a6c-158">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="15a6c-158">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="15a6c-159">Om du inte vill tooconfigure hello programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="15a6c-159">If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![URL: er och springer länken domän med enkel inloggning information](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_url.png)

    <span data-ttu-id="15a6c-161">I hello **inloggnings-URL** textruta typen hello URL:`https://fsso.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`</span><span class="sxs-lookup"><span data-stu-id="15a6c-161">In hello **Sign-on URL** textbox, type hello URL : `https://fsso.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`</span></span>    

5. <span data-ttu-id="15a6c-162">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="15a6c-162">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-springerlink-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="15a6c-164">toogenerate hello **Metadata** url, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="15a6c-164">toogenerate hello **Metadata** url, perform hello following steps:</span></span>

    <span data-ttu-id="15a6c-165">a.</span><span class="sxs-lookup"><span data-stu-id="15a6c-165">a.</span></span> <span data-ttu-id="15a6c-166">Klicka på **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="15a6c-166">Click **App registrations**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_appregistrations.png)
   
    <span data-ttu-id="15a6c-168">b.</span><span class="sxs-lookup"><span data-stu-id="15a6c-168">b.</span></span> <span data-ttu-id="15a6c-169">Klicka på **slutpunkter** tooopen **slutpunkter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="15a6c-169">Click **Endpoints** tooopen **Endpoints** dialog box.</span></span>  
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_endpointicon.png)

    <span data-ttu-id="15a6c-171">c.</span><span class="sxs-lookup"><span data-stu-id="15a6c-171">c.</span></span> <span data-ttu-id="15a6c-172">Klicka på hello Kopiera knappen toocopy **FEDERATION METADATADOKUMENTET** url och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="15a6c-172">Click hello copy button toocopy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_endpoint.png)
     
    <span data-ttu-id="15a6c-174">d.</span><span class="sxs-lookup"><span data-stu-id="15a6c-174">d.</span></span> <span data-ttu-id="15a6c-175">Gå nu toohello egenskapssida **Springer länken** och kopiera hello **program-Id** med **kopiera** knappen och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="15a6c-175">Now go toohello property page of **Springer Link** and copy hello **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_appid.png)

    <span data-ttu-id="15a6c-177">e.</span><span class="sxs-lookup"><span data-stu-id="15a6c-177">e.</span></span> <span data-ttu-id="15a6c-178">Generera hello **URL för tjänstmetadata** med hello följande mönster:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="15a6c-178">Generate hello **Metadata URL** using hello following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>

7. <span data-ttu-id="15a6c-179">tooconfigure enkel inloggning på **Springer länken** sida, behöver du toosend hello genereras **URL för tjänstmetadata** för[Springer länken supportteamet](mailto:identity@springernature.com).</span><span class="sxs-lookup"><span data-stu-id="15a6c-179">tooconfigure single sign-on on **Springer Link** side, you need toosend hello generated **Metadata URL** too[Springer Link support team](mailto:identity@springernature.com).</span></span>

> [!TIP]
> <span data-ttu-id="15a6c-180">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="15a6c-180">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="15a6c-181">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello ** Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="15a6c-181">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="15a6c-182">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="15a6c-182">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="15a6c-183">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="15a6c-183">Create an Azure AD test user</span></span>

<span data-ttu-id="15a6c-184">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="15a6c-184">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="15a6c-186">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="15a6c-186">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="15a6c-187">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="15a6c-187">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-springerlink-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="15a6c-189">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="15a6c-189">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-springerlink-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="15a6c-191">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="15a6c-191">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-springerlink-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="15a6c-193">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="15a6c-193">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-springerlink-tutorial/create_aaduser_04.png)

    <span data-ttu-id="15a6c-195">a.</span><span class="sxs-lookup"><span data-stu-id="15a6c-195">a.</span></span> <span data-ttu-id="15a6c-196">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="15a6c-196">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="15a6c-197">b.</span><span class="sxs-lookup"><span data-stu-id="15a6c-197">b.</span></span> <span data-ttu-id="15a6c-198">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="15a6c-198">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="15a6c-199">c.</span><span class="sxs-lookup"><span data-stu-id="15a6c-199">c.</span></span> <span data-ttu-id="15a6c-200">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="15a6c-200">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="15a6c-201">d.</span><span class="sxs-lookup"><span data-stu-id="15a6c-201">d.</span></span> <span data-ttu-id="15a6c-202">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="15a6c-202">Click **Create**.</span></span>
 
### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="15a6c-203">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="15a6c-203">Assign hello Azure AD test user</span></span>

<span data-ttu-id="15a6c-204">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSpringer länk.</span><span class="sxs-lookup"><span data-stu-id="15a6c-204">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSpringer Link.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="15a6c-206">**tooassign Britta Simon tooSpringer länken, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="15a6c-206">**tooassign Britta Simon tooSpringer Link, perform hello following steps:**</span></span>

1. <span data-ttu-id="15a6c-207">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="15a6c-207">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="15a6c-209">Välj i listan med program hello **Springer länk**.</span><span class="sxs-lookup"><span data-stu-id="15a6c-209">In hello applications list, select **Springer Link**.</span></span>

    ![hello Springer länken länken i listan med program hello](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_app.png)  

3. <span data-ttu-id="15a6c-211">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="15a6c-211">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="15a6c-213">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="15a6c-213">Click **Add** button.</span></span> <span data-ttu-id="15a6c-214">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="15a6c-214">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="15a6c-216">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="15a6c-216">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="15a6c-217">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="15a6c-217">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="15a6c-218">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="15a6c-218">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="15a6c-219">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="15a6c-219">Test single sign-on</span></span>

<span data-ttu-id="15a6c-220">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="15a6c-220">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="15a6c-221">Du bör få automatiskt inloggade tooyour Springer länken programmet när du klickar på hello Springer länken panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="15a6c-221">When you click hello Springer Link tile in hello Access Panel, you should get automatically signed-on tooyour Springer Link application.</span></span>
<span data-ttu-id="15a6c-222">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="15a6c-222">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="15a6c-223">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="15a6c-223">Additional resources</span></span>

* [<span data-ttu-id="15a6c-224">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="15a6c-224">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="15a6c-225">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="15a6c-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_203.png

