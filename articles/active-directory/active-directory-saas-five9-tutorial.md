---
title: "Självstudier: Azure Active Directory-integrering med Five9 Plus-kort (CTI, kontakta Center-agenter) | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Five9 Plus-kort (CTI, kontakta Center-agenter)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: 2e3ea8b5f2a6eaa8ad17d39e03fa490038b14561
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a><span data-ttu-id="b7642-103">Självstudier: Azure Active Directory-integrering med Five9 Plus-kort (CTI, kontakta Center-agenter)</span><span class="sxs-lookup"><span data-stu-id="b7642-103">Tutorial: Azure Active Directory integration with Five9 Plus Adapter (CTI, Contact Center Agents)</span></span>

<span data-ttu-id="b7642-104">I kursen får du lära dig hur toointegrate Five9 Plus nätverkskort (CTI, kontakta Center-agenter) med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b7642-104">In this tutorial, you learn how toointegrate Five9 Plus Adapter (CTI, Contact Center Agents) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b7642-105">Integrera Five9 Plus nätverkskort (CTI, kontakta Center-agenter) med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b7642-105">Integrating Five9 Plus Adapter (CTI, Contact Center Agents) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b7642-106">Du kan styra i Azure AD som har åtkomst tooFive9 Plus nätverkskort (CTI, kontakta Center-agenter)</span><span class="sxs-lookup"><span data-stu-id="b7642-106">You can control in Azure AD who has access tooFive9 Plus Adapter (CTI, Contact Center Agents)</span></span>
- <span data-ttu-id="b7642-107">Du kan aktivera dina användare tooautomatically get inloggade tooFive9 Plus nätverkskort (CTI, kontakta Center-agenter) (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b7642-107">You can enable your users tooautomatically get signed-on tooFive9 Plus Adapter (CTI, Contact Center Agents) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b7642-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b7642-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b7642-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b7642-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b7642-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b7642-110">Prerequisites</span></span>

<span data-ttu-id="b7642-111">tooconfigure Azure AD-integrering med Five9 Plus kort (CTI, kontakta Center-agenter), behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="b7642-111">tooconfigure Azure AD integration with Five9 Plus Adapter (CTI, Contact Center Agents), you need hello following items:</span></span>

- <span data-ttu-id="b7642-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b7642-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b7642-113">En Five9 Plus nätverkskort (CTI, kontakta Center-agenter) enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="b7642-113">A Five9 Plus Adapter (CTI, Contact Center Agents) single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b7642-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b7642-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b7642-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b7642-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b7642-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b7642-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b7642-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b7642-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b7642-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b7642-118">Scenario description</span></span>
<span data-ttu-id="b7642-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b7642-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b7642-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b7642-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b7642-121">Lägga till Five9 Plus kort (CTI, kontakta Center-agenter) från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b7642-121">Adding Five9 Plus Adapter (CTI, Contact Center Agents) from hello gallery</span></span>
2. <span data-ttu-id="b7642-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7642-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-hello-gallery"></a><span data-ttu-id="b7642-123">Lägga till Five9 Plus kort (CTI, kontakta Center-agenter) från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b7642-123">Adding Five9 Plus Adapter (CTI, Contact Center Agents) from hello gallery</span></span>
<span data-ttu-id="b7642-124">tooconfigure hello integrering av Five9 Plus nätverkskort (CTI, kontakta Center-agenter) i Azure AD, behöver du tooadd Five9 Plus nätverkskort (CTI, kontakta Center-agenter) hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="b7642-124">tooconfigure hello integration of Five9 Plus Adapter (CTI, Contact Center Agents) into Azure AD, you need tooadd Five9 Plus Adapter (CTI, Contact Center Agents) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b7642-125">**tooadd Five9 Plus nätverkskort (CTI, kontakta Center-agenter) från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b7642-125">**tooadd Five9 Plus Adapter (CTI, Contact Center Agents) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b7642-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b7642-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b7642-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="b7642-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b7642-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="b7642-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="b7642-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7642-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="b7642-133">Skriv i sökrutan hello **Five9 Plus nätverkskort (CTI, kontakta Center-agenter)**.</span><span class="sxs-lookup"><span data-stu-id="b7642-133">In hello search box, type **Five9 Plus Adapter (CTI, Contact Center Agents)**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-five9-tutorial/tutorial_five9_search.png)

5. <span data-ttu-id="b7642-135">Markera hello resultat på panelen **Five9 Plus nätverkskort (CTI, kontakta Center-agenter)**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="b7642-135">In hello results panel, select **Five9 Plus Adapter (CTI, Contact Center Agents)**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-five9-tutorial/tutorial_five9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b7642-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7642-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b7642-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Five9 Plus-kort (CTI, kontakta Center-agenter) baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b7642-138">In this section, you configure and test Azure AD single sign-on with Five9 Plus Adapter (CTI, Contact Center Agents) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b7642-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Five9 Plus nätverkskort (CTI, kontakta Center-agenter) är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b7642-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Five9 Plus Adapter (CTI, Contact Center Agents) is tooa user in Azure AD.</span></span> <span data-ttu-id="b7642-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Five9 Plus nätverkskort (CTI, kontakta Center-agenter) toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="b7642-140">In other words, a link relationship between an Azure AD user and hello related user in Five9 Plus Adapter (CTI, Contact Center Agents) needs toobe established.</span></span>

<span data-ttu-id="b7642-141">Tilldela hello värdet för hello i Five9 Plus-kort (CTI, kontakta Center-agenter) **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="b7642-141">In Five9 Plus Adapter (CTI, Contact Center Agents), assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b7642-142">tooconfigure och testa Azure AD enkel inloggning med Five9 Plus kort (CTI, kontakta Center-agenter), behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b7642-142">tooconfigure and test Azure AD single sign-on with Five9 Plus Adapter (CTI, Contact Center Agents), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b7642-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b7642-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b7642-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b7642-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b7642-145">**[Skapa en testanvändare Five9 Plus nätverkskort (CTI, kontakta Center-agenter)](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)**  -toohave en motsvarighet för Britta Simon i Five9 Plus kort (CTI, kontakta Center-agenter) som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="b7642-145">**[Creating a Five9 Plus Adapter (CTI, Contact Center Agents) test user](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)** - toohave a counterpart of Britta Simon in Five9 Plus Adapter (CTI, Contact Center Agents) that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b7642-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b7642-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b7642-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b7642-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b7642-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7642-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b7642-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Five9 Plus nätverkskort (CTI, kontakta Center-agenter).</span><span class="sxs-lookup"><span data-stu-id="b7642-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Five9 Plus Adapter (CTI, Contact Center Agents) application.</span></span>

<span data-ttu-id="b7642-150">**tooconfigure Azure AD enkel inloggning med Five9 Plus-kort (CTI, kontakta Center-agenter), utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="b7642-150">**tooconfigure Azure AD single sign-on with Five9 Plus Adapter (CTI, Contact Center Agents), perform hello following steps:**</span></span>

1. <span data-ttu-id="b7642-151">I hello Azure-portalen på hello **Five9 Plus nätverkskort (CTI, kontakta Center-agenter)** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b7642-151">In hello Azure portal, on hello **Five9 Plus Adapter (CTI, Contact Center Agents)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="b7642-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b7642-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-five9-tutorial/tutorial_five9_samlbase.png)

3. <span data-ttu-id="b7642-155">På hello **Five9 Plus nätverkskort (CTI, kontakta Center-agenter)-domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b7642-155">On hello **Five9 Plus Adapter (CTI, Contact Center Agents) Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-five9-tutorial/tutorial_five9_url.png)
    
    <span data-ttu-id="b7642-157">a.</span><span class="sxs-lookup"><span data-stu-id="b7642-157">a.</span></span> <span data-ttu-id="b7642-158">I hello **identifierare** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="b7642-158">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span>

    |    <span data-ttu-id="b7642-159">Miljö</span><span class="sxs-lookup"><span data-stu-id="b7642-159">Environment</span></span>      |       <span data-ttu-id="b7642-160">URL: EN</span><span class="sxs-lookup"><span data-stu-id="b7642-160">URL</span></span>      |
    | :-- | :-- |
    | <span data-ttu-id="b7642-161">För ”Five9 Plus Adapter för Microsoft Dynamics CRM”</span><span class="sxs-lookup"><span data-stu-id="b7642-161">For “Five9 Plus Adapter for Microsoft Dynamics CRM”</span></span> | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | <span data-ttu-id="b7642-162">För ”Five9 Plus kortet för Zendesk”</span><span class="sxs-lookup"><span data-stu-id="b7642-162">For “Five9 Plus Adapter for Zendesk”</span></span> | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | <span data-ttu-id="b7642-163">För ”Five9 Plus kortet för agenten skrivbord Toolkit”</span><span class="sxs-lookup"><span data-stu-id="b7642-163">For “Five9 Plus Adapter for Agent Desktop Toolkit”</span></span> | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    <span data-ttu-id="b7642-164">b.</span><span class="sxs-lookup"><span data-stu-id="b7642-164">b.</span></span> <span data-ttu-id="b7642-165">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="b7642-165">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>

    |      <span data-ttu-id="b7642-166">Miljö</span><span class="sxs-lookup"><span data-stu-id="b7642-166">Environment</span></span>     |      <span data-ttu-id="b7642-167">URL: EN</span><span class="sxs-lookup"><span data-stu-id="b7642-167">URL</span></span>      |
    | :--                  | :--           |
    | <span data-ttu-id="b7642-168">För ”Five9 Plus Adapter för Microsoft Dynamics CRM”</span><span class="sxs-lookup"><span data-stu-id="b7642-168">For “Five9 Plus Adapter for Microsoft Dynamics CRM”</span></span> | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | <span data-ttu-id="b7642-169">För ”Five9 Plus kortet för Zendesk”</span><span class="sxs-lookup"><span data-stu-id="b7642-169">For “Five9 Plus Adapter for Zendesk”</span></span> | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | <span data-ttu-id="b7642-170">För ”Five9 Plus kortet för agenten skrivbord Toolkit”</span><span class="sxs-lookup"><span data-stu-id="b7642-170">For “Five9 Plus Adapter for Agent Desktop Toolkit”</span></span> | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

4. <span data-ttu-id="b7642-171">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="b7642-171">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-five9-tutorial/tutorial_five9_certificate.png) 

5. <span data-ttu-id="b7642-173">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="b7642-173">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-five9-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b7642-175">På hello **Five9 Plus (CTI, kontakta Center-agenter) kortkonfiguration** klickar du på **konfigurera Five9 Plus nätverkskort (CTI, kontakta Center-agenter)** tooopen **konfigurerainloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="b7642-175">On hello **Five9 Plus Adapter (CTI, Contact Center Agents) Configuration** section, click **Configure Five9 Plus Adapter (CTI, Contact Center Agents)** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b7642-176">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="b7642-176">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-five9-tutorial/tutorial_five9_configure.png) 

7. <span data-ttu-id="b7642-178">tooconfigure enkel inloggning på **Five9 Plus nätverkskort (CTI, kontakta Center-agenter)** sida, behöver du toosend hello hämtas **Certificate(Base64), Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel** för[Five9 Plus nätverkskort (CTI, kontakta Center-agenter) supportteamet](https://www.five9.com/about/contact).</span><span class="sxs-lookup"><span data-stu-id="b7642-178">tooconfigure single sign-on on **Five9 Plus Adapter (CTI, Contact Center Agents)** side, you need toosend hello downloaded **Certificate(Base64), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Five9 Plus Adapter (CTI, Contact Center Agents) support team](https://www.five9.com/about/contact).</span></span> <span data-ttu-id="b7642-179">Även dessutom för att konfigurera enkel inloggning ytterligare Följ hello nedanstående steg enligt toohello nätverkskort:</span><span class="sxs-lookup"><span data-stu-id="b7642-179">Also additionally, for configuring SSO further please follow hello below steps according toohello adapter:</span></span>

    <span data-ttu-id="b7642-180">a.</span><span class="sxs-lookup"><span data-stu-id="b7642-180">a.</span></span> <span data-ttu-id="b7642-181">”Five9 Plus kortet för agenten skrivbord Toolkit” Admin-Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)</span><span class="sxs-lookup"><span data-stu-id="b7642-181">“Five9 Plus Adapter for Agent Desktop Toolkit” Admin Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)</span></span>
    
    <span data-ttu-id="b7642-182">b.</span><span class="sxs-lookup"><span data-stu-id="b7642-182">b.</span></span> <span data-ttu-id="b7642-183">”Five9 Plus Adapter för Microsoft Dynamics CRM” Admin-Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)</span><span class="sxs-lookup"><span data-stu-id="b7642-183">“Five9 Plus Adapter for Microsoft Dynamics CRM” Admin Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)</span></span>
    
    <span data-ttu-id="b7642-184">c.</span><span class="sxs-lookup"><span data-stu-id="b7642-184">c.</span></span> <span data-ttu-id="b7642-185">”Five9 Plus kortet för Zendesk” Admin-Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)</span><span class="sxs-lookup"><span data-stu-id="b7642-185">“Five9 Plus Adapter for Zendesk” Admin Guide: [http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](http://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)</span></span>


> [!TIP]
> <span data-ttu-id="b7642-186">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="b7642-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b7642-187">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="b7642-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b7642-188">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b7642-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b7642-189">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b7642-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="b7642-190">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b7642-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="b7642-192">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b7642-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b7642-193">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b7642-193">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b7642-195">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b7642-195">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b7642-197">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7642-197">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b7642-199">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b7642-199">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-five9-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b7642-201">a.</span><span class="sxs-lookup"><span data-stu-id="b7642-201">a.</span></span> <span data-ttu-id="b7642-202">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b7642-202">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b7642-203">b.</span><span class="sxs-lookup"><span data-stu-id="b7642-203">b.</span></span> <span data-ttu-id="b7642-204">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b7642-204">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b7642-205">c.</span><span class="sxs-lookup"><span data-stu-id="b7642-205">c.</span></span> <span data-ttu-id="b7642-206">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b7642-206">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b7642-207">d.</span><span class="sxs-lookup"><span data-stu-id="b7642-207">d.</span></span> <span data-ttu-id="b7642-208">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b7642-208">Click **Create**.</span></span>
 
### <a name="creating-a-five9-plus-adapter-cti-contact-center-agents-test-user"></a><span data-ttu-id="b7642-209">Skapa en testanvändare Five9 Plus nätverkskort (CTI, kontakta Center-agenter)</span><span class="sxs-lookup"><span data-stu-id="b7642-209">Creating a Five9 Plus Adapter (CTI, Contact Center Agents) test user</span></span>

<span data-ttu-id="b7642-210">I det här avsnittet skapar du en användare som kallas Britta Simon i Five9 Plus nätverkskort (CTI, kontakta Center-agenter).</span><span class="sxs-lookup"><span data-stu-id="b7642-210">In this section, you create a user called Britta Simon in Five9 Plus Adapter (CTI, Contact Center Agents).</span></span> <span data-ttu-id="b7642-211">Arbeta med [Five9 Plus nätverkskort (CTI, kontakta Center-agenter) supportteamet](https://www.five9.com/about/contact) att lägga till hello användare i hello Five9 Plus nätverkskort (CTI, kontakta Center-agenter)-plattformen.</span><span class="sxs-lookup"><span data-stu-id="b7642-211">Work with [Five9 Plus Adapter (CTI, Contact Center Agents) support team](https://www.five9.com/about/contact) to add hello users in hello Five9 Plus Adapter (CTI, Contact Center Agents) platform.</span></span> <span data-ttu-id="b7642-212">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b7642-212">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b7642-213">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="b7642-213">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b7642-214">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooFive9 Plus nätverkskort (CTI, kontakta Center-agenter).</span><span class="sxs-lookup"><span data-stu-id="b7642-214">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFive9 Plus Adapter (CTI, Contact Center Agents).</span></span>

![Tilldela användare][200] 

<span data-ttu-id="b7642-216">**tooassign Britta Simon tooFive9 Plus nätverkskort (CTI, kontakta Center-agenter), utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="b7642-216">**tooassign Britta Simon tooFive9 Plus Adapter (CTI, Contact Center Agents), perform hello following steps:**</span></span>

1. <span data-ttu-id="b7642-217">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b7642-217">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b7642-219">Välj i listan med program hello **Five9 Plus nätverkskort (CTI, kontakta Center-agenter)**.</span><span class="sxs-lookup"><span data-stu-id="b7642-219">In hello applications list, select **Five9 Plus Adapter (CTI, Contact Center Agents)**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-five9-tutorial/tutorial_five9_app.png) 

3. <span data-ttu-id="b7642-221">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b7642-221">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="b7642-223">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="b7642-223">Click **Add** button.</span></span> <span data-ttu-id="b7642-224">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7642-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="b7642-226">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="b7642-226">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b7642-227">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7642-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b7642-228">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b7642-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b7642-229">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b7642-229">Testing single sign-on</span></span>

<span data-ttu-id="b7642-230">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b7642-230">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b7642-231">När du klickar på hello Five9 Plus nätverkskort (CTI, kontakta Center-agenter) panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Five9 Plus nätverkskort (CTI, kontakta Center-agenter) program.</span><span class="sxs-lookup"><span data-stu-id="b7642-231">When you click hello Five9 Plus Adapter (CTI, Contact Center Agents) tile in hello Access Panel, you should get automatically signed-on tooyour Five9 Plus Adapter (CTI, Contact Center Agents) application.</span></span>
<span data-ttu-id="b7642-232">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b7642-232">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b7642-233">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b7642-233">Additional resources</span></span>

* [<span data-ttu-id="b7642-234">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b7642-234">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b7642-235">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b7642-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-five9-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-five9-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-five9-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-five9-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-five9-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-five9-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-five9-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-five9-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-five9-tutorial/tutorial_general_203.png

