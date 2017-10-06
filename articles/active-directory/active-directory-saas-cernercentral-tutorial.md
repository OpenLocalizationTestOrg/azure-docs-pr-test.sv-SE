---
title: "Självstudier: Azure Active Directory-integrering med Cerner Central | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Cerner Central."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2bc549d-d286-4679-854e-bb67c62b0475
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 3493d180e8f229b7cd228769f780f10208114889
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cerner-central"></a><span data-ttu-id="c441a-103">Självstudier: Azure Active Directory-integrering med Cerner Central</span><span class="sxs-lookup"><span data-stu-id="c441a-103">Tutorial: Azure Active Directory integration with Cerner Central</span></span>

<span data-ttu-id="c441a-104">I kursen får du lära dig hur toointegrate Cerner Central med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c441a-104">In this tutorial, you learn how toointegrate Cerner Central with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c441a-105">Integrera Cerner Central med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c441a-105">Integrating Cerner Central with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c441a-106">Du kan styra i Azure AD som har åtkomst tooCerner Central</span><span class="sxs-lookup"><span data-stu-id="c441a-106">You can control in Azure AD who has access tooCerner Central</span></span>
- <span data-ttu-id="c441a-107">Du kan aktivera din användare tooautomatically get inloggade tooCerner Central (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c441a-107">You can enable your users tooautomatically get signed-on tooCerner Central (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c441a-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c441a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c441a-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c441a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c441a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c441a-110">Prerequisites</span></span>

<span data-ttu-id="c441a-111">tooconfigure Azure AD-integrering med Cerner Central måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="c441a-111">tooconfigure Azure AD integration with Cerner Central, you need hello following items:</span></span>

- <span data-ttu-id="c441a-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c441a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c441a-113">En godkänd Cerner centrala System-kontot</span><span class="sxs-lookup"><span data-stu-id="c441a-113">An approved Cerner Central System Account</span></span>

> [!NOTE]
> <span data-ttu-id="c441a-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c441a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c441a-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c441a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c441a-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c441a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c441a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c441a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c441a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c441a-118">Scenario description</span></span>
<span data-ttu-id="c441a-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c441a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c441a-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c441a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c441a-121">Att lägga till Cerner Central från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c441a-121">Adding Cerner Central from hello gallery</span></span>
2. <span data-ttu-id="c441a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c441a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cerner-central-from-hello-gallery"></a><span data-ttu-id="c441a-123">Att lägga till Cerner Central från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c441a-123">Adding Cerner Central from hello gallery</span></span>
<span data-ttu-id="c441a-124">tooconfigure hello integrering av Cerner Central i Azure AD, behöver du tooadd Cerner Central hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="c441a-124">tooconfigure hello integration of Cerner Central into Azure AD, you need tooadd Cerner Central from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c441a-125">**tooadd Cerner Central från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c441a-125">**tooadd Cerner Central from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c441a-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c441a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c441a-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c441a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c441a-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="c441a-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="c441a-131">tooadd nya program, klickar du på **nytt program** knappen ovanpå hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c441a-131">tooadd new application, click **New application** button on top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="c441a-133">Skriv i sökrutan hello **Cerner Central**.</span><span class="sxs-lookup"><span data-stu-id="c441a-133">In hello search box, type **Cerner Central**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_search.png)

5. <span data-ttu-id="c441a-135">Markera hello resultat på panelen **Cerner Central**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="c441a-135">In hello results panel, select **Cerner Central**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c441a-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c441a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c441a-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Cerner Central baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c441a-138">In this section, you configure and test Azure AD single sign-on with Cerner Central based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c441a-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Cerner Central är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c441a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Cerner Central is tooa user in Azure AD.</span></span> <span data-ttu-id="c441a-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Cerner Central toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="c441a-140">In other words, a link relationship between an Azure AD user and hello related user in Cerner Central needs toobe established.</span></span>

<span data-ttu-id="c441a-141">tooconfigure och testa Azure AD enkel inloggning med Cerner Central, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c441a-141">tooconfigure and test Azure AD single sign-on with Cerner Central, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c441a-142">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c441a-142">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c441a-143">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c441a-143">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c441a-144">**[Skapa en testanvändare Cerner Central](#creating-a-cerner-central-test-user)**  -toohave en motsvarighet för Britta Simon i Cerner Central som är länkade toohello Azure AD-representation av hello användare.</span><span class="sxs-lookup"><span data-stu-id="c441a-144">**[Creating a Cerner Central test user](#creating-a-cerner-central-test-user)** - toohave a counterpart of Britta Simon in Cerner Central that is linked toohello Azure AD representation of hello user.</span></span>
4. <span data-ttu-id="c441a-145">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c441a-145">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c441a-146">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c441a-146">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c441a-147">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c441a-147">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c441a-148">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Cerner Central.</span><span class="sxs-lookup"><span data-stu-id="c441a-148">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Cerner Central application.</span></span>

<span data-ttu-id="c441a-149">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Cerner Central:**</span><span class="sxs-lookup"><span data-stu-id="c441a-149">**tooconfigure Azure AD single sign-on with Cerner Central, perform hello following steps:**</span></span>

1. <span data-ttu-id="c441a-150">I hello Azure-portalen på hello **Cerner Central** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c441a-150">In hello Azure portal, on hello **Cerner Central** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c441a-152">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c441a-152">On hello **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_samlbase.png)

3. <span data-ttu-id="c441a-154">På hello **Cerner centrala domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c441a-154">On hello **Cerner Central Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_url.png)

    <span data-ttu-id="c441a-156">a.</span><span class="sxs-lookup"><span data-stu-id="c441a-156">a.</span></span> <span data-ttu-id="c441a-157">I hello **identifierare** textruta hello TYPVÄRDE med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="c441a-157">In hello **Identifier** textbox, type hello value using hello following patterns:</span></span>
    
    | |
    |--|
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.sandboxcerner.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.sandboxcernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://sandboxcernercentral.com/session-api/protocol/saml2/metadata` |
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/metadata` |

    <span data-ttu-id="c441a-158">b.</span><span class="sxs-lookup"><span data-stu-id="c441a-158">b.</span></span> <span data-ttu-id="c441a-159">I hello **Reply URL** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="c441a-159">In hello **Reply URL** textbox, type a URL using hello following patterns:</span></span> 
    | |
    |--|
    | `https://<instancename>.cernercentral.com/session-api/protocol/saml2/sso` |
    | `https://cernercentral.com/<instasncename>` |
    | `https://sandboxcernercentral.com/<instancename>` |
    | `https://sandboxcernercentral.com/<instancename>` |
    | `https://<subdomain>.sandboxcernercentral.com/<instancename>` |

    > [!NOTE] 
    > <span data-ttu-id="c441a-160">Dessa värden är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="c441a-160">These values are not hello real.</span></span> <span data-ttu-id="c441a-161">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="c441a-161">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="c441a-162">Kontakta [Cerner Central supportteamet](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="c441a-162">Contact [Cerner Central support team](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations) tooget these values.</span></span>
 
4. <span data-ttu-id="c441a-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c441a-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cernercentral-tutorial/tutorial_general_400.png)

5. <span data-ttu-id="c441a-165">toogenerate hello **Metadata** url, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c441a-165">toogenerate hello **Metadata** url, perform hello following steps:</span></span>

    <span data-ttu-id="c441a-166">a.</span><span class="sxs-lookup"><span data-stu-id="c441a-166">a.</span></span> <span data-ttu-id="c441a-167">Klicka på **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="c441a-167">Click **App registrations**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_appregistrations.png)
   
    <span data-ttu-id="c441a-169">b.</span><span class="sxs-lookup"><span data-stu-id="c441a-169">b.</span></span> <span data-ttu-id="c441a-170">Klicka på **slutpunkter** tooopen **slutpunkter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c441a-170">Click **Endpoints** tooopen **Endpoints** dialog box.</span></span>  
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_endpointicon.png)

    <span data-ttu-id="c441a-172">c.</span><span class="sxs-lookup"><span data-stu-id="c441a-172">c.</span></span> <span data-ttu-id="c441a-173">Klicka på hello Kopiera knappen toocopy **FEDERATION METADATADOKUMENTET** url och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="c441a-173">Click hello copy button toocopy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_endpoint.png)
     
    <span data-ttu-id="c441a-175">d.</span><span class="sxs-lookup"><span data-stu-id="c441a-175">d.</span></span> <span data-ttu-id="c441a-176">Gå nu toohello egenskapssida **Cerner Central** och kopiera hello **program-Id** med **kopiera** knappen och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="c441a-176">Now go toohello property page of **Cerner Central** and copy hello **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_appid.png)

    <span data-ttu-id="c441a-178">e.</span><span class="sxs-lookup"><span data-stu-id="c441a-178">e.</span></span> <span data-ttu-id="c441a-179">Generera hello **URL för tjänstmetadata** med hello följande mönster:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="c441a-179">Generate hello **Metadata URL** using hello following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>

6. <span data-ttu-id="c441a-180">tooconfigure enkel inloggning på **Cerner Central** sida, behöver du toosend hello **URL för tjänstmetadata** för[Cerner Central support](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations).</span><span class="sxs-lookup"><span data-stu-id="c441a-180">tooconfigure single sign-on on **Cerner Central** side, you need toosend hello **Metadata URL** too[Cerner Central support](https://wiki.ucern.com/display/CernerCentral/Contacting+Cloud+Operations).</span></span> <span data-ttu-id="c441a-181">De konfigurera hello SSO program på klientsidan toocomplete hello integration.</span><span class="sxs-lookup"><span data-stu-id="c441a-181">They configure hello SSO on application side toocomplete hello integration.</span></span>

> [!TIP]
> <span data-ttu-id="c441a-182">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="c441a-182">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c441a-183">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="c441a-183">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c441a-184">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c441a-184">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c441a-185">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c441a-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="c441a-186">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c441a-186">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span> 

![Skapa Azure AD-användare][100]

<span data-ttu-id="c441a-188">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c441a-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c441a-189">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c441a-189">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c441a-191">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c441a-191">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c441a-193">tooopen hello **användare** dialogrutan klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c441a-193">tooopen hello **User** dialog, click **Add**.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c441a-195">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c441a-195">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cernercentral-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c441a-197">a.</span><span class="sxs-lookup"><span data-stu-id="c441a-197">a.</span></span> <span data-ttu-id="c441a-198">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c441a-198">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c441a-199">b.</span><span class="sxs-lookup"><span data-stu-id="c441a-199">b.</span></span> <span data-ttu-id="c441a-200">I hello **användarnamn** textruta typen hello **e-postadress** av Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c441a-200">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="c441a-201">c.</span><span class="sxs-lookup"><span data-stu-id="c441a-201">c.</span></span> <span data-ttu-id="c441a-202">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c441a-202">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c441a-203">d.</span><span class="sxs-lookup"><span data-stu-id="c441a-203">d.</span></span> <span data-ttu-id="c441a-204">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c441a-204">Click **Create**.</span></span>
 
### <a name="creating-a-cerner-central-test-user"></a><span data-ttu-id="c441a-205">Skapa en testanvändare Cerner Central</span><span class="sxs-lookup"><span data-stu-id="c441a-205">Creating a Cerner Central test user</span></span>

<span data-ttu-id="c441a-206">**Cerner Central** programmet tillåter autentisering från valfri provider som federerad identitet.</span><span class="sxs-lookup"><span data-stu-id="c441a-206">**Cerner Central** application allows authentication from any federated identity provider.</span></span> <span data-ttu-id="c441a-207">Om en användare kan toolog i toohello programmets startsida är federerad och har inte behov av manuell etablering.</span><span class="sxs-lookup"><span data-stu-id="c441a-207">If a user is able toolog in toohello application home page, they are federated and have no need for any manual provisioning.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c441a-208">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="c441a-208">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c441a-209">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCerner Central.</span><span class="sxs-lookup"><span data-stu-id="c441a-209">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCerner Central.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="c441a-211">**tooassign Britta Simon tooCerner Central, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="c441a-211">**tooassign Britta Simon tooCerner Central, perform hello following steps:**</span></span>

1. <span data-ttu-id="c441a-212">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c441a-212">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c441a-214">Välj i listan med program hello **Cerner Central**.</span><span class="sxs-lookup"><span data-stu-id="c441a-214">In hello applications list, select **Cerner Central**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cernercentral-tutorial/tutorial_cernercentral_app.png) 

3. <span data-ttu-id="c441a-216">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c441a-216">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="c441a-218">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c441a-218">Click **Add** button.</span></span> <span data-ttu-id="c441a-219">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c441a-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="c441a-221">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="c441a-221">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c441a-222">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c441a-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c441a-223">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c441a-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c441a-224">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c441a-224">Testing single sign-on</span></span>

<span data-ttu-id="c441a-225">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c441a-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c441a-226">Du bör få automatiskt inloggade tooyour Cerner Central programmet när du klickar på hello Cerner Central panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c441a-226">When you click hello Cerner Central tile in hello Access Panel, you should get automatically signed-on tooyour Cerner Central application.</span></span> <span data-ttu-id="c441a-227">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c441a-227">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c441a-228">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c441a-228">Additional resources</span></span>

* [<span data-ttu-id="c441a-229">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c441a-229">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c441a-230">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c441a-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cernercentral-tutorial/tutorial_general_203.png

