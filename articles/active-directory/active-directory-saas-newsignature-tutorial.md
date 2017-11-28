---
title: "Självstudier: Azure Active Directory-integrering med molnet för Microsoft Azure-hanteringsportalen | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och molnet hanteringsportal för Microsoft Azure."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4ea9f47c-25ca-42b0-a878-9e7aa6f34973
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 9596826e3dc1289b95009bf01ec8b86f823ef345
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cloud-management-portal-for-microsoft-azure"></a><span data-ttu-id="be430-103">Självstudier: Azure Active Directory-integrering med molntjänster Management Portal för Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="be430-103">Tutorial: Azure Active Directory integration with Cloud Management Portal for Microsoft Azure</span></span>

<span data-ttu-id="be430-104">I kursen får du lära dig hur toointegrate moln hanteringsportal för Microsoft Azure med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="be430-104">In this tutorial, you learn how toointegrate Cloud Management Portal for Microsoft Azure with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="be430-105">Integrera molnet hanteringsportal för Microsoft Azure med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="be430-105">Integrating Cloud Management Portal for Microsoft Azure with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="be430-106">Du kan styra i Azure AD som har åtkomst tooCloud hanteringsportal för Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="be430-106">You can control in Azure AD who has access tooCloud Management Portal for Microsoft Azure</span></span>
- <span data-ttu-id="be430-107">Du kan aktivera din användare tooautomatically get inloggade tooCloud hanteringsportal för Microsoft Azure (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="be430-107">You can enable your users tooautomatically get signed-on tooCloud Management Portal for Microsoft Azure (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="be430-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="be430-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="be430-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="be430-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="be430-110">Krav</span><span class="sxs-lookup"><span data-stu-id="be430-110">Prerequisites</span></span>

<span data-ttu-id="be430-111">tooconfigure Azure AD-integrering med molntjänster hanteringsportal för Microsoft Azure, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="be430-111">tooconfigure Azure AD integration with Cloud Management Portal for Microsoft Azure, you need hello following items:</span></span>

- <span data-ttu-id="be430-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="be430-112">An Azure AD subscription</span></span>
- <span data-ttu-id="be430-113">En molnet Management Portal för Microsoft Azure enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="be430-113">A Cloud Management Portal for Microsoft Azure single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="be430-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="be430-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="be430-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="be430-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="be430-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="be430-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="be430-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="be430-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="be430-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="be430-118">Scenario description</span></span>
<span data-ttu-id="be430-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="be430-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="be430-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="be430-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="be430-121">Att lägga till molnet hanteringsportal för Microsoft Azure från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="be430-121">Adding Cloud Management Portal for Microsoft Azure from hello gallery</span></span>
2. <span data-ttu-id="be430-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="be430-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cloud-management-portal-for-microsoft-azure-from-hello-gallery"></a><span data-ttu-id="be430-123">Att lägga till molnet hanteringsportal för Microsoft Azure från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="be430-123">Adding Cloud Management Portal for Microsoft Azure from hello gallery</span></span>
<span data-ttu-id="be430-124">tooconfigure hello integrering av molnet hanteringsportal för Microsoft Azure i Azure AD, behöver du tooadd moln Management Portal för Microsoft Azure från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="be430-124">tooconfigure hello integration of Cloud Management Portal for Microsoft Azure into Azure AD, you need tooadd Cloud Management Portal for Microsoft Azure from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="be430-125">**tooadd moln hanteringsportal för Microsoft Azure från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="be430-125">**tooadd Cloud Management Portal for Microsoft Azure from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="be430-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="be430-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="be430-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="be430-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="be430-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="be430-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="be430-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be430-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="be430-133">Skriv i sökrutan hello **molnet för Microsoft Azure-hanteringsportalen**.</span><span class="sxs-lookup"><span data-stu-id="be430-133">In hello search box, type **Cloud Management Portal for Microsoft Azure**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_search.png)

5. <span data-ttu-id="be430-135">Markera hello resultat på panelen **molnet för Microsoft Azure-hanteringsportalen**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="be430-135">In hello results panel, select **Cloud Management Portal for Microsoft Azure**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="be430-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="be430-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="be430-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med molnet hanteringsportal för Microsoft Azure baserad på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="be430-138">In this section, you configure and test Azure AD single sign-on with Cloud Management Portal for Microsoft Azure based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="be430-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i molnet Management Portal för Microsoft Azure är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="be430-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Cloud Management Portal for Microsoft Azure is tooa user in Azure AD.</span></span> <span data-ttu-id="be430-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användaren i molnet för Microsoft Azure-hanteringsportalen toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="be430-140">In other words, a link relationship between an Azure AD user and hello related user in Cloud Management Portal for Microsoft Azure needs toobe established.</span></span>

<span data-ttu-id="be430-141">I molnet Management Portal för Microsoft Azure, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="be430-141">In Cloud Management Portal for Microsoft Azure, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="be430-142">tooconfigure och testa Azure AD enkel inloggning med molnet Management Portal för Microsoft Azure, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="be430-142">tooconfigure and test Azure AD single sign-on with Cloud Management Portal for Microsoft Azure, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="be430-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="be430-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="be430-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="be430-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="be430-145">**[Skapa en moln Management Portal för Microsoft Azure testanvändare](#creating-a-cloud-management-portal-for-microsoft-azure-test-user)**  -toohave en motsvarighet för Britta Simon i molnet Management Portal för Microsoft Azure som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="be430-145">**[Creating a Cloud Management Portal for Microsoft Azure test user](#creating-a-cloud-management-portal-for-microsoft-azure-test-user)** - toohave a counterpart of Britta Simon in Cloud Management Portal for Microsoft Azure that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="be430-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="be430-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="be430-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="be430-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="be430-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="be430-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="be430-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i molnet Management-portalen för Microsoft Azure-program.</span><span class="sxs-lookup"><span data-stu-id="be430-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Cloud Management Portal for Microsoft Azure application.</span></span>

<span data-ttu-id="be430-150">**tooconfigure Azure AD enkel inloggning med molnet Management Portal för Microsoft Azure utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="be430-150">**tooconfigure Azure AD single sign-on with Cloud Management Portal for Microsoft Azure, perform hello following steps:**</span></span>

1. <span data-ttu-id="be430-151">I hello Azure-portalen på hello **molnet för Microsoft Azure-hanteringsportalen** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="be430-151">In hello Azure portal, on hello **Cloud Management Portal for Microsoft Azure** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="be430-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="be430-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_samlbase.png)

3. <span data-ttu-id="be430-155">På hello **moln hanteringsportal för URL: er och Microsoft Azure Domain** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="be430-155">On hello **Cloud Management Portal for Microsoft Azure Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_url.png)

    <span data-ttu-id="be430-157">a.</span><span class="sxs-lookup"><span data-stu-id="be430-157">a.</span></span> <span data-ttu-id="be430-158">I hello **inloggnings-URL** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="be430-158">In hello **Sign-on URL** textbox, type a URL using hello following patterns:</span></span> 
    
    | |
    |--|
    | `https://portal.newsignature.com/<instancename>` |   
    | `https://portal.igcm.com/<instancename>` |
    
    <span data-ttu-id="be430-159">b.</span><span class="sxs-lookup"><span data-stu-id="be430-159">b.</span></span> <span data-ttu-id="be430-160">I hello **identifierare** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="be430-160">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span> 
    
    | |
    |--|
    | `https://<subdomain>.igcm.com` |
    | `https://<subdomain>.newsignature.com` |

    <span data-ttu-id="be430-161">c.</span><span class="sxs-lookup"><span data-stu-id="be430-161">c.</span></span> <span data-ttu-id="be430-162">I hello **Reply URL** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="be430-162">In hello **Reply URL** textbox, type a URL using hello following patterns:</span></span> 
    
    | |
    |--|
    | `https://<subdomain>.igcm.com/<instancename>` |
    | `https://<subdomain>.newsignature.com` |
    | `https://<subdomain>.newsignature.com/<instancename>` |

    > [!NOTE] 
    > <span data-ttu-id="be430-163">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="be430-163">These values are not real.</span></span> <span data-ttu-id="be430-164">Uppdatera dessa värden med hello faktiska inloggnings-URL, identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="be430-164">Update these values with hello actual Sign-On URL, Identifier and Reply URL.</span></span> <span data-ttu-id="be430-165">Kontakta [moln Management Portal för Microsoft Azure-klient supportteamet](mailto:jczernuszka@newsignature.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="be430-165">Contact [Cloud Management Portal for Microsoft Azure Client support team](mailto:jczernuszka@newsignature.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="be430-166">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="be430-166">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_certificate.png) 

5. <span data-ttu-id="be430-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="be430-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-newsignature-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="be430-170">På hello **moln hanteringsportal för Microsoft Azure Configuration** klickar du på **konfigurera Portal för molnet för Microsoft Azure** tooopen **konfigurera inloggning**fönster.</span><span class="sxs-lookup"><span data-stu-id="be430-170">On hello **Cloud Management Portal for Microsoft Azure Configuration** section, click **Configure Cloud Management Portal for Microsoft Azure** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="be430-171">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="be430-171">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_configure.png) 

7. <span data-ttu-id="be430-173">tooconfigure enkel inloggning på **molnet för Microsoft Azure-hanteringsportalen** sida, behöver du toosend hello hämtas **certifikat**, **Sign-Out URL**, **SAML enkel inloggning Tjänstwebbadress** och **SAML enhets-ID** för[moln hanteringsportal för Microsoft Azure-teamet](mailto:jczernuszka@newsignature.com).</span><span class="sxs-lookup"><span data-stu-id="be430-173">tooconfigure single sign-on on **Cloud Management Portal for Microsoft Azure** side, you need toosend hello downloaded **Certificate**, **Sign-Out URL**, **SAML Single Sign-On Service URL** and **SAML Entity ID** too[Cloud Management Portal for Microsoft Azure support team](mailto:jczernuszka@newsignature.com).</span></span> <span data-ttu-id="be430-174">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="be430-174">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="be430-175">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="be430-175">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="be430-176">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="be430-176">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="be430-177">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="be430-177">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="be430-178">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="be430-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="be430-179">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="be430-179">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="be430-181">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="be430-181">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="be430-182">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="be430-182">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="be430-184">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="be430-184">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="be430-186">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be430-186">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="be430-188">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="be430-188">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-newsignature-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="be430-190">a.</span><span class="sxs-lookup"><span data-stu-id="be430-190">a.</span></span> <span data-ttu-id="be430-191">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="be430-191">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="be430-192">b.</span><span class="sxs-lookup"><span data-stu-id="be430-192">b.</span></span> <span data-ttu-id="be430-193">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="be430-193">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="be430-194">c.</span><span class="sxs-lookup"><span data-stu-id="be430-194">c.</span></span> <span data-ttu-id="be430-195">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="be430-195">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="be430-196">d.</span><span class="sxs-lookup"><span data-stu-id="be430-196">d.</span></span> <span data-ttu-id="be430-197">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="be430-197">Click **Create**.</span></span>
 
### <a name="creating-a-cloud-management-portal-for-microsoft-azure-test-user"></a><span data-ttu-id="be430-198">Skapa en moln Management Portal för Microsoft Azure testanvändare</span><span class="sxs-lookup"><span data-stu-id="be430-198">Creating a Cloud Management Portal for Microsoft Azure test user</span></span>

<span data-ttu-id="be430-199">hello syftet med det här avsnittet är toocreate en användare som heter Britta Simon i molnet Management-portalen för Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="be430-199">hello objective of this section is toocreate a user called Britta Simon in Cloud Management Portal for Microsoft Azure.</span></span> <span data-ttu-id="be430-200">Se tillsammans med [moln hanteringsportal för Microsoft Azure-teamet](mailto:jczernuszka@newsignature.com) tooadd hello användare i hello molnet Management Portal för Microsoft Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="be430-200">Please work with [Cloud Management Portal for Microsoft Azure support team](mailto:jczernuszka@newsignature.com) tooadd hello users in hello Cloud Management Portal for Microsoft Azure account.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="be430-201">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="be430-201">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="be430-202">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCloud för Microsoft Azure-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="be430-202">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCloud Management Portal for Microsoft Azure.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="be430-204">**tooassign Britta Simon tooCloud hanteringsportal för Microsoft Azure utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="be430-204">**tooassign Britta Simon tooCloud Management Portal for Microsoft Azure, perform hello following steps:**</span></span>

1. <span data-ttu-id="be430-205">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="be430-205">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="be430-207">Välj i listan med program hello **molnet för Microsoft Azure-hanteringsportalen**.</span><span class="sxs-lookup"><span data-stu-id="be430-207">In hello applications list, select **Cloud Management Portal for Microsoft Azure**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_app.png) 

3. <span data-ttu-id="be430-209">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="be430-209">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="be430-211">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="be430-211">Click **Add** button.</span></span> <span data-ttu-id="be430-212">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be430-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="be430-214">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="be430-214">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="be430-215">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be430-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="be430-216">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="be430-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="be430-217">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="be430-217">Testing single sign-on</span></span>

<span data-ttu-id="be430-218">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="be430-218">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>
<span data-ttu-id="be430-219">När du klickar på hello molnet Management Portal för Microsoft Azure-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour moln Management Portal för Microsoft Azure-program.</span><span class="sxs-lookup"><span data-stu-id="be430-219">When you click hello Cloud Management Portal for Microsoft Azure tile in hello Access Panel, you should get automatically signed-on tooyour Cloud Management Portal for Microsoft Azure application.</span></span>

<span data-ttu-id="be430-220">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="be430-220">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be430-221">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="be430-221">Additional resources</span></span>

* [<span data-ttu-id="be430-222">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="be430-222">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="be430-223">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="be430-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_203.png

