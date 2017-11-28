---
title: "Självstudier: Azure Active Directory-integrering med Egnyte | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Egnyte."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: d86fa28a1e7b23474cb76c08aef8539acadaf24d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-egnyte"></a><span data-ttu-id="3b9ec-103">Självstudier: Azure Active Directory-integrering med Egnyte</span><span class="sxs-lookup"><span data-stu-id="3b9ec-103">Tutorial: Azure Active Directory integration with Egnyte</span></span>

<span data-ttu-id="3b9ec-104">I kursen får du lära dig hur toointegrate Egnyte med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3b9ec-104">In this tutorial, you learn how toointegrate Egnyte with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3b9ec-105">Integrera Egnyte med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3b9ec-105">Integrating Egnyte with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3b9ec-106">Du kan styra i Azure AD som har åtkomst till tooEgnyte</span><span class="sxs-lookup"><span data-stu-id="3b9ec-106">You can control in Azure AD who has access tooEgnyte</span></span>
- <span data-ttu-id="3b9ec-107">Du kan aktivera din användare tooautomatically get inloggade tooEgnyte (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="3b9ec-107">You can enable your users tooautomatically get signed-on tooEgnyte (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3b9ec-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3b9ec-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="3b9ec-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3b9ec-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3b9ec-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3b9ec-110">Prerequisites</span></span>

<span data-ttu-id="3b9ec-111">tooconfigure Azure AD-integrering med Egnyte, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="3b9ec-111">tooconfigure Azure AD integration with Egnyte, you need hello following items:</span></span>

- <span data-ttu-id="3b9ec-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3b9ec-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3b9ec-113">En Egnyte enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="3b9ec-113">An Egnyte single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3b9ec-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3b9ec-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="3b9ec-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3b9ec-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3b9ec-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3b9ec-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3b9ec-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="3b9ec-118">Scenario description</span></span>
<span data-ttu-id="3b9ec-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3b9ec-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="3b9ec-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3b9ec-121">Att lägga till Egnyte från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="3b9ec-121">Adding Egnyte from hello gallery</span></span>
2. <span data-ttu-id="3b9ec-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3b9ec-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-egnyte-from-hello-gallery"></a><span data-ttu-id="3b9ec-123">Att lägga till Egnyte från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="3b9ec-123">Adding Egnyte from hello gallery</span></span>
<span data-ttu-id="3b9ec-124">tooconfigure hello integrering av Egnyte i Azure AD, behöver du tooadd Egnyte hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-124">tooconfigure hello integration of Egnyte into Azure AD, you need tooadd Egnyte from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3b9ec-125">**tooadd Egnyte från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3b9ec-125">**tooadd Egnyte from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3b9ec-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3b9ec-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3b9ec-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="3b9ec-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="3b9ec-133">Skriv i sökrutan hello **Egnyte**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-133">In hello search box, type **Egnyte**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_search.png)

5. <span data-ttu-id="3b9ec-135">Markera hello resultat på panelen **Egnyte**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-135">In hello results panel, select **Egnyte**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3b9ec-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3b9ec-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3b9ec-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Egnyte baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-138">In this section, you configure and test Azure AD single sign-on with Egnyte based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3b9ec-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Egnyte är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Egnyte is tooa user in Azure AD.</span></span> <span data-ttu-id="3b9ec-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Egnyte toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-140">In other words, a link relationship between an Azure AD user and hello related user in Egnyte needs toobe established.</span></span>

<span data-ttu-id="3b9ec-141">I Egnyte, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-141">In Egnyte, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="3b9ec-142">tooconfigure och testa Azure AD enkel inloggning med Egnyte, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="3b9ec-142">tooconfigure and test Azure AD single sign-on with Egnyte, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3b9ec-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3b9ec-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3b9ec-145">**[Skapa en testanvändare Egnyte](#creating-an-egnyte-test-user)**  -toohave en motsvarighet för Britta Simon i Egnyte som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-145">**[Creating an Egnyte test user](#creating-an-egnyte-test-user)** - toohave a counterpart of Britta Simon in Egnyte that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3b9ec-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3b9ec-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3b9ec-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3b9ec-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3b9ec-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Egnyte program.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Egnyte application.</span></span>

<span data-ttu-id="3b9ec-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Egnyte:**</span><span class="sxs-lookup"><span data-stu-id="3b9ec-150">**tooconfigure Azure AD single sign-on with Egnyte, perform hello following steps:**</span></span>

1. <span data-ttu-id="3b9ec-151">I hello Azure-portalen på hello **Egnyte** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-151">In hello Azure portal, on hello **Egnyte** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="3b9ec-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_samlbase.png)

3. <span data-ttu-id="3b9ec-155">På hello **Egnyte domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3b9ec-155">On hello **Egnyte Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_url.png)

    <span data-ttu-id="3b9ec-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.egnyte.com`</span><span class="sxs-lookup"><span data-stu-id="3b9ec-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.egnyte.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3b9ec-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-158">This value is not real.</span></span> <span data-ttu-id="3b9ec-159">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="3b9ec-160">Kontakta [Egnyte klienten supportteamet](https://www.egnyte.com/corp/contact_egnyte.html) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-160">Contact [Egnyte Client support team](https://www.egnyte.com/corp/contact_egnyte.html) tooget this value.</span></span> 
 
4. <span data-ttu-id="3b9ec-161">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_certificate.png) 

5. <span data-ttu-id="3b9ec-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-egnyte-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3b9ec-165">På hello **Egnyte Configuration** klickar du på **konfigurera Egnyte** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-165">On hello **Egnyte Configuration** section, click **Configure Egnyte** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="3b9ec-166">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="3b9ec-166">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_configure.png) 

7. <span data-ttu-id="3b9ec-168">Logga in tooyour Egnyte företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-168">In a different web browser window, log in tooyour Egnyte company site as an administrator.</span></span>

8. <span data-ttu-id="3b9ec-169">Klicka på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-169">Click **Settings**.</span></span>
   
   <span data-ttu-id="3b9ec-170">![Inställningar för](./media/active-directory-saas-egnyte-tutorial/ic787819.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="3b9ec-170">![Settings](./media/active-directory-saas-egnyte-tutorial/ic787819.png "Settings")</span></span>

9. <span data-ttu-id="3b9ec-171">Klicka på menyn hello **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-171">In hello menu, click **Settings**.</span></span>

   <span data-ttu-id="3b9ec-172">![Inställningar för](./media/active-directory-saas-egnyte-tutorial/ic787820.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="3b9ec-172">![Settings](./media/active-directory-saas-egnyte-tutorial/ic787820.png "Settings")</span></span>

10. <span data-ttu-id="3b9ec-173">Klicka på hello **Configuration** fliken och klicka sedan på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-173">Click hello **Configuration** tab, and then click **Security**.</span></span>

    <span data-ttu-id="3b9ec-174">![Säkerhet](./media/active-directory-saas-egnyte-tutorial/ic787821.png "säkerhet")</span><span class="sxs-lookup"><span data-stu-id="3b9ec-174">![Security](./media/active-directory-saas-egnyte-tutorial/ic787821.png "Security")</span></span>

11. <span data-ttu-id="3b9ec-175">I hello **autentisering för enkel inloggning** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3b9ec-175">In hello **Single Sign-On Authentication** section, perform hello following steps:</span></span>

    <span data-ttu-id="3b9ec-176">![Enkel inloggning för autentisering](./media/active-directory-saas-egnyte-tutorial/ic787822.png "enkel inloggning för autentisering")</span><span class="sxs-lookup"><span data-stu-id="3b9ec-176">![Single Sign On Authentication](./media/active-directory-saas-egnyte-tutorial/ic787822.png "Single Sign On Authentication")</span></span>   
    
    <span data-ttu-id="3b9ec-177">a.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-177">a.</span></span> <span data-ttu-id="3b9ec-178">Som **autentisering för enkel inloggning**väljer **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-178">As **Single sign-on authentication**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="3b9ec-179">b.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-179">b.</span></span> <span data-ttu-id="3b9ec-180">Som **identitetsleverantör**väljer **AzureAD**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-180">As **Identity provider**, select **AzureAD**.</span></span>
   
    <span data-ttu-id="3b9ec-181">c.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-181">c.</span></span> <span data-ttu-id="3b9ec-182">Klistra in **SAML enkel inloggning Tjänstwebbadress** kopieras från Azure-portalen till hello **identitet providern inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-182">Paste **SAML Single Sign-On Service URL** copied from Azure portal into hello **Identity provider login URL** textbox.</span></span>
   
    <span data-ttu-id="3b9ec-183">d.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-183">d.</span></span> <span data-ttu-id="3b9ec-184">Klistra in **SAML enhets-ID** som du har kopierat från Azure-portalen i hello **identitet providern enhets-ID** textruta.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-184">Paste **SAML Entity ID** which you have copied from Azure portal into hello **Identity provider entity ID** textbox.</span></span>
      
    <span data-ttu-id="3b9ec-185">e.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-185">e.</span></span> <span data-ttu-id="3b9ec-186">Öppna din Base64-kodade certifikatet i anteckningar som hämtas från Azure-portalen kopiera hello innehållet i den i Urklipp, och klistra in den toohello **providern identitetscertifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-186">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **Identity provider certificate** textbox.</span></span>
   
    <span data-ttu-id="3b9ec-187">f.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-187">f.</span></span> <span data-ttu-id="3b9ec-188">Som **standard mappning**väljer **e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-188">As **Default user mapping**, select **Email address**.</span></span>
   
    <span data-ttu-id="3b9ec-189">g.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-189">g.</span></span> <span data-ttu-id="3b9ec-190">Som **använder domänspecifika utfärdaren värdet**väljer **inaktiveras**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-190">As **Use domain-specific issuer value**, select **disabled**.</span></span>
   
    <span data-ttu-id="3b9ec-191">h.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-191">h.</span></span> <span data-ttu-id="3b9ec-192">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-192">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="3b9ec-193">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="3b9ec-193">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="3b9ec-194">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-194">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="3b9ec-195">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3b9ec-195">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3b9ec-196">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="3b9ec-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="3b9ec-197">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-197">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="3b9ec-199">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3b9ec-199">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3b9ec-200">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-200">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3b9ec-202">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-202">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3b9ec-204">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-204">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3b9ec-206">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3b9ec-206">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3b9ec-208">a.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-208">a.</span></span> <span data-ttu-id="3b9ec-209">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-209">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3b9ec-210">b.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-210">b.</span></span> <span data-ttu-id="3b9ec-211">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-211">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3b9ec-212">c.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-212">c.</span></span> <span data-ttu-id="3b9ec-213">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-213">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="3b9ec-214">d.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-214">d.</span></span> <span data-ttu-id="3b9ec-215">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-215">Click **Create**.</span></span>
 
### <a name="creating-an-egnyte-test-user"></a><span data-ttu-id="3b9ec-216">Skapa en testanvändare Egnyte</span><span class="sxs-lookup"><span data-stu-id="3b9ec-216">Creating an Egnyte test user</span></span>

<span data-ttu-id="3b9ec-217">tooenable Azure AD-användare toolog i tooEgnyte, måste de etableras i Egnyte.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-217">tooenable Azure AD users toolog in tooEgnyte, they must be provisioned into Egnyte.</span></span> <span data-ttu-id="3b9ec-218">Hello gäller Egnyte är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-218">In hello case of Egnyte, provisioning is a manual task.</span></span>

<span data-ttu-id="3b9ec-219">**tooprovision användarkonton, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3b9ec-219">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="3b9ec-220">Logga in tooyour **Egnyte** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-220">Log in tooyour **Egnyte** company site as administrator.</span></span>

2. <span data-ttu-id="3b9ec-221">Gå för**inställningar \> användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-221">Go too**Settings \> Users & Groups**.</span></span>

3. <span data-ttu-id="3b9ec-222">Klicka på **Lägg till nya användare**, och välj sedan hello typ för användare som du vill tooadd.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-222">Click **Add New User**, and then select hello type of user you want tooadd.</span></span>
   
   <span data-ttu-id="3b9ec-223">![Användare](./media/active-directory-saas-egnyte-tutorial/ic787824.png "användare")</span><span class="sxs-lookup"><span data-stu-id="3b9ec-223">![Users](./media/active-directory-saas-egnyte-tutorial/ic787824.png "Users")</span></span>

4. <span data-ttu-id="3b9ec-224">I hello **nya standardanvändare** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3b9ec-224">In hello **New Standard User** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="3b9ec-225">![Nya standardanvändare](./media/active-directory-saas-egnyte-tutorial/ic787825.png "ny vanlig användare")</span><span class="sxs-lookup"><span data-stu-id="3b9ec-225">![New Standard User](./media/active-directory-saas-egnyte-tutorial/ic787825.png "New Standard User")</span></span>   

   <span data-ttu-id="3b9ec-226">a.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-226">a.</span></span> <span data-ttu-id="3b9ec-227">Typen hello **e-post**, **användarnamn**, och annan information om ett giltigt Azure Active Directory-konto som du vill tooprovision.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-227">Type hello **Email**, **Username**, and other details of a valid Azure Active Directory account you want tooprovision.</span></span>
   
   <span data-ttu-id="3b9ec-228">b.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-228">b.</span></span> <span data-ttu-id="3b9ec-229">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-229">Click **Save**.</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="3b9ec-230">hello Azure Active Directory användare får ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-230">hello Azure Active Directory account holder will receive a notification email.</span></span>
    >

>[!NOTE]
><span data-ttu-id="3b9ec-231">Du kan använda något annat Egnyte användarens konto skapas verktyg eller API: er som tillhandahålls av Egnyte tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-231">You can use any other Egnyte user account creation tools or APIs provided by Egnyte tooprovision AAD user accounts.</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="3b9ec-232">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="3b9ec-232">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="3b9ec-233">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooEgnyte.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEgnyte.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="3b9ec-235">**tooassign Britta Simon tooEgnyte utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3b9ec-235">**tooassign Britta Simon tooEgnyte, perform hello following steps:**</span></span>

1. <span data-ttu-id="3b9ec-236">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="3b9ec-238">Välj i listan med program hello **Egnyte**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-238">In hello applications list, select **Egnyte**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_app.png) 

3. <span data-ttu-id="3b9ec-240">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="3b9ec-242">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-242">Click **Add** button.</span></span> <span data-ttu-id="3b9ec-243">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="3b9ec-245">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3b9ec-246">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3b9ec-247">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3b9ec-248">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3b9ec-248">Testing single sign-on</span></span>

<span data-ttu-id="3b9ec-249">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-249">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="3b9ec-250">Du bör få automatiskt inloggade tooyour Egnyte programmet när du klickar på hello Egnyte panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="3b9ec-250">When you click hello Egnyte tile in hello Access Panel, you should get automatically signed-on tooyour Egnyte application.</span></span>
<span data-ttu-id="3b9ec-251">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3b9ec-251">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="3b9ec-252">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3b9ec-252">Additional resources</span></span>

* [<span data-ttu-id="3b9ec-253">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3b9ec-253">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3b9ec-254">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3b9ec-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_203.png

