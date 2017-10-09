---
title: "Självstudier: Azure Active Directory-integrering med OfficeSpace programvara | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan OfficeSpace programvaran och Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 95d8413f-db98-4e2c-8097-9142ef1af823
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: b53afb648b8a6057c32c782d857e34c06e152c67
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-officespace-software"></a><span data-ttu-id="52c08-103">Självstudier: Azure Active Directory-integrering med OfficeSpace programvara</span><span class="sxs-lookup"><span data-stu-id="52c08-103">Tutorial: Azure Active Directory integration with OfficeSpace Software</span></span>

<span data-ttu-id="52c08-104">I kursen får du lära dig hur toointegrate OfficeSpace programvara med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="52c08-104">In this tutorial, you learn how toointegrate OfficeSpace Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="52c08-105">Integrera OfficeSpace programvara med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="52c08-105">Integrating OfficeSpace Software with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="52c08-106">Du kan styra i Azure AD som har åtkomst tooOfficeSpace programvara.</span><span class="sxs-lookup"><span data-stu-id="52c08-106">You can control in Azure AD who has access tooOfficeSpace Software.</span></span>
- <span data-ttu-id="52c08-107">Du kan låta dina användare tooautomatically get inloggade tooOfficeSpace programvara (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="52c08-107">You can enable your users tooautomatically get signed-on tooOfficeSpace Software (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="52c08-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="52c08-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="52c08-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="52c08-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="52c08-110">Krav</span><span class="sxs-lookup"><span data-stu-id="52c08-110">Prerequisites</span></span>

<span data-ttu-id="52c08-111">tooconfigure Azure AD-integrering med OfficeSpace programvara, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="52c08-111">tooconfigure Azure AD integration with OfficeSpace Software, you need hello following items:</span></span>

- <span data-ttu-id="52c08-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="52c08-112">An Azure AD subscription</span></span>
- <span data-ttu-id="52c08-113">En OfficeSpace programvara enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="52c08-113">A OfficeSpace Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="52c08-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="52c08-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="52c08-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="52c08-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="52c08-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="52c08-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="52c08-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="52c08-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="52c08-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="52c08-118">Scenario description</span></span>
<span data-ttu-id="52c08-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="52c08-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="52c08-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="52c08-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="52c08-121">Lägga till OfficeSpace programvara från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="52c08-121">Adding OfficeSpace Software from hello gallery</span></span>
2. <span data-ttu-id="52c08-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="52c08-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-officespace-software-from-hello-gallery"></a><span data-ttu-id="52c08-123">Lägga till OfficeSpace programvara från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="52c08-123">Adding OfficeSpace Software from hello gallery</span></span>
<span data-ttu-id="52c08-124">tooconfigure hello integrering av OfficeSpace programvara i Azure AD, behöver du tooadd OfficeSpace programvara från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="52c08-124">tooconfigure hello integration of OfficeSpace Software into Azure AD, you need tooadd OfficeSpace Software from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="52c08-125">**tooadd OfficeSpace programvara från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="52c08-125">**tooadd OfficeSpace Software from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="52c08-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="52c08-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="52c08-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="52c08-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="52c08-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="52c08-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="52c08-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="52c08-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="52c08-133">Skriv i sökrutan hello **OfficeSpace programvara**väljer **OfficeSpace programvara** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="52c08-133">In hello search box, type **OfficeSpace Software**, select **OfficeSpace Software** from result panel then click **Add** button tooadd hello application.</span></span>

    ![OfficeSpace programvara i hello resulterar lista](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="52c08-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="52c08-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="52c08-136">Du konfigurera och testa Azure AD enkel inloggning med OfficeSpace programvara baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="52c08-136">In this section, you configure and test Azure AD single sign-on with OfficeSpace Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="52c08-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i OfficeSpace programvara är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="52c08-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in OfficeSpace Software is tooa user in Azure AD.</span></span> <span data-ttu-id="52c08-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i OfficeSpace programvara toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="52c08-138">In other words, a link relationship between an Azure AD user and hello related user in OfficeSpace Software needs toobe established.</span></span>

<span data-ttu-id="52c08-139">I OfficeSpace programvara, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="52c08-139">In OfficeSpace Software, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="52c08-140">tooconfigure och testa Azure AD enkel inloggning med OfficeSpace programvara, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="52c08-140">tooconfigure and test Azure AD single sign-on with OfficeSpace Software, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="52c08-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="52c08-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="52c08-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="52c08-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="52c08-143">**[Skapa en testanvändare OfficeSpace programvara](#create-a-officespace-software-test-user)**  -toohave en motsvarighet för Britta Simon OfficeSpace programvara som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="52c08-143">**[Create a OfficeSpace Software test user](#create-a-officespace-software-test-user)** - toohave a counterpart of Britta Simon in OfficeSpace Software that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="52c08-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="52c08-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="52c08-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="52c08-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="52c08-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="52c08-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="52c08-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i OfficeSpace programmet.</span><span class="sxs-lookup"><span data-stu-id="52c08-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your OfficeSpace Software application.</span></span>

<span data-ttu-id="52c08-148">**Utför följande hello tooconfigure Azure AD enkel inloggning med OfficeSpace programvara:**</span><span class="sxs-lookup"><span data-stu-id="52c08-148">**tooconfigure Azure AD single sign-on with OfficeSpace Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="52c08-149">I hello Azure-portalen på hello **OfficeSpace programvara** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="52c08-149">In hello Azure portal, on hello **OfficeSpace Software** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="52c08-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="52c08-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_samlbase.png)

3. <span data-ttu-id="52c08-153">På hello **OfficeSpace programvara domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="52c08-153">On hello **OfficeSpace Software Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och OfficeSpace programvara domän med enkel inloggning information](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_url.png)

    <span data-ttu-id="52c08-155">a.</span><span class="sxs-lookup"><span data-stu-id="52c08-155">a.</span></span> <span data-ttu-id="52c08-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.officespacesoftware.com/users/sign_in/saml`</span><span class="sxs-lookup"><span data-stu-id="52c08-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.officespacesoftware.com/users/sign_in/saml`</span></span>

    <span data-ttu-id="52c08-157">b.</span><span class="sxs-lookup"><span data-stu-id="52c08-157">b.</span></span> <span data-ttu-id="52c08-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`<company name>.officespacesoftware.com`</span><span class="sxs-lookup"><span data-stu-id="52c08-158">In hello **Identifier** textbox, type a URL using hello following pattern: `<company name>.officespacesoftware.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="52c08-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="52c08-159">These values are not real.</span></span> <span data-ttu-id="52c08-160">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="52c08-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="52c08-161">Kontakta [OfficeSpace Programvaruklienten supportteamet](mailto:support@officespacesoftware.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="52c08-161">Contact [OfficeSpace Software Client support team](mailto:support@officespacesoftware.com) tooget these values.</span></span> 

4. <span data-ttu-id="52c08-162">OfficeSpace program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="52c08-162">OfficeSpace Software application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="52c08-163">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="52c08-163">Please configure hello following claims for this application.</span></span> <span data-ttu-id="52c08-164">Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="52c08-164">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="52c08-165">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="52c08-165">hello following screenshot shows an example for this.</span></span>
    
    ![Konfigurera attribut](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_attribute.png)

5. <span data-ttu-id="52c08-167">I hello **användarattribut** avsnittet hello **enkel inloggning** markerar **user.mail** som **användar-ID** och för varje rad som visas i hello tabellen nedan, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="52c08-167">In hello **User Attributes** section on hello **Single sign-on** dialog, select **user.mail**  as **User Identifier** and for each row shown in hello table below, perform hello following steps:</span></span>
    
    | <span data-ttu-id="52c08-168">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="52c08-168">Attribute Name</span></span> | <span data-ttu-id="52c08-169">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="52c08-169">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="52c08-170">E-post</span><span class="sxs-lookup"><span data-stu-id="52c08-170">email</span></span> | <span data-ttu-id="52c08-171">User.Mail</span><span class="sxs-lookup"><span data-stu-id="52c08-171">user.mail</span></span> |
    | <span data-ttu-id="52c08-172">namn</span><span class="sxs-lookup"><span data-stu-id="52c08-172">name</span></span> | <span data-ttu-id="52c08-173">User.DisplayName</span><span class="sxs-lookup"><span data-stu-id="52c08-173">user.displayname</span></span> |
    | <span data-ttu-id="52c08-174">Förnamn</span><span class="sxs-lookup"><span data-stu-id="52c08-174">first_name</span></span> | <span data-ttu-id="52c08-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="52c08-175">user.givenname</span></span> |
    | <span data-ttu-id="52c08-176">Efternamn</span><span class="sxs-lookup"><span data-stu-id="52c08-176">last_name</span></span> | <span data-ttu-id="52c08-177">User.surname</span><span class="sxs-lookup"><span data-stu-id="52c08-177">user.surname</span></span> |

    <span data-ttu-id="52c08-178">a.</span><span class="sxs-lookup"><span data-stu-id="52c08-178">a.</span></span> <span data-ttu-id="52c08-179">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="52c08-179">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![<span data-ttu-id="52c08-180">Konfigurera Lägg till</span><span class="sxs-lookup"><span data-stu-id="52c08-180">Configure Add</span></span> ](./media/active-directory-saas-officespace-tutorial/tutorial_attribute_04.png)

    ![Konfigurera attribut](./media/active-directory-saas-officespace-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="52c08-182">b.</span><span class="sxs-lookup"><span data-stu-id="52c08-182">b.</span></span> <span data-ttu-id="52c08-183">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="52c08-183">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="52c08-184">c.</span><span class="sxs-lookup"><span data-stu-id="52c08-184">c.</span></span> <span data-ttu-id="52c08-185">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="52c08-185">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="52c08-186">d.</span><span class="sxs-lookup"><span data-stu-id="52c08-186">d.</span></span> <span data-ttu-id="52c08-187">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="52c08-187">Click **Ok**</span></span>
 
6. <span data-ttu-id="52c08-188">På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet för hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="52c08-188">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of hello certificate.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_certificate.png) 

7. <span data-ttu-id="52c08-190">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="52c08-190">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-officespace-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="52c08-192">På hello **OfficeSpace programvarukonfiguration** klickar du på **konfigurera OfficeSpace programvara** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="52c08-192">On hello **OfficeSpace Software Configuration** section, click **Configure OfficeSpace Software** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="52c08-193">Kopiera hello **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="52c08-193">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfiguration av OfficeSpace programvara](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_configure.png) 

9. <span data-ttu-id="52c08-195">Logga in på din OfficeSpace programvara klient som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="52c08-195">In a different web browser window, log into your OfficeSpace Software tenant as an administrator.</span></span>

10. <span data-ttu-id="52c08-196">Gå för**inställningar** och på **kopplingar**.</span><span class="sxs-lookup"><span data-stu-id="52c08-196">Go too**Settings** and click **Connectors**.</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_002.png)

11. <span data-ttu-id="52c08-198">Klicka på **SAML-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="52c08-198">Click **SAML Authentication**.</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_003.png)

12. <span data-ttu-id="52c08-200">I hello **SAML-autentisering** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="52c08-200">In hello **SAML Authentication** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_004.png)

    <span data-ttu-id="52c08-202">a.</span><span class="sxs-lookup"><span data-stu-id="52c08-202">a.</span></span> <span data-ttu-id="52c08-203">I hello **logga ut providern url** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="52c08-203">In hello **Logout provider url** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="52c08-204">b.</span><span class="sxs-lookup"><span data-stu-id="52c08-204">b.</span></span> <span data-ttu-id="52c08-205">I hello **klienten idp mål-url** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="52c08-205">In hello **Client idp target url** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="52c08-206">c.</span><span class="sxs-lookup"><span data-stu-id="52c08-206">c.</span></span> <span data-ttu-id="52c08-207">Klistra in hello **tumavtrycket** värde som du har kopierat från Azure-portalen i hello **klienten IDP certifikat fingeravtryck** textruta.</span><span class="sxs-lookup"><span data-stu-id="52c08-207">Paste hello **Thumbprint** value which you have copied from Azure portal, into hello **Client IDP certificate fingerprint** textbox.</span></span> 

    <span data-ttu-id="52c08-208">d.</span><span class="sxs-lookup"><span data-stu-id="52c08-208">d.</span></span> <span data-ttu-id="52c08-209">Klicka på **Spara inställningar**.</span><span class="sxs-lookup"><span data-stu-id="52c08-209">Click **Save Settings**.</span></span>


> [!TIP]
> <span data-ttu-id="52c08-210">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="52c08-210">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="52c08-211">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="52c08-211">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="52c08-212">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="52c08-212">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="52c08-213">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="52c08-213">Create an Azure AD test user</span></span>

<span data-ttu-id="52c08-214">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="52c08-214">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="52c08-216">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="52c08-216">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="52c08-217">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="52c08-217">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-officespace-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="52c08-219">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="52c08-219">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-officespace-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="52c08-221">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="52c08-221">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-officespace-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="52c08-223">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="52c08-223">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-officespace-tutorial/create_aaduser_04.png)

    <span data-ttu-id="52c08-225">a.</span><span class="sxs-lookup"><span data-stu-id="52c08-225">a.</span></span> <span data-ttu-id="52c08-226">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="52c08-226">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="52c08-227">b.</span><span class="sxs-lookup"><span data-stu-id="52c08-227">b.</span></span> <span data-ttu-id="52c08-228">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="52c08-228">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="52c08-229">c.</span><span class="sxs-lookup"><span data-stu-id="52c08-229">c.</span></span> <span data-ttu-id="52c08-230">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="52c08-230">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="52c08-231">d.</span><span class="sxs-lookup"><span data-stu-id="52c08-231">d.</span></span> <span data-ttu-id="52c08-232">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="52c08-232">Click **Create**.</span></span>
 
### <a name="create-a-officespace-software-test-user"></a><span data-ttu-id="52c08-233">Skapa en testanvändare OfficeSpace programvara</span><span class="sxs-lookup"><span data-stu-id="52c08-233">Create a OfficeSpace Software test user</span></span>

<span data-ttu-id="52c08-234">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon OfficeSpace programvaran.</span><span class="sxs-lookup"><span data-stu-id="52c08-234">hello objective of this section is toocreate a user called Britta Simon in OfficeSpace Software.</span></span> <span data-ttu-id="52c08-235">OfficeSpace programvara stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="52c08-235">OfficeSpace Software supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="52c08-236">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="52c08-236">There is no action item for you in this section.</span></span> <span data-ttu-id="52c08-237">En ny användare skapas under ett försök tooaccess OfficeSpace programvara om det inte finns.</span><span class="sxs-lookup"><span data-stu-id="52c08-237">A new user will be created during an attempt tooaccess OfficeSpace Software if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="52c08-238">Om du behöver toocreate en användare manuellt, måste tooContact [OfficeSpace programvara supportteamet](mailto:support@officespacesoftware.com).</span><span class="sxs-lookup"><span data-stu-id="52c08-238">If you need toocreate an user manually, you need tooContact [OfficeSpace Software support team](mailto:support@officespacesoftware.com).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="52c08-239">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="52c08-239">Assign hello Azure AD test user</span></span>

<span data-ttu-id="52c08-240">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooOfficeSpace programvara.</span><span class="sxs-lookup"><span data-stu-id="52c08-240">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooOfficeSpace Software.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="52c08-242">**tooassign Britta Simon tooOfficeSpace programvara, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="52c08-242">**tooassign Britta Simon tooOfficeSpace Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="52c08-243">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="52c08-243">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="52c08-245">Välj i listan med program hello **OfficeSpace programvara**.</span><span class="sxs-lookup"><span data-stu-id="52c08-245">In hello applications list, select **OfficeSpace Software**.</span></span>

    ![Hej OfficeSpace programvarulänk i listan med program hello](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_app.png)  

3. <span data-ttu-id="52c08-247">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="52c08-247">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="52c08-249">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="52c08-249">Click **Add** button.</span></span> <span data-ttu-id="52c08-250">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="52c08-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="52c08-252">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="52c08-252">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="52c08-253">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="52c08-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="52c08-254">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="52c08-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="52c08-255">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="52c08-255">Test single sign-on</span></span>

<span data-ttu-id="52c08-256">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="52c08-256">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="52c08-257">När du klickar på hello OfficeSpace programvara panelen i hello åtkomstpanelen får automatiskt inloggade tooyour OfficeSpace program.</span><span class="sxs-lookup"><span data-stu-id="52c08-257">When you click hello OfficeSpace Software tile in hello Access Panel, you should get automatically signed-on tooyour OfficeSpace Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="52c08-258">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="52c08-258">Additional resources</span></span>

* [<span data-ttu-id="52c08-259">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="52c08-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="52c08-260">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="52c08-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_203.png

