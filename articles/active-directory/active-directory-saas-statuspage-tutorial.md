---
title: "Självstudier: Azure Active Directory-integrering med StatusPage | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och StatusPage."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f6ee8bb3-df43-4c0d-bf84-89f18deac4b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 7c6717017984241e9e459273ead4b5e062311120
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-statuspage"></a><span data-ttu-id="938b2-103">Självstudier: Azure Active Directory-integrering med StatusPage</span><span class="sxs-lookup"><span data-stu-id="938b2-103">Tutorial: Azure Active Directory integration with StatusPage</span></span>

<span data-ttu-id="938b2-104">I kursen får du lära dig hur toointegrate StatusPage med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="938b2-104">In this tutorial, you learn how toointegrate StatusPage with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="938b2-105">Integrera StatusPage med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="938b2-105">Integrating StatusPage with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="938b2-106">Du kan styra i Azure AD som har åtkomst till tooStatusPage</span><span class="sxs-lookup"><span data-stu-id="938b2-106">You can control in Azure AD who has access tooStatusPage</span></span>
- <span data-ttu-id="938b2-107">Du kan aktivera din användare tooautomatically get inloggade tooStatusPage (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="938b2-107">You can enable your users tooautomatically get signed-on tooStatusPage (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="938b2-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="938b2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="938b2-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="938b2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="938b2-110">Krav</span><span class="sxs-lookup"><span data-stu-id="938b2-110">Prerequisites</span></span>

<span data-ttu-id="938b2-111">tooconfigure Azure AD-integrering med StatusPage, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="938b2-111">tooconfigure Azure AD integration with StatusPage, you need hello following items:</span></span>

- <span data-ttu-id="938b2-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="938b2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="938b2-113">En StatusPage enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="938b2-113">A StatusPage single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="938b2-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="938b2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="938b2-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="938b2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="938b2-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="938b2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="938b2-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="938b2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="938b2-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="938b2-118">Scenario description</span></span>
<span data-ttu-id="938b2-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="938b2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="938b2-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="938b2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="938b2-121">Att lägga till StatusPage från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="938b2-121">Adding StatusPage from hello gallery</span></span>
2. <span data-ttu-id="938b2-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="938b2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-statuspage-from-hello-gallery"></a><span data-ttu-id="938b2-123">Att lägga till StatusPage från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="938b2-123">Adding StatusPage from hello gallery</span></span>
<span data-ttu-id="938b2-124">tooconfigure hello integrering av StatusPage i Azure AD, behöver du tooadd StatusPage hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="938b2-124">tooconfigure hello integration of StatusPage into Azure AD, you need tooadd StatusPage from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="938b2-125">**tooadd StatusPage från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="938b2-125">**tooadd StatusPage from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="938b2-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="938b2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="938b2-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="938b2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="938b2-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="938b2-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="938b2-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="938b2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="938b2-133">Skriv i sökrutan hello **StatusPage**.</span><span class="sxs-lookup"><span data-stu-id="938b2-133">In hello search box, type **StatusPage**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_search.png)

5. <span data-ttu-id="938b2-135">Markera hello resultat på panelen **StatusPage**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="938b2-135">In hello results panel, select **StatusPage**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="938b2-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="938b2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="938b2-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med StatusPage baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="938b2-138">In this section, you configure and test Azure AD single sign-on with StatusPage based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="938b2-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i StatusPage är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="938b2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in StatusPage is tooa user in Azure AD.</span></span> <span data-ttu-id="938b2-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i StatusPage toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="938b2-140">In other words, a link relationship between an Azure AD user and hello related user in StatusPage needs toobe established.</span></span>

<span data-ttu-id="938b2-141">I StatusPage, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="938b2-141">In StatusPage, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="938b2-142">tooconfigure och testa Azure AD enkel inloggning med StatusPage, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="938b2-142">tooconfigure and test Azure AD single sign-on with StatusPage, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="938b2-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="938b2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="938b2-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="938b2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="938b2-145">**[Skapa en testanvändare StatusPage](#creating-a-statuspage-test-user)**  -toohave en motsvarighet för Britta Simon i StatusPage som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="938b2-145">**[Creating a StatusPage test user](#creating-a-statuspage-test-user)** - toohave a counterpart of Britta Simon in StatusPage that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="938b2-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="938b2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="938b2-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="938b2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="938b2-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="938b2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="938b2-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt StatusPage program.</span><span class="sxs-lookup"><span data-stu-id="938b2-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your StatusPage application.</span></span>

<span data-ttu-id="938b2-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med StatusPage:**</span><span class="sxs-lookup"><span data-stu-id="938b2-150">**tooconfigure Azure AD single sign-on with StatusPage, perform hello following steps:**</span></span>

1. <span data-ttu-id="938b2-151">I hello Azure-portalen på hello **StatusPage** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="938b2-151">In hello Azure portal, on hello **StatusPage** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="938b2-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="938b2-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_samlbase.png)

3. <span data-ttu-id="938b2-155">På hello **StatusPage domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="938b2-155">On hello **StatusPage Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_url.png)

    <span data-ttu-id="938b2-157">a.</span><span class="sxs-lookup"><span data-stu-id="938b2-157">a.</span></span> <span data-ttu-id="938b2-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="938b2-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/` |
    | `https://<subdomain>.statuspage.io/` |

    <span data-ttu-id="938b2-159">b.</span><span class="sxs-lookup"><span data-stu-id="938b2-159">b.</span></span> <span data-ttu-id="938b2-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="938b2-160">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span> 
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/sso/saml/consume` |
    | `https://<subdomain>.statuspage.io/sso/saml/consume` |

    > [!NOTE]
    > <span data-ttu-id="938b2-161">Kontakta supportteamet för hello StatusPage på [ SupportTeam@statuspage.io ](mailto:SupportTeam@statuspage.io)toorequest metadata nödvändiga tooconfigure enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="938b2-161">Contact hello StatusPage support team at [SupportTeam@statuspage.io](mailto:SupportTeam@statuspage.io)toorequest metadata necessary tooconfigure single sign-on.</span></span> 
    >
    ><span data-ttu-id="938b2-162">a.</span><span class="sxs-lookup"><span data-stu-id="938b2-162">a.</span></span> <span data-ttu-id="938b2-163">Kopiera hello utfärdaren värde från hello metadata, och klistra in den i hello **identifierare** textruta.</span><span class="sxs-lookup"><span data-stu-id="938b2-163">From hello metadata, copy hello Issuer value, and then paste it into hello **Identifier** textbox.</span></span>
    >
    ><span data-ttu-id="938b2-164">b.</span><span class="sxs-lookup"><span data-stu-id="938b2-164">b.</span></span> <span data-ttu-id="938b2-165">Kopiera hello Reply URL från hello metadata, och klistra in den i hello **Reply URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="938b2-165">From hello metadata, copy hello Reply URL, and then paste it into hello **Reply URL** textbox.</span></span>

4. <span data-ttu-id="938b2-166">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="938b2-166">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_certificate.png) 

5. <span data-ttu-id="938b2-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="938b2-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="938b2-170">På hello **StatusPage Configuration** klickar du på **konfigurera StatusPage** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="938b2-170">On hello **StatusPage Configuration** section, click **Configure StatusPage** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="938b2-171">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="938b2-171">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_configure.png) 

7. <span data-ttu-id="938b2-173">Logga in tooyour StatusPage företagets webbplats som en administratör i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="938b2-173">In another browser window, sign on tooyour StatusPage company site as an administrator.</span></span>

8. <span data-ttu-id="938b2-174">I hello verktygsfältet klickar du på **Hantera konto**.</span><span class="sxs-lookup"><span data-stu-id="938b2-174">In hello main toolbar, click **Manage Account**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_06.png) 

10. <span data-ttu-id="938b2-176">Klicka på hello **enkel inloggning** fliken.</span><span class="sxs-lookup"><span data-stu-id="938b2-176">Click hello **Single Sign-on** tab.</span></span> 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_07.png) 

11. <span data-ttu-id="938b2-178">På installationssidan för hello SSO, utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="938b2-178">On hello SSO Setup page, perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_08.png) 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_09.png) 
 
    <span data-ttu-id="938b2-181">a.</span><span class="sxs-lookup"><span data-stu-id="938b2-181">a.</span></span> <span data-ttu-id="938b2-182">I hello **mål-URL för SSO** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="938b2-182">In hello **SSO Target URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="938b2-183">b.</span><span class="sxs-lookup"><span data-stu-id="938b2-183">b.</span></span> <span data-ttu-id="938b2-184">Öppna din hämtat certifikat i anteckningar, kopiera hello innehåll, och klistra in den i hello **certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="938b2-184">Open your downloaded certificate in Notepad, copy hello content, and then paste it into hello **Certificate** textbox.</span></span> 

    <span data-ttu-id="938b2-185">c.</span><span class="sxs-lookup"><span data-stu-id="938b2-185">c.</span></span> <span data-ttu-id="938b2-186">Klicka på **Spara konfiguration**.</span><span class="sxs-lookup"><span data-stu-id="938b2-186">Click **SAVE CONFIGURATION**.</span></span>

> [!TIP]
> <span data-ttu-id="938b2-187">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="938b2-187">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="938b2-188">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="938b2-188">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="938b2-189">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="938b2-189">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="938b2-190">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="938b2-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="938b2-191">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="938b2-191">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="938b2-193">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="938b2-193">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="938b2-194">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="938b2-194">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="938b2-196">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="938b2-196">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="938b2-198">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="938b2-198">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="938b2-200">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="938b2-200">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-statuspage-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="938b2-202">a.</span><span class="sxs-lookup"><span data-stu-id="938b2-202">a.</span></span> <span data-ttu-id="938b2-203">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="938b2-203">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="938b2-204">b.</span><span class="sxs-lookup"><span data-stu-id="938b2-204">b.</span></span> <span data-ttu-id="938b2-205">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="938b2-205">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="938b2-206">c.</span><span class="sxs-lookup"><span data-stu-id="938b2-206">c.</span></span> <span data-ttu-id="938b2-207">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="938b2-207">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="938b2-208">d.</span><span class="sxs-lookup"><span data-stu-id="938b2-208">d.</span></span> <span data-ttu-id="938b2-209">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="938b2-209">Click **Create**.</span></span>
 
### <a name="creating-a-statuspage-test-user"></a><span data-ttu-id="938b2-210">Skapa en testanvändare StatusPage</span><span class="sxs-lookup"><span data-stu-id="938b2-210">Creating a StatusPage test user</span></span>

<span data-ttu-id="938b2-211">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i StatusPage.</span><span class="sxs-lookup"><span data-stu-id="938b2-211">hello objective of this section is toocreate a user called Britta Simon in StatusPage.</span></span>

<span data-ttu-id="938b2-212">StatusPage stöder just-in-time-etablering.</span><span class="sxs-lookup"><span data-stu-id="938b2-212">StatusPage supports just-in-time provisioning.</span></span> <span data-ttu-id="938b2-213">Du har redan aktiverats i [konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="938b2-213">You have already enabled it in [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

<span data-ttu-id="938b2-214">**toocreate en användare som kallas Britta Simon i StatusPage, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="938b2-214">**toocreate a user called Britta Simon in StatusPage, perform hello following steps:**</span></span>

1. <span data-ttu-id="938b2-215">Inloggning tooyour StatusPage företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="938b2-215">Sign-on tooyour StatusPage company site as an administrator.</span></span>

2. <span data-ttu-id="938b2-216">Hello-menyn överst hello **Hantera konto**.</span><span class="sxs-lookup"><span data-stu-id="938b2-216">In hello menu on hello top, click **Manage Account**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_06.png)

3. <span data-ttu-id="938b2-218">Klicka på hello **gruppmedlemmar** fliken.</span><span class="sxs-lookup"><span data-stu-id="938b2-218">Click hello **Team Members** tab.</span></span> 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_10.png) 

4. <span data-ttu-id="938b2-220">Klicka på **Lägg till TEAMMEDLEM**.</span><span class="sxs-lookup"><span data-stu-id="938b2-220">Click **ADD TEAM MEMBER**.</span></span> 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_11.png) 

5. <span data-ttu-id="938b2-222">Typen hello **e-postadress**, **Förnamn**, och **Sur namn** av en giltig användare du vill använda tooprovision hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="938b2-222">Type hello **Email Address**, **First Name**, and **Sur Name** of a valid user you want tooprovision into hello related textboxes.</span></span> 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_12.png) 

6. <span data-ttu-id="938b2-224">Som **rollen**, Välj **administratör**.</span><span class="sxs-lookup"><span data-stu-id="938b2-224">As **Role**, choose **Client Administrator**.</span></span>

7. <span data-ttu-id="938b2-225">Klicka på **skapa konto**.</span><span class="sxs-lookup"><span data-stu-id="938b2-225">Click **CREATE ACCOUNT**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="938b2-226">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="938b2-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="938b2-227">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooStatusPage.</span><span class="sxs-lookup"><span data-stu-id="938b2-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooStatusPage.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="938b2-229">**tooassign Britta Simon tooStatusPage utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="938b2-229">**tooassign Britta Simon tooStatusPage, perform hello following steps:**</span></span>

1. <span data-ttu-id="938b2-230">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="938b2-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="938b2-232">Välj i listan med program hello **StatusPage**.</span><span class="sxs-lookup"><span data-stu-id="938b2-232">In hello applications list, select **StatusPage**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_app.png) 

3. <span data-ttu-id="938b2-234">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="938b2-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="938b2-236">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="938b2-236">Click **Add** button.</span></span> <span data-ttu-id="938b2-237">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="938b2-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="938b2-239">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="938b2-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="938b2-240">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="938b2-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="938b2-241">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="938b2-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="938b2-242">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="938b2-242">Testing single sign-on</span></span>

<span data-ttu-id="938b2-243">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="938b2-243">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="938b2-244">Du bör få automatiskt inloggade tooyour StatusPage programmet när du klickar på hello StatusPage panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="938b2-244">When you click hello StatusPage tile in hello Access Panel, you should get automatically signed-on tooyour StatusPage application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="938b2-245">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="938b2-245">Additional resources</span></span>

* [<span data-ttu-id="938b2-246">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="938b2-246">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="938b2-247">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="938b2-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_203.png

