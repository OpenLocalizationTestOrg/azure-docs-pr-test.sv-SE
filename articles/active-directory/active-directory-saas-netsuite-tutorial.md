---
title: "Självstudier: Azure Active Directory-integrering med Netsuite | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Netsuite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 7cf205d5bda5333872fb589e57f4779a8670b595
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a><span data-ttu-id="5ed4a-103">Självstudier: Azure Active Directory-integrering med Netsuite</span><span class="sxs-lookup"><span data-stu-id="5ed4a-103">Tutorial: Azure Active Directory integration with Netsuite</span></span>

<span data-ttu-id="5ed4a-104">I kursen får du lära dig hur toointegrate Netsuite med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="5ed4a-104">In this tutorial, you learn how toointegrate Netsuite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5ed4a-105">Integrera Netsuite med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="5ed4a-105">Integrating Netsuite with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5ed4a-106">Du kan styra i Azure AD som har åtkomst till tooNetsuite</span><span class="sxs-lookup"><span data-stu-id="5ed4a-106">You can control in Azure AD who has access tooNetsuite</span></span>
- <span data-ttu-id="5ed4a-107">Du kan aktivera din användare tooautomatically get inloggade tooNetsuite (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="5ed4a-107">You can enable your users tooautomatically get signed-on tooNetsuite (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5ed4a-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5ed4a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5ed4a-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5ed4a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5ed4a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="5ed4a-110">Prerequisites</span></span>

<span data-ttu-id="5ed4a-111">tooconfigure Azure AD-integrering med Netsuite, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="5ed4a-111">tooconfigure Azure AD integration with Netsuite, you need hello following items:</span></span>

- <span data-ttu-id="5ed4a-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5ed4a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5ed4a-113">En Netsuite enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="5ed4a-113">A Netsuite single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5ed4a-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5ed4a-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="5ed4a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5ed4a-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5ed4a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5ed4a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5ed4a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="5ed4a-118">Scenario description</span></span>
<span data-ttu-id="5ed4a-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5ed4a-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="5ed4a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5ed4a-121">Att lägga till Netsuite från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5ed4a-121">Adding Netsuite from hello gallery</span></span>
2. <span data-ttu-id="5ed4a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5ed4a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-netsuite-from-hello-gallery"></a><span data-ttu-id="5ed4a-123">Att lägga till Netsuite från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5ed4a-123">Adding Netsuite from hello gallery</span></span>
<span data-ttu-id="5ed4a-124">tooconfigure hello integrering av Netsuite i Azure AD, behöver du tooadd Netsuite hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-124">tooconfigure hello integration of Netsuite into Azure AD, you need tooadd Netsuite from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5ed4a-125">**tooadd Netsuite från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5ed4a-125">**tooadd Netsuite from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5ed4a-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5ed4a-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5ed4a-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="5ed4a-131">Klicka på **nytt program** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="5ed4a-133">Skriv i sökrutan hello **Netsuite**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-133">In hello search box, type **Netsuite**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_search.png)

5. <span data-ttu-id="5ed4a-135">Markera hello resultat på panelen **Netsuite**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-135">In hello results panel, select **Netsuite**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5ed4a-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5ed4a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5ed4a-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Netsuite baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-138">In this section, you configure and test Azure AD single sign-on with Netsuite based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5ed4a-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Netsuite är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Netsuite is tooa user in Azure AD.</span></span> <span data-ttu-id="5ed4a-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Netsuite toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-140">In other words, a link relationship between an Azure AD user and hello related user in Netsuite needs toobe established.</span></span>

<span data-ttu-id="5ed4a-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Netsuite.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Netsuite.</span></span>

<span data-ttu-id="5ed4a-142">tooconfigure och testa Azure AD enkel inloggning med Netsuite, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="5ed4a-142">tooconfigure and test Azure AD single sign-on with Netsuite, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5ed4a-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5ed4a-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5ed4a-145">**[Skapa en testanvändare Netsuite](#creating-a-netsuite-test-user)**  -toohave en motsvarighet för Britta Simon i Netsuite som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-145">**[Creating a Netsuite test user](#creating-a-netsuite-test-user)** - toohave a counterpart of Britta Simon in Netsuite that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5ed4a-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5ed4a-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5ed4a-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5ed4a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5ed4a-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Netsuite program.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Netsuite application.</span></span>

<span data-ttu-id="5ed4a-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Netsuite:**</span><span class="sxs-lookup"><span data-stu-id="5ed4a-150">**tooconfigure Azure AD single sign-on with Netsuite, perform hello following steps:**</span></span>

1. <span data-ttu-id="5ed4a-151">I hello Azure-portalen på hello **Netsuite** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-151">In hello Azure portal, on hello **Netsuite** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="5ed4a-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_samlbase.png)

3. <span data-ttu-id="5ed4a-155">På hello **Netsuite domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5ed4a-155">On hello **Netsuite Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_url.png)

    <span data-ttu-id="5ed4a-157">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret: `https://<tenant-name>.netsuite.com/saml2/acs` `https://<tenant-name>.na1.netsuite.com/saml2/acs` `https://<tenant-name>.na2.netsuite.com/saml2/acs` `https://<tenant-name>.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs``https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`</span><span class="sxs-lookup"><span data-stu-id="5ed4a-157">In hello **Reply URL** textbox, type a URL using hello following pattern:   `https://<tenant-name>.netsuite.com/saml2/acs` `https://<tenant-name>.na1.netsuite.com/saml2/acs` `https://<tenant-name>.na2.netsuite.com/saml2/acs` `https://<tenant-name>.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5ed4a-158">Det här värdet är inte verkliga värde.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-158">This value is not real value.</span></span> <span data-ttu-id="5ed4a-159">Hello uppdateringsvärde med hello faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-159">Update hello value with hello actual Reply URL.</span></span> <span data-ttu-id="5ed4a-160">Kontakta [Netsuite supportteamet](http://www.netsuite.com/portal/services/support.shtml) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-160">Contact [Netsuite support team](http://www.netsuite.com/portal/services/support.shtml) tooget this value.</span></span>
 
4. <span data-ttu-id="5ed4a-161">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_certificate.png) 

5. <span data-ttu-id="5ed4a-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5ed4a-165">På hello **Netsuite Configuration** klickar du på **konfigurera Netsuite** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-165">On hello **Netsuite Configuration** section, click **Configure Netsuite** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5ed4a-166">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="5ed4a-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_configure.png) 

7. <span data-ttu-id="5ed4a-168">Öppna en ny flik i webbläsaren och logga in på webbplatsen Netsuite företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-168">Open a new tab in your browser, and sign into your Netsuite company site as an administrator.</span></span>

8. <span data-ttu-id="5ed4a-169">I hello verktygsfältet hello överst på hello sidan klickar du på **installationsprogrammet**, klicka på **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-169">In hello toolbar at hello top of hello page, click **Setup**, then click **Setup Manager**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

9. <span data-ttu-id="5ed4a-171">Från hello **installationsaktiviteter** väljer **integrering**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-171">From hello **Setup Tasks** list, select **Integration**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-integration.png)

10. <span data-ttu-id="5ed4a-173">I hello **hantera autentisering** klickar du på **SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-173">In hello **Manage Authentication** section, click **SAML Single Sign-on**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-saml.png)

11. <span data-ttu-id="5ed4a-175">På hello **SAML installationsprogrammet** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5ed4a-175">On hello **SAML Setup** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="5ed4a-176">a.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-176">a.</span></span> <span data-ttu-id="5ed4a-177">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** värde från **Snabbreferens** avsnitt i **konfigurera inloggning** och klistra in den i hello **identitetsleverantören. Inloggningssidan** i Netsuite.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-177">Copy hello **SAML Single Sign-On Service URL** value from **Quick Reference** section of **Configure sign-on** and paste it into hello **Identity Provider Login Page** field in Netsuite.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png)
  
    <span data-ttu-id="5ed4a-179">b.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-179">b.</span></span> <span data-ttu-id="5ed4a-180">Välj i Netsuite, **primära autentiseringsmetod**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-180">In Netsuite, select **Primary Authentication Method**.</span></span>

    <span data-ttu-id="5ed4a-181">c.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-181">c.</span></span> <span data-ttu-id="5ed4a-182">För fältet hello **SAMLV2 identitet providern Metadata**väljer **överför IDP metadatafil**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-182">For hello field labeled **SAMLV2 Identity Provider Metadata**, select **Upload IDP Metadata File**.</span></span> <span data-ttu-id="5ed4a-183">Klicka på **Bläddra** tooupload hello metadatafilen som du hämtade från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-183">Then click **Browse** tooupload hello metadata file that you downloaded from Azure portal.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png)

    <span data-ttu-id="5ed4a-185">d.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-185">d.</span></span> <span data-ttu-id="5ed4a-186">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-186">Click **Submit**.</span></span>

12. <span data-ttu-id="5ed4a-187">I Azure AD, klickar du på **visa och redigera andra användarattribut** kryssrutan och Lägg till attribut.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-187">In Azure AD, Click on **View and edit all other user attributes** check-box and add attribute.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-attributes.png)

13. <span data-ttu-id="5ed4a-189">För hello **attributnamn** anger i `account`.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-189">For hello **Attribute Name** field, type in `account`.</span></span> <span data-ttu-id="5ed4a-190">För hello **attributvärdet** anger i din Netsuite konto-ID. Det här värdet är konstant och ändra med kontot.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-190">For hello **Attribute Value** field, type in your Netsuite account ID.This value is constant and change with account.</span></span> <span data-ttu-id="5ed4a-191">Instruktioner om hur toofind konto-ID ingår nedan:</span><span class="sxs-lookup"><span data-stu-id="5ed4a-191">Instructions on how toofind your account ID are included below:</span></span>

      ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-add-attribute.png)

    <span data-ttu-id="5ed4a-193">a.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-193">a.</span></span> <span data-ttu-id="5ed4a-194">Klicka på Netsuite, **installationsprogrammet** hello övre navigeringsfältet-menyn.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-194">In Netsuite, click **Setup** from hello top navigation menu.</span></span>

    <span data-ttu-id="5ed4a-195">b.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-195">b.</span></span> <span data-ttu-id="5ed4a-196">Klicka på under hello **installationsaktiviteter** hello vänstra navigeringsfönstret-menyn, Välj hello **integrering** avsnittet och klicka på **Web Services-inställningar**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-196">Then click under hello **Setup Tasks** section of hello left navigation menu, select hello **Integration** section, and click **Web Services Preferences**.</span></span>

    <span data-ttu-id="5ed4a-197">c.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-197">c.</span></span> <span data-ttu-id="5ed4a-198">Kopiera Netsuite konto-ID och klistra in den i hello **attributvärdet** i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-198">Copy your Netsuite Account ID and paste it into hello **Attribute Value** field in Azure AD.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-account-id.png)

14. <span data-ttu-id="5ed4a-200">Innan användarna kan utföra enkel inloggning till Netsuite, måste de först tilldelas hello lämpliga behörigheter i Netsuite.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-200">Before users can perform single sign-on into Netsuite, they must first be assigned hello appropriate permissions in Netsuite.</span></span> <span data-ttu-id="5ed4a-201">Följ instruktionerna för hello nedan tooassign dessa behörigheter.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-201">Follow hello instructions below tooassign these permissions.</span></span>

    <span data-ttu-id="5ed4a-202">a.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-202">a.</span></span> <span data-ttu-id="5ed4a-203">På menyn övre navigeringsfältet hello **installationsprogrammet**, klicka på **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-203">On hello top navigation menu, click **Setup**, then click **Setup Manager**.</span></span>
      
      ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    <span data-ttu-id="5ed4a-205">b.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-205">b.</span></span> <span data-ttu-id="5ed4a-206">Välj på menyn vänstra navigeringsfönstret hello **användare och roller för**, klicka på **hantera roller**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-206">On hello left navigation menu, select **Users/Roles**, then click **Manage Roles**.</span></span>
      
      ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-manage-roles.png)

    <span data-ttu-id="5ed4a-208">c.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-208">c.</span></span> <span data-ttu-id="5ed4a-209">Klicka på **ny roll**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-209">Click **New Role**.</span></span>

    <span data-ttu-id="5ed4a-210">d.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-210">d.</span></span> <span data-ttu-id="5ed4a-211">Ange en **namn** för nya rollen och välj hello **enkel inloggning endast** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-211">Type in a **Name** for your new role, and select hello **Single Sign-On Only** checkbox.</span></span>
      
      ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-new-role.png)

    <span data-ttu-id="5ed4a-213">e.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-213">e.</span></span> <span data-ttu-id="5ed4a-214">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-214">Click **Save**.</span></span>

    <span data-ttu-id="5ed4a-215">f.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-215">f.</span></span> <span data-ttu-id="5ed4a-216">Hello-menyn överst hello **behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-216">In hello menu on hello top, click **Permissions**.</span></span> <span data-ttu-id="5ed4a-217">Klicka på **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-217">Then click **Setup**.</span></span>
      
       ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-sso.png)

    <span data-ttu-id="5ed4a-219">g.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-219">g.</span></span> <span data-ttu-id="5ed4a-220">Välj **ställa in SAM Single Sign-on**, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-220">Select **Set Up SAM Single Sign-on**, and then click **Add**.</span></span>

    <span data-ttu-id="5ed4a-221">h.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-221">h.</span></span> <span data-ttu-id="5ed4a-222">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-222">Click **Save**.</span></span>

    <span data-ttu-id="5ed4a-223">Jag.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-223">i.</span></span> <span data-ttu-id="5ed4a-224">På menyn övre navigeringsfältet hello **installationsprogrammet**, klicka på **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-224">On hello top navigation menu, click **Setup**, then click **Setup Manager**.</span></span>
      
       ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    <span data-ttu-id="5ed4a-226">j.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-226">j.</span></span> <span data-ttu-id="5ed4a-227">Välj på menyn vänstra navigeringsfönstret hello **användare och roller för**, klicka på **hantera användare**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-227">On hello left navigation menu, select **Users/Roles**, then click **Manage Users**.</span></span>
      
       ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-manage-users.png)

    <span data-ttu-id="5ed4a-229">k.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-229">k.</span></span> <span data-ttu-id="5ed4a-230">Välj en testanvändare.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-230">Select a test user.</span></span> <span data-ttu-id="5ed4a-231">Klicka på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-231">Then click **Edit**.</span></span>
      
       ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-edit-user.png)

    <span data-ttu-id="5ed4a-233">l.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-233">l.</span></span> <span data-ttu-id="5ed4a-234">Välj hello roll som du har skapat och klicka på hello roller dialogrutan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-234">On hello Roles dialog, select hello role that you have created and click **Add**.</span></span>
      
       ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-add-role.png)

    <span data-ttu-id="5ed4a-236">m.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-236">m.</span></span> <span data-ttu-id="5ed4a-237">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-237">Click **Save**.</span></span>
    
> [!TIP]
> <span data-ttu-id="5ed4a-238">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="5ed4a-238">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5ed4a-239">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-239">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5ed4a-240">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5ed4a-240">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5ed4a-241">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="5ed4a-241">Creating an Azure AD test user</span></span>
<span data-ttu-id="5ed4a-242">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-242">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="5ed4a-244">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5ed4a-244">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5ed4a-245">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-245">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_01.png) 

2.  <span data-ttu-id="5ed4a-247">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-247">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5ed4a-249">Hello överkant hello dialogrutan, klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-249">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5ed4a-251">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5ed4a-251">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5ed4a-253">a.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-253">a.</span></span> <span data-ttu-id="5ed4a-254">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-254">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5ed4a-255">b.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-255">b.</span></span> <span data-ttu-id="5ed4a-256">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-256">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5ed4a-257">c.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-257">c.</span></span> <span data-ttu-id="5ed4a-258">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-258">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5ed4a-259">d.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-259">d.</span></span> <span data-ttu-id="5ed4a-260">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-260">Click **Create**.</span></span> 

### <a name="creating-a-netsuite-test-user"></a><span data-ttu-id="5ed4a-261">Skapa en testanvändare Netsuite</span><span class="sxs-lookup"><span data-stu-id="5ed4a-261">Creating a Netsuite test user</span></span>

<span data-ttu-id="5ed4a-262">I det här avsnittet skapas en användare som kallas Britta Simon i Netsuite.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-262">In this section, a user called Britta Simon is created in Netsuite.</span></span> <span data-ttu-id="5ed4a-263">Netsuite stöder just-in-time-allokering som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-263">Netsuite supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="5ed4a-264">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-264">There is no action item for you in this section.</span></span> <span data-ttu-id="5ed4a-265">Om en användare inte redan finns i Netsuite, skapas en ny när du försöker tooaccess Netsuite.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-265">If a user doesn't already exist in Netsuite, a new one is created when you attempt tooaccess Netsuite.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5ed4a-266">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="5ed4a-266">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5ed4a-267">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooNetsuite.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-267">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNetsuite.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="5ed4a-269">**tooassign Britta Simon tooNetsuite utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5ed4a-269">**tooassign Britta Simon tooNetsuite, perform hello following steps:**</span></span>

1. <span data-ttu-id="5ed4a-270">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-270">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="5ed4a-272">Välj i listan med program hello **Netsuite**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-272">In hello applications list, select **Netsuite**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_app.png) 

3. <span data-ttu-id="5ed4a-274">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-274">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="5ed4a-276">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-276">Click **Add** button.</span></span> <span data-ttu-id="5ed4a-277">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-277">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="5ed4a-279">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-279">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5ed4a-280">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-280">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5ed4a-281">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-281">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5ed4a-282">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5ed4a-282">Testing single sign-on</span></span>

<span data-ttu-id="5ed4a-283">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-283">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5ed4a-284">tootest enkel inloggning inställningarna, öppna hello åtkomstpanelen på [https://myapps.microsoft.com](https://myapps.microsoft.com/), logga in på hello testkonto och på **Netsuite**.</span><span class="sxs-lookup"><span data-stu-id="5ed4a-284">tootest your single sign-on settings, open hello Access Panel at [https://myapps.microsoft.com](https://myapps.microsoft.com/), sign into hello test account, and click **Netsuite**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5ed4a-285">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="5ed4a-285">Additional resources</span></span>

* [<span data-ttu-id="5ed4a-286">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5ed4a-286">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5ed4a-287">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5ed4a-287">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="5ed4a-288">Konfigurera Användaretablering</span><span class="sxs-lookup"><span data-stu-id="5ed4a-288">Configure User Provisioning</span></span>](active-directory-saas-netsuite-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_203.png

