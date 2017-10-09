---
title: "Självstudier: Azure Active Directory-integrering med ADP Globalview | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ADP Globalview."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffb6464f-714d-41a9-869a-2b7e5ae9f125
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: aee2d56f05b486d12facbc41c9503455094604ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adp-globalview"></a><span data-ttu-id="baa01-103">Självstudier: Azure Active Directory-integrering med ADP Globalview</span><span class="sxs-lookup"><span data-stu-id="baa01-103">Tutorial: Azure Active Directory integration with ADP Globalview</span></span>

<span data-ttu-id="baa01-104">I kursen får du lära dig hur toointegrate ADP Globalview med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="baa01-104">In this tutorial, you learn how toointegrate ADP Globalview with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="baa01-105">Integrera ADP Globalview med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="baa01-105">Integrating ADP Globalview with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="baa01-106">Du kan styra i Azure AD som har åtkomst tooADP Globalview</span><span class="sxs-lookup"><span data-stu-id="baa01-106">You can control in Azure AD who has access tooADP Globalview</span></span>
- <span data-ttu-id="baa01-107">Du kan aktivera din användare tooautomatically get inloggade tooADP Globalview (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="baa01-107">You can enable your users tooautomatically get signed-on tooADP Globalview (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="baa01-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="baa01-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="baa01-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="baa01-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="baa01-110">Krav</span><span class="sxs-lookup"><span data-stu-id="baa01-110">Prerequisites</span></span>

<span data-ttu-id="baa01-111">tooconfigure Azure AD-integrering med ADP Globalview måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="baa01-111">tooconfigure Azure AD integration with ADP Globalview, you need hello following items:</span></span>

- <span data-ttu-id="baa01-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="baa01-112">An Azure AD subscription</span></span>
- <span data-ttu-id="baa01-113">En ADP Globalview enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="baa01-113">An ADP Globalview single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="baa01-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="baa01-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="baa01-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="baa01-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="baa01-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="baa01-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="baa01-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="baa01-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="baa01-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="baa01-118">Scenario description</span></span>
<span data-ttu-id="baa01-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="baa01-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="baa01-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="baa01-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="baa01-121">Att lägga till ADP Globalview från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="baa01-121">Adding ADP Globalview from hello gallery</span></span>
2. <span data-ttu-id="baa01-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="baa01-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adp-globalview-from-hello-gallery"></a><span data-ttu-id="baa01-123">Att lägga till ADP Globalview från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="baa01-123">Adding ADP Globalview from hello gallery</span></span>
<span data-ttu-id="baa01-124">tooconfigure hello integrering av ADP Globalview i Azure AD, behöver du tooadd ADP Globalview hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="baa01-124">tooconfigure hello integration of ADP Globalview into Azure AD, you need tooadd ADP Globalview from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="baa01-125">**tooadd ADP Globalview från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="baa01-125">**tooadd ADP Globalview from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="baa01-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="baa01-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="baa01-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="baa01-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="baa01-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="baa01-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="baa01-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="baa01-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="baa01-133">Skriv i sökrutan hello **ADP Globalview**.</span><span class="sxs-lookup"><span data-stu-id="baa01-133">In hello search box, type **ADP Globalview**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_search.png)

5. <span data-ttu-id="baa01-135">Markera hello resultat på panelen **ADP Globalview**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="baa01-135">In hello results panel, select **ADP Globalview**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="baa01-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="baa01-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="baa01-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ADP Globalview baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="baa01-138">In this section, you configure and test Azure AD single sign-on with ADP Globalview based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="baa01-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ADP Globalview är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="baa01-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ADP Globalview is tooa user in Azure AD.</span></span> <span data-ttu-id="baa01-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i ADP Globalview toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="baa01-140">In other words, a link relationship between an Azure AD user and hello related user in ADP Globalview needs toobe established.</span></span>

<span data-ttu-id="baa01-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i ADP Globalview.</span><span class="sxs-lookup"><span data-stu-id="baa01-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ADP Globalview.</span></span>

<span data-ttu-id="baa01-142">tooconfigure och testa Azure AD enkel inloggning med ADP Globalview, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="baa01-142">tooconfigure and test Azure AD single sign-on with ADP Globalview, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="baa01-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="baa01-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="baa01-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="baa01-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="baa01-145">**[Skapa en testanvändare ADP Globalview](#creating-an-adp-globalview-test-user)**  -toohave en motsvarighet för Britta Simon i ADP Globalview som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="baa01-145">**[Creating an ADP Globalview test user](#creating-an-adp-globalview-test-user)** - toohave a counterpart of Britta Simon in ADP Globalview that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="baa01-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="baa01-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="baa01-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="baa01-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="baa01-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="baa01-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="baa01-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet ADP Globalview.</span><span class="sxs-lookup"><span data-stu-id="baa01-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ADP Globalview application.</span></span>

<span data-ttu-id="baa01-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med ADP Globalview:**</span><span class="sxs-lookup"><span data-stu-id="baa01-150">**tooconfigure Azure AD single sign-on with ADP Globalview, perform hello following steps:**</span></span>

1. <span data-ttu-id="baa01-151">I hello Azure-portalen på hello **ADP Globalview** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="baa01-151">In hello Azure portal, on hello **ADP Globalview** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="baa01-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="baa01-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_samlbase.png)

3. <span data-ttu-id="baa01-155">På hello **ADP Globalview domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="baa01-155">On hello **ADP Globalview Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_url.png)

     <span data-ttu-id="baa01-157">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret: `https://<subdomain>.globalview.adp.com/federate` eller`https://<subdomain>.globalview.adp.com/federate2`</span><span class="sxs-lookup"><span data-stu-id="baa01-157">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.globalview.adp.com/federate` or `https://<subdomain>.globalview.adp.com/federate2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="baa01-158">hello-värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="baa01-158">hello value is not real.</span></span> <span data-ttu-id="baa01-159">Uppdatera hello värdet med hello faktiska identifierare.</span><span class="sxs-lookup"><span data-stu-id="baa01-159">Update hello value with hello actual Identifier.</span></span> <span data-ttu-id="baa01-160">Kontakta [ADP Globalview stöd](https://www.adp.com/contact-us/overview.aspx) tooget hello värde.</span><span class="sxs-lookup"><span data-stu-id="baa01-160">Contact [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx) tooget hello value.</span></span>
 
4. <span data-ttu-id="baa01-161">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="baa01-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_certificate.png) 

5. <span data-ttu-id="baa01-163">hello ADP GlobalView program förväntar hello SAML intyg i ett specifikt format, vilket kräver att du tooadd attributet mappningar tooyour SAML-token attribut-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="baa01-163">hello ADP GlobalView application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> 

6. <span data-ttu-id="baa01-164">hello följande skärmbild visar ett exempel för den.</span><span class="sxs-lookup"><span data-stu-id="baa01-164">hello following screenshot shows an example for it.</span></span> <span data-ttu-id="baa01-165">hello anspråk namn alltid vara **”PersonImmutableID”** och hello-värde som vi har mappat tooExtensionAttribute2 som innehåller hello EmployeeID för hello användare.</span><span class="sxs-lookup"><span data-stu-id="baa01-165">hello claim names always be **"PersonImmutableID"** and hello value of which we have mapped tooExtensionAttribute2, which contains hello EmployeeID of hello user.</span></span> <span data-ttu-id="baa01-166">Här hello mappning från Azure AD tooADP GlobalView görs på hello EmployeeID, men du kan mappa den tooa annat värde också baserat på dina inställningar för programmet.</span><span class="sxs-lookup"><span data-stu-id="baa01-166">Here hello user mapping from Azure AD tooADP GlobalView is done on hello EmployeeID but you can map it tooa different value also based on your application settings.</span></span> <span data-ttu-id="baa01-167">Du kan arbeta med hello ADP GlobalView team första toouse hello rätt ID för en användare och mappa värdet med hello **”PersonImmutableID”** anspråk.</span><span class="sxs-lookup"><span data-stu-id="baa01-167">You can work with hello ADP GlobalView team first toouse hello correct identifier of a user and map that value with hello **"PersonImmutableID"** claim.</span></span> <span data-ttu-id="baa01-168">Du kan också mappa hello e-post och UserID anspråk som hello bilden.</span><span class="sxs-lookup"><span data-stu-id="baa01-168">You can also map hello Email and UserID claim as shown in hello picture.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_attribute.png)

7. <span data-ttu-id="baa01-170">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attributet enligt hello bild och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="baa01-170">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="baa01-171">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="baa01-171">Attribute Name</span></span> | <span data-ttu-id="baa01-172">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="baa01-172">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="baa01-173">personalimmutableid</span><span class="sxs-lookup"><span data-stu-id="baa01-173">personalimmutableid</span></span> | <span data-ttu-id="baa01-174">User.extensionattribute2</span><span class="sxs-lookup"><span data-stu-id="baa01-174">user.extensionattribute2</span></span> |
    | <span data-ttu-id="baa01-175">E-post</span><span class="sxs-lookup"><span data-stu-id="baa01-175">email</span></span>               | <span data-ttu-id="baa01-176">User.Mail</span><span class="sxs-lookup"><span data-stu-id="baa01-176">user.mail</span></span> |
    | <span data-ttu-id="baa01-177">användar-ID</span><span class="sxs-lookup"><span data-stu-id="baa01-177">userid</span></span>              | <span data-ttu-id="baa01-178">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="baa01-178">user.userprincipalname</span></span>|
    
    <span data-ttu-id="baa01-179">a.</span><span class="sxs-lookup"><span data-stu-id="baa01-179">a.</span></span> <span data-ttu-id="baa01-180">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="baa01-180">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adglobalview-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adglobalview-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="baa01-183">b.</span><span class="sxs-lookup"><span data-stu-id="baa01-183">b.</span></span> <span data-ttu-id="baa01-184">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="baa01-184">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="baa01-185">c.</span><span class="sxs-lookup"><span data-stu-id="baa01-185">c.</span></span> <span data-ttu-id="baa01-186">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="baa01-186">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="baa01-187">d.</span><span class="sxs-lookup"><span data-stu-id="baa01-187">d.</span></span> <span data-ttu-id="baa01-188">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="baa01-188">Click **Ok**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="baa01-189">Innan du kan konfigurera hello SAML-kontroll måste toocontact din [ADP Globalview stöd](https://www.adp.com/contact-us/overview.aspx) och begära hello värdet för attributet för hello-Unik identifierare för din klient.</span><span class="sxs-lookup"><span data-stu-id="baa01-189">Before you can configure hello SAML assertion, you need toocontact your [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx) and request hello value of hello unique identifier attribute for your tenant.</span></span> <span data-ttu-id="baa01-190">Du behöver det här värdet tooconfigure hello anpassat anspråk för ditt program.</span><span class="sxs-lookup"><span data-stu-id="baa01-190">You need this value tooconfigure hello custom claim for your application.</span></span> 

8. <span data-ttu-id="baa01-191">På hello **ADP Globalview Configuration** klickar du på **konfigurera ADP Globalview** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="baa01-191">On hello **ADP Globalview Configuration** section, click **Configure ADP Globalview** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="baa01-192">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="baa01-192">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_configure.png) 

9. <span data-ttu-id="baa01-194">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="baa01-194">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adglobalview-tutorial/tutorial_general_400.png)

10. <span data-ttu-id="baa01-196">tooconfigure enkel inloggning på **ADP Globalview** sida, behöver du toosend hello hämtas **certifikat (Base64)**, **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel** för[ADP Globalview support](https://www.adp.com/contact-us/overview.aspx).</span><span class="sxs-lookup"><span data-stu-id="baa01-196">tooconfigure single sign-on on **ADP Globalview** side, you need toosend hello downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[ADP Globalview support](https://www.adp.com/contact-us/overview.aspx).</span></span>

> [!TIP]
> <span data-ttu-id="baa01-197">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="baa01-197">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="baa01-198">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="baa01-198">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="baa01-199">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="baa01-199">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="baa01-200">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="baa01-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="baa01-201">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="baa01-201">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="baa01-203">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="baa01-203">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="baa01-204">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="baa01-204">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_01.png) 

2.  <span data-ttu-id="baa01-206">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="baa01-206">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="baa01-208">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="baa01-208">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="baa01-210">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="baa01-210">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="baa01-212">a.</span><span class="sxs-lookup"><span data-stu-id="baa01-212">a.</span></span> <span data-ttu-id="baa01-213">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="baa01-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="baa01-214">b.</span><span class="sxs-lookup"><span data-stu-id="baa01-214">b.</span></span> <span data-ttu-id="baa01-215">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="baa01-215">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="baa01-216">c.</span><span class="sxs-lookup"><span data-stu-id="baa01-216">c.</span></span> <span data-ttu-id="baa01-217">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="baa01-217">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="baa01-218">d.</span><span class="sxs-lookup"><span data-stu-id="baa01-218">d.</span></span> <span data-ttu-id="baa01-219">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="baa01-219">Click **Create**.</span></span>
 
### <a name="creating-an-adp-globalview-test-user"></a><span data-ttu-id="baa01-220">Skapa en testanvändare ADP Globalview</span><span class="sxs-lookup"><span data-stu-id="baa01-220">Creating an ADP Globalview test user</span></span>

<span data-ttu-id="baa01-221">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i ADP GlobalView.</span><span class="sxs-lookup"><span data-stu-id="baa01-221">hello objective of this section is toocreate a user called Britta Simon in ADP GlobalView.</span></span> <span data-ttu-id="baa01-222">Arbeta med [ADP Globalview stöd](https://www.adp.com/contact-us/overview.aspx) tooadd hello användare i hello ADP GlobalView konto.</span><span class="sxs-lookup"><span data-stu-id="baa01-222">Work with [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx) tooadd hello users in hello ADP GlobalView account.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="baa01-223">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="baa01-223">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="baa01-224">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooADP Globalview.</span><span class="sxs-lookup"><span data-stu-id="baa01-224">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooADP Globalview.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="baa01-226">**tooassign Britta Simon tooADP Globalview, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="baa01-226">**tooassign Britta Simon tooADP Globalview, perform hello following steps:**</span></span>

1. <span data-ttu-id="baa01-227">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="baa01-227">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="baa01-229">Välj i listan med program hello **ADP Globalview**.</span><span class="sxs-lookup"><span data-stu-id="baa01-229">In hello applications list, select **ADP Globalview**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_app.png) 

3. <span data-ttu-id="baa01-231">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="baa01-231">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="baa01-233">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="baa01-233">Click **Add** button.</span></span> <span data-ttu-id="baa01-234">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="baa01-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="baa01-236">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="baa01-236">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="baa01-237">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="baa01-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="baa01-238">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="baa01-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="baa01-239">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="baa01-239">Testing single sign-on</span></span>

<span data-ttu-id="baa01-240">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="baa01-240">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="baa01-241">Du bör få automatiskt inloggade tooyour ADP GlobalView programmet när du klickar på hello ADP GlobalView panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="baa01-241">When you click hello ADP GlobalView tile in hello Access Panel, you should get automatically signed-on tooyour ADP GlobalView application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="baa01-242">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="baa01-242">Additional resources</span></span>

* [<span data-ttu-id="baa01-243">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="baa01-243">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="baa01-244">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="baa01-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_203.png

