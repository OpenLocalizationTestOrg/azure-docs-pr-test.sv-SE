---
title: "Självstudier: Azure Active Directory-integrering med HR2day av Merces | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och HR2day av Merces."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 853d08c9-27b1-48d4-b8e7-3705140eb67f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: 257233767e1fddbaf115878cb0455acf61b2f5f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a><span data-ttu-id="6cd13-103">Självstudier: Azure Active Directory-integrering med HR2day av Merces</span><span class="sxs-lookup"><span data-stu-id="6cd13-103">Tutorial: Azure Active Directory integration with HR2day by Merces</span></span>

<span data-ttu-id="6cd13-104">I kursen får du lära dig hur toointegrate HR2day av Merces med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6cd13-104">In this tutorial, you learn how toointegrate HR2day by Merces with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6cd13-105">Integrera HR2day av Merces med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="6cd13-105">Integrating HR2day by Merces with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6cd13-106">Du kan styra i Azure AD som har åtkomst till tooHR2day av Merces.</span><span class="sxs-lookup"><span data-stu-id="6cd13-106">You can control in Azure AD who has access tooHR2day by Merces.</span></span>
- <span data-ttu-id="6cd13-107">Du kan aktivera användarna tooautomatically hämta inloggad tooHR2day genom Merces med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="6cd13-107">You can enable your users tooautomatically get signed in tooHR2day by Merces with their Azure AD accounts.</span></span>
- <span data-ttu-id="6cd13-108">Du kan hantera dina konton i en central plats--hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6cd13-108">You can manage your accounts in one central location--hello Azure portal.</span></span>

<span data-ttu-id="6cd13-109">Mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6cd13-109">For more information about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6cd13-110">Krav</span><span class="sxs-lookup"><span data-stu-id="6cd13-110">Prerequisites</span></span>

<span data-ttu-id="6cd13-111">tooconfigure Azure AD-integrering med HR2day av Merces måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="6cd13-111">tooconfigure Azure AD integration with HR2day by Merces, you need hello following items:</span></span>

- <span data-ttu-id="6cd13-112">En Azure AD-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6cd13-112">An Azure AD subscription.</span></span>
- <span data-ttu-id="6cd13-113">En HR2day av Merces enkel inloggning aktiverad prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6cd13-113">An HR2day by Merces single sign-on enabled subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="6cd13-114">Vi rekommenderar inte att du använder en produktion miljö tootest hello stegen i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="6cd13-114">We don't recommend using a production environment tootest hello steps in this tutorial.</span></span>

<span data-ttu-id="6cd13-115">tootest hello stegen i den här självstudiekursen, Följ dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="6cd13-115">tootest hello steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="6cd13-116">Använd inte din produktionsmiljö om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="6cd13-116">Don't use your production environment unless it's necessary.</span></span>
- <span data-ttu-id="6cd13-117">Hämta en [en månaders kostnadsfri utvärderingsversion av Azure AD](https://azure.microsoft.com/pricing/free-trial/) om det inte redan har.</span><span class="sxs-lookup"><span data-stu-id="6cd13-117">Get a [one-month free trial of Azure AD](https://azure.microsoft.com/pricing/free-trial/) if you don't already have it.</span></span>  

## <a name="scenario-description"></a><span data-ttu-id="6cd13-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="6cd13-118">Scenario description</span></span>
<span data-ttu-id="6cd13-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="6cd13-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6cd13-120">hello-scenario som beskrivs här består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="6cd13-120">hello scenario that's outlined here consists of two main building blocks:</span></span>

1. <span data-ttu-id="6cd13-121">Lägger till HR2day av Merces från hello-galleriet.</span><span class="sxs-lookup"><span data-stu-id="6cd13-121">Adding HR2day by Merces from hello gallery.</span></span>
2. <span data-ttu-id="6cd13-122">Konfigurera och testa Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6cd13-122">Configuring and testing Azure AD single sign-on.</span></span>

## <a name="add-hr2day-by-merces-from-hello-gallery"></a><span data-ttu-id="6cd13-123">Lägg till HR2day av Merces från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6cd13-123">Add HR2day by Merces from hello gallery</span></span>
<span data-ttu-id="6cd13-124">tooconfigure hello integrering av HR2day av Merces i Azure AD och lägga till HR2day av Merces från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="6cd13-124">tooconfigure hello integration of HR2day by Merces into Azure AD, add HR2day by Merces from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6cd13-125">**tooadd HR2day av Merces från galleriet hello vidta hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6cd13-125">**tooadd HR2day by Merces from hello gallery, take hello following steps:**</span></span>

1. <span data-ttu-id="6cd13-126">I hello [Azure-portalen](https://portal.azure.com), på hello vänstra navigeringsfönstret, Välj hello **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6cd13-126">In hello [Azure portal](https://portal.azure.com), on hello left navigation pane, select hello **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6cd13-128">Gå för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-128">Go too**Enterprise applications**.</span></span> <span data-ttu-id="6cd13-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="6cd13-131">tooadd ett nytt program väljer hello **nytt program** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6cd13-131">tooadd a new application, select hello **New application** button on hello top of hello dialog box.</span></span>

    ![Program][3]

4. <span data-ttu-id="6cd13-133">Skriv i sökrutan hello **HR2day av Merces**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-133">In hello search box, type **HR2day by Merces**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_search.png)

5. <span data-ttu-id="6cd13-135">Markera hello resultat på panelen **HR2day av Merces**, och välj sedan hello **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="6cd13-135">In hello results panel, select **HR2day by Merces**, and then select hello **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="6cd13-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6cd13-137">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="6cd13-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med HR2day av Merces baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="6cd13-138">In this section, you configure and test Azure AD single sign-on with HR2day by Merces based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6cd13-139">För enkel inloggning toowork måste Azure AD tooknow som hello motsvarighet användaren i HR2day av Merces är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6cd13-139">For single sign-on toowork, Azure AD needs tooknow who hello counterpart user in HR2day by Merces is tooa user in Azure AD.</span></span> <span data-ttu-id="6cd13-140">Du behöver med andra ord tooestablish en länk mellan en Azure AD-användare och hello relaterade användare i HR2day av Merces.</span><span class="sxs-lookup"><span data-stu-id="6cd13-140">In other words, you need tooestablish a link between an Azure AD user and hello related user in HR2day by Merces.</span></span>

<span data-ttu-id="6cd13-141">Tilldela hello i HR2day av Merces **användarnamn** i Azure AD för **användarnamn** tooestablish hello relationen.</span><span class="sxs-lookup"><span data-stu-id="6cd13-141">In HR2day by Merces, assign hello **user name** in Azure AD too **Username** tooestablish hello relationship.</span></span>

<span data-ttu-id="6cd13-142">tooconfigure och testa Azure AD enkel inloggning med HR2day av Merces, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="6cd13-142">tooconfigure and test Azure AD single sign-on with HR2day by Merces, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6cd13-143">[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on): aktivera din användare toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="6cd13-143">[Configure Azure AD single sign-on](#configuring-azure-ad-single-sign-on): Enable your users toouse this feature.</span></span>
2. <span data-ttu-id="6cd13-144">[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user): testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6cd13-144">[Create an Azure AD test user](#creating-an-azure-ad-test-user): Test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6cd13-145">[Skapa en HR2day av Merces testanvändare](#creating-an-hr2day-by-merces-test-user): skapa en motsvarighet för Britta Simon i HR2day av Merces som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="6cd13-145">[Create an HR2day by Merces test user](#creating-an-hr2day-by-merces-test-user): Create a counterpart of Britta Simon in HR2day by Merces that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6cd13-146">[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user): aktivera Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6cd13-146">[Assign hello Azure AD test user](#assigning-the-azure-ad-test-user): Enable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6cd13-147">[Testa enkel inloggning](#testing-single-sign-on): Kontrollera om hello konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="6cd13-147">[Test single sign-on](#testing-single-sign-on): Verify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="6cd13-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6cd13-148">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="6cd13-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din HR2day av Merces program.</span><span class="sxs-lookup"><span data-stu-id="6cd13-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your HR2day by Merces application.</span></span>

<span data-ttu-id="6cd13-150">**tooconfigure Azure AD enkel inloggning med HR2day av Merces, vidta hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6cd13-150">**tooconfigure Azure AD single sign-on with HR2day by Merces, take hello following steps:**</span></span>

1. <span data-ttu-id="6cd13-151">I hello Azure-portalen på hello **HR2day av Merces** programmet integration anger **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-151">In hello Azure portal, on hello **HR2day by Merces** application integration page, select **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="6cd13-153">tooenable enkel inloggning, i hello **enkel inloggning** dialogrutan **läge** som **SAML-baserade inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-153">tooenable single sign-on, in hello **Single sign-on** dialog box, select **Mode** as **SAML-based Sign-on**.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_samlbase.png)

3. <span data-ttu-id="6cd13-155">I hello **HR2day Merces domänen och URL: er** avsnittet, vidta hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6cd13-155">In hello **HR2day by Merces Domain and URLs** section, take hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_url.png)

    <span data-ttu-id="6cd13-157">a.</span><span class="sxs-lookup"><span data-stu-id="6cd13-157">a.</span></span> <span data-ttu-id="6cd13-158">I hello **inloggnings-URL** Skriv en URL med hjälp av hello följer mönstret: `https://<tenantname>.force.com/<instancename>`.</span><span class="sxs-lookup"><span data-stu-id="6cd13-158">In hello **Sign-on URL** box, type a URL by using hello following pattern: `https://<tenantname>.force.com/<instancename>`.</span></span>

    <span data-ttu-id="6cd13-159">b.</span><span class="sxs-lookup"><span data-stu-id="6cd13-159">b.</span></span> <span data-ttu-id="6cd13-160">I hello **identifierare** Skriv en URL med hjälp av hello följer mönstret: `https://hr2day.force.com/<companyname>`.</span><span class="sxs-lookup"><span data-stu-id="6cd13-160">In hello **Identifier** box, type a URL by using hello following pattern: `https://hr2day.force.com/<companyname>`.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6cd13-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="6cd13-161">These values are not real.</span></span> <span data-ttu-id="6cd13-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="6cd13-162">Update these values with hello actual sign-on URL and identifier.</span></span> <span data-ttu-id="6cd13-163">Kontakta hello [HR2day av Merces klienten supportteamet](mailto:servicedesk@merces.nl) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="6cd13-163">Contact hello [HR2day by Merces client support team](mailto:servicedesk@merces.nl) tooget these values.</span></span> 
 


4. <span data-ttu-id="6cd13-164">På hello **SAML-signeringscertifikat** väljer **Certificate(Base64)**, och sedan spara hello certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="6cd13-164">On hello **SAML Signing Certificate** section, select **Certificate(Base64)**, and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_certificate.png) 

5. <span data-ttu-id="6cd13-166">Det här avsnittet beskrivs hur tooenable användare tooauthenticate tooHR2day av Merces med sitt konto i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6cd13-166">This section describes how tooenable users tooauthenticate tooHR2day by Merces with their account in Azure AD.</span></span> <span data-ttu-id="6cd13-167">De kan göra detta med hjälp av federation som baseras på hello SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="6cd13-167">They do this by using federation that's based on hello SAML protocol.</span></span>

    <span data-ttu-id="6cd13-168">Din HR2day av Merces program förväntar hello SAML intyg i ett specifikt format, vilket kräver tooadd attributet mappningar tooyour SAML-token.</span><span class="sxs-lookup"><span data-stu-id="6cd13-168">Your HR2day by Merces application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token.</span></span> <span data-ttu-id="6cd13-169">hello följande skärmbild visar ett exempel på detta.</span><span class="sxs-lookup"><span data-stu-id="6cd13-169">hello following screenshot shows an example of this.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_00.png)
    
    > [!NOTE] 
    <span data-ttu-id="6cd13-171">Innan du kan konfigurera hello SAML-kontroll måste du kontakta hello [HR2day av Merces klienten supportteamet](mailto:servicedesk@merces.nl) och begära hello värdet för attributet för hello-Unik identifierare för din klient.</span><span class="sxs-lookup"><span data-stu-id="6cd13-171">Before you can configure hello SAML assertion, you must contact hello [HR2day by Merces Client support team](mailto:servicedesk@merces.nl) and request hello value of hello unique identifier attribute for your tenant.</span></span> <span data-ttu-id="6cd13-172">Du behöver det här värdet toocomplete hello stegen i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6cd13-172">You need this value toocomplete hello steps in hello next section.</span></span> 

6. <span data-ttu-id="6cd13-173">I hello **enkel inloggning** i dialogrutan hello **användarattribut** avsnittet, konfigurera hello SAML-token attribut som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="6cd13-173">In hello **Single sign-on** dialog box, in hello **User Attributes** section, configure hello SAML token attribute as shown in hello following image.</span></span> <span data-ttu-id="6cd13-174">Därefter börja hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="6cd13-174">Then take hello following steps.</span></span>
    
      | <span data-ttu-id="6cd13-175">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="6cd13-175">Attribute name</span></span>    |   <span data-ttu-id="6cd13-176">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="6cd13-176">Attribute value</span></span> |  
    | ------------------- | -------------------- |    
    | <span data-ttu-id="6cd13-177">ATTR_LOGINCLAIM</span><span class="sxs-lookup"><span data-stu-id="6cd13-177">ATTR_LOGINCLAIM</span></span> | <span data-ttu-id="6cd13-178">koppling ([e] ”102938475Z” ”, @”</span><span class="sxs-lookup"><span data-stu-id="6cd13-178">join([mail],"102938475Z","@"</span></span> |
    
      <span data-ttu-id="6cd13-179">a.</span><span class="sxs-lookup"><span data-stu-id="6cd13-179">a.</span></span> <span data-ttu-id="6cd13-180">tooopen hello **lägga till attributet** markerar **Lägg till attributet**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-180">tooopen hello **Add Attribute** dialog, select **Add attribute**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="6cd13-183">b.</span><span class="sxs-lookup"><span data-stu-id="6cd13-183">b.</span></span> <span data-ttu-id="6cd13-184">I hello **namn** skriver **ATTR_LOGINCLAIM**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-184">In hello **Name** box, type **ATTR_LOGINCLAIM**.</span></span>

    <span data-ttu-id="6cd13-185">c.</span><span class="sxs-lookup"><span data-stu-id="6cd13-185">c.</span></span> <span data-ttu-id="6cd13-186">Från hello **värdet** väljer **Join()**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-186">From hello **Value** list, select **Join()**.</span></span>

    <span data-ttu-id="6cd13-187">d.</span><span class="sxs-lookup"><span data-stu-id="6cd13-187">d.</span></span> <span data-ttu-id="6cd13-188">Från hello **sträng1** väljer **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-188">From hello **String1** list, select **user.mail**.</span></span>

    <span data-ttu-id="6cd13-189">e.</span><span class="sxs-lookup"><span data-stu-id="6cd13-189">e.</span></span> <span data-ttu-id="6cd13-190">För **sträng2**, ange hello Unik identifierare som tillhandahålls av din HR2day-teamet.</span><span class="sxs-lookup"><span data-stu-id="6cd13-190">For **String2**, type hello unique identifier that's provided by your HR2day team.</span></span>

    <span data-ttu-id="6cd13-191">f.</span><span class="sxs-lookup"><span data-stu-id="6cd13-191">f.</span></span> <span data-ttu-id="6cd13-192">I hello **avgränsare** skriver  **@** .</span><span class="sxs-lookup"><span data-stu-id="6cd13-192">In hello **Separator** box, type **@**.</span></span>
    
    <span data-ttu-id="6cd13-193">g.</span><span class="sxs-lookup"><span data-stu-id="6cd13-193">g.</span></span> <span data-ttu-id="6cd13-194">Välj **Ok**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-194">Select **Ok**.</span></span>

7. <span data-ttu-id="6cd13-195">Välj hello **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="6cd13-195">Select hello **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="6cd13-197">I hello **HR2day Merces konfigurationen** väljer **konfigurera HR2day av Merces** tooopen hello **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="6cd13-197">In hello **HR2day by Merces Configuration** section, select **Configure HR2day by Merces** tooopen hello **Configure sign-on** window.</span></span> <span data-ttu-id="6cd13-198">Kopiera hello **Sign-Out URL**, **SAML enhets-ID**, och **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6cd13-198">Copy hello **Sign-Out URL**, **SAML Entity ID**, and **SAML Single Sign-On Service URL** from hello **Quick Reference** section.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_configure.png) 

9. <span data-ttu-id="6cd13-200">tooconfigure enkel inloggning för ditt program bör du kontakta hello [HR2day av Merces klienten supportteamet](mailTo:servicedesk@merces.nl).</span><span class="sxs-lookup"><span data-stu-id="6cd13-200">tooconfigure SSO  for your application, contact hello [HR2day by Merces client support team](mailTo:servicedesk@merces.nl).</span></span> <span data-ttu-id="6cd13-201">Koppla hello hämtas **Certificate(Base64)** filen tooyour e-post.</span><span class="sxs-lookup"><span data-stu-id="6cd13-201">Attach hello downloaded **Certificate(Base64)** file tooyour email.</span></span> <span data-ttu-id="6cd13-202">Ger också hello **Sign-Out URL**, **SAML enhets-ID**, och **SAML inloggning tjänst-URL för enkel** så att de kan konfigureras för SSO-integrering.</span><span class="sxs-lookup"><span data-stu-id="6cd13-202">Also provide hello **Sign-Out URL**, **SAML Entity ID**, and **SAML Single Sign-On Service URL** so that they can be configured for SSO integration.</span></span>

    > [!NOTE]
    ><span data-ttu-id="6cd13-203">Nämnt toohello Merces team som den här integreringen behöver hello enhets-ID toobe med hello mönster **https://hr2day.force.com/INSTANCENAME**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-203">Mention toohello Merces team that this integration needs hello Entity ID toobe set with hello pattern **https://hr2day.force.com/INSTANCENAME**.</span></span>

    > [!TIP]
    ><span data-ttu-id="6cd13-204">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="6cd13-204">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6cd13-205">När du lägger till den här appen från hello **Active Directory** > **företagsprogram** avsnitt, Välj hello **enkel inloggning** fliken. Sedan åtkomst hello inbäddade dokumentationen via hello **Configuration** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="6cd13-205">After you add this app from hello **Active Directory** > **Enterprise Applications** section, select hello **Single Sign-On** tab. Then access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6cd13-206">Du kan läsa mer om hello inbäddade dokumentationen funktionen i hello [Azure AD inbäddade dokumentationen]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="6cd13-206">You can read more about hello embedded documentation feature in hello [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="6cd13-207">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="6cd13-207">Create an Azure AD test user</span></span>
<span data-ttu-id="6cd13-208">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6cd13-208">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="6cd13-210">**toocreate en testanvändare i Azure AD ta hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6cd13-210">**toocreate a test user in Azure AD, take hello following steps:**</span></span>

1. <span data-ttu-id="6cd13-211">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, Välj hello **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6cd13-211">In hello **Azure portal**, on hello left navigation pane, select hello **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6cd13-213">toodisplay hello lista över användare, gå för**användare och grupper**, och välj sedan **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-213">toodisplay hello list of users, go too**Users and groups**, and then select **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6cd13-215">tooopen hello **användare** dialogrutan **Lägg till** överst hello hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6cd13-215">tooopen hello **User** dialog box, select **Add** on hello top of hello dialog box.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6cd13-217">I hello **användaren** dialogrutan vidta hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6cd13-217">In hello **User** dialog box, take hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hr2day-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6cd13-219">a.</span><span class="sxs-lookup"><span data-stu-id="6cd13-219">a.</span></span> <span data-ttu-id="6cd13-220">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-220">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6cd13-221">b.</span><span class="sxs-lookup"><span data-stu-id="6cd13-221">b.</span></span> <span data-ttu-id="6cd13-222">I hello **användarnamn** rutan, typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6cd13-222">In hello **User name** box, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6cd13-223">c.</span><span class="sxs-lookup"><span data-stu-id="6cd13-223">c.</span></span> <span data-ttu-id="6cd13-224">Välj **visa lösenordet**, och sedan skriva ned hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="6cd13-224">Select **Show Password**, and then write down hello password.</span></span>

    <span data-ttu-id="6cd13-225">d.</span><span class="sxs-lookup"><span data-stu-id="6cd13-225">d.</span></span> <span data-ttu-id="6cd13-226">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-226">Select **Create**.</span></span>
 
### <a name="create-an-hr2day-by-merces-test-user"></a><span data-ttu-id="6cd13-227">Skapa en HR2day av Merces testanvändare</span><span class="sxs-lookup"><span data-stu-id="6cd13-227">Create an HR2day by Merces test user</span></span>

<span data-ttu-id="6cd13-228">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i HR2day av Merces.</span><span class="sxs-lookup"><span data-stu-id="6cd13-228">hello objective of this section is toocreate a user called Britta Simon in HR2day by Merces.</span></span> <span data-ttu-id="6cd13-229">tooadd hello hello HR2day kontot, arbeta med hello [HR2day av Merces klienten supportteamet](mailto:servicedesk@merces.nl).</span><span class="sxs-lookup"><span data-stu-id="6cd13-229">tooadd hello users in hello HR2day account, work with hello [HR2day by Merces client support team](mailto:servicedesk@merces.nl).</span></span> 

> [!NOTE]
> <span data-ttu-id="6cd13-230">Om du behöver toocreate en användare manuellt kan kontakta hello [HR2day av Merces klienten supportteamet](mailto:servicedesk@merces.nl).</span><span class="sxs-lookup"><span data-stu-id="6cd13-230">If you need toocreate a user manually, contact hello [HR2day by Merces client support team](mailto:servicedesk@merces.nl).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="6cd13-231">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="6cd13-231">Assign hello Azure AD test user</span></span>

<span data-ttu-id="6cd13-232">I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att ge sina access tooHR2day av Merces.</span><span class="sxs-lookup"><span data-stu-id="6cd13-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooHR2day by Merces.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="6cd13-234">**tooassign Britta Simon tooHR2day av Merces, vidta hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6cd13-234">**tooassign Britta Simon tooHR2day by Merces, take hello following steps:**</span></span>

1. <span data-ttu-id="6cd13-235">I hello Azure portal, öppna hello program visa går toohello directory vy och sedan gå för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-235">In hello Azure portal, open hello applications view, go toohello directory view, and then go too**Enterprise applications**.</span></span> <span data-ttu-id="6cd13-236">Välj därefter **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-236">Next, select **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="6cd13-238">Välj i listan med program hello **HR2day av Merces**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-238">In hello applications list, select **HR2day by Merces**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_app.png) 

3. <span data-ttu-id="6cd13-240">Välj hello menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-240">In hello menu on hello left, select **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="6cd13-242">Välj hello **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="6cd13-242">Select hello **Add** button.</span></span> <span data-ttu-id="6cd13-243">Sedan hello **Lägg uppdrag** dialogrutan **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-243">Then, in hello **Add Assignment** dialog box, select **Users and groups**.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="6cd13-245">I hello **användare och grupper** i dialogrutan hello **användare** väljer **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-245">In hello **Users and groups** dialog box, in hello **Users** list, select **Britta Simon**.</span></span>

6. <span data-ttu-id="6cd13-246">Klicka på hello **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="6cd13-246">Click hello **Select** button.</span></span>

7. <span data-ttu-id="6cd13-247">I hello **Lägg uppdrag** dialogrutan **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="6cd13-247">In hello **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="6cd13-248">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6cd13-248">Test single sign-on</span></span>

<span data-ttu-id="6cd13-249">hello syftet med det här avsnittet är tootest Azure AD enkel inloggning konfigurationen med hjälp av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6cd13-249">hello objective of this section is tootest your Azure AD single sign-on configuration by using hello Access Panel.</span></span>  

<span data-ttu-id="6cd13-250">När du väljer hello HR2day av Merces panelen i hello åtkomstpanelen loggas du automatiskt hämta tooyour HR2day av Merces program.</span><span class="sxs-lookup"><span data-stu-id="6cd13-250">When you select hello HR2day by Merces tile in hello Access Panel, you automatically get signed in  tooyour HR2day by Merces application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6cd13-251">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="6cd13-251">Additional resources</span></span>

* [<span data-ttu-id="6cd13-252">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6cd13-252">List of tutorials about how tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6cd13-253">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6cd13-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_203.png

