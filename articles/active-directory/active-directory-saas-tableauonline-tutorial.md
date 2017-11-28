---
title: "Självstudier: Azure Active Directory-integrering med Tableau Online | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Tableau Online."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d4b1149-ba3b-4f4e-8bce-9791316b730d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 590b2674270c340b4750c7b6feeaf4f0df4bf853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a><span data-ttu-id="d3b24-103">Självstudier: Azure Active Directory-integrering med Tableau Online</span><span class="sxs-lookup"><span data-stu-id="d3b24-103">Tutorial: Azure Active Directory integration with Tableau Online</span></span>

<span data-ttu-id="d3b24-104">I kursen får du lära dig hur toointegrate Tableau Online med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d3b24-104">In this tutorial, you learn how toointegrate Tableau Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d3b24-105">Integrera Tableau Online med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d3b24-105">Integrating Tableau Online with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d3b24-106">Du kan styra i Azure AD som har åtkomst tooTableau Online</span><span class="sxs-lookup"><span data-stu-id="d3b24-106">You can control in Azure AD who has access tooTableau Online</span></span>
- <span data-ttu-id="d3b24-107">Du kan aktivera din användare tooautomatically get inloggade tooTableau Online (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d3b24-107">You can enable your users tooautomatically get signed-on tooTableau Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d3b24-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d3b24-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d3b24-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d3b24-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d3b24-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d3b24-110">Prerequisites</span></span>

<span data-ttu-id="d3b24-111">tooconfigure Azure AD-integrering med Tableau Online, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="d3b24-111">tooconfigure Azure AD integration with Tableau Online, you need hello following items:</span></span>

- <span data-ttu-id="d3b24-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d3b24-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d3b24-113">En Tableau Online enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="d3b24-113">A Tableau Online single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d3b24-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d3b24-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d3b24-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d3b24-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d3b24-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d3b24-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d3b24-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d3b24-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d3b24-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d3b24-118">Scenario description</span></span>
<span data-ttu-id="d3b24-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d3b24-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d3b24-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d3b24-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d3b24-121">Att lägga till Tableau Online från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d3b24-121">Adding Tableau Online from hello gallery</span></span>
2. <span data-ttu-id="d3b24-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d3b24-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tableau-online-from-hello-gallery"></a><span data-ttu-id="d3b24-123">Att lägga till Tableau Online från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d3b24-123">Adding Tableau Online from hello gallery</span></span>
<span data-ttu-id="d3b24-124">tooconfigure hello integrering av Tableau Online i Azure AD, behöver du tooadd Tableau Online hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="d3b24-124">tooconfigure hello integration of Tableau Online into Azure AD, you need tooadd Tableau Online from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d3b24-125">**tooadd Tableau Online från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d3b24-125">**tooadd Tableau Online from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d3b24-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d3b24-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d3b24-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d3b24-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d3b24-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="d3b24-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d3b24-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d3b24-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d3b24-133">Skriv i sökrutan hello **Tableau Online**.</span><span class="sxs-lookup"><span data-stu-id="d3b24-133">In hello search box, type **Tableau Online**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_search.png)

5. <span data-ttu-id="d3b24-135">Markera hello resultat på panelen **Tableau Online**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="d3b24-135">In hello results panel, select **Tableau Online**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d3b24-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d3b24-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d3b24-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Tableau Online baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d3b24-138">In this section, you configure and test Azure AD single sign-on with Tableau Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d3b24-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Tableau Online är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d3b24-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Tableau Online is tooa user in Azure AD.</span></span> <span data-ttu-id="d3b24-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Tableau Online toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="d3b24-140">In other words, a link relationship between an Azure AD user and hello related user in Tableau Online needs toobe established.</span></span>

<span data-ttu-id="d3b24-141">Tilldela hello värdet hello i Tableau Online **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="d3b24-141">In Tableau Online, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d3b24-142">tooconfigure och testa Azure AD enkel inloggning med Tableau Online, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d3b24-142">tooconfigure and test Azure AD single sign-on with Tableau Online, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d3b24-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d3b24-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d3b24-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d3b24-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d3b24-145">**[Skapa en Tableau Online testanvändare](#creating-a-tableau-online-test-user)**  -toohave en motsvarighet för Britta Simon i Tableau Online som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d3b24-145">**[Creating a Tableau Online test user](#creating-a-tableau-online-test-user)** - toohave a counterpart of Britta Simon in Tableau Online that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d3b24-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d3b24-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d3b24-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d3b24-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d3b24-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d3b24-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d3b24-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Tableau Online.</span><span class="sxs-lookup"><span data-stu-id="d3b24-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Tableau Online application.</span></span>

<span data-ttu-id="d3b24-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med Tableau Online:**</span><span class="sxs-lookup"><span data-stu-id="d3b24-150">**tooconfigure Azure AD single sign-on with Tableau Online, perform hello following steps:**</span></span>

1. <span data-ttu-id="d3b24-151">I hello Azure-portalen på hello **Tableau Online** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d3b24-151">In hello Azure portal, on hello **Tableau Online** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d3b24-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d3b24-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_samlbase.png)

3. <span data-ttu-id="d3b24-155">På hello **Tableau Online domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d3b24-155">On hello **Tableau Online Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_url.png)
    
    <span data-ttu-id="d3b24-157">a.</span><span class="sxs-lookup"><span data-stu-id="d3b24-157">a.</span></span> <span data-ttu-id="d3b24-158">I hello **inloggnings-URL** textruta typen hello URL:`https://sso.online.tableau.com`</span><span class="sxs-lookup"><span data-stu-id="d3b24-158">In hello **Sign-on URL** textbox, type hello URL: `https://sso.online.tableau.com`</span></span>

    <span data-ttu-id="d3b24-159">b.</span><span class="sxs-lookup"><span data-stu-id="d3b24-159">b.</span></span> <span data-ttu-id="d3b24-160">I hello **identifierare** textruta typen hello URL:`https://sso.online.tableau.com/public/sp/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="d3b24-160">In hello **Identifier** textbox, type hello URL: `https://sso.online.tableau.com/public/sp/<instancename>`</span></span>

4. <span data-ttu-id="d3b24-161">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="d3b24-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_certificate.png) 

5. <span data-ttu-id="d3b24-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d3b24-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d3b24-165">I ett annat fönster i webbläsaren, inloggning tooyour Tableau Online-programmet.</span><span class="sxs-lookup"><span data-stu-id="d3b24-165">In a different browser window, sign-on tooyour Tableau Online application.</span></span> <span data-ttu-id="d3b24-166">Gå för**inställningar** och sedan **autentisering**.</span><span class="sxs-lookup"><span data-stu-id="d3b24-166">Go too**Settings** and then **Authentication**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)
    
7. <span data-ttu-id="d3b24-168">tooenable SAML under **autentiseringstyper** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d3b24-168">tooenable SAML, Under **Authentication Types** section.</span></span> <span data-ttu-id="d3b24-169">Kontrollera hello **enkel inloggning med SAML** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="d3b24-169">Check hello **Single sign-on with SAML** checkbox.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

8. <span data-ttu-id="d3b24-171">Rulla nedåt tills **metadata importfilen till Tableau Online** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d3b24-171">Scroll down until **Import metadata file into Tableau Online** section.</span></span>  <span data-ttu-id="d3b24-172">Klicka på Bläddra och importera hello metadatafil som du har hämtat från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d3b24-172">Click Browse and import hello metadata file you have downloaded from Azure AD.</span></span> <span data-ttu-id="d3b24-173">Klicka på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="d3b24-173">Then, click **Apply**.</span></span>
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

9. <span data-ttu-id="d3b24-175">I hello **matchar intyg** avsnittet, infoga hello motsvarande identitetsleverantör assertion namn för **e-postadress**, **Förnamn**, och **efternamn** .</span><span class="sxs-lookup"><span data-stu-id="d3b24-175">In hello **Match assertions** section, insert hello corresponding Identity Provider assertion name for **email address**, **first name**, and **last name**.</span></span> <span data-ttu-id="d3b24-176">tooget informationen från Azure AD:</span><span class="sxs-lookup"><span data-stu-id="d3b24-176">tooget this information from Azure AD:</span></span> 
  
    <span data-ttu-id="d3b24-177">a.</span><span class="sxs-lookup"><span data-stu-id="d3b24-177">a.</span></span> <span data-ttu-id="d3b24-178">I hello Azure-portalen, går du vidare hello **Tableau Online** sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="d3b24-178">In hello Azure portal, go on hello **Tableau Online** application integration page.</span></span>
    
    <span data-ttu-id="d3b24-179">b.</span><span class="sxs-lookup"><span data-stu-id="d3b24-179">b.</span></span> <span data-ttu-id="d3b24-180">Välj hello hello attribut under **”visa och redigera andra användarattribut”** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="d3b24-180">In hello attributes section, Select hello **"view and edit all other user attributes"** checkbox.</span></span> 
    
   ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/attributesection.png)
      
    <span data-ttu-id="d3b24-182">c.</span><span class="sxs-lookup"><span data-stu-id="d3b24-182">c.</span></span> <span data-ttu-id="d3b24-183">Kopiera hello namnområdesvärdet för dessa attribut: givenname, e- och efternamn med hjälp av hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d3b24-183">Copy hello namespace value for these attributes: givenname, email and surname by using hello following steps:</span></span>

   ![Azure AD-Single Sign-On](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)
    
    <span data-ttu-id="d3b24-185">d.</span><span class="sxs-lookup"><span data-stu-id="d3b24-185">d.</span></span> <span data-ttu-id="d3b24-186">Klicka på **user.givenname** värde</span><span class="sxs-lookup"><span data-stu-id="d3b24-186">Click **user.givenname** value</span></span> 
    
    <span data-ttu-id="d3b24-187">e.</span><span class="sxs-lookup"><span data-stu-id="d3b24-187">e.</span></span> <span data-ttu-id="d3b24-188">Kopiera hello värdet från hello **namnområde** textruta.</span><span class="sxs-lookup"><span data-stu-id="d3b24-188">Copy hello value from hello **namespace** textbox.</span></span>

   ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/attributesection2.png)

    <span data-ttu-id="d3b24-190">f.</span><span class="sxs-lookup"><span data-stu-id="d3b24-190">f.</span></span> <span data-ttu-id="d3b24-191">toocopy hello namesapce värden för hello e-post och efternamn Följ hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="d3b24-191">toocopy hello namesapce values for hello email and surname follow hello preceding steps.</span></span>

    <span data-ttu-id="d3b24-192">g.</span><span class="sxs-lookup"><span data-stu-id="d3b24-192">g.</span></span> <span data-ttu-id="d3b24-193">Växla till toohello Tableau Online program och sedan ange hello **Tableau Onlineattribut** avsnittet på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d3b24-193">Switch toohello Tableau Online application, then set hello **Tableau Online Attributes** section as follows:</span></span>
     * <span data-ttu-id="d3b24-194">E-post: **e** eller **userprincipalname**</span><span class="sxs-lookup"><span data-stu-id="d3b24-194">Email: **mail** or **userprincipalname**</span></span>
     * <span data-ttu-id="d3b24-195">Förnamn: **givenname**</span><span class="sxs-lookup"><span data-stu-id="d3b24-195">First name: **givenname**</span></span>
     * <span data-ttu-id="d3b24-196">Efternamn: **efternamn**</span><span class="sxs-lookup"><span data-stu-id="d3b24-196">Last name: **surname**</span></span>
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

> [!TIP]
> <span data-ttu-id="d3b24-198">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="d3b24-198">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d3b24-199">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="d3b24-199">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d3b24-200">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d3b24-200">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d3b24-201">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d3b24-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="d3b24-202">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d3b24-202">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d3b24-204">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d3b24-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d3b24-205">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d3b24-205">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d3b24-207">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d3b24-207">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d3b24-209">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d3b24-209">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d3b24-211">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d3b24-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d3b24-213">a.</span><span class="sxs-lookup"><span data-stu-id="d3b24-213">a.</span></span> <span data-ttu-id="d3b24-214">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d3b24-214">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d3b24-215">b.</span><span class="sxs-lookup"><span data-stu-id="d3b24-215">b.</span></span> <span data-ttu-id="d3b24-216">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d3b24-216">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d3b24-217">c.</span><span class="sxs-lookup"><span data-stu-id="d3b24-217">c.</span></span> <span data-ttu-id="d3b24-218">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d3b24-218">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d3b24-219">d.</span><span class="sxs-lookup"><span data-stu-id="d3b24-219">d.</span></span> <span data-ttu-id="d3b24-220">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d3b24-220">Click **Create**.</span></span>
 
### <a name="creating-a-tableau-online-test-user"></a><span data-ttu-id="d3b24-221">Skapa en Tableau Online testanvändare</span><span class="sxs-lookup"><span data-stu-id="d3b24-221">Creating a Tableau Online test user</span></span>

<span data-ttu-id="d3b24-222">I det här avsnittet skapar du en användare som kallas Britta Simon i Tableau Online.</span><span class="sxs-lookup"><span data-stu-id="d3b24-222">In this section, you create a user called Britta Simon in Tableau Online.</span></span>

1. <span data-ttu-id="d3b24-223">På **Tableau Online**, klickar du på **inställningar** och sedan **autentisering** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d3b24-223">On **Tableau Online**, click **Settings** and then **Authentication** section.</span></span> <span data-ttu-id="d3b24-224">Rulla nedåt för**Välj användare** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d3b24-224">Scroll down too**Select Users** section.</span></span> <span data-ttu-id="d3b24-225">Klicka på **lägga till användare** och sedan **ange e-postadresser**.</span><span class="sxs-lookup"><span data-stu-id="d3b24-225">Click **Add Users** and then **Enter Email Addresses**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. <span data-ttu-id="d3b24-227">Välj **lägga till användare för enkel inloggning (SSO) autentisering**.</span><span class="sxs-lookup"><span data-stu-id="d3b24-227">Select **Add users for single sign-on (SSO) authentication**.</span></span> <span data-ttu-id="d3b24-228">I hello **ange e-postadresser** textruta Lägg tillbritta.simon@contoso.com</span><span class="sxs-lookup"><span data-stu-id="d3b24-228">In hello **Enter Email Addresses** textbox add britta.simon@contoso.com</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)
3. <span data-ttu-id="d3b24-230">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d3b24-230">Click **Create**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d3b24-231">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="d3b24-231">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d3b24-232">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTableau Online.</span><span class="sxs-lookup"><span data-stu-id="d3b24-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTableau Online.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d3b24-234">**tooassign Britta Simon tooTableau Online, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="d3b24-234">**tooassign Britta Simon tooTableau Online, perform hello following steps:**</span></span>

1. <span data-ttu-id="d3b24-235">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d3b24-235">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d3b24-237">Välj i listan med program hello **Tableau Online**.</span><span class="sxs-lookup"><span data-stu-id="d3b24-237">In hello applications list, select **Tableau Online**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_app.png) 

3. <span data-ttu-id="d3b24-239">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d3b24-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d3b24-241">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d3b24-241">Click **Add** button.</span></span> <span data-ttu-id="d3b24-242">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d3b24-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d3b24-244">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="d3b24-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d3b24-245">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d3b24-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d3b24-246">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d3b24-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d3b24-247">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d3b24-247">Testing single sign-on</span></span>

<span data-ttu-id="d3b24-248">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d3b24-248">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="d3b24-249">Du bör få automatiskt inloggade tooyour Tableau Online programmet när du klickar på hello Tableau Online panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d3b24-249">When you click hello Tableau Online tile in hello Access Panel, you should get automatically signed-on tooyour Tableau Online application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d3b24-250">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d3b24-250">Additional resources</span></span>

* [<span data-ttu-id="d3b24-251">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d3b24-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d3b24-252">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d3b24-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png

