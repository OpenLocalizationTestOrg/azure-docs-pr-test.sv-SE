---
title: "Självstudier: Azure Active Directory-integrering med Workrite | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Workrite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2a5c2956-a011-4d5c-877b-80679b6587b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: a663374ae3c8b102b53d8cf05a9cb083b80dbb83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workrite"></a><span data-ttu-id="74881-103">Självstudier: Azure Active Directory-integrering med Workrite</span><span class="sxs-lookup"><span data-stu-id="74881-103">Tutorial: Azure Active Directory integration with Workrite</span></span>

<span data-ttu-id="74881-104">I kursen får du lära dig hur toointegrate Workrite med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="74881-104">In this tutorial, you learn how toointegrate Workrite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="74881-105">Integrera Workrite med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="74881-105">Integrating Workrite with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="74881-106">Du kan styra i Azure AD som har åtkomst till tooWorkrite.</span><span class="sxs-lookup"><span data-stu-id="74881-106">You can control in Azure AD who has access tooWorkrite.</span></span>
- <span data-ttu-id="74881-107">Du kan låta dina användare tooautomatically get inloggade tooWorkrite (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="74881-107">You can enable your users tooautomatically get signed-on tooWorkrite (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="74881-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="74881-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="74881-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="74881-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="74881-110">Krav</span><span class="sxs-lookup"><span data-stu-id="74881-110">Prerequisites</span></span>

<span data-ttu-id="74881-111">tooconfigure Azure AD-integrering med Workrite, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="74881-111">tooconfigure Azure AD integration with Workrite, you need hello following items:</span></span>

- <span data-ttu-id="74881-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="74881-112">An Azure AD subscription</span></span>
- <span data-ttu-id="74881-113">En Workrite enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="74881-113">A Workrite single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="74881-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="74881-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="74881-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="74881-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="74881-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="74881-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="74881-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="74881-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="74881-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="74881-118">Scenario description</span></span>
<span data-ttu-id="74881-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="74881-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="74881-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="74881-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="74881-121">Att lägga till Workrite från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="74881-121">Adding Workrite from hello gallery</span></span>
2. <span data-ttu-id="74881-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="74881-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workrite-from-hello-gallery"></a><span data-ttu-id="74881-123">Att lägga till Workrite från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="74881-123">Adding Workrite from hello gallery</span></span>
<span data-ttu-id="74881-124">tooconfigure hello integrering av Workrite i Azure AD, behöver du tooadd Workrite hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="74881-124">tooconfigure hello integration of Workrite into Azure AD, you need tooadd Workrite from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="74881-125">**tooadd Workrite från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="74881-125">**tooadd Workrite from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="74881-126">I hello ** [Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="74881-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="74881-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="74881-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="74881-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="74881-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="74881-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="74881-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="74881-133">Skriv i sökrutan hello **Workrite**väljer **Workrite** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="74881-133">In hello search box, type **Workrite**, select **Workrite** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Workrite i hello resultatlistan](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="74881-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="74881-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="74881-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Workrite baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="74881-136">In this section, you configure and test Azure AD single sign-on with Workrite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="74881-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Workrite är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="74881-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Workrite is tooa user in Azure AD.</span></span> <span data-ttu-id="74881-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Workrite toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="74881-138">In other words, a link relationship between an Azure AD user and hello related user in Workrite needs toobe established.</span></span>

<span data-ttu-id="74881-139">I Workrite, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="74881-139">In Workrite, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="74881-140">tooconfigure och testa Azure AD enkel inloggning med Workrite, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="74881-140">tooconfigure and test Azure AD single sign-on with Workrite, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="74881-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on) ** -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="74881-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="74881-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user) ** -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="74881-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="74881-143">**[Skapa en testanvändare Workrite](#create-a-workrite-test-user) ** -toohave en motsvarighet för Britta Simon i Workrite som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="74881-143">**[Create a Workrite test user](#create-a-workrite-test-user)** - toohave a counterpart of Britta Simon in Workrite that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="74881-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user) ** -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="74881-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="74881-145">**[Testa enkel inloggning](#test-single-sign-on) ** -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="74881-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="74881-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="74881-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="74881-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Workrite program.</span><span class="sxs-lookup"><span data-stu-id="74881-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Workrite application.</span></span>

<span data-ttu-id="74881-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Workrite:**</span><span class="sxs-lookup"><span data-stu-id="74881-148">**tooconfigure Azure AD single sign-on with Workrite, perform hello following steps:**</span></span>

1. <span data-ttu-id="74881-149">I hello Azure-portalen på hello **Workrite** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="74881-149">In hello Azure portal, on hello **Workrite** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="74881-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="74881-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_samlbase.png)

3. <span data-ttu-id="74881-153">På hello **Workrite domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="74881-153">On hello **Workrite Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och Workrite domän med enkel inloggning information](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_url.png)

    <span data-ttu-id="74881-155">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://app.workrite.co.uk/securelogin/samlgateway.aspx?id=<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="74881-155">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://app.workrite.co.uk/securelogin/samlgateway.aspx?id=<uniqueid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="74881-156">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="74881-156">This value is not real.</span></span> <span data-ttu-id="74881-157">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="74881-157">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="74881-158">Kontakta [Workrite klienten supportteamet](mailto:support@workrite.co.uk) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="74881-158">Contact [Workrite Client support team](mailto:support@workrite.co.uk) tooget this value.</span></span>

4. <span data-ttu-id="74881-159">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="74881-159">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_certificate.png) 

5. <span data-ttu-id="74881-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="74881-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-workrite-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="74881-163">På hello **Workrite Configuration** klickar du på **konfigurera Workrite** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="74881-163">On hello **Workrite Configuration** section, click **Configure Workrite** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="74881-164">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="74881-164">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Workrite konfiguration](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_configure.png) 

7. <span data-ttu-id="74881-166">tooconfigure enkel inloggning på **Workrite** sida, behöver du toosend hello hämtas **Certificate(Base64), Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** för[Workrite supportteam](mailto:support@workrite.co.uk).</span><span class="sxs-lookup"><span data-stu-id="74881-166">tooconfigure single sign-on on **Workrite** side, you need toosend hello downloaded **Certificate(Base64), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Workrite support team](mailto:support@workrite.co.uk).</span></span>

> [!TIP]
> <span data-ttu-id="74881-167">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="74881-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="74881-168">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello ** Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="74881-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="74881-169">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="74881-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="74881-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="74881-170">Create an Azure AD test user</span></span>

<span data-ttu-id="74881-171">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="74881-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="74881-173">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="74881-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="74881-174">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="74881-174">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-workrite-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="74881-176">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="74881-176">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-workrite-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="74881-178">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="74881-178">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-workrite-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="74881-180">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="74881-180">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-workrite-tutorial/create_aaduser_04.png)

    <span data-ttu-id="74881-182">a.</span><span class="sxs-lookup"><span data-stu-id="74881-182">a.</span></span> <span data-ttu-id="74881-183">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="74881-183">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="74881-184">b.</span><span class="sxs-lookup"><span data-stu-id="74881-184">b.</span></span> <span data-ttu-id="74881-185">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="74881-185">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="74881-186">c.</span><span class="sxs-lookup"><span data-stu-id="74881-186">c.</span></span> <span data-ttu-id="74881-187">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="74881-187">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="74881-188">d.</span><span class="sxs-lookup"><span data-stu-id="74881-188">d.</span></span> <span data-ttu-id="74881-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="74881-189">Click **Create**.</span></span>
 
### <a name="create-a-workrite-test-user"></a><span data-ttu-id="74881-190">Skapa en testanvändare Workrite</span><span class="sxs-lookup"><span data-stu-id="74881-190">Create a Workrite test user</span></span>

<span data-ttu-id="74881-191">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Workrite.</span><span class="sxs-lookup"><span data-stu-id="74881-191">hello objective of this section is toocreate a user called Britta Simon in Workrite.</span></span>

<span data-ttu-id="74881-192">**toocreate en användare som kallas Britta Simon i Workrite, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="74881-192">**toocreate a user called Britta Simon in Workrite, perform hello following steps:**</span></span>

1. <span data-ttu-id="74881-193">Inloggning tooyour workrite företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="74881-193">Sign on tooyour workrite company site as administrator.</span></span>

2. <span data-ttu-id="74881-194">Hello navigeringsfönstret, klicka på **Admin**.</span><span class="sxs-lookup"><span data-stu-id="74881-194">In hello navigation pane, click **Admin**.</span></span>
   
    ![-Administratörer kontroll][400]

3. <span data-ttu-id="74881-196">Gå tooQuick länkar och klicka sedan på **skapar du en användare**.</span><span class="sxs-lookup"><span data-stu-id="74881-196">Go tooQuick Links, and then click **Create a User**.</span></span>
   
    ![Skapa användare][401]

4. <span data-ttu-id="74881-198">På hello **skapa användare** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="74881-198">On hello **Create User** dialog, perform hello following steps:</span></span>
   
    ![Skapa användare Dailog][402]
    
    <span data-ttu-id="74881-200">a.</span><span class="sxs-lookup"><span data-stu-id="74881-200">a.</span></span> <span data-ttu-id="74881-201">I hello **e-post** textruta typen hello användarens e-postadress som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="74881-201">In hello **Email** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="74881-202">b.</span><span class="sxs-lookup"><span data-stu-id="74881-202">b.</span></span> <span data-ttu-id="74881-203">I hello **Förnamn** textruta typen hello firstname för användare som Britta.</span><span class="sxs-lookup"><span data-stu-id="74881-203">In hello **First Name** textbox, type hello firstname of user like Britta.</span></span>

    <span data-ttu-id="74881-204">c.</span><span class="sxs-lookup"><span data-stu-id="74881-204">c.</span></span> <span data-ttu-id="74881-205">I hello **efternamn** textruta typen hello efternamn för användaren som Simon.</span><span class="sxs-lookup"><span data-stu-id="74881-205">In hello **Surname** textbox, type hello surname of user like Simon.</span></span>
    
    <span data-ttu-id="74881-206">d.</span><span class="sxs-lookup"><span data-stu-id="74881-206">d.</span></span> <span data-ttu-id="74881-207">Välj **administratör** som **Välj rollen**.</span><span class="sxs-lookup"><span data-stu-id="74881-207">Select **Client Administrator** as **Choose Role**.</span></span>
    
    <span data-ttu-id="74881-208">e.</span><span class="sxs-lookup"><span data-stu-id="74881-208">e.</span></span> <span data-ttu-id="74881-209">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="74881-209">Click **Save**.</span></span>   

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="74881-210">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="74881-210">Assign hello Azure AD test user</span></span>

<span data-ttu-id="74881-211">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooWorkrite.</span><span class="sxs-lookup"><span data-stu-id="74881-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWorkrite.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="74881-213">**tooassign Britta Simon tooWorkrite utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="74881-213">**tooassign Britta Simon tooWorkrite, perform hello following steps:**</span></span>

1. <span data-ttu-id="74881-214">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="74881-214">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="74881-216">Välj i listan med program hello **Workrite**.</span><span class="sxs-lookup"><span data-stu-id="74881-216">In hello applications list, select **Workrite**.</span></span>

    ![Hej Workrite länken i listan med program hello](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_app.png)  

3. <span data-ttu-id="74881-218">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="74881-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="74881-220">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="74881-220">Click **Add** button.</span></span> <span data-ttu-id="74881-221">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="74881-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="74881-223">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="74881-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="74881-224">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="74881-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="74881-225">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="74881-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="74881-226">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="74881-226">Test single sign-on</span></span>

<span data-ttu-id="74881-227">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="74881-227">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="74881-228">Du bör få automatiskt inloggade tooyour Workrite programmet när du klickar på hello Workrite panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="74881-228">When you click hello Workrite tile in hello Access Panel, you should get automatically signed-on tooyour Workrite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="74881-229">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="74881-229">Additional resources</span></span>

* [<span data-ttu-id="74881-230">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="74881-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="74881-231">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="74881-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_203.png
[400]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_400.png
[401]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_401.png
[402]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_402.png

