---
title: "Självstudier: Azure Active Directory-integrering med M-filer | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och M-filer."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4536fd49-3a65-4cff-9620-860904f726d0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: e1d268da53312b1d2c12e29d66b1a5b66d0cdcce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-m-files"></a><span data-ttu-id="651a8-103">Självstudier: Azure Active Directory-integrering med M-filer</span><span class="sxs-lookup"><span data-stu-id="651a8-103">Tutorial: Azure Active Directory integration with M-Files</span></span>

<span data-ttu-id="651a8-104">I kursen får du lära dig hur toointegrate M-filer med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="651a8-104">In this tutorial, you learn how toointegrate M-Files with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="651a8-105">Integrera M-filer med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="651a8-105">Integrating M-Files with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="651a8-106">Du kan styra i Azure AD som har åtkomst till tooM-filer</span><span class="sxs-lookup"><span data-stu-id="651a8-106">You can control in Azure AD who has access tooM-Files</span></span>
- <span data-ttu-id="651a8-107">Du kan aktivera användare tooautomatically get inloggade tooM-filer (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="651a8-107">You can enable your users tooautomatically get signed-on tooM-Files (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="651a8-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="651a8-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="651a8-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="651a8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="651a8-110">Krav</span><span class="sxs-lookup"><span data-stu-id="651a8-110">Prerequisites</span></span>

<span data-ttu-id="651a8-111">tooconfigure Azure AD-integrering med M-filer, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="651a8-111">tooconfigure Azure AD integration with M-Files, you need hello following items:</span></span>

- <span data-ttu-id="651a8-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="651a8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="651a8-113">En M-filer enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="651a8-113">A M-Files single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="651a8-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="651a8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="651a8-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="651a8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="651a8-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="651a8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="651a8-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="651a8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="651a8-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="651a8-118">Scenario description</span></span>
<span data-ttu-id="651a8-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="651a8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="651a8-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="651a8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="651a8-121">Lägga till M-filer från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="651a8-121">Adding M-Files from hello gallery</span></span>
2. <span data-ttu-id="651a8-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="651a8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-m-files-from-hello-gallery"></a><span data-ttu-id="651a8-123">Lägga till M-filer från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="651a8-123">Adding M-Files from hello gallery</span></span>
<span data-ttu-id="651a8-124">tooconfigure hello integrering av M-filer i Azure AD, behöver du tooadd M-filer från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="651a8-124">tooconfigure hello integration of M-Files into Azure AD, you need tooadd M-Files from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="651a8-125">**tooadd M-filer från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="651a8-125">**tooadd M-Files from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="651a8-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="651a8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="651a8-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="651a8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="651a8-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="651a8-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="651a8-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="651a8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="651a8-133">Skriv i sökrutan hello **M-filer**.</span><span class="sxs-lookup"><span data-stu-id="651a8-133">In hello search box, type **M-Files**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_search.png)

5. <span data-ttu-id="651a8-135">Markera hello resultat på panelen **M filer**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="651a8-135">In hello results panel, select **M-Files**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="651a8-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="651a8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="651a8-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med M-filer baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="651a8-138">In this section, you configure and test Azure AD single sign-on with M-Files based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="651a8-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i M-filer är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="651a8-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in M-Files is tooa user in Azure AD.</span></span> <span data-ttu-id="651a8-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i M-filer toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="651a8-140">In other words, a link relationship between an Azure AD user and hello related user in M-Files needs toobe established.</span></span>

<span data-ttu-id="651a8-141">I M-filer, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="651a8-141">In M-Files, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="651a8-142">tooconfigure och testa Azure AD enkel inloggning med M-filer, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="651a8-142">tooconfigure and test Azure AD single sign-on with M-Files, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="651a8-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="651a8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="651a8-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="651a8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="651a8-145">**[Skapa en testanvändare M filer](#creating-a-m-files-test-user)**  -toohave en motsvarighet för Britta Simon i M-filer som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="651a8-145">**[Creating a M-Files test user](#creating-a-m-files-test-user)** - toohave a counterpart of Britta Simon in M-Files that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="651a8-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="651a8-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="651a8-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="651a8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="651a8-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="651a8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="651a8-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet M-filer.</span><span class="sxs-lookup"><span data-stu-id="651a8-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your M-Files application.</span></span>

<span data-ttu-id="651a8-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med M-filer:**</span><span class="sxs-lookup"><span data-stu-id="651a8-150">**tooconfigure Azure AD single sign-on with M-Files, perform hello following steps:**</span></span>

1. <span data-ttu-id="651a8-151">I hello Azure-portalen på hello **M filer** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="651a8-151">In hello Azure portal, on hello **M-Files** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="651a8-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="651a8-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_samlbase.png)

3. <span data-ttu-id="651a8-155">På hello **M filer domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="651a8-155">On hello **M-Files Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_url.png)

    <span data-ttu-id="651a8-157">a.</span><span class="sxs-lookup"><span data-stu-id="651a8-157">a.</span></span> <span data-ttu-id="651a8-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenantname>.cloudvault.m-files.com/authentication/MFiles.AuthenticationProviders.Core/sso`</span><span class="sxs-lookup"><span data-stu-id="651a8-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenantname>.cloudvault.m-files.com/authentication/MFiles.AuthenticationProviders.Core/sso`</span></span>

    <span data-ttu-id="651a8-159">b.</span><span class="sxs-lookup"><span data-stu-id="651a8-159">b.</span></span> <span data-ttu-id="651a8-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenantname>.cloudvault.m-files.com`</span><span class="sxs-lookup"><span data-stu-id="651a8-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenantname>.cloudvault.m-files.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="651a8-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="651a8-161">These values are not real.</span></span> <span data-ttu-id="651a8-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="651a8-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="651a8-163">Kontakta [M-filer som klienten supportteamet](mailto:support@m-files.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="651a8-163">Contact [M-Files Client support team](mailto:support@m-files.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="651a8-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="651a8-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_certificate.png) 

5. <span data-ttu-id="651a8-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="651a8-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-m-files-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="651a8-168">tooget SSO konfigurerats för ditt program bör du kontakta [M filer supportteam](mailto:support@m-files.com) och ge dem hello hämtas Metadata.</span><span class="sxs-lookup"><span data-stu-id="651a8-168">tooget SSO configured for your application, contact [M-Files support team](mailto:support@m-files.com) and provide them hello downloaded Metadata.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="651a8-169">Följ hello nästa steg om du vill tooconfigure enkel inloggning för dig miljon filen skrivbordsprogram.</span><span class="sxs-lookup"><span data-stu-id="651a8-169">Follow hello next steps if you want tooconfigure SSO for you M-File desktop application.</span></span> <span data-ttu-id="651a8-170">Inga ytterligare åtgärder krävs om du bara vill tooconfigure enkel inloggning för webbversionen M-filer.</span><span class="sxs-lookup"><span data-stu-id="651a8-170">No extra steps are required if you only want tooconfigure SSO for M-Files web version.</span></span>  

7. <span data-ttu-id="651a8-171">Följ hello nästa steg tooconfigure hello M-fil skrivbordsprogram tooenable enkel inloggning med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="651a8-171">Follow hello next steps tooconfigure hello M-File desktop application tooenable SSO with Azure AD.</span></span> <span data-ttu-id="651a8-172">toodownload M-filer, gå för[M filer hämta](https://www.m-files.com/en/download-latest-version) sidan.</span><span class="sxs-lookup"><span data-stu-id="651a8-172">toodownload M-Files, go too[M-Files download](https://www.m-files.com/en/download-latest-version) page.</span></span>

8. <span data-ttu-id="651a8-173">Öppna hello **M filer Desktop inställningar** fönster.</span><span class="sxs-lookup"><span data-stu-id="651a8-173">Open hello **M-Files Desktop Settings** window.</span></span> <span data-ttu-id="651a8-174">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="651a8-174">Then, click **Add**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_10.png)

9. <span data-ttu-id="651a8-176">På hello **dokumentet valvet anslutningsegenskaper** fönstret utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="651a8-176">On hello **Document Vault Connection Properties** window, perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_11.png)  

    <span data-ttu-id="651a8-178">Under hello avsnittet servertyp hello värden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="651a8-178">Under hello Server section type, hello values as follows:</span></span>  

    <span data-ttu-id="651a8-179">a.</span><span class="sxs-lookup"><span data-stu-id="651a8-179">a.</span></span> <span data-ttu-id="651a8-180">För **namn**, typen `<tenant-name>.cloudvault.m-files.com`.</span><span class="sxs-lookup"><span data-stu-id="651a8-180">For **Name**, type `<tenant-name>.cloudvault.m-files.com`.</span></span> 
 
    <span data-ttu-id="651a8-181">b.</span><span class="sxs-lookup"><span data-stu-id="651a8-181">b.</span></span> <span data-ttu-id="651a8-182">För **portnummer**, typen **4466**.</span><span class="sxs-lookup"><span data-stu-id="651a8-182">For **Port Number**, type **4466**.</span></span> 

    <span data-ttu-id="651a8-183">c.</span><span class="sxs-lookup"><span data-stu-id="651a8-183">c.</span></span> <span data-ttu-id="651a8-184">För **protokollet**väljer **HTTPS**.</span><span class="sxs-lookup"><span data-stu-id="651a8-184">For **Protocol**, select **HTTPS**.</span></span> 

    <span data-ttu-id="651a8-185">d.</span><span class="sxs-lookup"><span data-stu-id="651a8-185">d.</span></span> <span data-ttu-id="651a8-186">I hello **autentisering** väljer **specifika Windows-användare**.</span><span class="sxs-lookup"><span data-stu-id="651a8-186">In hello **Authentication** field, select **Specific Windows user**.</span></span> <span data-ttu-id="651a8-187">Du uppmanas sedan med en signering sida.</span><span class="sxs-lookup"><span data-stu-id="651a8-187">Then, you are prompted with a signing page.</span></span> <span data-ttu-id="651a8-188">Infoga Azure AD-autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="651a8-188">Insert your Azure AD credentials.</span></span> 

    <span data-ttu-id="651a8-189">e.</span><span class="sxs-lookup"><span data-stu-id="651a8-189">e.</span></span> <span data-ttu-id="651a8-190">För hello **valvet på servern**, Välj hello motsvarande valv på servern.</span><span class="sxs-lookup"><span data-stu-id="651a8-190">For hello **Vault on Server**,  select hello corresponding vault on server.</span></span>
 
    <span data-ttu-id="651a8-191">f.</span><span class="sxs-lookup"><span data-stu-id="651a8-191">f.</span></span> <span data-ttu-id="651a8-192">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="651a8-192">Click **OK**.</span></span>

> [!TIP]
> <span data-ttu-id="651a8-193">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="651a8-193">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="651a8-194">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="651a8-194">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="651a8-195">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="651a8-195">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="651a8-196">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="651a8-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="651a8-197">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="651a8-197">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="651a8-199">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="651a8-199">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="651a8-200">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="651a8-200">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-m-files-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="651a8-202">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="651a8-202">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-m-files-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="651a8-204">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="651a8-204">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-m-files-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="651a8-206">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="651a8-206">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-m-files-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="651a8-208">a.</span><span class="sxs-lookup"><span data-stu-id="651a8-208">a.</span></span> <span data-ttu-id="651a8-209">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="651a8-209">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="651a8-210">b.</span><span class="sxs-lookup"><span data-stu-id="651a8-210">b.</span></span> <span data-ttu-id="651a8-211">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="651a8-211">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="651a8-212">c.</span><span class="sxs-lookup"><span data-stu-id="651a8-212">c.</span></span> <span data-ttu-id="651a8-213">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="651a8-213">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="651a8-214">d.</span><span class="sxs-lookup"><span data-stu-id="651a8-214">d.</span></span> <span data-ttu-id="651a8-215">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="651a8-215">Click **Create**.</span></span>
 
### <a name="creating-a-m-files-test-user"></a><span data-ttu-id="651a8-216">Skapa en testanvändare M-filer</span><span class="sxs-lookup"><span data-stu-id="651a8-216">Creating a M-Files test user</span></span>

<span data-ttu-id="651a8-217">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i M-filer.</span><span class="sxs-lookup"><span data-stu-id="651a8-217">hello objective of this section is toocreate a user called Britta Simon in M-Files.</span></span> <span data-ttu-id="651a8-218">Arbeta med [M filer supportteam](mailto:support@m-files.com) tooadd hello användare i hello M-filer.</span><span class="sxs-lookup"><span data-stu-id="651a8-218">Work with  [M-Files support team](mailto:support@m-files.com) tooadd hello users in hello M-Files.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="651a8-219">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="651a8-219">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="651a8-220">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst till tooM-filer.</span><span class="sxs-lookup"><span data-stu-id="651a8-220">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooM-Files.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="651a8-222">**tooassign Britta Simon tooM-filer, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="651a8-222">**tooassign Britta Simon tooM-Files, perform hello following steps:**</span></span>

1. <span data-ttu-id="651a8-223">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="651a8-223">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="651a8-225">Välj i listan med program hello **M-filer**.</span><span class="sxs-lookup"><span data-stu-id="651a8-225">In hello applications list, select **M-Files**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_app.png) 

3. <span data-ttu-id="651a8-227">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="651a8-227">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="651a8-229">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="651a8-229">Click **Add** button.</span></span> <span data-ttu-id="651a8-230">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="651a8-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="651a8-232">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="651a8-232">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="651a8-233">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="651a8-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="651a8-234">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="651a8-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="651a8-235">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="651a8-235">Testing single sign-on</span></span>

<span data-ttu-id="651a8-236">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="651a8-236">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="651a8-237">När du klickar på hello M filer panelen i hello åtkomstpanelen, du bör få programmet automatiskt inloggade tooyour M-filer.</span><span class="sxs-lookup"><span data-stu-id="651a8-237">When you click hello M-Files tile in hello Access Panel, you should get automatically signed-on tooyour M-Files application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="651a8-238">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="651a8-238">Additional resources</span></span>

* [<span data-ttu-id="651a8-239">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="651a8-239">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="651a8-240">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="651a8-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_203.png

