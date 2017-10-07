---
title: "Självstudier: Azure Active Directory-integrering med YouEarnedIt | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och YouEarnedIt."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3011d44d-dfcf-4061-888f-cff90fbc8150
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: cc9a8ae2f92751cf3fadbeec23c8319c83728a33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-youearnedit"></a><span data-ttu-id="d4aca-103">Självstudier: Azure Active Directory-integrering med YouEarnedIt</span><span class="sxs-lookup"><span data-stu-id="d4aca-103">Tutorial: Azure Active Directory integration with YouEarnedIt</span></span>

<span data-ttu-id="d4aca-104">I kursen får du lära dig hur toointegrate YouEarnedIt med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d4aca-104">In this tutorial, you learn how toointegrate YouEarnedIt with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d4aca-105">Integrera YouEarnedIt med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d4aca-105">Integrating YouEarnedIt with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d4aca-106">Du kan styra i Azure AD som har åtkomst till tooYouEarnedIt.</span><span class="sxs-lookup"><span data-stu-id="d4aca-106">You can control in Azure AD who has access tooYouEarnedIt.</span></span>
- <span data-ttu-id="d4aca-107">Du kan låta dina användare tooautomatically get inloggade tooYouEarnedIt (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="d4aca-107">You can enable your users tooautomatically get signed-on tooYouEarnedIt (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="d4aca-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d4aca-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="d4aca-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d4aca-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d4aca-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d4aca-110">Prerequisites</span></span>

<span data-ttu-id="d4aca-111">tooconfigure Azure AD-integrering med YouEarnedIt, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="d4aca-111">tooconfigure Azure AD integration with YouEarnedIt, you need hello following items:</span></span>

- <span data-ttu-id="d4aca-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d4aca-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d4aca-113">En YouEarnedIt enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="d4aca-113">A YouEarnedIt single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d4aca-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d4aca-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d4aca-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d4aca-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d4aca-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d4aca-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d4aca-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d4aca-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d4aca-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d4aca-118">Scenario description</span></span>
<span data-ttu-id="d4aca-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d4aca-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d4aca-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d4aca-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d4aca-121">Att lägga till YouEarnedIt från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d4aca-121">Adding YouEarnedIt from hello gallery</span></span>
2. <span data-ttu-id="d4aca-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d4aca-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-youearnedit-from-hello-gallery"></a><span data-ttu-id="d4aca-123">Att lägga till YouEarnedIt från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d4aca-123">Adding YouEarnedIt from hello gallery</span></span>
<span data-ttu-id="d4aca-124">tooconfigure hello integrering av YouEarnedIt i Azure AD, behöver du tooadd YouEarnedIt hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="d4aca-124">tooconfigure hello integration of YouEarnedIt into Azure AD, you need tooadd YouEarnedIt from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d4aca-125">**tooadd YouEarnedIt från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d4aca-125">**tooadd YouEarnedIt from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d4aca-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d4aca-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="d4aca-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d4aca-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d4aca-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="d4aca-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="d4aca-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d4aca-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="d4aca-133">Skriv i sökrutan hello **YouEarnedt**väljer **YouEarnedt** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="d4aca-133">In hello search box, type **YouEarnedt**, select  **YouEarnedt**  from result panel then click **Add** button tooadd hello application.</span></span>

    ![YouEarnedIt i hello resultatlistan](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="d4aca-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d4aca-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="d4aca-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med YouEarnedIt baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d4aca-136">In this section, you configure and test Azure AD single sign-on with YouEarnedIt based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d4aca-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i YouEarnedIt är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d4aca-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in YouEarnedIt is tooa user in Azure AD.</span></span> <span data-ttu-id="d4aca-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i YouEarnedIt toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="d4aca-138">In other words, a link relationship between an Azure AD user and hello related user in YouEarnedIt needs toobe established.</span></span>

<span data-ttu-id="d4aca-139">I YouEarnedIt, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="d4aca-139">In YouEarnedIt, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d4aca-140">tooconfigure och testa Azure AD enkel inloggning med YouEarnedIt, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d4aca-140">tooconfigure and test Azure AD single sign-on with YouEarnedIt, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d4aca-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d4aca-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d4aca-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d4aca-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d4aca-143">**[Skapa en testanvändare YouEarnedIt](#create-a-youearnedit-test-user)**  -toohave en motsvarighet för Britta Simon i YouEarnedIt som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d4aca-143">**[Create a YouEarnedIt test user](#create-a-youearnedit-test-user)** - toohave a counterpart of Britta Simon in YouEarnedIt that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d4aca-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d4aca-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d4aca-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d4aca-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="d4aca-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d4aca-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="d4aca-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt YouEarnedIt program.</span><span class="sxs-lookup"><span data-stu-id="d4aca-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your YouEarnedIt application.</span></span>

<span data-ttu-id="d4aca-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med YouEarnedIt:**</span><span class="sxs-lookup"><span data-stu-id="d4aca-148">**tooconfigure Azure AD single sign-on with YouEarnedIt, perform hello following steps:**</span></span>

1. <span data-ttu-id="d4aca-149">I hello Azure-portalen på hello **YouEarnedIt** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d4aca-149">In hello Azure portal, on hello **YouEarnedIt** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="d4aca-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d4aca-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_samlbase.png)

3. <span data-ttu-id="d4aca-153">På hello **YouEarnedIt domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d4aca-153">On hello **YouEarnedIt Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och YouEarnedIt domän med enkel inloggning information](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_url.png)

    <span data-ttu-id="d4aca-155">a.</span><span class="sxs-lookup"><span data-stu-id="d4aca-155">a.</span></span> <span data-ttu-id="d4aca-156">I hello **inloggnings-URL** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="d4aca-156">In hello **Sign-on URL** textbox, type a URL using hello following patterns:</span></span> 
    | <span data-ttu-id="d4aca-157">Miljö</span><span class="sxs-lookup"><span data-stu-id="d4aca-157">Environment</span></span>  | <span data-ttu-id="d4aca-158">Mönstret</span><span class="sxs-lookup"><span data-stu-id="d4aca-158">Pattern</span></span>  |
    |:--- |:--- |
    | <span data-ttu-id="d4aca-159">Produktion</span><span class="sxs-lookup"><span data-stu-id="d4aca-159">Production</span></span> | `https://<company name>.youearnedit.com/users/sign_in` |
    | <span data-ttu-id="d4aca-160">Sandbox</span><span class="sxs-lookup"><span data-stu-id="d4aca-160">Sandbox</span></span>  |`https://<company name>.sandbox.youearnedit.com/users/sign_in` |

    <span data-ttu-id="d4aca-161">b.</span><span class="sxs-lookup"><span data-stu-id="d4aca-161">b.</span></span> <span data-ttu-id="d4aca-162">I hello **identifierare** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="d4aca-162">In hello **Identifier** textbox, type a URL using hello following patterns:</span></span>
    | <span data-ttu-id="d4aca-163">Miljö</span><span class="sxs-lookup"><span data-stu-id="d4aca-163">Environment</span></span>  | <span data-ttu-id="d4aca-164">Mönstret</span><span class="sxs-lookup"><span data-stu-id="d4aca-164">Pattern</span></span>  |
    |:--- |:--- |
    | <span data-ttu-id="d4aca-165">Produktion</span><span class="sxs-lookup"><span data-stu-id="d4aca-165">Production</span></span> | `https://<company name>.youearnedit.com` |
    | <span data-ttu-id="d4aca-166">Sandbox</span><span class="sxs-lookup"><span data-stu-id="d4aca-166">Sandbox</span></span>  |`https://<company name>.sandbox.youearnedit.com` |

    > [!NOTE] 
    > <span data-ttu-id="d4aca-167">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="d4aca-167">These values are not real.</span></span> <span data-ttu-id="d4aca-168">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="d4aca-168">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d4aca-169">Kontakta [YouEarnedIt klienten supportteamet](https://youearnedit.freshdesk.com/support/tickets/new) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="d4aca-169">Contact [YouEarnedIt Client support team](https://youearnedit.freshdesk.com/support/tickets/new) tooget these values.</span></span> 
 
4. <span data-ttu-id="d4aca-170">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="d4aca-170">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_certificate.png) 

5. <span data-ttu-id="d4aca-172">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d4aca-172">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-youearnedit-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d4aca-174">På hello **YouEarnedIt Configuration** klickar du på **konfigurera YouEarnedIt** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="d4aca-174">On hello **YouEarnedIt Configuration** section, click **Configure YouEarnedIt** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d4aca-175">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="d4aca-175">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![YouEarnedIt konfiguration](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_configure.png) 

7. <span data-ttu-id="d4aca-177">tooconfigure enkel inloggning på **YouEarnedIt** sida, behöver du toosend hello hämtas **Certificate(Base64)** och **SAML inloggning tjänst-URL för enkel** för[YouEarnedIt supportteamet](https://youearnedit.freshdesk.com/support/tickets/new).</span><span class="sxs-lookup"><span data-stu-id="d4aca-177">tooconfigure single sign-on on **YouEarnedIt** side, you need toosend hello downloaded **Certificate(Base64)** and **SAML Single Sign-On Service URL** too[YouEarnedIt support team](https://youearnedit.freshdesk.com/support/tickets/new).</span></span> <span data-ttu-id="d4aca-178">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="d4aca-178">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="d4aca-179">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="d4aca-179">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d4aca-180">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="d4aca-180">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d4aca-181">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d4aca-181">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="d4aca-182">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d4aca-182">Create an Azure AD test user</span></span>

<span data-ttu-id="d4aca-183">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d4aca-183">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="d4aca-185">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d4aca-185">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d4aca-186">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="d4aca-186">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="d4aca-188">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d4aca-188">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="d4aca-190">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d4aca-190">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="d4aca-192">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d4aca-192">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_04.png)

    <span data-ttu-id="d4aca-194">a.</span><span class="sxs-lookup"><span data-stu-id="d4aca-194">a.</span></span> <span data-ttu-id="d4aca-195">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d4aca-195">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d4aca-196">b.</span><span class="sxs-lookup"><span data-stu-id="d4aca-196">b.</span></span> <span data-ttu-id="d4aca-197">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d4aca-197">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="d4aca-198">c.</span><span class="sxs-lookup"><span data-stu-id="d4aca-198">c.</span></span> <span data-ttu-id="d4aca-199">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="d4aca-199">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="d4aca-200">d.</span><span class="sxs-lookup"><span data-stu-id="d4aca-200">d.</span></span> <span data-ttu-id="d4aca-201">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d4aca-201">Click **Create**.</span></span>
 
### <a name="create-a-youearnedit-test-user"></a><span data-ttu-id="d4aca-202">Skapa en testanvändare YouEarnedIt</span><span class="sxs-lookup"><span data-stu-id="d4aca-202">Create a YouEarnedIt test user</span></span>

<span data-ttu-id="d4aca-203">I det här avsnittet skapar du en användare som kallas Britta Simon i YouEarnedIt.</span><span class="sxs-lookup"><span data-stu-id="d4aca-203">In this section, you create a user called Britta Simon in YouEarnedIt.</span></span> <span data-ttu-id="d4aca-204">Se tillsammans med YouEarnedIt support-teamet tooadd hello användare i hello YouEarnedIt plattform.</span><span class="sxs-lookup"><span data-stu-id="d4aca-204">Please work with YouEarnedIt support team tooadd hello users in hello YouEarnedIt platform.</span></span>

>[!NOTE]
><span data-ttu-id="d4aca-205">YouEarnedIt räknar hello identitetsleverantör toosupply en e-postadress eller ett användarnamn i hello NameID attribut.</span><span class="sxs-lookup"><span data-stu-id="d4aca-205">YouEarnedIt expect hello Identity Provider toosupply an EmailAddress  or UserName in hello NameID attribute.</span></span> <span data-ttu-id="d4aca-206">Autentiseringen misslyckas om en motsvarande användarnamn eller e-postadress finns inte i hello databas eller inte matchar varandra exakt.</span><span class="sxs-lookup"><span data-stu-id="d4aca-206">Authentication will fail if a corresponding UserName or EmailAddress is not found within hello database or does not match exactly.</span></span> <span data-ttu-id="d4aca-207">Detta kräver att konton importeras till hello YouEarnedIt systemet innan hello SSO-integration (vanligtvis antingen via API: et eller CSV-import).</span><span class="sxs-lookup"><span data-stu-id="d4aca-207">This will require that accounts be imported into hello YouEarnedIt system before hello SSO integration (Typically either via API or CSV import).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="d4aca-208">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="d4aca-208">Assign hello Azure AD test user</span></span>

<span data-ttu-id="d4aca-209">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooYouEarnedIt.</span><span class="sxs-lookup"><span data-stu-id="d4aca-209">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooYouEarnedIt.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="d4aca-211">**tooassign Britta Simon tooYouEarnedIt utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d4aca-211">**tooassign Britta Simon tooYouEarnedIt, perform hello following steps:**</span></span>

1. <span data-ttu-id="d4aca-212">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d4aca-212">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d4aca-214">Välj i listan med program hello **YouEarnedIt**.</span><span class="sxs-lookup"><span data-stu-id="d4aca-214">In hello applications list, select **YouEarnedIt**.</span></span>

    ![Hej YouEarnedIt länken i listan med program hello](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_app.png)  

3. <span data-ttu-id="d4aca-216">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d4aca-216">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="d4aca-218">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d4aca-218">Click **Add** button.</span></span> <span data-ttu-id="d4aca-219">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d4aca-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="d4aca-221">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="d4aca-221">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d4aca-222">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d4aca-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d4aca-223">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d4aca-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="d4aca-224">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d4aca-224">Test single sign-on</span></span>

<span data-ttu-id="d4aca-225">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d4aca-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d4aca-226">Du bör få automatiskt inloggade tooyour YouEarnedIt programmet när du klickar på hello YouEarnedIt panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d4aca-226">When you click hello YouEarnedIt tile in hello Access Panel, you should get automatically signed-on tooyour YouEarnedIt application.</span></span>
<span data-ttu-id="d4aca-227">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d4aca-227">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d4aca-228">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d4aca-228">Additional resources</span></span>

* [<span data-ttu-id="d4aca-229">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d4aca-229">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d4aca-230">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d4aca-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_203.png

