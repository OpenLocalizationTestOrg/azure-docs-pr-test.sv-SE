---
title: "Självstudier: Azure Active Directory-integrering med SAP Business objektet molntjänster | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SAP Business objektet moln."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6c5e44f0-4e52-463f-b879-834d80a55cdf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: a3e9bd93897271531f91bcbc50cd361e8a20551e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a><span data-ttu-id="2166c-103">Självstudier: Azure Active Directory-integrering med SAP Business objektet moln</span><span class="sxs-lookup"><span data-stu-id="2166c-103">Tutorial: Azure Active Directory integration with SAP Business Object Cloud</span></span>

<span data-ttu-id="2166c-104">I kursen får du lära dig hur toointegrate SAP Business objektet moln med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="2166c-104">In this tutorial, you learn how toointegrate SAP Business Object Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2166c-105">Du får hello följande fördelar när du integrerar SAP Business objektet moln med Azure AD:</span><span class="sxs-lookup"><span data-stu-id="2166c-105">You get hello following benefits when you integrate SAP Business Object Cloud with Azure AD:</span></span>

- <span data-ttu-id="2166c-106">Du kan styra vem som har åtkomst tooSAP Business objektet moln i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2166c-106">In Azure AD, you can control who has access tooSAP Business Object Cloud.</span></span>
- <span data-ttu-id="2166c-107">Du kan logga in automatiskt dina användare tooSAP Business objektet moln med hjälp av enkel inloggning och användarens Azure AD-kontot.</span><span class="sxs-lookup"><span data-stu-id="2166c-107">You can automatically sign in your users tooSAP Business Object Cloud by using single sign-on and a user's Azure AD account.</span></span>
- <span data-ttu-id="2166c-108">Du kan hantera dina konton i en enda central plats, hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2166c-108">You can manage your accounts in one, central location, hello Azure portal.</span></span>

<span data-ttu-id="2166c-109">toolearn mer information om programvara som en tjänst (SaaS) app-integrering med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2166c-109">toolearn more about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2166c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="2166c-110">Prerequisites</span></span>

<span data-ttu-id="2166c-111">tooset av Azure AD-integrering med SAP Business objektet molnet måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="2166c-111">tooset up Azure AD integration with SAP Business Object Cloud, you need hello following items:</span></span>

- <span data-ttu-id="2166c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="2166c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2166c-113">En SAP Business objektet molnet, med enkel inloggning aktiverad</span><span class="sxs-lookup"><span data-stu-id="2166c-113">An SAP Business Object Cloud, with single sign-on turned on</span></span>

> [!NOTE]
> <span data-ttu-id="2166c-114">Om du testar hello stegen i den här kursen rekommenderar vi att du inte testa dem i en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="2166c-114">If you test hello steps in this tutorial, we recommend that you don't test them in a production environment.</span></span>

<span data-ttu-id="2166c-115">Rekommendationer för att testa hello stegen i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="2166c-115">Recommendations for testing hello steps in this tutorial:</span></span>

- <span data-ttu-id="2166c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="2166c-116">Do not use your production environment, unless it's necessary.</span></span>
- <span data-ttu-id="2166c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [skaffa en kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2166c-117">If you don't have an Azure AD trial environment, you can [get a one-month free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2166c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="2166c-118">Scenario description</span></span>
<span data-ttu-id="2166c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="2166c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> 

<span data-ttu-id="2166c-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="2166c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2166c-121">Lägg till SAP Business objektet moln från hello-galleriet.</span><span class="sxs-lookup"><span data-stu-id="2166c-121">Add SAP Business Object Cloud from hello gallery.</span></span>
2. <span data-ttu-id="2166c-122">Konfigurera och testa Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2166c-122">Set up and test Azure AD single sign-on.</span></span>

## <a name="add-sap-business-object-cloud-from-hello-gallery"></a><span data-ttu-id="2166c-123">Lägg till SAP Business objektet moln från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="2166c-123">Add SAP Business Object Cloud from hello gallery</span></span>
<span data-ttu-id="2166c-124">tooset in hello-integration för SAP Business objektet moln med Azure AD hello Gallery och lägga till SAP Business objektet molnet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="2166c-124">tooset up hello integration of SAP Business Object Cloud with Azure AD, in hello gallery, add SAP Business Object Cloud tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2166c-125">tooadd SAP Business objektet moln från galleriet hello:</span><span class="sxs-lookup"><span data-stu-id="2166c-125">tooadd SAP Business Object Cloud from hello gallery:</span></span>

1. <span data-ttu-id="2166c-126">I hello [Azure-portalen](https://portal.azure.com), i hello vänstra menyn, Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2166c-126">In hello [Azure portal](https://portal.azure.com), in hello left menu, select **Azure Active Directory**.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="2166c-128">Välj **företagsprogram**, och välj sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2166c-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![hello Enterprise program sida][2]
    
3. <span data-ttu-id="2166c-130">tooadd nya program, Välj **nytt program**.</span><span class="sxs-lookup"><span data-stu-id="2166c-130">tooadd a new application, select **New application**.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="2166c-132">Skriv i sökrutan hello **SAP Business objektet molnet**.</span><span class="sxs-lookup"><span data-stu-id="2166c-132">In hello search box, enter **SAP Business Object Cloud**.</span></span>

    ![hello sökrutan](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_search.png)

5. <span data-ttu-id="2166c-134">Markera hello resultat på panelen **SAP Business objektet molnet**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="2166c-134">In hello results panel, select **SAP Business Object Cloud**, and then select **Add**.</span></span>

    ![SAP Business objektet molnet i hello resultatlistan](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_addfromgallery.png)

##  <a name="set-up-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="2166c-136">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2166c-136">Set up and test Azure AD single sign-on</span></span>

<span data-ttu-id="2166c-137">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning till SAP Business objektet moln baserat på en användare med namnet *Britta Simon*.</span><span class="sxs-lookup"><span data-stu-id="2166c-137">In this section, you set up and test Azure AD single sign-on with SAP Business Object Cloud based on a test user named *Britta Simon*.</span></span>

<span data-ttu-id="2166c-138">För enkel inloggning toowork måste Azure AD tooknow hello Azure AD motsvarighet användaren i molnet för SAP Business-objektet.</span><span class="sxs-lookup"><span data-stu-id="2166c-138">For single sign-on toowork, Azure AD needs tooknow hello Azure AD counterpart user in SAP Business Object Cloud.</span></span> <span data-ttu-id="2166c-139">En länk relationen mellan en Azure AD-användare och hello relaterade användaren i SAP Business objektet molnet måste upprättas.</span><span class="sxs-lookup"><span data-stu-id="2166c-139">A link relationship between an Azure AD user and hello related user in SAP Business Object Cloud must be established.</span></span>

<span data-ttu-id="2166c-140">tooestablish hello länka relationen i SAP Business objektet molnet för **användarnamn**, tilldela hello värdet för hello **användarnamn** i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2166c-140">tooestablish hello link relationship, in SAP Business Object Cloud, for **Username**, assign hello value of hello **user name** in Azure AD.</span></span>

<span data-ttu-id="2166c-141">tooconfigure och testa Azure AD enkel inloggning med SAP Business objektet moln, fullständig hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="2166c-141">tooconfigure and test Azure AD single sign-on with SAP Business Object Cloud, complete hello following tasks:</span></span>

1. <span data-ttu-id="2166c-142">[Konfigurera Azure AD enkel inloggning](#set-up-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="2166c-142">[Set up Azure AD single sign-on](#set-up-azure-ad-single-sign-on).</span></span> <span data-ttu-id="2166c-143">Ställer in en användare toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="2166c-143">Sets up a user toouse this feature.</span></span>
2. <span data-ttu-id="2166c-144">[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="2166c-144">[Create an Azure AD test user](#create-an-azure-ad-test-user).</span></span> <span data-ttu-id="2166c-145">Testerna Azure AD enkel inloggning med hello användaren Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2166c-145">Tests Azure AD single sign-on with hello user Britta Simon.</span></span>
3. <span data-ttu-id="2166c-146">[Skapa en SAP Business objektet molnet testanvändare](#create-an-sap-business-object-cloud-test-user).</span><span class="sxs-lookup"><span data-stu-id="2166c-146">[Create an SAP Business Object Cloud test user](#create-an-sap-business-object-cloud-test-user).</span></span> <span data-ttu-id="2166c-147">Skapar en motsvarighet för Britta Simon i SAP Business objektet moln som är länkade toohello Azure AD-representation av hello användare.</span><span class="sxs-lookup"><span data-stu-id="2166c-147">Creates a counterpart of Britta Simon in SAP Business Object Cloud that is linked toohello Azure AD representation of hello user.</span></span>
4. <span data-ttu-id="2166c-148">[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="2166c-148">[Assign hello Azure AD test user](#assign-the-azure-ad-test-user).</span></span> <span data-ttu-id="2166c-149">Ställer in Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2166c-149">Sets up Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2166c-150">[Testa enkel inloggning](#test-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="2166c-150">[Test single sign-on](#test-single-sign-on).</span></span> <span data-ttu-id="2166c-151">Verifierar att hello-konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="2166c-151">Verifies that hello configuration works.</span></span>

### <a name="set-up-azure-ad-single-sign-on"></a><span data-ttu-id="2166c-152">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2166c-152">Set up Azure AD single sign-on</span></span>

<span data-ttu-id="2166c-153">I det här avsnittet Aktivera du Azure AD enkel inloggning i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2166c-153">In this section, you turn on Azure AD single sign-on in hello Azure portal.</span></span> <span data-ttu-id="2166c-154">Sedan kan ställa du in enkel inloggning i ditt program för SAP Business objektet molnet.</span><span class="sxs-lookup"><span data-stu-id="2166c-154">Then, you set up single sign-on in your SAP Business Object Cloud application.</span></span>

<span data-ttu-id="2166c-155">tooset in Azure AD enkel inloggning med SAP Business objektet molnet:</span><span class="sxs-lookup"><span data-stu-id="2166c-155">tooset up Azure AD single sign-on with SAP Business Object Cloud:</span></span>

1. <span data-ttu-id="2166c-156">I hello Azure-portalen på hello **SAP Business objektet molnet** programmet integration anger **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2166c-156">In hello Azure portal, on hello **SAP Business Object Cloud** application integration page, select **Single sign-on**.</span></span>

    ![Välja enkel inloggning][4]

2. <span data-ttu-id="2166c-158">På hello **enkel inloggning** sidan för **läge**väljer **SAML-baserade inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2166c-158">On hello **Single sign-on** page, for **Mode**, select **SAML-based Sign-on**.</span></span>
 
    ![Välj SAML-baserade inloggning](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_samlbase.png)

3. <span data-ttu-id="2166c-160">Under **SAP Business objektet molnet domän och URL: er**fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2166c-160">Under **SAP Business Object Cloud Domain and URLs**, complete hello following steps:</span></span>

    1. <span data-ttu-id="2166c-161">I hello **inloggnings-URL** ange en URL som har hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="2166c-161">In hello **Sign-on URL** box, enter a URL that has hello following pattern:</span></span> 
    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    2. <span data-ttu-id="2166c-162">I hello **identifierare** ange en URL som har hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="2166c-162">In hello **Identifier** box, enter a URL that has hello following pattern:</span></span>
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    ![URL: er och SAP Business objektet molnet domän webbadresser](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_url.png)
 
    > [!NOTE] 
    > <span data-ttu-id="2166c-164">hello värden i dessa URL: er är bara exempel.</span><span class="sxs-lookup"><span data-stu-id="2166c-164">hello values in these URLs are for demonstration only.</span></span> <span data-ttu-id="2166c-165">Uppdatera hello värden med hello faktiska inloggning URL och identifierare URL.</span><span class="sxs-lookup"><span data-stu-id="2166c-165">Update hello values with hello actual sign-on URL and identifier URL.</span></span> <span data-ttu-id="2166c-166">tooget hello inloggnings-URL, kontakta hello [SAP Business objektet Cloud klienten supportteamet](https://www.sap.com/product/analytics/cloud-analytics.support.html).</span><span class="sxs-lookup"><span data-stu-id="2166c-166">tooget hello sign-on URL, contact hello [SAP Business Object Cloud Client support team](https://www.sap.com/product/analytics/cloud-analytics.support.html).</span></span> <span data-ttu-id="2166c-167">Du kan hämta hello identifierare URL genom att hämta hello SAP Business objektet molnmetadata från hello-administrationskonsolen.</span><span class="sxs-lookup"><span data-stu-id="2166c-167">You can get hello identifier URL by downloading hello SAP Business Object Cloud metadata from hello admin console.</span></span> <span data-ttu-id="2166c-168">Detta beskrivs senare i självstudiekursen hello.</span><span class="sxs-lookup"><span data-stu-id="2166c-168">This is explained later in hello tutorial.</span></span> 

4. <span data-ttu-id="2166c-169">Under **SAML-signeringscertifikat**väljer **XML-Metadata för**.</span><span class="sxs-lookup"><span data-stu-id="2166c-169">Under **SAML Signing Certificate**, select **Metadata XML**.</span></span> <span data-ttu-id="2166c-170">Spara hello metadatafil på datorn.</span><span class="sxs-lookup"><span data-stu-id="2166c-170">Then, save hello metadata file on your computer.</span></span>

    ![Välj XML-Metadata](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_certificate.png) 

5. <span data-ttu-id="2166c-172">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2166c-172">Select **Save**.</span></span>

    ![Klicka på Spara](./media/active-directory-saas-sapboc-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2166c-174">Logga in tooyour SAP Business objektet molnet företagets platsen som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="2166c-174">In a different web browser window, sign in tooyour SAP Business Object Cloud company site as an administrator.</span></span>

7. <span data-ttu-id="2166c-175">Välj **menyn** > **System** > **Administration**.</span><span class="sxs-lookup"><span data-stu-id="2166c-175">Select **Menu** > **System** > **Administration**.</span></span>
    
    ![Välj menyn och sedan på System och Administration](./media/active-directory-saas-sapboc-tutorial/config1.png)

8. <span data-ttu-id="2166c-177">På hello **säkerhet** fliken, väljer hello **redigera** (penna) ikon.</span><span class="sxs-lookup"><span data-stu-id="2166c-177">On hello **Security** tab, select hello **Edit** (pen) icon.</span></span>
    
    ![På fliken Säkerhet hello Välj hello redigeringsikonen](./media/active-directory-saas-sapboc-tutorial/config2.png)  

9. <span data-ttu-id="2166c-179">För **autentiseringsmetod**väljer **SAML enkel inloggning (SSO)**.</span><span class="sxs-lookup"><span data-stu-id="2166c-179">For **Authentication Method**, select **SAML Single Sign-On (SSO)**.</span></span>

    ![Välj SAML enkel inloggning för hello autentiseringsmetod](./media/active-directory-saas-sapboc-tutorial/config3.png)  

10. <span data-ttu-id="2166c-181">toodownload hello service provider metadata (steg 1), Välj **hämta**.</span><span class="sxs-lookup"><span data-stu-id="2166c-181">toodownload hello service provider metadata (Step 1), select **Download**.</span></span> <span data-ttu-id="2166c-182">Hello metadatafil hitta och kopiera hello **ID för entiteterna** värde.</span><span class="sxs-lookup"><span data-stu-id="2166c-182">In hello metadata file, find and copy hello **entityID** value.</span></span> <span data-ttu-id="2166c-183">I hello Azure portal under **SAP Business objektet molnet domän och URL: er**, klistra in hello värde i hello **identifierare** rutan.</span><span class="sxs-lookup"><span data-stu-id="2166c-183">In hello Azure portal, under **SAP Business Object Cloud Domain and URLs**, paste hello value in hello **Identifier** box.</span></span>

    ![Kopiera och klistra in hello ID för entiteterna värde](./media/active-directory-saas-sapboc-tutorial/config4.png)  

11. <span data-ttu-id="2166c-185">tooupload hello service provider metadata (steg 2) i hello-filen som du hämtade från hello Azure-portalen under **överför identitetsleverantör metadata**väljer **överför**.</span><span class="sxs-lookup"><span data-stu-id="2166c-185">tooupload hello service provider metadata (Step 2) in hello file that you downloaded from hello Azure portal, under **Upload Identity Provider metadata**, select **Upload**.</span></span>  

    ![Välj överför under överför identitetsleverantör metadata](./media/active-directory-saas-sapboc-tutorial/config5.png)

12. <span data-ttu-id="2166c-187">I hello **användarattribut** listan, Välj hello användarattribut (steg3) som du vill toouse för din implementering.</span><span class="sxs-lookup"><span data-stu-id="2166c-187">In hello **User Attribute** list, select hello user attribute (Step 3) that you want toouse for your implementation.</span></span> <span data-ttu-id="2166c-188">Den här användarattribut mappar toohello identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="2166c-188">This user attribute maps toohello identity provider.</span></span> <span data-ttu-id="2166c-189">tooenter ett anpassat attribut hello användare på sidan Använd hello **mappning av anpassade SAML** alternativet.</span><span class="sxs-lookup"><span data-stu-id="2166c-189">tooenter a custom attribute on hello user's page, use hello **Custom SAML Mapping** option.</span></span> <span data-ttu-id="2166c-190">Du kan också välja antingen **e-post** eller **ANVÄNDAR-ID** som hello användarattribut.</span><span class="sxs-lookup"><span data-stu-id="2166c-190">Or, you can select either **Email** or **USER ID** as hello user attribute.</span></span> <span data-ttu-id="2166c-191">Vi valde i vårt exempel **e-post** som vi har mappats hello identifierare användaranspråk med hello **userprincipalname** attribut i hello **användarattribut** avsnitt i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2166c-191">In our example, we selected **Email** because we mapped hello user identifier claim with hello **userprincipalname** attribute in hello **User Attributes** section in hello Azure portal.</span></span> <span data-ttu-id="2166c-192">Detta ger ett unikt e-post, som skickas toohello molnapp SAP Business objekt i varje lyckat SAML-svar.</span><span class="sxs-lookup"><span data-stu-id="2166c-192">This provides a unique user email, which is sent toohello SAP Business Object Cloud application in every successful SAML response.</span></span>

    ![Välj användarattribut](./media/active-directory-saas-sapboc-tutorial/config6.png)

13. <span data-ttu-id="2166c-194">tooverify hello konto med hello identitetsprovider (steg 4) i hello **inloggningen autentiseringsuppgifter (e-post)** ange hello användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="2166c-194">tooverify hello account with hello identity provider (Step 4), in hello **Login Credential (Email)** box, enter hello user's email address.</span></span> <span data-ttu-id="2166c-195">Markera **verifiera kontot**.</span><span class="sxs-lookup"><span data-stu-id="2166c-195">Then, select **Verify Account**.</span></span> <span data-ttu-id="2166c-196">hello läggs inloggningsuppgifter toohello användarkonto.</span><span class="sxs-lookup"><span data-stu-id="2166c-196">hello system adds sign-in credentials toohello user account.</span></span>

    ![Ange e-postadress och välj verifiera konto](./media/active-directory-saas-sapboc-tutorial/config7.png)

14. <span data-ttu-id="2166c-198">Välj hello **spara** ikon.</span><span class="sxs-lookup"><span data-stu-id="2166c-198">Select hello **Save** icon.</span></span>

    ![Spara ikon](./media/active-directory-saas-sapboc-tutorial/save.png)

> [!TIP]
> <span data-ttu-id="2166c-200">Du kan läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="2166c-200">You can read a concise version of these instructions in hello [Azure portal](https://portal.azure.com), while you are setting up your app!</span></span> <span data-ttu-id="2166c-201">När du har lagt till hello appen genom att välja **Active Directory** > **företagsprogram**väljer hello **enkel inloggning** fliken. Du kan komma åt inbäddade hello-dokumentationen i hello **Configuration** avsnitt på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="2166c-201">After you add hello app by selecting **Active Directory** > **Enterprise Applications**, select hello **Single Sign-On** tab. You can access hello embedded documentation in hello **Configuration** section, at hello bottom of hello page.</span></span> <span data-ttu-id="2166c-202">Mer information finns i [Azure AD inbäddade dokumentationen]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="2166c-202">For more information, see [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="2166c-203">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="2166c-203">Create an Azure AD test user</span></span>
<span data-ttu-id="2166c-204">I det här avsnittet skapar du en användare med namnet Britta Simon i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2166c-204">In this section, you create a test user named Britta Simon in hello Azure portal.</span></span>

<span data-ttu-id="2166c-205">toocreate en testanvändare i Azure AD:</span><span class="sxs-lookup"><span data-stu-id="2166c-205">toocreate a test user in Azure AD:</span></span>

1. <span data-ttu-id="2166c-206">Välj i hello Azure-portalen hello vänstra menyn **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2166c-206">In hello Azure portal, in hello left menu, select **Azure Active Directory**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sapboc-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2166c-208">toodisplay hello lista över användare, Välj **användare och grupper**, och välj sedan **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="2166c-208">toodisplay hello list of users, select **Users and groups**, and then select **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sapboc-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2166c-210">tooopen hello **användare** dialogrutan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="2166c-210">tooopen hello **User** dialog box, select **Add**.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sapboc-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2166c-212">I hello **användaren** dialogrutan, fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2166c-212">In hello **User** dialog box, complete hello following steps:</span></span>
 
    1. <span data-ttu-id="2166c-213">I hello **namn** ange **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2166c-213">In hello **Name** box, enter **BrittaSimon**.</span></span>

    2. <span data-ttu-id="2166c-214">I hello **användarnamn** ange hello hello användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="2166c-214">In hello **User name** box, enter hello email address of hello user Britta Simon.</span></span>

    3. <span data-ttu-id="2166c-215">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="2166c-215">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    4. <span data-ttu-id="2166c-216">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2166c-216">Select **Create**.</span></span>

        ![hello användardialogrutan](./media/active-directory-saas-sapboc-tutorial/create_aaduser_04.png) 

    ![Skapa Azure AD-användare][100]

### <a name="create-an-sap-business-object-cloud-test-user"></a><span data-ttu-id="2166c-219">Skapa en SAP Business objektet molnet testanvändare</span><span class="sxs-lookup"><span data-stu-id="2166c-219">Create an SAP Business Object Cloud test user</span></span>

<span data-ttu-id="2166c-220">Azure AD-användare måste etableras i SAP Business objektet moln innan de kan logga in tooSAP Business objektet molnet.</span><span class="sxs-lookup"><span data-stu-id="2166c-220">Azure AD users must be provisioned in SAP Business Object Cloud before they can sign in tooSAP Business Object Cloud.</span></span> <span data-ttu-id="2166c-221">I SAP Business objektet moln är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="2166c-221">In SAP Business Object Cloud, provisioning is a manual task.</span></span>

<span data-ttu-id="2166c-222">tooprovision ett användarkonto:</span><span class="sxs-lookup"><span data-stu-id="2166c-222">tooprovision a user account:</span></span>

1. <span data-ttu-id="2166c-223">Logga in tooyour SAP Business objektet molnet företagets platsen som en administratör.</span><span class="sxs-lookup"><span data-stu-id="2166c-223">Sign in tooyour SAP Business Object Cloud company site as an administrator.</span></span>

2. <span data-ttu-id="2166c-224">Välj **menyn** > **säkerhet** > **användare**.</span><span class="sxs-lookup"><span data-stu-id="2166c-224">Select **Menu** > **Security** > **Users**.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-sapboc-tutorial/user1.png)

3. <span data-ttu-id="2166c-226">På hello **användare** tooadd ny användarinformation på sidan Välj  **+** .</span><span class="sxs-lookup"><span data-stu-id="2166c-226">On hello **Users** page, tooadd new user details, select **+**.</span></span> 

    ![Lägg till användare på sidan](./media/active-directory-saas-sapboc-tutorial/user4.png)

    <span data-ttu-id="2166c-228">Slutför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2166c-228">Then, complete hello following steps:</span></span>

    1. <span data-ttu-id="2166c-229">I hello **ANVÄNDAR-ID** ange hello användar-ID för hello användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="2166c-229">In hello **USER ID** box, enter hello user ID of hello user, like **Britta**.</span></span>

    2. <span data-ttu-id="2166c-230">I hello **Förnamn** ange hello förnamn hello användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="2166c-230">In hello **FIRST NAME** box, enter hello first name of hello user, like **Britta**.</span></span>

    3. <span data-ttu-id="2166c-231">I hello **efternamn** ange hello efternamn hello användaren som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="2166c-231">In hello **LAST NAME** box, enter hello last name of hello user, like **Simon**.</span></span>

    4. <span data-ttu-id="2166c-232">I hello **VISNINGSNAMN** ange hello hello användarens fullständiga namn som **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="2166c-232">In hello **DISPLAY NAME** box, enter hello full name of hello user, like **Britta Simon**.</span></span>

    5. <span data-ttu-id="2166c-233">I hello **e-post** ange hello hello användarens e-postadress som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="2166c-233">In hello **E-MAIL** box, enter hello email address of hello user, like **brittasimon@contoso.com**.</span></span>

    6. <span data-ttu-id="2166c-234">På hello **Välj roller** väljer hello rätt roll för hello användare, och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="2166c-234">On hello **Select Roles** page, select hello appropriate role for hello user, and then select **OK**.</span></span>

      ![Välja en roll](./media/active-directory-saas-sapboc-tutorial/user3.png)

    7. <span data-ttu-id="2166c-236">Välj hello **spara** ikon.</span><span class="sxs-lookup"><span data-stu-id="2166c-236">Select hello **Save** icon.</span></span>  


### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="2166c-237">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="2166c-237">Assign hello Azure AD test user</span></span>

<span data-ttu-id="2166c-238">I det här avsnittet Tillåt hello användaren Britta Simon toouse Azure AD enkel inloggning genom att tilldela hello användarens konto åtkomst tooSAP Business objektet molnet.</span><span class="sxs-lookup"><span data-stu-id="2166c-238">In this section, you allow hello user Britta Simon toouse Azure AD single sign-on by granting hello user account access tooSAP Business Object Cloud.</span></span>

<span data-ttu-id="2166c-239">tooassign Britta Simon tooSAP Business objektet molnet:</span><span class="sxs-lookup"><span data-stu-id="2166c-239">tooassign Britta Simon tooSAP Business Object Cloud:</span></span>

1. <span data-ttu-id="2166c-240">Öppna hello program i hello Azure-portalen, och sedan gå toohello directory vyn.</span><span class="sxs-lookup"><span data-stu-id="2166c-240">In hello Azure portal, open hello applications view, and then go toohello directory view.</span></span> <span data-ttu-id="2166c-241">Välj **företagsprogram**, och välj sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2166c-241">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="2166c-243">Välj i listan med program hello **SAP Business objektet molnet**.</span><span class="sxs-lookup"><span data-stu-id="2166c-243">In hello applications list, select **SAP Business Object Cloud**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_app.png) 

3. <span data-ttu-id="2166c-245">Välj hello vänstra menyn **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="2166c-245">In hello left menu, select **Users and groups**.</span></span>

    ![Välj användare och grupper][202] 

4. <span data-ttu-id="2166c-247">Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="2166c-247">Select **Add**.</span></span> <span data-ttu-id="2166c-248">Klicka sedan på hello **Lägg uppdrag** väljer **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="2166c-248">Then, on hello **Add Assignment** page, select **Users and groups**.</span></span>

    ![hello tilldelning av lägger du till sidan][203]

5. <span data-ttu-id="2166c-250">På hello **användare och grupper** i hello lista över användare, Välj sidan **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="2166c-250">On hello **Users and groups** page, in hello list of users, select **Britta Simon**.</span></span>

6. <span data-ttu-id="2166c-251">På hello **användare och grupper** väljer **Välj**.</span><span class="sxs-lookup"><span data-stu-id="2166c-251">On hello **Users and groups** page, select **Select**.</span></span>

7. <span data-ttu-id="2166c-252">På hello **Lägg uppdrag** väljer **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="2166c-252">On hello **Add Assignment** page, select **Assign**.</span></span>

![Tilldela hello användarroll][200] 
    
### <a name="test-single-sign-on"></a><span data-ttu-id="2166c-254">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2166c-254">Test single sign-on</span></span>

<span data-ttu-id="2166c-255">I det här avsnittet testa du Azure AD enkel inloggning konfigurationen med hjälp av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="2166c-255">In this section, you test your Azure AD single sign-on configuration by using hello access panel.</span></span>

<span data-ttu-id="2166c-256">När du väljer hello SAP Business objektet molnet panelen i hello åtkomstpanelen bör vara loggas du automatiskt tooyour molnapp SAP Business-objekt.</span><span class="sxs-lookup"><span data-stu-id="2166c-256">When you select hello SAP Business Object Cloud tile in hello access panel, you should be automatically signed in tooyour SAP Business Object Cloud application.</span></span>

<span data-ttu-id="2166c-257">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2166c-257">For more information about hello access panel, see [Introduction toohello access panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2166c-258">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2166c-258">Additional resources</span></span>

* [<span data-ttu-id="2166c-259">Lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2166c-259">List of tutorials on how toointegrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2166c-260">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2166c-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_203.png

