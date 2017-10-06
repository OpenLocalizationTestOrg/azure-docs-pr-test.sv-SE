---
title: "Självstudier: Azure Active Directory-integrering med Workpath | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Workpath."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 320b0daf-14be-4813-b59b-25a6a5070690
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 69f443f314edb7c8c489a6c193e09b6f8fe6795a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workpath"></a><span data-ttu-id="d1f55-103">Självstudier: Azure Active Directory-integrering med Workpath</span><span class="sxs-lookup"><span data-stu-id="d1f55-103">Tutorial: Azure Active Directory integration with Workpath</span></span>

<span data-ttu-id="d1f55-104">I kursen får du lära dig hur toointegrate Workpath med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d1f55-104">In this tutorial, you learn how toointegrate Workpath with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d1f55-105">Integrera Workpath med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d1f55-105">Integrating Workpath with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d1f55-106">Du kan styra i Azure AD som har åtkomst till tooWorkpath</span><span class="sxs-lookup"><span data-stu-id="d1f55-106">You can control in Azure AD who has access tooWorkpath</span></span>
- <span data-ttu-id="d1f55-107">Du kan aktivera din användare tooautomatically get inloggade tooWorkpath (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d1f55-107">You can enable your users tooautomatically get signed-on tooWorkpath (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d1f55-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d1f55-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d1f55-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d1f55-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d1f55-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d1f55-110">Prerequisites</span></span>

<span data-ttu-id="d1f55-111">tooconfigure Azure AD-integrering med Workpath, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="d1f55-111">tooconfigure Azure AD integration with Workpath, you need hello following items:</span></span>

- <span data-ttu-id="d1f55-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d1f55-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d1f55-113">En Workpath enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="d1f55-113">A Workpath single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d1f55-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d1f55-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d1f55-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d1f55-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d1f55-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d1f55-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d1f55-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d1f55-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d1f55-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d1f55-118">Scenario description</span></span>
<span data-ttu-id="d1f55-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d1f55-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d1f55-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d1f55-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d1f55-121">Att lägga till Workpath från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d1f55-121">Adding Workpath from hello gallery</span></span>
2. <span data-ttu-id="d1f55-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d1f55-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workpath-from-hello-gallery"></a><span data-ttu-id="d1f55-123">Att lägga till Workpath från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d1f55-123">Adding Workpath from hello gallery</span></span>
<span data-ttu-id="d1f55-124">tooconfigure hello integrering av Workpath i Azure AD, behöver du tooadd Workpath hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="d1f55-124">tooconfigure hello integration of Workpath into Azure AD, you need tooadd Workpath from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d1f55-125">**tooadd Workpath från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d1f55-125">**tooadd Workpath from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d1f55-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d1f55-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d1f55-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d1f55-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d1f55-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="d1f55-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d1f55-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d1f55-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d1f55-133">Skriv i sökrutan hello **Workpath**.</span><span class="sxs-lookup"><span data-stu-id="d1f55-133">In hello search box, type **Workpath**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_search.png)

5. <span data-ttu-id="d1f55-135">Markera hello resultat på panelen **Workpath**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="d1f55-135">In hello results panel, select **Workpath**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d1f55-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d1f55-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d1f55-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Workpath baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d1f55-138">In this section, you configure and test Azure AD single sign-on with Workpath based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d1f55-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Workpath är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d1f55-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Workpath is tooa user in Azure AD.</span></span> <span data-ttu-id="d1f55-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Workpath toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="d1f55-140">In other words, a link relationship between an Azure AD user and hello related user in Workpath needs toobe established.</span></span>

<span data-ttu-id="d1f55-141">I Workpath, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="d1f55-141">In Workpath, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d1f55-142">tooconfigure och testa Azure AD enkel inloggning med Workpath, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d1f55-142">tooconfigure and test Azure AD single sign-on with Workpath, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d1f55-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d1f55-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d1f55-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d1f55-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d1f55-145">**[Skapa en testanvändare Workpath](#creating-a-workpath-test-user)**  -toohave en motsvarighet för Britta Simon i Workpath som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d1f55-145">**[Creating a Workpath test user](#creating-a-workpath-test-user)** - toohave a counterpart of Britta Simon in Workpath that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d1f55-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d1f55-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d1f55-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d1f55-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d1f55-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d1f55-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d1f55-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Workpath program.</span><span class="sxs-lookup"><span data-stu-id="d1f55-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Workpath application.</span></span>

<span data-ttu-id="d1f55-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Workpath:**</span><span class="sxs-lookup"><span data-stu-id="d1f55-150">**tooconfigure Azure AD single sign-on with Workpath, perform hello following steps:**</span></span>

1. <span data-ttu-id="d1f55-151">I hello Azure-portalen på hello **Workpath** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d1f55-151">In hello Azure portal, on hello **Workpath** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d1f55-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d1f55-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_samlbase.png)

3. <span data-ttu-id="d1f55-155">På hello **Workpath domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d1f55-155">On hello **Workpath Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_url.png)

    <span data-ttu-id="d1f55-157">a.</span><span class="sxs-lookup"><span data-stu-id="d1f55-157">a.</span></span> <span data-ttu-id="d1f55-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://api.workpath.com/v1/saml/metadata/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="d1f55-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://api.workpath.com/v1/saml/metadata/<instancename>`</span></span>

    <span data-ttu-id="d1f55-159">b.</span><span class="sxs-lookup"><span data-stu-id="d1f55-159">b.</span></span> <span data-ttu-id="d1f55-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://api.workpath.com/v1/saml/assert/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="d1f55-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://api.workpath.com/v1/saml/assert/<instancename>`</span></span>

4. <span data-ttu-id="d1f55-161">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="d1f55-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="d1f55-162">Om du inte vill tooconfigure hello programmet i **SP** initierade läge, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d1f55-162">If you wish tooconfigure hello application in **SP** initiated mode, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_url1.png)

    <span data-ttu-id="d1f55-164">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.workpath.com/ `</span><span class="sxs-lookup"><span data-stu-id="d1f55-164">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.workpath.com/ `</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d1f55-165">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="d1f55-165">These values are not real.</span></span> <span data-ttu-id="d1f55-166">Uppdatera dessa värden med hello faktiska inloggnings-URL, identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="d1f55-166">Update these values with hello actual Sign-on URL, Identifier and Reply URL.</span></span> <span data-ttu-id="d1f55-167">Kontakta [Workpath supportteamet](https://help.workpath.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="d1f55-167">Contact [Workpath support team](https://help.workpath.com) tooget these values.</span></span>

5. <span data-ttu-id="d1f55-168">Workpath program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="d1f55-168">Workpath application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="d1f55-169">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="d1f55-169">Configure hello following claims for this application.</span></span> <span data-ttu-id="d1f55-170">Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="d1f55-170">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="d1f55-171">hello följande skärmbild visar ett exempel på den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="d1f55-171">hello following screenshot shows an example for this configuration.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_attributes.png)
    
6. <span data-ttu-id="d1f55-173">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attributet enligt hello bild och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d1f55-173">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="d1f55-174">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="d1f55-174">Attribute Name</span></span> | <span data-ttu-id="d1f55-175">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="d1f55-175">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="d1f55-176">Förnamn</span><span class="sxs-lookup"><span data-stu-id="d1f55-176">first_name</span></span> | <span data-ttu-id="d1f55-177">User.givenName</span><span class="sxs-lookup"><span data-stu-id="d1f55-177">user.givenname</span></span> |
    | <span data-ttu-id="d1f55-178">Efternamn</span><span class="sxs-lookup"><span data-stu-id="d1f55-178">last_name</span></span> | <span data-ttu-id="d1f55-179">User.surname</span><span class="sxs-lookup"><span data-stu-id="d1f55-179">user.surname</span></span> |
    
    <span data-ttu-id="d1f55-180">a.</span><span class="sxs-lookup"><span data-stu-id="d1f55-180">a.</span></span> <span data-ttu-id="d1f55-181">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d1f55-181">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="d1f55-183">b.</span><span class="sxs-lookup"><span data-stu-id="d1f55-183">b.</span></span> <span data-ttu-id="d1f55-184">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="d1f55-184">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="d1f55-186">c.</span><span class="sxs-lookup"><span data-stu-id="d1f55-186">c.</span></span> <span data-ttu-id="d1f55-187">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="d1f55-187">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="d1f55-188">d.</span><span class="sxs-lookup"><span data-stu-id="d1f55-188">d.</span></span> <span data-ttu-id="d1f55-189">Lämna hello **Namespace** textruta tomt.</span><span class="sxs-lookup"><span data-stu-id="d1f55-189">Leave hello **Namespace** textbox blank.</span></span>
    
    <span data-ttu-id="d1f55-190">e.</span><span class="sxs-lookup"><span data-stu-id="d1f55-190">e.</span></span> <span data-ttu-id="d1f55-191">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d1f55-191">Click **Ok**.</span></span>
    

7. <span data-ttu-id="d1f55-192">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="d1f55-192">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_certificate.png) 

8. <span data-ttu-id="d1f55-194">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d1f55-194">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="d1f55-196">På hello **Workpath Configuration** klickar du på **konfigurera Workpath** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="d1f55-196">On hello **Workpath Configuration** section, click **Configure Workpath** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d1f55-197">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="d1f55-197">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_configure.png) 

10. <span data-ttu-id="d1f55-199">tooconfigure enkel inloggning på **Workpath** sida, behöver du toosend hello hämtas **XML-Metadata för**, **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel** för[Workpath supportteamet](https://help.workpath.com).</span><span class="sxs-lookup"><span data-stu-id="d1f55-199">tooconfigure single sign-on on **Workpath** side, you need toosend hello downloaded **Metadata XML**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Workpath support team](https://help.workpath.com).</span></span> 

> [!TIP]
> <span data-ttu-id="d1f55-200">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="d1f55-200">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d1f55-201">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="d1f55-201">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d1f55-202">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d1f55-202">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d1f55-203">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d1f55-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="d1f55-204">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d1f55-204">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d1f55-206">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d1f55-206">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d1f55-207">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d1f55-207">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workpath-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d1f55-209">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d1f55-209">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workpath-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d1f55-211">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d1f55-211">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workpath-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d1f55-213">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d1f55-213">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workpath-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d1f55-215">a.</span><span class="sxs-lookup"><span data-stu-id="d1f55-215">a.</span></span> <span data-ttu-id="d1f55-216">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d1f55-216">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d1f55-217">b.</span><span class="sxs-lookup"><span data-stu-id="d1f55-217">b.</span></span> <span data-ttu-id="d1f55-218">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d1f55-218">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d1f55-219">c.</span><span class="sxs-lookup"><span data-stu-id="d1f55-219">c.</span></span> <span data-ttu-id="d1f55-220">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d1f55-220">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d1f55-221">d.</span><span class="sxs-lookup"><span data-stu-id="d1f55-221">d.</span></span> <span data-ttu-id="d1f55-222">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d1f55-222">Click **Create**.</span></span>
 
### <a name="creating-a-workpath-test-user"></a><span data-ttu-id="d1f55-223">Skapa en testanvändare Workpath</span><span class="sxs-lookup"><span data-stu-id="d1f55-223">Creating a Workpath test user</span></span>

<span data-ttu-id="d1f55-224">Workpath stöder bara i tid användaretablering.</span><span class="sxs-lookup"><span data-stu-id="d1f55-224">Workpath supports Just in time user provisioning.</span></span> <span data-ttu-id="d1f55-225">Efter autentisering skapas användare i hello program automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d1f55-225">After authentication users are created in hello application automatically.</span></span> 


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d1f55-226">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="d1f55-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d1f55-227">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooWorkpath.</span><span class="sxs-lookup"><span data-stu-id="d1f55-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWorkpath.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d1f55-229">**tooassign Britta Simon tooWorkpath utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d1f55-229">**tooassign Britta Simon tooWorkpath, perform hello following steps:**</span></span>

1. <span data-ttu-id="d1f55-230">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d1f55-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d1f55-232">Välj i listan med program hello **Workpath**.</span><span class="sxs-lookup"><span data-stu-id="d1f55-232">In hello applications list, select **Workpath**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workpath-tutorial/tutorial_workpath_app.png) 

3. <span data-ttu-id="d1f55-234">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d1f55-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d1f55-236">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d1f55-236">Click **Add** button.</span></span> <span data-ttu-id="d1f55-237">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d1f55-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d1f55-239">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="d1f55-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d1f55-240">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d1f55-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d1f55-241">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d1f55-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d1f55-242">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d1f55-242">Testing single sign-on</span></span>

<span data-ttu-id="d1f55-243">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d1f55-243">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d1f55-244">Du bör få automatiskt inloggade tooyour Workpath programmet när du klickar på hello Workpath panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d1f55-244">When you click hello Workpath tile in hello Access Panel, you should get automatically signed-on tooyour Workpath application.</span></span>
<span data-ttu-id="d1f55-245">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d1f55-245">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d1f55-246">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d1f55-246">Additional resources</span></span>

* [<span data-ttu-id="d1f55-247">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d1f55-247">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d1f55-248">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d1f55-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workpath-tutorial/tutorial_general_203.png

