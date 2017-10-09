---
title: "Självstudier: Azure Active Directory-integrering med FreshGrade | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och FreshGrade."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1055bba6-f4df-462e-bc9b-1ad5ada0f638
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: e2fe7cedd45290945ec5624453a9675abdd7726d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshgrade"></a><span data-ttu-id="f8b80-103">Självstudier: Azure Active Directory-integrering med FreshGrade</span><span class="sxs-lookup"><span data-stu-id="f8b80-103">Tutorial: Azure Active Directory integration with FreshGrade</span></span>

<span data-ttu-id="f8b80-104">I kursen får du lära dig hur toointegrate FreshGrade med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="f8b80-104">In this tutorial, you learn how toointegrate FreshGrade with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f8b80-105">Integrera FreshGrade med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f8b80-105">Integrating FreshGrade with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f8b80-106">Du kan styra i Azure AD som har åtkomst till tooFreshGrade</span><span class="sxs-lookup"><span data-stu-id="f8b80-106">You can control in Azure AD who has access tooFreshGrade</span></span>
- <span data-ttu-id="f8b80-107">Du kan aktivera din användare tooautomatically get inloggade tooFreshGrade (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="f8b80-107">You can enable your users tooautomatically get signed-on tooFreshGrade (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f8b80-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f8b80-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f8b80-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f8b80-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8b80-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f8b80-110">Prerequisites</span></span>

<span data-ttu-id="f8b80-111">tooconfigure Azure AD-integrering med FreshGrade, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="f8b80-111">tooconfigure Azure AD integration with FreshGrade, you need hello following items:</span></span>

- <span data-ttu-id="f8b80-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f8b80-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f8b80-113">En FreshGrade enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="f8b80-113">A FreshGrade single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f8b80-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f8b80-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f8b80-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f8b80-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f8b80-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f8b80-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f8b80-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f8b80-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f8b80-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="f8b80-118">Scenario description</span></span>
<span data-ttu-id="f8b80-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="f8b80-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f8b80-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="f8b80-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f8b80-121">Att lägga till FreshGrade från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f8b80-121">Adding FreshGrade from hello gallery</span></span>
2. <span data-ttu-id="f8b80-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f8b80-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-freshgrade-from-hello-gallery"></a><span data-ttu-id="f8b80-123">Att lägga till FreshGrade från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f8b80-123">Adding FreshGrade from hello gallery</span></span>
<span data-ttu-id="f8b80-124">tooconfigure hello integrering av FreshGrade i Azure AD, behöver du tooadd FreshGrade hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="f8b80-124">tooconfigure hello integration of FreshGrade into Azure AD, you need tooadd FreshGrade from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f8b80-125">**tooadd FreshGrade från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f8b80-125">**tooadd FreshGrade from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f8b80-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f8b80-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f8b80-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f8b80-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f8b80-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="f8b80-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="f8b80-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f8b80-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="f8b80-133">Skriv i sökrutan hello **FreshGrade**.</span><span class="sxs-lookup"><span data-stu-id="f8b80-133">In hello search box, type **FreshGrade**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_search.png)

5. <span data-ttu-id="f8b80-135">Markera hello resultat på panelen **FreshGrade**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="f8b80-135">In hello results panel, select **FreshGrade**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f8b80-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f8b80-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f8b80-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med FreshGrade baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="f8b80-138">In this section, you configure and test Azure AD single sign-on with FreshGrade based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f8b80-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i FreshGrade är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f8b80-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in FreshGrade is tooa user in Azure AD.</span></span> <span data-ttu-id="f8b80-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i FreshGrade toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="f8b80-140">In other words, a link relationship between an Azure AD user and hello related user in FreshGrade needs toobe established.</span></span>

<span data-ttu-id="f8b80-141">I FreshGrade, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="f8b80-141">In FreshGrade, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f8b80-142">tooconfigure och testa Azure AD enkel inloggning med FreshGrade, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="f8b80-142">tooconfigure and test Azure AD single sign-on with FreshGrade, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f8b80-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f8b80-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f8b80-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f8b80-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f8b80-145">**[Skapa en testanvändare FreshGrade](#creating-a-freshgrade-test-user)**  -toohave en motsvarighet för Britta Simon i FreshGrade som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="f8b80-145">**[Creating a FreshGrade test user](#creating-a-freshgrade-test-user)** - toohave a counterpart of Britta Simon in FreshGrade that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f8b80-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f8b80-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f8b80-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f8b80-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f8b80-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f8b80-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f8b80-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt FreshGrade program.</span><span class="sxs-lookup"><span data-stu-id="f8b80-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your FreshGrade application.</span></span>

<span data-ttu-id="f8b80-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med FreshGrade:**</span><span class="sxs-lookup"><span data-stu-id="f8b80-150">**tooconfigure Azure AD single sign-on with FreshGrade, perform hello following steps:**</span></span>

1. <span data-ttu-id="f8b80-151">I hello Azure-portalen på hello **FreshGrade** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f8b80-151">In hello Azure portal, on hello **FreshGrade** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="f8b80-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f8b80-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_samlbase.png)

3. <span data-ttu-id="f8b80-155">På hello **FreshGrade domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f8b80-155">On hello **FreshGrade Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_url.png)

    <span data-ttu-id="f8b80-157">a.</span><span class="sxs-lookup"><span data-stu-id="f8b80-157">a.</span></span> <span data-ttu-id="f8b80-158">I hello **inloggnings-URL** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="f8b80-158">In hello **Sign-on URL** textbox, type a URL using hello following patterns:</span></span> 
      | |
      |--|
      | `https://<subdomain>.freshgrade.com/login` |    
      | `https://<subdomain>.onboarding.freshgrade.com/login` |

    <span data-ttu-id="f8b80-159">b.</span><span class="sxs-lookup"><span data-stu-id="f8b80-159">b.</span></span> <span data-ttu-id="f8b80-160">I hello **identifierare** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="f8b80-160">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span> 
      | |
      |--|
      | `https://login.onboarding.freshgrade.com:443/saml/metadata/alias/<instancename>` |      
      | `https://login.freshgrade.com:443/saml/metadata/alias/<instancename>` |

    > [!NOTE] 
    > <span data-ttu-id="f8b80-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="f8b80-161">These values are not real.</span></span> <span data-ttu-id="f8b80-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="f8b80-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f8b80-163">Kontakta [FreshGrade klienten supportteamet](mailTo:support@freshgrade.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="f8b80-163">Contact [FreshGrade Client support team](mailTo:support@freshgrade.com) tooget these values.</span></span> 
 


4. <span data-ttu-id="f8b80-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="f8b80-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_certificate.png) 

5. <span data-ttu-id="f8b80-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f8b80-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshgrade-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f8b80-168">På hello **FreshGrade Configuration** klickar du på **konfigurera FreshGrade** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="f8b80-168">On hello **FreshGrade Configuration** section, click **Configure FreshGrade** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="f8b80-169">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="f8b80-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_configure.png) 

7. <span data-ttu-id="f8b80-171">toogenerate hello **Metadata** url, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f8b80-171">toogenerate hello **Metadata** url, perform hello following steps:</span></span>

    <span data-ttu-id="f8b80-172">a.</span><span class="sxs-lookup"><span data-stu-id="f8b80-172">a.</span></span> <span data-ttu-id="f8b80-173">Klicka på **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="f8b80-173">Click **App registrations**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_appregistrations.png)
   
    <span data-ttu-id="f8b80-175">b.</span><span class="sxs-lookup"><span data-stu-id="f8b80-175">b.</span></span> <span data-ttu-id="f8b80-176">Klicka på **slutpunkter** tooopen **slutpunkter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f8b80-176">Click **Endpoints** tooopen **Endpoints** dialog box.</span></span>  
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_endpointicon.png)

    <span data-ttu-id="f8b80-178">c.</span><span class="sxs-lookup"><span data-stu-id="f8b80-178">c.</span></span> <span data-ttu-id="f8b80-179">Klicka på hello Kopiera knappen toocopy **FEDERATION METADATADOKUMENTET** url och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="f8b80-179">Click hello copy button toocopy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_endpoint.png)
     
    <span data-ttu-id="f8b80-181">d.</span><span class="sxs-lookup"><span data-stu-id="f8b80-181">d.</span></span> <span data-ttu-id="f8b80-182">Gå nu toohello egenskapssida **FreshGrade** och kopiera hello **program-Id** med **kopiera** knappen och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="f8b80-182">Now go toohello property page of **FreshGrade** and copy hello **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_appid.png)

    <span data-ttu-id="f8b80-184">e.</span><span class="sxs-lookup"><span data-stu-id="f8b80-184">e.</span></span> <span data-ttu-id="f8b80-185">Generera hello **URL för tjänstmetadata** med hello följande mönster:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="f8b80-185">Generate hello **Metadata URL** using hello following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>

8. <span data-ttu-id="f8b80-186">tooconfigure enkel inloggning på **FreshGrade** sida, behöver du toosend hello **URL för tjänstmetadata** och **SAML enkel inloggning Tjänstwebbadress** för[FreshGrade supportteam](mailTo:support@freshgrade.com).</span><span class="sxs-lookup"><span data-stu-id="f8b80-186">tooconfigure single sign-on on **FreshGrade** side, you need toosend hello **Metadata URL** and **SAML Single Sign-On Service URL** too[FreshGrade support team](mailTo:support@freshgrade.com).</span></span> <span data-ttu-id="f8b80-187">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="f8b80-187">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="f8b80-188">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="f8b80-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f8b80-189">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="f8b80-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f8b80-190">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f8b80-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f8b80-191">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f8b80-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="f8b80-192">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f8b80-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="f8b80-194">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f8b80-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f8b80-195">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f8b80-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshgrade-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f8b80-197">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f8b80-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshgrade-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f8b80-199">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f8b80-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshgrade-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f8b80-201">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f8b80-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshgrade-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f8b80-203">a.</span><span class="sxs-lookup"><span data-stu-id="f8b80-203">a.</span></span> <span data-ttu-id="f8b80-204">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f8b80-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f8b80-205">b.</span><span class="sxs-lookup"><span data-stu-id="f8b80-205">b.</span></span> <span data-ttu-id="f8b80-206">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f8b80-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f8b80-207">c.</span><span class="sxs-lookup"><span data-stu-id="f8b80-207">c.</span></span> <span data-ttu-id="f8b80-208">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="f8b80-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f8b80-209">d.</span><span class="sxs-lookup"><span data-stu-id="f8b80-209">d.</span></span> <span data-ttu-id="f8b80-210">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f8b80-210">Click **Create**.</span></span>
 
### <a name="creating-a-freshgrade-test-user"></a><span data-ttu-id="f8b80-211">Skapa en testanvändare FreshGrade</span><span class="sxs-lookup"><span data-stu-id="f8b80-211">Creating a FreshGrade test user</span></span>

<span data-ttu-id="f8b80-212">I det här avsnittet skapar du en användare som kallas Britta Simon i FreshGrade.</span><span class="sxs-lookup"><span data-stu-id="f8b80-212">In this section, you create a user called Britta Simon in FreshGrade.</span></span> <span data-ttu-id="f8b80-213">Se tillsammans med [FreshGrade supportteamet](mailTo:support@freshgrade.com) tooadd hello användare i hello FreshGrade plattform.</span><span class="sxs-lookup"><span data-stu-id="f8b80-213">Please work with [FreshGrade support team](mailTo:support@freshgrade.com) tooadd hello users in hello FreshGrade platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f8b80-214">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="f8b80-214">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f8b80-215">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooFreshGrade.</span><span class="sxs-lookup"><span data-stu-id="f8b80-215">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFreshGrade.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="f8b80-217">**tooassign Britta Simon tooFreshGrade utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f8b80-217">**tooassign Britta Simon tooFreshGrade, perform hello following steps:**</span></span>

1. <span data-ttu-id="f8b80-218">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f8b80-218">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="f8b80-220">Välj i listan med program hello **FreshGrade**.</span><span class="sxs-lookup"><span data-stu-id="f8b80-220">In hello applications list, select **FreshGrade**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshgrade-tutorial/tutorial_freshgrade_app.png) 

3. <span data-ttu-id="f8b80-222">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f8b80-222">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="f8b80-224">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="f8b80-224">Click **Add** button.</span></span> <span data-ttu-id="f8b80-225">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f8b80-225">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="f8b80-227">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="f8b80-227">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f8b80-228">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f8b80-228">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f8b80-229">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f8b80-229">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f8b80-230">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f8b80-230">Testing single sign-on</span></span>

<span data-ttu-id="f8b80-231">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f8b80-231">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f8b80-232">Du bör få automatiskt inloggade tooyour FreshGrade programmet när du klickar på hello FreshGrade panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f8b80-232">When you click hello FreshGrade tile in hello Access Panel, you should get automatically signed-on tooyour FreshGrade application.</span></span>
<span data-ttu-id="f8b80-233">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f8b80-233">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8b80-234">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f8b80-234">Additional resources</span></span>

* [<span data-ttu-id="f8b80-235">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f8b80-235">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f8b80-236">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f8b80-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshgrade-tutorial/tutorial_general_203.png

