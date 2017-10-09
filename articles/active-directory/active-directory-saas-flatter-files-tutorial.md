---
title: "Självstudier: Azure Active Directory-integrering med plattare filer | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och plattare filer."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f86fe5e3-0e91-40d6-869c-3df6912d27ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 73ca2613b7bbaf9992ecf624ff5defabaa44f7a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a><span data-ttu-id="a030b-103">Självstudier: Azure Active Directory-integrering med plattare filer</span><span class="sxs-lookup"><span data-stu-id="a030b-103">Tutorial: Azure Active Directory integration with Flatter Files</span></span>

<span data-ttu-id="a030b-104">I kursen får du lära dig hur toointegrate plattare filer med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a030b-104">In this tutorial, you learn how toointegrate Flatter Files with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a030b-105">Integrera plattare filer med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a030b-105">Integrating Flatter Files with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a030b-106">Du kan styra i Azure AD som har åtkomst till tooFlatter filer</span><span class="sxs-lookup"><span data-stu-id="a030b-106">You can control in Azure AD who has access tooFlatter Files</span></span>
- <span data-ttu-id="a030b-107">Du kan aktivera din användare tooautomatically hämta inloggade tooFlatter filer (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a030b-107">You can enable your users tooautomatically get signed-on tooFlatter Files (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a030b-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a030b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a030b-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a030b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a030b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a030b-110">Prerequisites</span></span>

<span data-ttu-id="a030b-111">tooconfigure Azure AD-integrering med plattare filer, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a030b-111">tooconfigure Azure AD integration with Flatter Files, you need hello following items:</span></span>

- <span data-ttu-id="a030b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a030b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a030b-113">En plattare filer enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="a030b-113">A Flatter Files single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a030b-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a030b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a030b-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a030b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a030b-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a030b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a030b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a030b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a030b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a030b-118">Scenario description</span></span>
<span data-ttu-id="a030b-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a030b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a030b-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a030b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a030b-121">Lägga till plattare filer från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a030b-121">Adding Flatter Files from hello gallery</span></span>
2. <span data-ttu-id="a030b-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a030b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-flatter-files-from-hello-gallery"></a><span data-ttu-id="a030b-123">Lägga till plattare filer från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a030b-123">Adding Flatter Files from hello gallery</span></span>
<span data-ttu-id="a030b-124">tooconfigure hello integrering av plattare filer i Azure AD, behöver du tooadd plattare filer från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="a030b-124">tooconfigure hello integration of Flatter Files into Azure AD, you need tooadd Flatter Files from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a030b-125">**tooadd plattare filer från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a030b-125">**tooadd Flatter Files from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a030b-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a030b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a030b-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a030b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a030b-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="a030b-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a030b-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a030b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a030b-133">Skriv i sökrutan hello **plattare filer**.</span><span class="sxs-lookup"><span data-stu-id="a030b-133">In hello search box, type **Flatter Files**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_search.png)

5. <span data-ttu-id="a030b-135">Markera hello resultat på panelen **plattare filer**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="a030b-135">In hello results panel, select **Flatter Files**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a030b-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a030b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a030b-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med plattare filer baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a030b-138">In this section, you configure and test Azure AD single sign-on with Flatter Files based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a030b-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i plattare filer är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a030b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Flatter Files is tooa user in Azure AD.</span></span> <span data-ttu-id="a030b-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i plattare filer toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="a030b-140">In other words, a link relationship between an Azure AD user and hello related user in Flatter Files needs toobe established.</span></span>

<span data-ttu-id="a030b-141">Tilldela hello värdet hello i plattare filer **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a030b-141">In Flatter Files, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a030b-142">tooconfigure och testa Azure AD enkel inloggning med plattare filer, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a030b-142">tooconfigure and test Azure AD single sign-on with Flatter Files, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a030b-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a030b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a030b-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a030b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a030b-145">**[Skapa en testanvändare plattare filer](#creating-a-flatter-files-test-user)**  -toohave en motsvarighet för Britta Simon i plattare filer som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a030b-145">**[Creating a Flatter Files test user](#creating-a-flatter-files-test-user)** - toohave a counterpart of Britta Simon in Flatter Files that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a030b-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a030b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a030b-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a030b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a030b-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a030b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a030b-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet plattare filer.</span><span class="sxs-lookup"><span data-stu-id="a030b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Flatter Files application.</span></span>

<span data-ttu-id="a030b-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med plattare filer:**</span><span class="sxs-lookup"><span data-stu-id="a030b-150">**tooconfigure Azure AD single sign-on with Flatter Files, perform hello following steps:**</span></span>

1. <span data-ttu-id="a030b-151">I hello Azure-portalen på hello **plattare filer** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a030b-151">In hello Azure portal, on hello **Flatter Files** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a030b-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a030b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_samlbase.png)

3. <span data-ttu-id="a030b-155">På hello **plattare filer domän och URL: er** avsnittet, hello användaren har inte tooperform alla steg som hello app redan redan är integrerade med Azure.</span><span class="sxs-lookup"><span data-stu-id="a030b-155">On hello **Flatter Files Domain and URLs** section, hello user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_url.png)
 
4. <span data-ttu-id="a030b-157">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="a030b-157">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_certificate.png) 

5. <span data-ttu-id="a030b-159">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a030b-159">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a030b-161">På hello **plattare filer Configuration** klickar du på **konfigurera plattare filer** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="a030b-161">On hello **Flatter Files Configuration** section, click **Configure Flatter Files** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a030b-162">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="a030b-162">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_configure.png) 

7. <span data-ttu-id="a030b-164">Inloggning tooyour plattare filer programmet som administratör.</span><span class="sxs-lookup"><span data-stu-id="a030b-164">Sign-on tooyour Flatter Files application as an administrator.</span></span>

8. <span data-ttu-id="a030b-165">Klicka på **INSTRUMENTPANELEN**.</span><span class="sxs-lookup"><span data-stu-id="a030b-165">Click **DASHBOARD**.</span></span> 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  

9. <span data-ttu-id="a030b-167">Klicka på **inställningar**, och utför sedan följande steg på hello hello **företagets** fliken:</span><span class="sxs-lookup"><span data-stu-id="a030b-167">Click **Settings**, and then perform hello following steps on hello **Company** tab:</span></span> 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  
    
    <span data-ttu-id="a030b-169">a.</span><span class="sxs-lookup"><span data-stu-id="a030b-169">a.</span></span> <span data-ttu-id="a030b-170">Välj **använder SAML 2.0 för autentisering**.</span><span class="sxs-lookup"><span data-stu-id="a030b-170">Select **Use SAML 2.0 for Authentication**.</span></span>
    
    <span data-ttu-id="a030b-171">b.</span><span class="sxs-lookup"><span data-stu-id="a030b-171">b.</span></span> <span data-ttu-id="a030b-172">Klicka på **konfigurerar du SAML**.</span><span class="sxs-lookup"><span data-stu-id="a030b-172">Click **Configure SAML**.</span></span>

8. <span data-ttu-id="a030b-173">På hello **SAML-konfiguration** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a030b-173">On hello **SAML Configuration** dialog, perform hello following steps:</span></span> 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  
   
    <span data-ttu-id="a030b-175">a.</span><span class="sxs-lookup"><span data-stu-id="a030b-175">a.</span></span> <span data-ttu-id="a030b-176">I hello **domän** textruta, ange en registrerad domän.</span><span class="sxs-lookup"><span data-stu-id="a030b-176">In hello **Domain** textbox, type your registered domain.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="a030b-177">Om du inte har en registrerad domän ännu, kontakta din plattare filer supportteam via [ support@flatterfiles.com ](mailto:support@flatterfiles.com).</span><span class="sxs-lookup"><span data-stu-id="a030b-177">If you don't have a registered domain yet, contact your Flatter Files support team via [support@flatterfiles.com](mailto:support@flatterfiles.com).</span></span> 
    
    <span data-ttu-id="a030b-178">b.</span><span class="sxs-lookup"><span data-stu-id="a030b-178">b.</span></span> <span data-ttu-id="a030b-179">I **identitet providern URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat formuläret Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a030b-179">In **Identity Provider URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied form Azure portal.</span></span>
   
    <span data-ttu-id="a030b-180">c.</span><span class="sxs-lookup"><span data-stu-id="a030b-180">c.</span></span>  <span data-ttu-id="a030b-181">Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **providern identitetscertifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="a030b-181">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **Identity Provider Certificate** textbox.</span></span>

    <span data-ttu-id="a030b-182">d.</span><span class="sxs-lookup"><span data-stu-id="a030b-182">d.</span></span> <span data-ttu-id="a030b-183">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="a030b-183">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="a030b-184">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="a030b-184">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a030b-185">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="a030b-185">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a030b-186">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a030b-186">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a030b-187">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a030b-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="a030b-188">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a030b-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a030b-190">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a030b-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a030b-191">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a030b-191">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a030b-193">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a030b-193">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a030b-195">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a030b-195">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a030b-197">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a030b-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a030b-199">a.</span><span class="sxs-lookup"><span data-stu-id="a030b-199">a.</span></span> <span data-ttu-id="a030b-200">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a030b-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a030b-201">b.</span><span class="sxs-lookup"><span data-stu-id="a030b-201">b.</span></span> <span data-ttu-id="a030b-202">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a030b-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a030b-203">c.</span><span class="sxs-lookup"><span data-stu-id="a030b-203">c.</span></span> <span data-ttu-id="a030b-204">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a030b-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a030b-205">d.</span><span class="sxs-lookup"><span data-stu-id="a030b-205">d.</span></span> <span data-ttu-id="a030b-206">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a030b-206">Click **Create**.</span></span>
 
### <a name="creating-a-flatter-files-test-user"></a><span data-ttu-id="a030b-207">Skapa en testanvändare plattare filer</span><span class="sxs-lookup"><span data-stu-id="a030b-207">Creating a Flatter Files test user</span></span>

<span data-ttu-id="a030b-208">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i plattare filer.</span><span class="sxs-lookup"><span data-stu-id="a030b-208">hello objective of this section is toocreate a user called Britta Simon in Flatter Files.</span></span>

<span data-ttu-id="a030b-209">**toocreate en användare som kallas Britta Simon i plattare filer, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a030b-209">**toocreate a user called Britta Simon in Flatter Files, perform hello following steps:**</span></span>

1. <span data-ttu-id="a030b-210">Logga in tooyour **plattare filer** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="a030b-210">Sign on tooyour **Flatter Files** company site as administrator.</span></span>

2. <span data-ttu-id="a030b-211">Hello navigeringsfönstret hello vänster, klicka på **inställningar**, och klicka sedan på hello **användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="a030b-211">In hello navigation pane on hello left, click **Settings**, and then click hello **Users** tab.</span></span>
   
    ![Skapa en plattare filer användare](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)

3. <span data-ttu-id="a030b-213">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="a030b-213">Click **Add User**.</span></span> 

4. <span data-ttu-id="a030b-214">På hello **Lägg till användare** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a030b-214">On hello **Add User** dialog, perform hello following steps:</span></span>
   
    ![Skapa en plattare filer användare](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)

    <span data-ttu-id="a030b-216">a.</span><span class="sxs-lookup"><span data-stu-id="a030b-216">a.</span></span> <span data-ttu-id="a030b-217">I hello **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="a030b-217">In hello **First Name** textbox, type **Britta**.</span></span>
   
    <span data-ttu-id="a030b-218">b.</span><span class="sxs-lookup"><span data-stu-id="a030b-218">b.</span></span> <span data-ttu-id="a030b-219">I hello **efternamn** textruta typen **Simon**.</span><span class="sxs-lookup"><span data-stu-id="a030b-219">In hello **Last Name** textbox, type **Simon**.</span></span> 
   
    <span data-ttu-id="a030b-220">c.</span><span class="sxs-lookup"><span data-stu-id="a030b-220">c.</span></span> <span data-ttu-id="a030b-221">I hello **e-postadress** textruta Skriv Brittas e-postadress i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a030b-221">In hello **Email Address** textbox, type Britta's email address in hello Azure portal.</span></span>
   
    <span data-ttu-id="a030b-222">d.</span><span class="sxs-lookup"><span data-stu-id="a030b-222">d.</span></span> <span data-ttu-id="a030b-223">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="a030b-223">Click **Submit**.</span></span>   


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a030b-224">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="a030b-224">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a030b-225">I det här avsnittet kan du aktivera Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst till tooFlatter filer.</span><span class="sxs-lookup"><span data-stu-id="a030b-225">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFlatter Files.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a030b-227">**tooassign Britta Simon tooFlatter filer, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a030b-227">**tooassign Britta Simon tooFlatter Files, perform hello following steps:**</span></span>

1. <span data-ttu-id="a030b-228">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a030b-228">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a030b-230">Välj i listan med program hello **plattare filer**.</span><span class="sxs-lookup"><span data-stu-id="a030b-230">In hello applications list, select **Flatter Files**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatterfiles_app.png) 

3. <span data-ttu-id="a030b-232">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a030b-232">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a030b-234">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a030b-234">Click **Add** button.</span></span> <span data-ttu-id="a030b-235">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a030b-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a030b-237">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="a030b-237">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a030b-238">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a030b-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a030b-239">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a030b-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a030b-240">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a030b-240">Testing single sign-on</span></span>

<span data-ttu-id="a030b-241">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a030b-241">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a030b-242">Du bör få automatiskt inloggade tooyour plattare filer programmet när du klickar på hello plattare filer panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a030b-242">When you click hello Flatter Files tile in hello Access Panel, you should get automatically signed-on tooyour Flatter Files application.</span></span>
<span data-ttu-id="a030b-243">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a030b-243">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a030b-244">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a030b-244">Additional resources</span></span>

* [<span data-ttu-id="a030b-245">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a030b-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a030b-246">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a030b-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png

