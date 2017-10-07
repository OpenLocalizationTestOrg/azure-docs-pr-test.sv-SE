---
title: "Självstudier: Azure Active Directory-integrering med Qlik mening Enterprise | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Qlik mening Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 153edb3f8cd41790d4e9a74f15cfb806e5e9e3b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a><span data-ttu-id="c8718-103">Självstudier: Azure Active Directory-integrering med Qlik mening Enterprise</span><span class="sxs-lookup"><span data-stu-id="c8718-103">Tutorial: Azure Active Directory integration with Qlik Sense Enterprise</span></span>

<span data-ttu-id="c8718-104">I kursen får du lära dig hur toointegrate Qlik mening Enterprise med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c8718-104">In this tutorial, you learn how toointegrate Qlik Sense Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c8718-105">Integrera Qlik mening Enterprise med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c8718-105">Integrating Qlik Sense Enterprise with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c8718-106">Du kan styra i Azure AD som har åtkomst tooQlik mening Enterprise.</span><span class="sxs-lookup"><span data-stu-id="c8718-106">You can control in Azure AD who has access tooQlik Sense Enterprise.</span></span>
- <span data-ttu-id="c8718-107">Du kan låta dina användare tooautomatically get inloggade tooQlik mening Enterprise (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="c8718-107">You can enable your users tooautomatically get signed-on tooQlik Sense Enterprise (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="c8718-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c8718-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="c8718-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c8718-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c8718-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c8718-110">Prerequisites</span></span>

<span data-ttu-id="c8718-111">tooconfigure Azure AD-integrering med Qlik mening Enterprise, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="c8718-111">tooconfigure Azure AD integration with Qlik Sense Enterprise, you need hello following items:</span></span>

- <span data-ttu-id="c8718-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c8718-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c8718-113">En Qlik mening Enterprise enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="c8718-113">A Qlik Sense Enterprise single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c8718-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c8718-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c8718-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c8718-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c8718-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c8718-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c8718-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c8718-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c8718-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c8718-118">Scenario description</span></span>
<span data-ttu-id="c8718-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c8718-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c8718-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c8718-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c8718-121">Att lägga till Qlik mening Enterprise från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c8718-121">Adding Qlik Sense Enterprise from hello gallery</span></span>
2. <span data-ttu-id="c8718-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c8718-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-qlik-sense-enterprise-from-hello-gallery"></a><span data-ttu-id="c8718-123">Att lägga till Qlik mening Enterprise från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c8718-123">Adding Qlik Sense Enterprise from hello gallery</span></span>
<span data-ttu-id="c8718-124">tooconfigure hello integrering av Qlik mening Enterprise i Azure AD, behöver du tooadd Qlik mening Enterprise hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="c8718-124">tooconfigure hello integration of Qlik Sense Enterprise into Azure AD, you need tooadd Qlik Sense Enterprise from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c8718-125">**tooadd Qlik mening Enterprise från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c8718-125">**tooadd Qlik Sense Enterprise from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c8718-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c8718-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="c8718-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c8718-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c8718-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="c8718-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="c8718-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c8718-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="c8718-133">Skriv i sökrutan hello **Qlik mening Enterprise**väljer **Qlik mening Enterprise** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="c8718-133">In hello search box, type **Qlik Sense Enterprise**, select **Qlik Sense Enterprise** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Qlik mening i resultatlistan hello företag](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c8718-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c8718-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="c8718-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Qlik mening Enterprise baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c8718-136">In this section, you configure and test Azure AD single sign-on with Qlik Sense Enterprise based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c8718-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Qlik mening företag är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c8718-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Qlik Sense Enterprise is tooa user in Azure AD.</span></span> <span data-ttu-id="c8718-138">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Qlik mening Enterprise toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="c8718-138">In other words, a link relationship between an Azure AD user and hello related user in Qlik Sense Enterprise needs toobe established.</span></span>

<span data-ttu-id="c8718-139">Tilldela hello värdet för hello i Qlik mening Enterprise **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="c8718-139">In Qlik Sense Enterprise, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c8718-140">tooconfigure och testa Azure AD enkel inloggning med Qlik mening Enterprise, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c8718-140">tooconfigure and test Azure AD single sign-on with Qlik Sense Enterprise, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c8718-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c8718-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c8718-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c8718-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c8718-143">**[Skapa en testanvändare Qlik mening Enterprise](#create-a-qlik-sense-enterprise-test-user)**  -toohave en motsvarighet för Britta Simon i Qlik mening företag som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="c8718-143">**[Create a Qlik Sense Enterprise test user](#create-a-qlik-sense-enterprise-test-user)** - toohave a counterpart of Britta Simon in Qlik Sense Enterprise that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c8718-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c8718-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c8718-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c8718-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c8718-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c8718-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c8718-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Qlik mening Enterprise-programmet.</span><span class="sxs-lookup"><span data-stu-id="c8718-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Qlik Sense Enterprise application.</span></span>

<span data-ttu-id="c8718-148">**tooconfigure Azure AD enkel inloggning med Qlik mening Enterprise, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c8718-148">**tooconfigure Azure AD single sign-on with Qlik Sense Enterprise, perform hello following steps:**</span></span>

1. <span data-ttu-id="c8718-149">I hello Azure-portalen på hello **Qlik mening Enterprise** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c8718-149">In hello Azure portal, on hello **Qlik Sense Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="c8718-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c8718-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_samlbase.png)

3. <span data-ttu-id="c8718-153">På hello **Qlik mening företagsdomänen och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c8718-153">On hello **Qlik Sense Enterprise Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och Qlik mening företagsdomänen enkel inloggning information](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_url.png)

    <span data-ttu-id="c8718-155">a.</span><span class="sxs-lookup"><span data-stu-id="c8718-155">a.</span></span> <span data-ttu-id="c8718-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`</span><span class="sxs-lookup"><span data-stu-id="c8718-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="c8718-157">Observera hello avslutande snedstreck hello slutet av den här URI.</span><span class="sxs-lookup"><span data-stu-id="c8718-157">Note hello trailing slash at hello end of this URI.</span></span> <span data-ttu-id="c8718-158">Det krävs.</span><span class="sxs-lookup"><span data-stu-id="c8718-158">It is required.</span></span>
    
    <span data-ttu-id="c8718-159">b.</span><span class="sxs-lookup"><span data-stu-id="c8718-159">b.</span></span> <span data-ttu-id="c8718-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="c8718-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|

    > [!NOTE] 
    > <span data-ttu-id="c8718-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="c8718-161">These values are not real.</span></span> <span data-ttu-id="c8718-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare, som beskrivs senare i den här kursen eller kontakta [Qlik mening Enterprise-klienten supportteamet](https://www.qlik.com/us/services/support) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="c8718-162">Update these values with hello actual Sign-On URL and Identifier, Which are explained later in this tutorial or contact [Qlik Sense Enterprise Client support team](https://www.qlik.com/us/services/support) tooget these values.</span></span> 

4. <span data-ttu-id="c8718-163">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="c8718-163">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_certificate.png) 

5. <span data-ttu-id="c8718-165">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c8718-165">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c8718-167">Förbereda hello Federation Metadata XML-filen så att du kan ladda upp tooQlik mening servern.</span><span class="sxs-lookup"><span data-stu-id="c8718-167">Prepare hello Federation Metadata XML file so that you can upload that tooQlik Sense server.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="c8718-168">Innan du laddar upp hello IdP metadata toohello Qlik mening server hello-filen måste redigeras toobe tooremove information tooensure ska fungera korrekt mellan Azure AD och Qlik mening server.</span><span class="sxs-lookup"><span data-stu-id="c8718-168">Before uploading hello IdP metadata toohello Qlik Sense server, hello file needs toobe edited tooremove information tooensure proper operation between Azure AD and Qlik Sense server.</span></span>
    
    ![QlikSense][qs24]
   
    <span data-ttu-id="c8718-170">a.</span><span class="sxs-lookup"><span data-stu-id="c8718-170">a.</span></span> <span data-ttu-id="c8718-171">Öppna hello FederationMetaData.xml fil som du har hämtat från Azure-portalen i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="c8718-171">Open hello FederationMetaData.xml file, which you have downloaded from Azure portal in a text editor.</span></span>
   
    <span data-ttu-id="c8718-172">b.</span><span class="sxs-lookup"><span data-stu-id="c8718-172">b.</span></span> <span data-ttu-id="c8718-173">Söka efter hello värde **RoleDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="c8718-173">Search for hello value **RoleDescriptor**.</span></span>  <span data-ttu-id="c8718-174">Det finns fyra poster (två par inledande och avslutande elementtaggar).</span><span class="sxs-lookup"><span data-stu-id="c8718-174">There are four entries (two pairs of opening and closing element tags).</span></span>
   
    <span data-ttu-id="c8718-175">c.</span><span class="sxs-lookup"><span data-stu-id="c8718-175">c.</span></span> <span data-ttu-id="c8718-176">Ta bort hello RoleDescriptor taggar och all information mellan från hello-fil.</span><span class="sxs-lookup"><span data-stu-id="c8718-176">Delete hello RoleDescriptor tags and all information in between from hello file.</span></span>
   
    <span data-ttu-id="c8718-177">d.</span><span class="sxs-lookup"><span data-stu-id="c8718-177">d.</span></span> <span data-ttu-id="c8718-178">Spara hello-filen och spara den i närheten för användning senare i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="c8718-178">Save hello file and keep it nearby for use later in this document.</span></span>

7. <span data-ttu-id="c8718-179">Navigera toohello Qlik meningsfullt Qlik Management Console (QMC) som en användare som kan skapa virtuella proxykonfigurationer.</span><span class="sxs-lookup"><span data-stu-id="c8718-179">Navigate toohello Qlik Sense Qlik Management Console (QMC) as a user who can create virtual proxy configurations.</span></span>

8. <span data-ttu-id="c8718-180">I hello QMC, klickar du på hello **virtuella proxyservrar** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="c8718-180">In hello QMC, click on hello **Virtual Proxies** menu item.</span></span>
   
    ![QlikSense][qs6] 

9. <span data-ttu-id="c8718-182">Hello ned hello-skärmen, klickar du på hello **Skapa nytt** knappen.</span><span class="sxs-lookup"><span data-stu-id="c8718-182">At hello bottom of hello screen, click hello **Create new** button.</span></span>
   
    ![QlikSense][qs7]

10. <span data-ttu-id="c8718-184">hello visas virtuella proxy redigera.</span><span class="sxs-lookup"><span data-stu-id="c8718-184">hello Virtual proxy edit screen appears.</span></span>  <span data-ttu-id="c8718-185">På hello är höger sida av hello-skärmen en meny för att göra konfigurationsalternativ visas.</span><span class="sxs-lookup"><span data-stu-id="c8718-185">On hello right side of hello screen is a menu for making configuration options visible.</span></span>
   
    ![QlikSense][qs9]

11. <span data-ttu-id="c8718-187">Ange hello identifieringsinformation för hello Azure virtuella proxykonfigurationen med hello identifiering kontrolleras kan.</span><span class="sxs-lookup"><span data-stu-id="c8718-187">With hello Identification menu option checked, enter hello identifying information for hello Azure virtual proxy configuration.</span></span>
    
    ![QlikSense][qs8]  
    
    <span data-ttu-id="c8718-189">a.</span><span class="sxs-lookup"><span data-stu-id="c8718-189">a.</span></span> <span data-ttu-id="c8718-190">Hej **beskrivning** fältet är ett eget namn för hello virtuella proxykonfiguration.</span><span class="sxs-lookup"><span data-stu-id="c8718-190">hello **Description** field is a friendly name for hello virtual proxy configuration.</span></span>  <span data-ttu-id="c8718-191">Ange ett värde för en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="c8718-191">Enter a value for a description.</span></span>
    
    <span data-ttu-id="c8718-192">b.</span><span class="sxs-lookup"><span data-stu-id="c8718-192">b.</span></span> <span data-ttu-id="c8718-193">Hej **prefixet** identifierar fältet hello virtuella proxy slutpunkt för att ansluta tooQlik mening med Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c8718-193">hello **Prefix** field identifies hello virtual proxy endpoint for connecting tooQlik Sense with Azure AD Single Sign-On.</span></span>  <span data-ttu-id="c8718-194">Ange ett unikt prefix-namn för den här virtuella proxy.</span><span class="sxs-lookup"><span data-stu-id="c8718-194">Enter a unique prefix name for this virtual proxy.</span></span>
    
    <span data-ttu-id="c8718-195">c.</span><span class="sxs-lookup"><span data-stu-id="c8718-195">c.</span></span> <span data-ttu-id="c8718-196">**Tidsgräns för inaktivitet (minuter)** är hello tidsgränsen för anslutningar via den här virtuella proxy.</span><span class="sxs-lookup"><span data-stu-id="c8718-196">**Session inactivity timeout (minutes)** is hello timeout for connections through this virtual proxy.</span></span>
    
    <span data-ttu-id="c8718-197">d.</span><span class="sxs-lookup"><span data-stu-id="c8718-197">d.</span></span> <span data-ttu-id="c8718-198">Hej **sessionsnamnet cookie-huvud** är hello cookie namn lagra hello sessions-ID för hello Qlik mening sessionen för en användare tar emot efter en lyckad autentisering.</span><span class="sxs-lookup"><span data-stu-id="c8718-198">hello **Session cookie header name** is hello cookie name storing hello session identifier for hello Qlik Sense session a user receives after successful authentication.</span></span>  <span data-ttu-id="c8718-199">Det här namnet måste vara unikt.</span><span class="sxs-lookup"><span data-stu-id="c8718-199">This name must be unique.</span></span>

12. <span data-ttu-id="c8718-200">Klicka på hello autentisering menyn alternativet toomake den synlig.</span><span class="sxs-lookup"><span data-stu-id="c8718-200">Click on hello Authentication menu option toomake it visible.</span></span>  <span data-ttu-id="c8718-201">hello visas autentisering.</span><span class="sxs-lookup"><span data-stu-id="c8718-201">hello Authentication screen appears.</span></span>
    
    ![QlikSense][qs10]
    
    <span data-ttu-id="c8718-203">a.</span><span class="sxs-lookup"><span data-stu-id="c8718-203">a.</span></span> <span data-ttu-id="c8718-204">Hej **anonym åtkomstläge** nedrullningsbara avgör om anonyma användare kan komma åt Qlik mening via hello virtuella proxy.</span><span class="sxs-lookup"><span data-stu-id="c8718-204">hello **Anonymous access mode** drop down determines if anonymous users may access Qlik Sense through hello virtual proxy.</span></span>  <span data-ttu-id="c8718-205">hello standardalternativet är ingen anonym användare.</span><span class="sxs-lookup"><span data-stu-id="c8718-205">hello default option is No anonymous user.</span></span>
    
    <span data-ttu-id="c8718-206">b.</span><span class="sxs-lookup"><span data-stu-id="c8718-206">b.</span></span> <span data-ttu-id="c8718-207">Hej **autentiseringsmetod** nedrullningsbara styr hello autentisering schemat hello virtuella proxy används.</span><span class="sxs-lookup"><span data-stu-id="c8718-207">hello **Authentication method** drop-down determines hello authentication scheme hello virtual proxy will use.</span></span>  <span data-ttu-id="c8718-208">Välj SAML hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="c8718-208">Select SAML from hello drop-down list.</span></span>  <span data-ttu-id="c8718-209">Fler alternativ visas som ett resultat.</span><span class="sxs-lookup"><span data-stu-id="c8718-209">More options appear as a result.</span></span>
    
    <span data-ttu-id="c8718-210">c.</span><span class="sxs-lookup"><span data-stu-id="c8718-210">c.</span></span> <span data-ttu-id="c8718-211">I hello **SAML URI värdfältet**, inkommande hello hostname användare ange tooaccess Qlik mening via den här virtuella SAML-proxyn.</span><span class="sxs-lookup"><span data-stu-id="c8718-211">In hello **SAML host URI field**, input hello hostname users enter tooaccess Qlik Sense through this SAML virtual proxy.</span></span>  <span data-ttu-id="c8718-212">hello värdnamn är hello uri för hello Qlik mening server.</span><span class="sxs-lookup"><span data-stu-id="c8718-212">hello hostname is hello uri of hello Qlik Sense server.</span></span>
    
    <span data-ttu-id="c8718-213">d.</span><span class="sxs-lookup"><span data-stu-id="c8718-213">d.</span></span> <span data-ttu-id="c8718-214">I hello **SAML enhets-ID**, ange hello samma värde som angetts för hello SAML värdfältet URI.</span><span class="sxs-lookup"><span data-stu-id="c8718-214">In hello **SAML entity ID**, enter hello same value entered for hello SAML host URI field.</span></span>
    
    <span data-ttu-id="c8718-215">e.</span><span class="sxs-lookup"><span data-stu-id="c8718-215">e.</span></span> <span data-ttu-id="c8718-216">Hej **SAML IdP metadata** är hello fil redigeras tidigare i hello **redigera Federation Metadata från Azure AD-konfiguration** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c8718-216">hello **SAML IdP metadata** is hello file edited earlier in hello **Edit Federation Metadata from Azure AD Configuration** section.</span></span>  <span data-ttu-id="c8718-217">**Innan du laddar upp hello IdP metadata hello-filen måste redigeras toobe** tooremove information tooensure ska fungera korrekt mellan Azure AD och Qlik mening server.</span><span class="sxs-lookup"><span data-stu-id="c8718-217">**Before uploading hello IdP metadata, hello file needs toobe edited** tooremove information tooensure proper operation between Azure AD and Qlik Sense server.</span></span>  <span data-ttu-id="c8718-218">**Se toohello anvisningarna ovan om hello-filen har ännu toobe redigeras.**</span><span class="sxs-lookup"><span data-stu-id="c8718-218">**Please refer toohello instructions above if hello file has yet toobe edited.**</span></span>  <span data-ttu-id="c8718-219">Om hello-filen har redigerats klickar du på hello Bläddra och välj hello redigerade metadata filen tooupload den toohello virtuella proxykonfiguration.</span><span class="sxs-lookup"><span data-stu-id="c8718-219">If hello file has been edited click on hello Browse button and select hello edited metadata file tooupload it toohello virtual proxy configuration.</span></span>
    
    <span data-ttu-id="c8718-220">f.</span><span class="sxs-lookup"><span data-stu-id="c8718-220">f.</span></span> <span data-ttu-id="c8718-221">Ange hello namn eller schemat referens för hello SAML-attributet som representerar hello **UserID** Azure AD skickar toohello Qlik mening server.</span><span class="sxs-lookup"><span data-stu-id="c8718-221">Enter hello attribute name or schema reference for hello SAML attribute representing hello **UserID** Azure AD sends toohello Qlik Sense server.</span></span>  <span data-ttu-id="c8718-222">Referensinformation för schemat finns i hello Azure-app skärmar efter konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c8718-222">Schema reference information is available in hello Azure app screens post configuration.</span></span>  <span data-ttu-id="c8718-223">namnattributet för toouse hello ange `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="c8718-223">toouse hello name attribute, enter `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
    
    <span data-ttu-id="c8718-224">g.</span><span class="sxs-lookup"><span data-stu-id="c8718-224">g.</span></span> <span data-ttu-id="c8718-225">Ange hello värde för hello **användarkatalog** som ska vara anslutna toousers när de autentiserar tooQlik mening servern via Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c8718-225">Enter hello value for hello **user directory** that will be attached toousers when they authenticate tooQlik Sense server through Azure AD.</span></span>  <span data-ttu-id="c8718-226">Hårdkodad värden måste omges av **hakparentes []**.</span><span class="sxs-lookup"><span data-stu-id="c8718-226">Hardcoded values must be surrounded by **square brackets []**.</span></span>  <span data-ttu-id="c8718-227">toouse ett attribut som skickas i hello Azure AD SAML assertion ange hello hello attributets namn i den här textrutan **utan** hakparenteser.</span><span class="sxs-lookup"><span data-stu-id="c8718-227">toouse an attribute sent in hello Azure AD SAML assertion, enter hello name of hello attribute in this text box **without** square brackets.</span></span>
    
    <span data-ttu-id="c8718-228">h.</span><span class="sxs-lookup"><span data-stu-id="c8718-228">h.</span></span> <span data-ttu-id="c8718-229">Hej **SAML Signeringsalgoritm** anger hello service provider (i det här fallet Qlik mening server) om certifikatsignering för hello virtuella proxykonfiguration.</span><span class="sxs-lookup"><span data-stu-id="c8718-229">hello **SAML signing algorithm** sets hello service provider (in this case Qlik Sense server) certificate signing for hello virtual proxy configuration.</span></span>  <span data-ttu-id="c8718-230">Om Qlik mening servern använder ett betrott certifikat som genereras med hjälp av Microsoft Enhanced RSA och AES Cryptographic Provider, ändra hello SAML Signeringsalgoritm för**SHA-256**.</span><span class="sxs-lookup"><span data-stu-id="c8718-230">If Qlik Sense server uses a trusted certificate generated using Microsoft Enhanced RSA and AES Cryptographic Provider, change hello SAML signing algorithm too**SHA-256**.</span></span>
    
    <span data-ttu-id="c8718-231">Jag.</span><span class="sxs-lookup"><span data-stu-id="c8718-231">i.</span></span> <span data-ttu-id="c8718-232">hello SAML attributet mappning avsnittet tillåter ytterligare attribut som grupper toobe skickas tooQlik mening för användning i säkerhetsregler.</span><span class="sxs-lookup"><span data-stu-id="c8718-232">hello SAML attribute mapping section allows for additional attributes like groups toobe sent tooQlik Sense for use in security rules.</span></span>

13. <span data-ttu-id="c8718-233">Klicka på hello **BELASTNINGSUTJÄMNING** menyn alternativet toomake den synlig.</span><span class="sxs-lookup"><span data-stu-id="c8718-233">Click on hello **LOAD BALANCING** menu option toomake it visible.</span></span>  <span data-ttu-id="c8718-234">hello visas belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="c8718-234">hello Load Balancing screen appears.</span></span>
    
    ![QlikSense][qs11]

14. <span data-ttu-id="c8718-236">Klicka på hello **Lägg till ny servernoden** knappen Välj motorn nod eller noder Qlik mening ska skicka sessioner toofor belastningsutjämning ändamål och på hello **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c8718-236">Click on hello **Add new server node** button, select engine node or nodes Qlik Sense will send sessions toofor load balancing purposes, and click hello **Add** button.</span></span>
    
    ![QlikSense][qs12]

15. <span data-ttu-id="c8718-238">Klicka på hello avancerade menyn alternativet toomake den synlig.</span><span class="sxs-lookup"><span data-stu-id="c8718-238">Click on hello Advanced menu option toomake it visible.</span></span> <span data-ttu-id="c8718-239">Avancerade hello-skärmen visas.</span><span class="sxs-lookup"><span data-stu-id="c8718-239">hello Advanced screen appears.</span></span>
    
    ![QlikSense][qs13]
    
    <span data-ttu-id="c8718-241">hello värden vitlista identifierar värdnamn som accepteras när du ansluter toohello Qlik mening server.</span><span class="sxs-lookup"><span data-stu-id="c8718-241">hello Host white list identifies hostnames that are accepted when connecting toohello Qlik Sense server.</span></span>  <span data-ttu-id="c8718-242">**Ange hello värdnamn användarna anger när du ansluter tooQlik mening server.**</span><span class="sxs-lookup"><span data-stu-id="c8718-242">**Enter hello hostname users will specify when connecting tooQlik Sense server.**</span></span> <span data-ttu-id="c8718-243">hello-värdnamnet är hello samma värde som hello uri för SAML-värden utan hello https://.</span><span class="sxs-lookup"><span data-stu-id="c8718-243">hello hostname is hello same value as hello SAML host uri without hello https://.</span></span>

16. <span data-ttu-id="c8718-244">Klicka på hello **tillämpa** knappen.</span><span class="sxs-lookup"><span data-stu-id="c8718-244">Click hello **Apply** button.</span></span>
    
    ![QlikSense][qs14]

17. <span data-ttu-id="c8718-246">Klicka på OK tooaccept hello varning om proxyservrar länkade toohello virtuella proxy kommer att startas om.</span><span class="sxs-lookup"><span data-stu-id="c8718-246">Click OK tooaccept hello warning message that states proxies linked toohello virtual proxy will be restarted.</span></span>
    
    ![QlikSense][qs15]

18. <span data-ttu-id="c8718-248">Hello höger på hello-skärmen visas hello associerade objekt.</span><span class="sxs-lookup"><span data-stu-id="c8718-248">On hello right side of hello screen, hello Associated items menu appears.</span></span>  <span data-ttu-id="c8718-249">Klicka på hello **proxyservrar** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="c8718-249">Click on hello **Proxies** menu option.</span></span>
    
    ![QlikSense][qs16]

19. <span data-ttu-id="c8718-251">hello visas proxy.</span><span class="sxs-lookup"><span data-stu-id="c8718-251">hello proxy screen appears.</span></span>  <span data-ttu-id="c8718-252">Klicka på hello **länk** längst hello nedre toolink proxy toohello virtuella proxy.</span><span class="sxs-lookup"><span data-stu-id="c8718-252">Click hello **Link** button at hello bottom toolink a proxy toohello virtual proxy.</span></span>
    
    ![QlikSense][qs17]

20. <span data-ttu-id="c8718-254">Välj hello proxy-nod som stöd för den här virtuella proxyanslutningen och på hello **länk** knappen.</span><span class="sxs-lookup"><span data-stu-id="c8718-254">Select hello proxy node that will support this virtual proxy connection and click hello **Link** button.</span></span>  <span data-ttu-id="c8718-255">När du länkar visas hello proxy under associerade proxyservrar.</span><span class="sxs-lookup"><span data-stu-id="c8718-255">After linking, hello proxy will be listed under associated proxies.</span></span>
    
    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

21. <span data-ttu-id="c8718-258">Efter fem tooten sekunder hello uppdatera QMC meddelandet visas.</span><span class="sxs-lookup"><span data-stu-id="c8718-258">After about five tooten seconds, hello Refresh QMC message will appear.</span></span>  <span data-ttu-id="c8718-259">Klicka på hello **uppdatera QMC** knappen.</span><span class="sxs-lookup"><span data-stu-id="c8718-259">Click hello **Refresh QMC** button.</span></span>
    
    ![QlikSense][qs20]

22. <span data-ttu-id="c8718-261">När hello QMC uppdaterar, klicka på hello **virtuella proxyservrar** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="c8718-261">When hello QMC refreshes, click on hello **Virtual proxies** menu item.</span></span> <span data-ttu-id="c8718-262">hello ny SAML virtuella proxy post anges i hello tabell på hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="c8718-262">hello new SAML virtual proxy entry is listed in hello table on hello screen.</span></span>  <span data-ttu-id="c8718-263">Klicka på hello virtuella proxy post.</span><span class="sxs-lookup"><span data-stu-id="c8718-263">Single click on hello virtual proxy entry.</span></span>
    
    ![QlikSense][qs51]

23. <span data-ttu-id="c8718-265">Längst ned hello hello-skärmen aktiveras hello hämta SP metadata knappen.</span><span class="sxs-lookup"><span data-stu-id="c8718-265">At hello bottom of hello screen, hello Download SP metadata button will activate.</span></span>  <span data-ttu-id="c8718-266">Klicka på hello **hämta SP metadata** tooa för knappen toosave hello metadatafil.</span><span class="sxs-lookup"><span data-stu-id="c8718-266">Click hello **Download SP metadata** button toosave hello metadata tooa file.</span></span>
    
    ![QlikSense][qs52]

24. <span data-ttu-id="c8718-268">Öppna hello sp-metadatafil.</span><span class="sxs-lookup"><span data-stu-id="c8718-268">Open hello sp metadata file.</span></span>  <span data-ttu-id="c8718-269">Se hello **ID för entiteterna** transaktionen och hello **AssertionConsumerService** transaktionen.</span><span class="sxs-lookup"><span data-stu-id="c8718-269">Observe hello **entityID** entry and hello **AssertionConsumerService** entry.</span></span>  <span data-ttu-id="c8718-270">Dessa värden är likvärdig toohello **identifierare** och hello **inloggning URL** i programkonfigurationen hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c8718-270">These values are equivalent toohello **Identifier** and hello **Sign on URL** in hello Azure AD application configuration.</span></span> <span data-ttu-id="c8718-271">Klistra in dessa värden i hello **Qlik mening företagsdomänen och URL: er** avsnitt i hello Azure AD-programkonfigurationen om de matchar inte varandra, och sedan bör du ersätta dem i konfigurationsguiden för hello Azure AD-App.</span><span class="sxs-lookup"><span data-stu-id="c8718-271">Paste these values in hello **Qlik Sense Enterprise Domain and URLs** section in hello Azure AD application configuration if they are not matching, then you should replace them in hello Azure AD App configuration wizard.</span></span>
    
    ![QlikSense][qs53]

> [!TIP]
> <span data-ttu-id="c8718-273">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="c8718-273">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c8718-274">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="c8718-274">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c8718-275">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c8718-275">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c8718-276">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c8718-276">Create an Azure AD test user</span></span>
<span data-ttu-id="c8718-277">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c8718-277">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="c8718-279">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c8718-279">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c8718-280">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="c8718-280">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

   ![hello Azure Active Directory-knappen](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="c8718-282">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c8718-282">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

   ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="c8718-284">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c8718-284">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

   ![hello webbinställningar](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="c8718-286">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c8718-286">In hello **User** dialog box, perform hello following steps:</span></span>

   ![hello användardialogrutan](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png)

   <span data-ttu-id="c8718-288">a.</span><span class="sxs-lookup"><span data-stu-id="c8718-288">a.</span></span> <span data-ttu-id="c8718-289">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c8718-289">In hello **Name** box, type **BrittaSimon**.</span></span>

   <span data-ttu-id="c8718-290">b.</span><span class="sxs-lookup"><span data-stu-id="c8718-290">b.</span></span> <span data-ttu-id="c8718-291">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c8718-291">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

   <span data-ttu-id="c8718-292">c.</span><span class="sxs-lookup"><span data-stu-id="c8718-292">c.</span></span> <span data-ttu-id="c8718-293">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="c8718-293">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

   <span data-ttu-id="c8718-294">d.</span><span class="sxs-lookup"><span data-stu-id="c8718-294">d.</span></span> <span data-ttu-id="c8718-295">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c8718-295">Click **Create**.</span></span>
 
### <a name="create-a-qlik-sense-enterprise-test-user"></a><span data-ttu-id="c8718-296">Skapa en testanvändare Qlik mening Enterprise</span><span class="sxs-lookup"><span data-stu-id="c8718-296">Create a Qlik Sense Enterprise test user</span></span>

<span data-ttu-id="c8718-297">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Qlik mening Enterprise.</span><span class="sxs-lookup"><span data-stu-id="c8718-297">In this section, you create a user called Britta Simon in Qlik Sense Enterprise.</span></span> <span data-ttu-id="c8718-298">Arbeta med [Qlik mening Enterprise-klienten supportteamet](https://www.qlik.com/us/services/support) att lägga till hello användare i hello Qlik mening Enterprise-plattformen.</span><span class="sxs-lookup"><span data-stu-id="c8718-298">Work with [Qlik Sense Enterprise Client support team](https://www.qlik.com/us/services/support) to add hello users in hello Qlik Sense Enterprise platform.</span></span> <span data-ttu-id="c8718-299">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c8718-299">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="c8718-300">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="c8718-300">Assign hello Azure AD test user</span></span>

<span data-ttu-id="c8718-301">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooQlik mening Enterprise.</span><span class="sxs-lookup"><span data-stu-id="c8718-301">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooQlik Sense Enterprise.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="c8718-303">**tooassign Britta Simon tooQlik mening Enterprise, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c8718-303">**tooassign Britta Simon tooQlik Sense Enterprise, perform hello following steps:**</span></span>

1. <span data-ttu-id="c8718-304">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c8718-304">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c8718-306">Välj i listan med program hello **Qlik mening Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="c8718-306">In hello applications list, select **Qlik Sense Enterprise**.</span></span>

    ![Hej Qlik mening Enterprise länken i listan med program hello](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_app.png)  

3. <span data-ttu-id="c8718-308">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c8718-308">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="c8718-310">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c8718-310">Click **Add** button.</span></span> <span data-ttu-id="c8718-311">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c8718-311">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="c8718-313">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="c8718-313">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c8718-314">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c8718-314">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c8718-315">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c8718-315">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c8718-316">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c8718-316">Test single sign-on</span></span>

<span data-ttu-id="c8718-317">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c8718-317">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c8718-318">När du klickar på hello Qlik mening Enterprise-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Qlik mening företagsprogram.</span><span class="sxs-lookup"><span data-stu-id="c8718-318">When you click hello Qlik Sense Enterprise tile in hello Access Panel, you should get automatically signed-on tooyour Qlik Sense Enterprise application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c8718-319">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c8718-319">Additional resources</span></span>

* [<span data-ttu-id="c8718-320">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c8718-320">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c8718-321">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c8718-321">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

