---
title: "Självstudier: Azure Active Directory-integrering med Proofpoint på begäran | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Proofpoint på begäran."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 773e7f7d-ec31-411b-860d-6a6633335d43
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: 0f9472ddc01f2c18ffc9e8d2b59a17b3b595515e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-proofpoint-on-demand"></a><span data-ttu-id="d80d2-103">Självstudier: Azure Active Directory-integrering med Proofpoint på begäran</span><span class="sxs-lookup"><span data-stu-id="d80d2-103">Tutorial: Azure Active Directory integration with Proofpoint on Demand</span></span>

<span data-ttu-id="d80d2-104">I kursen får du lära dig hur toointegrate Proofpoint på begäran med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d80d2-104">In this tutorial, you learn how toointegrate Proofpoint on Demand with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d80d2-105">Integrera Proofpoint på begäran med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d80d2-105">Integrating Proofpoint on Demand with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d80d2-106">Du kan styra i Azure AD som har åtkomst till tooProofpoint på begäran</span><span class="sxs-lookup"><span data-stu-id="d80d2-106">You can control in Azure AD who has access tooProofpoint on Demand</span></span>
- <span data-ttu-id="d80d2-107">Du kan aktivera din användare tooautomatically get inloggade tooProofpoint på begäran (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d80d2-107">You can enable your users tooautomatically get signed-on tooProofpoint on Demand (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d80d2-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d80d2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d80d2-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d80d2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d80d2-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d80d2-110">Prerequisites</span></span>

<span data-ttu-id="d80d2-111">tooconfigure Azure AD-integrering med Proofpoint på begäran måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="d80d2-111">tooconfigure Azure AD integration with Proofpoint on Demand, you need hello following items:</span></span>

- <span data-ttu-id="d80d2-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d80d2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d80d2-113">En Proofpoint på begäran enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="d80d2-113">A Proofpoint on Demand single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d80d2-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d80d2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d80d2-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d80d2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d80d2-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d80d2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d80d2-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d80d2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d80d2-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d80d2-118">Scenario description</span></span>
<span data-ttu-id="d80d2-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d80d2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d80d2-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d80d2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d80d2-121">Att lägga till Proofpoint på begäran från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d80d2-121">Adding Proofpoint on Demand from hello gallery</span></span>
2. <span data-ttu-id="d80d2-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d80d2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-proofpoint-on-demand-from-hello-gallery"></a><span data-ttu-id="d80d2-123">Att lägga till Proofpoint på begäran från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d80d2-123">Adding Proofpoint on Demand from hello gallery</span></span>
<span data-ttu-id="d80d2-124">tooconfigure hello integrering av Proofpoint på begäran i Azure AD, behöver du tooadd Proofpoint på begäran från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="d80d2-124">tooconfigure hello integration of Proofpoint on Demand into Azure AD, you need tooadd Proofpoint on Demand from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d80d2-125">**tooadd Proofpoint på begäran från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d80d2-125">**tooadd Proofpoint on Demand from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d80d2-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d80d2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d80d2-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d80d2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d80d2-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="d80d2-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d80d2-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d80d2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d80d2-133">Skriv i sökrutan hello **Proofpoint på begäran**.</span><span class="sxs-lookup"><span data-stu-id="d80d2-133">In hello search box, type **Proofpoint on Demand**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_search.png)

5. <span data-ttu-id="d80d2-135">Markera hello resultat på panelen **Proofpoint på begäran**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="d80d2-135">In hello results panel, select **Proofpoint on Demand**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d80d2-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d80d2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d80d2-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Proofpoint på begäran baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d80d2-138">In this section, you configure and test Azure AD single sign-on with Proofpoint on Demand based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d80d2-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Proofpoint på begäran är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d80d2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Proofpoint on Demand is tooa user in Azure AD.</span></span> <span data-ttu-id="d80d2-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Proofpoint på begäran toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="d80d2-140">In other words, a link relationship between an Azure AD user and hello related user in Proofpoint on Demand needs toobe established.</span></span>

<span data-ttu-id="d80d2-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Proofpoint på begäran.</span><span class="sxs-lookup"><span data-stu-id="d80d2-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Proofpoint on Demand.</span></span>

<span data-ttu-id="d80d2-142">tooconfigure och testa Azure AD enkel inloggning med Proofpoint på begäran måste toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d80d2-142">tooconfigure and test Azure AD single sign-on with Proofpoint on Demand, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d80d2-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d80d2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d80d2-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d80d2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d80d2-145">**[Skapa en Proofpoint på begäran testanvändare](#creating-a-proofpoint-on-demand-test-user)**  -toohave en motsvarighet för Britta Simon i Proofpoint på begäran som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d80d2-145">**[Creating a Proofpoint on Demand test user](#creating-a-proofpoint-on-demand-test-user)** - toohave a counterpart of Britta Simon in Proofpoint on Demand that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d80d2-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d80d2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d80d2-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d80d2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d80d2-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d80d2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d80d2-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din Proofpoint på begäran-programmet.</span><span class="sxs-lookup"><span data-stu-id="d80d2-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Proofpoint on Demand application.</span></span>

<span data-ttu-id="d80d2-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Proofpoint på begäran:**</span><span class="sxs-lookup"><span data-stu-id="d80d2-150">**tooconfigure Azure AD single sign-on with Proofpoint on Demand, perform hello following steps:**</span></span>

1. <span data-ttu-id="d80d2-151">I hello Azure-portalen på hello **Proofpoint på begäran** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d80d2-151">In hello Azure portal, on hello **Proofpoint on Demand** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d80d2-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d80d2-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
  
    ![Konfigurera enkel inloggning](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_samlbase.png)

3. <span data-ttu-id="d80d2-155">På hello **Proofpoint på begäran domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d80d2-155">On hello **Proofpoint on Demand Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_url.png)

    <span data-ttu-id="d80d2-157">a.In hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<hostname>.pphosted.com/ppssamlsp_hostname`</span><span class="sxs-lookup"><span data-stu-id="d80d2-157">a.In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<hostname>.pphosted.com/ppssamlsp_hostname`</span></span>

    <span data-ttu-id="d80d2-158">b.</span><span class="sxs-lookup"><span data-stu-id="d80d2-158">b.</span></span> <span data-ttu-id="d80d2-159">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<hostname>.pphosted.com/ppssamlsp`</span><span class="sxs-lookup"><span data-stu-id="d80d2-159">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<hostname>.pphosted.com/ppssamlsp`</span></span>

    <span data-ttu-id="d80d2-160">c.</span><span class="sxs-lookup"><span data-stu-id="d80d2-160">c.</span></span>  <span data-ttu-id="d80d2-161">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<hostname>.pphosted.com:portnumber/v1/samlauth/samlconsumer`</span><span class="sxs-lookup"><span data-stu-id="d80d2-161">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<hostname>.pphosted.com:portnumber/v1/samlauth/samlconsumer`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="d80d2-162">Dessa värden är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="d80d2-162">These values are not hello real.</span></span> <span data-ttu-id="d80d2-163">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="d80d2-163">Update these values with hello actual Identifier, Reply URL and Sign-On URL.</span></span> <span data-ttu-id="d80d2-164">Kontakta [Proofpoint på begäran klienten supportteamet](https://www.proofpoint.com/us/support-services) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="d80d2-164">Contact [Proofpoint on Demand Client support team](https://www.proofpoint.com/us/support-services) tooget these values.</span></span> 

4. <span data-ttu-id="d80d2-165">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="d80d2-165">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_certificate.png) 

5. <span data-ttu-id="d80d2-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d80d2-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="d80d2-169">På hello **Proofpoint på begäran-konfiguration** klickar du på **konfigurera Proofpoint på begäran** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="d80d2-169">On hello **Proofpoint on Demand Configuration** section, click **Configure Proofpoint on Demand** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d80d2-170">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="d80d2-170">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_configure.png) 

7. <span data-ttu-id="d80d2-172">tooconfigure enkel inloggning på **Proofpoint på begäran** sida, behöver du toosend hello hämtas **Certificate(Base64)**,**SAML enhets-ID**, och **SAML Enkel inloggning Tjänstwebbadress** för[Proofpoint på begäran klienten supportteamet](https://www.proofpoint.com/us/support-services).</span><span class="sxs-lookup"><span data-stu-id="d80d2-172">tooconfigure single sign-on on **Proofpoint on Demand** side, you need toosend hello downloaded **Certificate(Base64)**,**SAML Entity ID**, and **SAML Single Sign-On Service URL** too[Proofpoint on Demand Client support team](https://www.proofpoint.com/us/support-services).</span></span>

> [!TIP]
> <span data-ttu-id="d80d2-173">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="d80d2-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d80d2-174">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="d80d2-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d80d2-175">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d80d2-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d80d2-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d80d2-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="d80d2-177">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d80d2-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d80d2-179">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d80d2-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d80d2-180">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d80d2-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d80d2-182">Dessa värden är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="d80d2-182">These values are not hello real.</span></span> <span data-ttu-id="d80d2-183">Uppdatera dessa värden med hello faktiska</span><span class="sxs-lookup"><span data-stu-id="d80d2-183">Update these values with hello actual</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d80d2-185">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d80d2-185">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d80d2-187">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d80d2-187">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d80d2-189">a.</span><span class="sxs-lookup"><span data-stu-id="d80d2-189">a.</span></span> <span data-ttu-id="d80d2-190">I hello **namn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="d80d2-190">In hello **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="d80d2-191">b.</span><span class="sxs-lookup"><span data-stu-id="d80d2-191">b.</span></span> <span data-ttu-id="d80d2-192">I hello **användarnamn** textruta typen hello **e-postadress** av Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d80d2-192">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="d80d2-193">c.</span><span class="sxs-lookup"><span data-stu-id="d80d2-193">c.</span></span> <span data-ttu-id="d80d2-194">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d80d2-194">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d80d2-195">d.</span><span class="sxs-lookup"><span data-stu-id="d80d2-195">d.</span></span> <span data-ttu-id="d80d2-196">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d80d2-196">Click **Create**.</span></span>
 
### <a name="creating-a-proofpoint-on-demand-test-user"></a><span data-ttu-id="d80d2-197">Skapa en Proofpoint på begäran testanvändare</span><span class="sxs-lookup"><span data-stu-id="d80d2-197">Creating a Proofpoint on Demand test user</span></span>

<span data-ttu-id="d80d2-198">I det här avsnittet skapar du en användare som kallas Britta Simon i Proofpoint på begäran.</span><span class="sxs-lookup"><span data-stu-id="d80d2-198">In this section, you create a user called Britta Simon in Proofpoint on Demand.</span></span> <span data-ttu-id="d80d2-199">Arbeta med [Proofpoint på begäran klienten supportteamet](https://www.proofpoint.com/us/support-services) tooadd användare i hello Proofpoint på begäran-plattformen.</span><span class="sxs-lookup"><span data-stu-id="d80d2-199">Work with [Proofpoint on Demand Client support team](https://www.proofpoint.com/us/support-services) tooadd users in hello Proofpoint on Demand platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d80d2-200">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="d80d2-200">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d80d2-201">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooProofpoint på begäran.</span><span class="sxs-lookup"><span data-stu-id="d80d2-201">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooProofpoint on Demand.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d80d2-203">**tooassign Britta Simon tooProofpoint på begäran, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="d80d2-203">**tooassign Britta Simon tooProofpoint on Demand, perform hello following steps:**</span></span>

1. <span data-ttu-id="d80d2-204">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d80d2-204">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d80d2-206">Välj i listan med program hello **Proofpoint på begäran**.</span><span class="sxs-lookup"><span data-stu-id="d80d2-206">In hello applications list, select **Proofpoint on Demand**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_app.png) 

3. <span data-ttu-id="d80d2-208">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d80d2-208">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d80d2-210">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d80d2-210">Click **Add** button.</span></span> <span data-ttu-id="d80d2-211">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d80d2-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d80d2-213">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="d80d2-213">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d80d2-214">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d80d2-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d80d2-215">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d80d2-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d80d2-216">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d80d2-216">Testing single sign-on</span></span>

<span data-ttu-id="d80d2-217">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d80d2-217">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d80d2-218">När du klickar på hello **Proofpoint på begäran** panelen på hello åtkomstpanelen, bör vara loggas du automatiskt på tooyour Proofpoint på begäran-programmet.</span><span class="sxs-lookup"><span data-stu-id="d80d2-218">When you click hello **Proofpoint on Demand** tile on hello Access Panel, you should be automatically signed on tooyour Proofpoint on Demand application.</span></span>
<span data-ttu-id="d80d2-219">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d80d2-219">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>  

## <a name="additional-resources"></a><span data-ttu-id="d80d2-220">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d80d2-220">Additional resources</span></span>

* [<span data-ttu-id="d80d2-221">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d80d2-221">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d80d2-222">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d80d2-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_203.png

