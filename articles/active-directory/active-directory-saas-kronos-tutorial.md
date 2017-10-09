---
title: "Självstudier: Azure Active Directory-integrering med Kronos | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Kronos."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e28d6191-c375-43c6-b2df-22daa88d9939
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 16fd5c203162d10b78f51b00d79017adaf8632c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kronos"></a><span data-ttu-id="8b410-103">Självstudier: Azure Active Directory-integrering med Kronos</span><span class="sxs-lookup"><span data-stu-id="8b410-103">Tutorial: Azure Active Directory integration with Kronos</span></span>

<span data-ttu-id="8b410-104">I kursen får du lära dig hur toointegrate Kronos med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="8b410-104">In this tutorial, you learn how toointegrate Kronos with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8b410-105">Integrera Kronos med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="8b410-105">Integrating Kronos with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8b410-106">Du kan styra i Azure AD som har åtkomst till tooKronos</span><span class="sxs-lookup"><span data-stu-id="8b410-106">You can control in Azure AD who has access tooKronos</span></span>
- <span data-ttu-id="8b410-107">Du kan aktivera din användare tooautomatically get inloggade tooKronos (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="8b410-107">You can enable your users tooautomatically get signed-on tooKronos (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8b410-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8b410-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8b410-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8b410-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8b410-110">Krav</span><span class="sxs-lookup"><span data-stu-id="8b410-110">Prerequisites</span></span>

<span data-ttu-id="8b410-111">tooconfigure Azure AD-integrering med Kronos, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="8b410-111">tooconfigure Azure AD integration with Kronos, you need hello following items:</span></span>

- <span data-ttu-id="8b410-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="8b410-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8b410-113">En **Kronos arbetsstyrka centrala** SSO aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="8b410-113">A **Kronos Workforce Central** SSO enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8b410-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="8b410-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8b410-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="8b410-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8b410-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="8b410-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8b410-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8b410-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8b410-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="8b410-118">Scenario description</span></span>
<span data-ttu-id="8b410-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="8b410-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8b410-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="8b410-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8b410-121">Att lägga till Kronos från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="8b410-121">Adding Kronos from hello gallery</span></span>
2. <span data-ttu-id="8b410-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8b410-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kronos-from-hello-gallery"></a><span data-ttu-id="8b410-123">Att lägga till Kronos från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="8b410-123">Adding Kronos from hello gallery</span></span>
<span data-ttu-id="8b410-124">tooconfigure hello integrering av Kronos i Azure AD, behöver du tooadd Kronos hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="8b410-124">tooconfigure hello integration of Kronos into Azure AD, you need tooadd Kronos from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8b410-125">**tooadd Kronos från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8b410-125">**tooadd Kronos from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8b410-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8b410-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8b410-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="8b410-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8b410-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="8b410-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="8b410-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8b410-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="8b410-133">Skriv i sökrutan hello **Kronos**.</span><span class="sxs-lookup"><span data-stu-id="8b410-133">In hello search box, type **Kronos**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_search.png)

5. <span data-ttu-id="8b410-135">Markera hello resultat på panelen **Kronos**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="8b410-135">In hello results panel, select **Kronos**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8b410-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8b410-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8b410-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Kronos baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="8b410-138">In this section, you configure and test Azure AD single sign-on with Kronos based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8b410-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Kronos är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8b410-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Kronos is tooa user in Azure AD.</span></span> <span data-ttu-id="8b410-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Kronos toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="8b410-140">In other words, a link relationship between an Azure AD user and hello related user in Kronos needs toobe established.</span></span>

<span data-ttu-id="8b410-141">I Kronos, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="8b410-141">In Kronos, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="8b410-142">tooconfigure och testa Azure AD enkel inloggning med Kronos, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="8b410-142">tooconfigure and test Azure AD single sign-on with Kronos, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8b410-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="8b410-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8b410-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8b410-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8b410-145">**[Skapa en testanvändare Kronos](#creating-a-kronos-test-user)**  -toohave en motsvarighet för Britta Simon i Kronos som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="8b410-145">**[Creating a Kronos test user](#creating-a-kronos-test-user)** - toohave a counterpart of Britta Simon in Kronos that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8b410-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8b410-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8b410-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="8b410-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8b410-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8b410-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8b410-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Kronos program.</span><span class="sxs-lookup"><span data-stu-id="8b410-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Kronos application.</span></span>

<span data-ttu-id="8b410-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Kronos:**</span><span class="sxs-lookup"><span data-stu-id="8b410-150">**tooconfigure Azure AD single sign-on with Kronos, perform hello following steps:**</span></span>

1. <span data-ttu-id="8b410-151">I hello Azure-portalen på hello **Kronos** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="8b410-151">In hello Azure portal, on hello **Kronos** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="8b410-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8b410-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_samlbase.png)

3. <span data-ttu-id="8b410-155">På hello **Kronos domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8b410-155">On hello **Kronos Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_url.png)

    <span data-ttu-id="8b410-157">a.</span><span class="sxs-lookup"><span data-stu-id="8b410-157">a.</span></span> <span data-ttu-id="8b410-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.kronos.net/`</span><span class="sxs-lookup"><span data-stu-id="8b410-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.kronos.net/`</span></span>

    <span data-ttu-id="8b410-159">b.</span><span class="sxs-lookup"><span data-stu-id="8b410-159">b.</span></span> <span data-ttu-id="8b410-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.kronos.net/wfc/navigator/logonWithUID`</span><span class="sxs-lookup"><span data-stu-id="8b410-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.kronos.net/wfc/navigator/logonWithUID`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8b410-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="8b410-161">These values are not real.</span></span> <span data-ttu-id="8b410-162">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="8b410-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="8b410-163">Kontakta [Kronos supportteam](https://www.kronos.in/contact/en-in/form) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="8b410-163">Contact [Kronos support team](https://www.kronos.in/contact/en-in/form) tooget these values.</span></span>
 
4. <span data-ttu-id="8b410-164">Tillämpningsprogrammet Kronos förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="8b410-164">Your Kronos application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="8b410-165">Arbeta med [Kronos supportteam](https://www.kronos.in/contact/en-in/form) första tooidentify hello rätt användar-ID, som är mappad till hello program.</span><span class="sxs-lookup"><span data-stu-id="8b410-165">Work with [Kronos support team](https://www.kronos.in/contact/en-in/form) first tooidentify hello correct user identifier, which is mapped into hello application.</span></span> <span data-ttu-id="8b410-166">Ta dig hello vägledning om hello-attribut som de vill också toouse för att mappningen.</span><span class="sxs-lookup"><span data-stu-id="8b410-166">Also please take hello guidance about hello attribute, which they want toouse for this mapping.</span></span>
 
     <span data-ttu-id="8b410-167">Microsoft rekommenderar att du använder hello **”NameIdentifier”** attribut som användaridentifierare.</span><span class="sxs-lookup"><span data-stu-id="8b410-167">Microsoft recommends using hello **"NameIdentifier"** attribute as user identifier.</span></span> <span data-ttu-id="8b410-168">Du kan hantera hello värden för attributen från hello **”användarattribut”** avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="8b410-168">You can manage hello values of these attributes from hello **"User Attributes"** section on application integration page.</span></span>
     
     <span data-ttu-id="8b410-169">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="8b410-169">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="8b410-170">Vi har här mappat hello **användar-ID (nameid)** med **ExtractMailPrefix()** funktion **user.userprincipalname**.</span><span class="sxs-lookup"><span data-stu-id="8b410-170">Here we have mapped hello **User Identifier (nameid)** with **ExtractMailPrefix()** function of **user.userprincipalname**.</span></span> <span data-ttu-id="8b410-171">Detta ger hello prefixvärdet av e-post för hello-användare som är hello unikt användar-ID.</span><span class="sxs-lookup"><span data-stu-id="8b410-171">This gives hello prefix value of email of hello user which is hello unique User ID.</span></span> <span data-ttu-id="8b410-172">Toohello Kronos program skickas i varje lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="8b410-172">This is sent toohello Kronos application in every successful response.</span></span> 
     
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_attribute.png)

5. <span data-ttu-id="8b410-174">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="8b410-174">In hello **User Attributes** section on hello **Single sign-on** dialog:</span></span>

    <span data-ttu-id="8b410-175">a.</span><span class="sxs-lookup"><span data-stu-id="8b410-175">a.</span></span> <span data-ttu-id="8b410-176">Välj i listrutan för hello användar-ID, **ExtractMailPrefix**.</span><span class="sxs-lookup"><span data-stu-id="8b410-176">In hello User Identifier dropdown list, select **ExtractMailPrefix**.</span></span>

    <span data-ttu-id="8b410-177">b.</span><span class="sxs-lookup"><span data-stu-id="8b410-177">b.</span></span> <span data-ttu-id="8b410-178">I hello **e** listrutan, Välj **user.userprincipalname**.</span><span class="sxs-lookup"><span data-stu-id="8b410-178">In hello **Mail** dropdown list, select **user.userprincipalname**.</span></span>

6. <span data-ttu-id="8b410-179">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="8b410-179">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_certificate.png) 

7. <span data-ttu-id="8b410-181">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="8b410-181">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kronos-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="8b410-183">tooconfigure enkel inloggning på **Kronos** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Kronos supportteam](https://www.kronos.in/contact/en-in/form).</span><span class="sxs-lookup"><span data-stu-id="8b410-183">tooconfigure single sign-on on **Kronos** side, you need toosend hello downloaded **Metadata XML** too[Kronos support team](https://www.kronos.in/contact/en-in/form).</span></span> 

> [!TIP]
> <span data-ttu-id="8b410-184">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="8b410-184">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8b410-185">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="8b410-185">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8b410-186">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8b410-186">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8b410-187">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="8b410-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="8b410-188">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8b410-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="8b410-190">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8b410-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8b410-191">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8b410-191">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8b410-193">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="8b410-193">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8b410-195">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8b410-195">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8b410-197">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8b410-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8b410-199">a.</span><span class="sxs-lookup"><span data-stu-id="8b410-199">a.</span></span> <span data-ttu-id="8b410-200">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8b410-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8b410-201">b.</span><span class="sxs-lookup"><span data-stu-id="8b410-201">b.</span></span> <span data-ttu-id="8b410-202">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8b410-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8b410-203">c.</span><span class="sxs-lookup"><span data-stu-id="8b410-203">c.</span></span> <span data-ttu-id="8b410-204">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="8b410-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8b410-205">d.</span><span class="sxs-lookup"><span data-stu-id="8b410-205">d.</span></span> <span data-ttu-id="8b410-206">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="8b410-206">Click **Create**.</span></span>
 
### <a name="creating-a-kronos-test-user"></a><span data-ttu-id="8b410-207">Skapa en testanvändare Kronos</span><span class="sxs-lookup"><span data-stu-id="8b410-207">Creating a Kronos test user</span></span>

<span data-ttu-id="8b410-208">I det här avsnittet skapar du en användare som kallas Britta Simon i Kronos.</span><span class="sxs-lookup"><span data-stu-id="8b410-208">In this section, you create a user called Britta Simon in Kronos.</span></span> <span data-ttu-id="8b410-209">Kronos programmet måste alla hello användare toobe etableras i hello program innan du utför enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8b410-209">Kronos application needs all hello users toobe provisioned in hello application before doing SSO.</span></span> 

<span data-ttu-id="8b410-210">Arbeta med hello [Kronos supportteam](https://www.kronos.in/contact/en-in/form) tooprovision dessa användare till hello program.</span><span class="sxs-lookup"><span data-stu-id="8b410-210">Work with hello [Kronos support team](https://www.kronos.in/contact/en-in/form) tooprovision all these users into hello application.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8b410-211">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="8b410-211">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8b410-212">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooKronos.</span><span class="sxs-lookup"><span data-stu-id="8b410-212">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKronos.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="8b410-214">**tooassign Britta Simon tooKronos utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8b410-214">**tooassign Britta Simon tooKronos, perform hello following steps:**</span></span>

1. <span data-ttu-id="8b410-215">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8b410-215">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="8b410-217">Välj i listan med program hello **Kronos**.</span><span class="sxs-lookup"><span data-stu-id="8b410-217">In hello applications list, select **Kronos**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_app.png) 

3. <span data-ttu-id="8b410-219">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="8b410-219">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="8b410-221">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="8b410-221">Click **Add** button.</span></span> <span data-ttu-id="8b410-222">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8b410-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="8b410-224">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="8b410-224">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8b410-225">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8b410-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8b410-226">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8b410-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8b410-227">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8b410-227">Testing single sign-on</span></span>

<span data-ttu-id="8b410-228">I det här avsnittet kan du testa din Azure AD SSO-konfiguration med hjälp av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="8b410-228">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="8b410-229">Du bör få automatiskt inloggade tooyour Kronos programmet när du klickar på hello Kronos panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="8b410-229">When you click hello Kronos tile in hello Access Panel, you should get automatically signed-on tooyour Kronos application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8b410-230">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="8b410-230">Additional resources</span></span>

* [<span data-ttu-id="8b410-231">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8b410-231">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8b410-232">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8b410-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_203.png

