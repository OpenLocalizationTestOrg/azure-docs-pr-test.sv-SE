---
title: "Självstudier: Azure Active Directory-integrering med ServiceChannel | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ServiceChannel."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c3546eab-96b5-489b-a309-b895eb428053
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/3/2017
ms.author: jeedes
ms.openlocfilehash: 956371a1e99dcba4137c271ecfe8a62b9ec64a99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicechannel"></a><span data-ttu-id="b4fe4-103">Självstudier: Azure Active Directory-integrering med ServiceChannel</span><span class="sxs-lookup"><span data-stu-id="b4fe4-103">Tutorial: Azure Active Directory integration with ServiceChannel</span></span>

<span data-ttu-id="b4fe4-104">I kursen får du lära dig hur toointegrate ServiceChannel med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b4fe4-104">In this tutorial, you learn how toointegrate ServiceChannel with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b4fe4-105">Integrera ServiceChannel med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b4fe4-105">Integrating ServiceChannel with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b4fe4-106">Du kan styra i Azure AD som har åtkomst till tooServiceChannel</span><span class="sxs-lookup"><span data-stu-id="b4fe4-106">You can control in Azure AD who has access tooServiceChannel</span></span>
- <span data-ttu-id="b4fe4-107">Du kan aktivera din användare tooautomatically get inloggade tooServiceChannel (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b4fe4-107">You can enable your users tooautomatically get signed-on tooServiceChannel (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b4fe4-108">Du kan hantera dina konton i en central plats - hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="b4fe4-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="b4fe4-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b4fe4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b4fe4-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b4fe4-110">Prerequisites</span></span>

<span data-ttu-id="b4fe4-111">tooconfigure Azure AD-integrering med ServiceChannel, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="b4fe4-111">tooconfigure Azure AD integration with ServiceChannel, you need hello following items:</span></span>

- <span data-ttu-id="b4fe4-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b4fe4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b4fe4-113">En ServiceChannel enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="b4fe4-113">A ServiceChannel single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b4fe4-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b4fe4-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b4fe4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b4fe4-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="b4fe4-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b4fe4-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b4fe4-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b4fe4-118">Scenario description</span></span>
<span data-ttu-id="b4fe4-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b4fe4-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b4fe4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b4fe4-121">Att lägga till ServiceChannel från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b4fe4-121">Adding ServiceChannel from hello gallery</span></span>
2. <span data-ttu-id="b4fe4-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b4fe4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-servicechannel-from-hello-gallery"></a><span data-ttu-id="b4fe4-123">Att lägga till ServiceChannel från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b4fe4-123">Adding ServiceChannel from hello gallery</span></span>
<span data-ttu-id="b4fe4-124">tooconfigure hello integrering av ServiceChannel i Azure AD, behöver du tooadd ServiceChannel hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-124">tooconfigure hello integration of ServiceChannel into Azure AD, you need tooadd ServiceChannel from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b4fe4-125">**tooadd ServiceChannel från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b4fe4-125">**tooadd ServiceChannel from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b4fe4-126">I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b4fe4-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b4fe4-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="b4fe4-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="b4fe4-133">Skriv i sökrutan hello **ServiceChannel**.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-133">In hello search box, type **ServiceChannel**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_000.png)

5. <span data-ttu-id="b4fe4-135">Markera hello resultat på panelen **ServiceChannel**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-135">In hello results panel, select **ServiceChannel**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_2.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b4fe4-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b4fe4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b4fe4-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ServiceChannel baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-138">In this section, you configure and test Azure AD single sign-on with ServiceChannel based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b4fe4-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ServiceChannel är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ServiceChannel is tooa user in Azure AD.</span></span> <span data-ttu-id="b4fe4-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i ServiceChannel toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-140">In other words, a link relationship between an Azure AD user and hello related user in ServiceChannel needs toobe established.</span></span>

<span data-ttu-id="b4fe4-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i ServiceChannel.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ServiceChannel.</span></span>

<span data-ttu-id="b4fe4-142">tooconfigure och testa Azure AD enkel inloggning med ServiceChannel, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b4fe4-142">tooconfigure and test Azure AD single sign-on with ServiceChannel, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b4fe4-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b4fe4-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b4fe4-145">**[Skapa en testanvändare ServiceChannel](#creating-a-servicechannel-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-145">**[Creating a ServiceChannel test user](#creating-a-servicechannel-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="b4fe4-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b4fe4-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b4fe4-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b4fe4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b4fe4-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt ServiceChannel-program.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your ServiceChannel application.</span></span>

<span data-ttu-id="b4fe4-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med ServiceChannel:**</span><span class="sxs-lookup"><span data-stu-id="b4fe4-150">**tooconfigure Azure AD single sign-on with ServiceChannel, perform hello following steps:**</span></span>

1. <span data-ttu-id="b4fe4-151">I hello Azure Management portal på hello **ServiceChannel** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-151">In hello Azure Management portal, on hello **ServiceChannel** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="b4fe4-153">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_01.png)

3. <span data-ttu-id="b4fe4-155">På hello **ServiceChannel domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b4fe4-155">On hello **ServiceChannel Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_urls.png)

    <span data-ttu-id="b4fe4-157">a.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-157">a.</span></span> <span data-ttu-id="b4fe4-158">I hello **identifierare** textruta hello TYPVÄRDE som:`http://adfs.<domain>.com/adfs/service/trust`</span><span class="sxs-lookup"><span data-stu-id="b4fe4-158">In hello **Identifier** textbox, type hello value as: `http://adfs.<domain>.com/adfs/service/trust`</span></span>

    <span data-ttu-id="b4fe4-159">b.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-159">b.</span></span> <span data-ttu-id="b4fe4-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<customer domain>.servicechannel.com/saml/acs`</span><span class="sxs-lookup"><span data-stu-id="b4fe4-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<customer domain>.servicechannel.com/saml/acs`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b4fe4-161">Observera att detta inte är hello verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="b4fe4-162">Du har tooupdate dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-162">You have tooupdate these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="b4fe4-163">Vi rekommenderar här du toouse hello unikt värde i strängen i hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="b4fe4-164">Kontakta [ServiceChannel supportteamet](https://servicechannel.zendesk.com/hc/en-us) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-164">Contact [ServiceChannel support team](https://servicechannel.zendesk.com/hc/en-us) tooget these values.</span></span>

4. <span data-ttu-id="b4fe4-165">Tillämpningsprogrammet ServiceChannel förväntar hello SAML intyg i ett specifikt format, vilket kräver att du tooadd attributet mappningar tooyour SAML-token attribut-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-165">Your ServiceChannel application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="b4fe4-166">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-166">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="b4fe4-167">**NameIdentifier (användar-ID)** är hello endast obligatoriska anspråk och hello standardvärdet är **user.userprincipalname** men ServiceChannel förväntar sig den här toobe som mappats till **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-167">**NameIdentifier(User Identifier)** is hello only mandatory claim and hello default value is **user.userprincipalname** but ServiceChannel expects this toobe mapped with **user.mail**.</span></span> <span data-ttu-id="b4fe4-168">Om du planerar tooenable bara In Time användaretablering, bör du lägga till hello följande anspråk som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-168">If you are planning tooenable Just In Time user provisioning, then you should add hello following claims as shown below.</span></span> <span data-ttu-id="b4fe4-169">**Rollen** anspråk måste mappas för toobe**user.assignedroles** som innehåller hello roll hello användare.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-169">**Role** claim needs toobe mapped too**user.assignedroles** which contains hello role of hello user.</span></span>  

    <span data-ttu-id="b4fe4-170">Du kan se ServiceChannel guide [här](https://servicechannel.zendesk.com/hc/en-us/articles/217514326-Azure-AD-Configuration-Example) för mer information om anspråk.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-170">You can refer ServiceChannel guide [here](https://servicechannel.zendesk.com/hc/en-us/articles/217514326-Azure-AD-Configuration-Example) for more guidance on claims.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_attribute.png)

    > [!NOTE] 
    > <span data-ttu-id="b4fe4-172">Klicka på [här](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/) tooknow hur tooconfigure **rollen** i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b4fe4-172">Please click [here](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/) tooknow how tooconfigure **Role** in Azure AD</span></span>

5. <span data-ttu-id="b4fe4-173">I **användarattribut** klickar du på **visa och redigera andra användarattribut** och ange hello-attribut.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-173">In **User Attributes** section, click **View and edit all other user attributes** and set hello attributes.</span></span>

    | <span data-ttu-id="b4fe4-174">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="b4fe4-174">Attribute Name</span></span> | <span data-ttu-id="b4fe4-175">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="b4fe4-175">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="b4fe4-176">Roll</span><span class="sxs-lookup"><span data-stu-id="b4fe4-176">Role</span></span>| <span data-ttu-id="b4fe4-177">User.assignedroles</span><span class="sxs-lookup"><span data-stu-id="b4fe4-177">user.assignedroles</span></span> |

    <span data-ttu-id="b4fe4-178">a.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-178">a.</span></span> <span data-ttu-id="b4fe4-179">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-179">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_05.png)
    
    <span data-ttu-id="b4fe4-182">b.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-182">b.</span></span> <span data-ttu-id="b4fe4-183">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-183">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="b4fe4-184">c.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-184">c.</span></span> <span data-ttu-id="b4fe4-185">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-185">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="b4fe4-186">d.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-186">d.</span></span> <span data-ttu-id="b4fe4-187">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="b4fe4-187">Click **Ok**</span></span>
    
6. <span data-ttu-id="b4fe4-188">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-188">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_05.png) 

7. <span data-ttu-id="b4fe4-190">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-190">Click **Save**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="b4fe4-192">På hello **ServiceChannel Configuration** klickar du på **konfigurera ServiceChannel** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-192">On hello **ServiceChannel Configuration** section, click **Configure ServiceChannel** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b4fe4-193">Observera hello **SAML Enitity ID** från hello **Snabbreferens** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-193">Please note hello **SAML Enitity ID** from hello **Quick Reference** section.</span></span>

9. <span data-ttu-id="b4fe4-194">tooconfigure enkel inloggning på **ServiceChannel** sida, behöver du toosend hello hämtas **certifikat (Base64)** och **SAML enhets-ID** för[ ServiceChannel supportteamet](https://servicechannel.zendesk.com/hc/en-us).</span><span class="sxs-lookup"><span data-stu-id="b4fe4-194">tooconfigure single sign-on on **ServiceChannel** side, you need toosend hello downloaded **certificate (Base64)** and **SAML Entity ID** too[ServiceChannel support team](https://servicechannel.zendesk.com/hc/en-us).</span></span> <span data-ttu-id="b4fe4-195">De ska ställa in detta i ordning toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-195">They will set this up in order toohave hello SAML SSO connection set properly on both sides.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b4fe4-196">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b4fe4-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="b4fe4-197">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-197">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="b4fe4-199">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b4fe4-199">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b4fe4-200">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-200">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b4fe4-202">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-202">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b4fe4-204">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-204">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b4fe4-206">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b4fe4-206">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b4fe4-208">a.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-208">a.</span></span> <span data-ttu-id="b4fe4-209">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-209">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b4fe4-210">b.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-210">b.</span></span> <span data-ttu-id="b4fe4-211">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-211">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b4fe4-212">c.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-212">c.</span></span> <span data-ttu-id="b4fe4-213">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-213">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b4fe4-214">d.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-214">d.</span></span> <span data-ttu-id="b4fe4-215">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-215">Click **Create**.</span></span> 

### <a name="creating-a-servicechannel-test-user"></a><span data-ttu-id="b4fe4-216">Skapa en testanvändare ServiceChannel</span><span class="sxs-lookup"><span data-stu-id="b4fe4-216">Creating a ServiceChannel test user</span></span>

<span data-ttu-id="b4fe4-217">Programmet stöder bara i tid användaretablering och authentication-användare kommer att skapas i hello program automatiskt.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-217">Application supports Just in time user provisioning and after authentication users will be created in hello application automatically.</span></span> <span data-ttu-id="b4fe4-218">För fullständig användaretablering, kontakta [ServiceChannel-teamet](https://servicechannel.zendesk.com/hc/en-us)</span><span class="sxs-lookup"><span data-stu-id="b4fe4-218">For full user provisioning, please contact [ServiceChannel support team](https://servicechannel.zendesk.com/hc/en-us)</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b4fe4-219">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="b4fe4-219">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b4fe4-220">I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooServiceChannel sin åtkomst.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-220">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooServiceChannel.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="b4fe4-222">**tooassign Britta Simon tooServiceChannel utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b4fe4-222">**tooassign Britta Simon tooServiceChannel, perform hello following steps:**</span></span>

1. <span data-ttu-id="b4fe4-223">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-223">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b4fe4-225">Välj i listan med program hello **ServiceChannel**.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-225">In hello applications list, select **ServiceChannel**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_app01.png) 

3. <span data-ttu-id="b4fe4-227">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-227">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="b4fe4-229">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-229">Click **Add** button.</span></span> <span data-ttu-id="b4fe4-230">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="b4fe4-232">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-232">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b4fe4-233">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b4fe4-234">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b4fe4-235">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b4fe4-235">Testing single sign-on</span></span>

<span data-ttu-id="b4fe4-236">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-236">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b4fe4-237">Du bör få automatiskt inloggade tooyour ServiceChannel programmet när du klickar på hello ServiceChannel-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b4fe4-237">When you click hello ServiceChannel tile in hello Access Panel, you should get automatically signed-on tooyour ServiceChannel application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b4fe4-238">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b4fe4-238">Additional resources</span></span>

* [<span data-ttu-id="b4fe4-239">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b4fe4-239">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b4fe4-240">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b4fe4-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_203.png