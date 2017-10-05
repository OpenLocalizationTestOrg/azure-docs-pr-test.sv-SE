---
title: "Självstudier: Azure Active Directory-integrering med SAP Business objektet molntjänster | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och SAP Business objektet moln."
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
ms.openlocfilehash: 6d517c5e302ac36e5bba2053998c75f8f4d42683
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a><span data-ttu-id="4a280-103">Självstudier: Azure Active Directory-integrering med SAP Business objektet moln</span><span class="sxs-lookup"><span data-stu-id="4a280-103">Tutorial: Azure Active Directory integration with SAP Business Object Cloud</span></span>

<span data-ttu-id="4a280-104">Lär dig hur du integrerar SAP Business objektet moln med Azure Active Directory (AD Azure) i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="4a280-104">In this tutorial, you learn how to integrate SAP Business Object Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4a280-105">När du integrerar SAP Business objektet moln med Azure AD får du följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="4a280-105">You get the following benefits when you integrate SAP Business Object Cloud with Azure AD:</span></span>

- <span data-ttu-id="4a280-106">Du kan styra vem som har åtkomst till SAP Business objektet moln i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4a280-106">In Azure AD, you can control who has access to SAP Business Object Cloud.</span></span>
- <span data-ttu-id="4a280-107">Du kan logga in automatiskt användarna till SAP Business objektet moln med hjälp av enkel inloggning och användarens Azure AD-kontot.</span><span class="sxs-lookup"><span data-stu-id="4a280-107">You can automatically sign in your users to SAP Business Object Cloud by using single sign-on and a user's Azure AD account.</span></span>
- <span data-ttu-id="4a280-108">Du kan hantera dina konton i en enda central plats och Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4a280-108">You can manage your accounts in one, central location, the Azure portal.</span></span>

<span data-ttu-id="4a280-109">Läs mer om programvara som en tjänst (SaaS) appintegrering med Azure AD i [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4a280-109">To learn more about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4a280-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4a280-110">Prerequisites</span></span>

<span data-ttu-id="4a280-111">Om du vill konfigurera Azure AD-integrering med SAP Business objektet molnet, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="4a280-111">To set up Azure AD integration with SAP Business Object Cloud, you need the following items:</span></span>

- <span data-ttu-id="4a280-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4a280-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4a280-113">En SAP Business objektet molnet, med enkel inloggning aktiverad</span><span class="sxs-lookup"><span data-stu-id="4a280-113">An SAP Business Object Cloud, with single sign-on turned on</span></span>

> [!NOTE]
> <span data-ttu-id="4a280-114">Om du testar stegen i den här kursen rekommenderar vi att du inte testa dem i en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="4a280-114">If you test the steps in this tutorial, we recommend that you don't test them in a production environment.</span></span>

<span data-ttu-id="4a280-115">Rekommendationer för att testa stegen i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="4a280-115">Recommendations for testing the steps in this tutorial:</span></span>

- <span data-ttu-id="4a280-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="4a280-116">Do not use your production environment, unless it's necessary.</span></span>
- <span data-ttu-id="4a280-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [skaffa en kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4a280-117">If you don't have an Azure AD trial environment, you can [get a one-month free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4a280-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="4a280-118">Scenario description</span></span>
<span data-ttu-id="4a280-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="4a280-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> 

<span data-ttu-id="4a280-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="4a280-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4a280-121">Lägg till SAP Business objektet moln från galleriet.</span><span class="sxs-lookup"><span data-stu-id="4a280-121">Add SAP Business Object Cloud from the gallery.</span></span>
2. <span data-ttu-id="4a280-122">Konfigurera och testa Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4a280-122">Set up and test Azure AD single sign-on.</span></span>

## <a name="add-sap-business-object-cloud-from-the-gallery"></a><span data-ttu-id="4a280-123">Lägg till SAP Business objektet moln från galleriet</span><span class="sxs-lookup"><span data-stu-id="4a280-123">Add SAP Business Object Cloud from the gallery</span></span>
<span data-ttu-id="4a280-124">Lägg till SAP Business objektet moln i listan över hanterade SaaS-appar för att ställa in SAP Business objektet moln med Azure AD-integrering i galleriet.</span><span class="sxs-lookup"><span data-stu-id="4a280-124">To set up the integration of SAP Business Object Cloud with Azure AD, in the gallery, add SAP Business Object Cloud to your list of managed SaaS apps.</span></span>

<span data-ttu-id="4a280-125">Lägg till SAP Business objektet moln från galleriet:</span><span class="sxs-lookup"><span data-stu-id="4a280-125">To add SAP Business Object Cloud from the gallery:</span></span>

1. <span data-ttu-id="4a280-126">I den [Azure-portalen](https://portal.azure.com), i den vänstra menyn, Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="4a280-126">In the [Azure portal](https://portal.azure.com), in the left menu, select **Azure Active Directory**.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="4a280-128">Välj **företagsprogram**, och välj sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4a280-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Sidan Enterprise program][2]
    
3. <span data-ttu-id="4a280-130">Om du vill lägga till ett nytt program, Välj **nytt program**.</span><span class="sxs-lookup"><span data-stu-id="4a280-130">To add a new application, select **New application**.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="4a280-132">I sökrutan anger **SAP Business objektet molnet**.</span><span class="sxs-lookup"><span data-stu-id="4a280-132">In the search box, enter **SAP Business Object Cloud**.</span></span>

    ![Sökrutan](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_search.png)

5. <span data-ttu-id="4a280-134">Välj i resultatpanelen **SAP Business objektet molnet**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="4a280-134">In the results panel, select **SAP Business Object Cloud**, and then select **Add**.</span></span>

    ![SAP Business objektet moln i resultatlistan](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_addfromgallery.png)

##  <a name="set-up-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="4a280-136">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4a280-136">Set up and test Azure AD single sign-on</span></span>

<span data-ttu-id="4a280-137">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning till SAP Business objektet moln baserat på en användare med namnet *Britta Simon*.</span><span class="sxs-lookup"><span data-stu-id="4a280-137">In this section, you set up and test Azure AD single sign-on with SAP Business Object Cloud based on a test user named *Britta Simon*.</span></span>

<span data-ttu-id="4a280-138">För enkel inloggning ska fungera, Azure AD som behöver veta Azure AD motsvarighet användaren i molnet för SAP Business-objektet.</span><span class="sxs-lookup"><span data-stu-id="4a280-138">For single sign-on to work, Azure AD needs to know the Azure AD counterpart user in SAP Business Object Cloud.</span></span> <span data-ttu-id="4a280-139">En länk förhållandet mellan en Azure AD-användare och relaterade användaren i SAP Business objektet molnet måste upprättas.</span><span class="sxs-lookup"><span data-stu-id="4a280-139">A link relationship between an Azure AD user and the related user in SAP Business Object Cloud must be established.</span></span>

<span data-ttu-id="4a280-140">Upprätta länk-relationen i SAP Business objektet molnet för **användarnamn**, tilldela värdet för den **användarnamn** i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4a280-140">To establish the link relationship, in SAP Business Object Cloud, for **Username**, assign the value of the **user name** in Azure AD.</span></span>

<span data-ttu-id="4a280-141">Om du vill konfigurera och testa Azure AD enkel inloggning till SAP Business objektet moln, utför följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="4a280-141">To configure and test Azure AD single sign-on with SAP Business Object Cloud, complete the following tasks:</span></span>

1. <span data-ttu-id="4a280-142">[Konfigurera Azure AD enkel inloggning](#set-up-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="4a280-142">[Set up Azure AD single sign-on](#set-up-azure-ad-single-sign-on).</span></span> <span data-ttu-id="4a280-143">Ställer in en användare att använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4a280-143">Sets up a user to use this feature.</span></span>
2. <span data-ttu-id="4a280-144">[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="4a280-144">[Create an Azure AD test user](#create-an-azure-ad-test-user).</span></span> <span data-ttu-id="4a280-145">Testerna Azure AD enkel inloggning med användaren Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4a280-145">Tests Azure AD single sign-on with the user Britta Simon.</span></span>
3. <span data-ttu-id="4a280-146">[Skapa en SAP Business objektet molnet testanvändare](#create-an-sap-business-object-cloud-test-user).</span><span class="sxs-lookup"><span data-stu-id="4a280-146">[Create an SAP Business Object Cloud test user](#create-an-sap-business-object-cloud-test-user).</span></span> <span data-ttu-id="4a280-147">Skapar en motsvarighet för Britta Simon i SAP Business objektet moln som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="4a280-147">Creates a counterpart of Britta Simon in SAP Business Object Cloud that is linked to the Azure AD representation of the user.</span></span>
4. <span data-ttu-id="4a280-148">[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="4a280-148">[Assign the Azure AD test user](#assign-the-azure-ad-test-user).</span></span> <span data-ttu-id="4a280-149">Ställer in Britta Simon använda Azure AD för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4a280-149">Sets up Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4a280-150">[Testa enkel inloggning](#test-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="4a280-150">[Test single sign-on](#test-single-sign-on).</span></span> <span data-ttu-id="4a280-151">Kontrollerar att konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="4a280-151">Verifies that the configuration works.</span></span>

### <a name="set-up-azure-ad-single-sign-on"></a><span data-ttu-id="4a280-152">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4a280-152">Set up Azure AD single sign-on</span></span>

<span data-ttu-id="4a280-153">I det här avsnittet Aktivera du Azure AD enkel inloggning i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4a280-153">In this section, you turn on Azure AD single sign-on in the Azure portal.</span></span> <span data-ttu-id="4a280-154">Sedan kan ställa du in enkel inloggning i ditt program för SAP Business objektet molnet.</span><span class="sxs-lookup"><span data-stu-id="4a280-154">Then, you set up single sign-on in your SAP Business Object Cloud application.</span></span>

<span data-ttu-id="4a280-155">Konfigurera Azure AD enkel inloggning med SAP Business objektet molnet:</span><span class="sxs-lookup"><span data-stu-id="4a280-155">To set up Azure AD single sign-on with SAP Business Object Cloud:</span></span>

1. <span data-ttu-id="4a280-156">I Azure-portalen på den **SAP Business objektet molnet** programmet integration anger **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="4a280-156">In the Azure portal, on the **SAP Business Object Cloud** application integration page, select **Single sign-on**.</span></span>

    ![Välja enkel inloggning][4]

2. <span data-ttu-id="4a280-158">På den **enkel inloggning** sidan för **läge**väljer **SAML-baserade inloggning**.</span><span class="sxs-lookup"><span data-stu-id="4a280-158">On the **Single sign-on** page, for **Mode**, select **SAML-based Sign-on**.</span></span>
 
    ![Välj SAML-baserade inloggning](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_samlbase.png)

3. <span data-ttu-id="4a280-160">Under **SAP Business objektet molnet domän och URL: er**, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="4a280-160">Under **SAP Business Object Cloud Domain and URLs**, complete the following steps:</span></span>

    1. <span data-ttu-id="4a280-161">I den **inloggnings-URL** ange en URL som har följande mönster:</span><span class="sxs-lookup"><span data-stu-id="4a280-161">In the **Sign-on URL** box, enter a URL that has the following pattern:</span></span> 
    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    2. <span data-ttu-id="4a280-162">I den **identifierare** ange en URL som har följande mönster:</span><span class="sxs-lookup"><span data-stu-id="4a280-162">In the **Identifier** box, enter a URL that has the following pattern:</span></span>
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    ![URL: er och SAP Business objektet molnet domän webbadresser](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_url.png)
 
    > [!NOTE] 
    > <span data-ttu-id="4a280-164">Värdena i dessa URL: er är bara exempel.</span><span class="sxs-lookup"><span data-stu-id="4a280-164">The values in these URLs are for demonstration only.</span></span> <span data-ttu-id="4a280-165">Uppdatera värdena med faktiska inloggnings-URL och Identifierare.</span><span class="sxs-lookup"><span data-stu-id="4a280-165">Update the values with the actual sign-on URL and identifier URL.</span></span> <span data-ttu-id="4a280-166">För att få den inloggnings-URL kan kontakta den [SAP Business objektet Cloud klienten supportteamet](https://www.sap.com/product/analytics/cloud-analytics.support.html).</span><span class="sxs-lookup"><span data-stu-id="4a280-166">To get the sign-on URL, contact the [SAP Business Object Cloud Client support team](https://www.sap.com/product/analytics/cloud-analytics.support.html).</span></span> <span data-ttu-id="4a280-167">Du kan hämta ID-URL genom att hämta molnmetadata för SAP Business objekt från administrationskonsolen.</span><span class="sxs-lookup"><span data-stu-id="4a280-167">You can get the identifier URL by downloading the SAP Business Object Cloud metadata from the admin console.</span></span> <span data-ttu-id="4a280-168">Detta beskrivs senare under kursen.</span><span class="sxs-lookup"><span data-stu-id="4a280-168">This is explained later in the tutorial.</span></span> 

4. <span data-ttu-id="4a280-169">Under **SAML-signeringscertifikat**väljer **XML-Metadata för**.</span><span class="sxs-lookup"><span data-stu-id="4a280-169">Under **SAML Signing Certificate**, select **Metadata XML**.</span></span> <span data-ttu-id="4a280-170">Spara metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="4a280-170">Then, save the metadata file on your computer.</span></span>

    ![Välj XML-Metadata](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_certificate.png) 

5. <span data-ttu-id="4a280-172">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="4a280-172">Select **Save**.</span></span>

    ![Klicka på Spara](./media/active-directory-saas-sapboc-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4a280-174">I en annan webbläsarfönster loggar du in på din SAP Business objektet molnet företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="4a280-174">In a different web browser window, sign in to your SAP Business Object Cloud company site as an administrator.</span></span>

7. <span data-ttu-id="4a280-175">Välj **menyn** > **System** > **Administration**.</span><span class="sxs-lookup"><span data-stu-id="4a280-175">Select **Menu** > **System** > **Administration**.</span></span>
    
    ![Välj menyn och sedan på System och Administration](./media/active-directory-saas-sapboc-tutorial/config1.png)

8. <span data-ttu-id="4a280-177">På den **säkerhet** väljer den **redigera** (penna) ikon.</span><span class="sxs-lookup"><span data-stu-id="4a280-177">On the **Security** tab, select the **Edit** (pen) icon.</span></span>
    
    ![På fliken säkerhet, väljer du ikonen Redigera](./media/active-directory-saas-sapboc-tutorial/config2.png)  

9. <span data-ttu-id="4a280-179">För **autentiseringsmetod**väljer **SAML enkel inloggning (SSO)**.</span><span class="sxs-lookup"><span data-stu-id="4a280-179">For **Authentication Method**, select **SAML Single Sign-On (SSO)**.</span></span>

    ![Välj SAML enkel inloggning för autentiseringsmetod](./media/active-directory-saas-sapboc-tutorial/config3.png)  

10. <span data-ttu-id="4a280-181">För att hämta metadata för service provider (steg 1), Välj **hämta**.</span><span class="sxs-lookup"><span data-stu-id="4a280-181">To download the service provider metadata (Step 1), select **Download**.</span></span> <span data-ttu-id="4a280-182">Hitta i metadatafilen och kopiera den **ID för entiteterna** värde.</span><span class="sxs-lookup"><span data-stu-id="4a280-182">In the metadata file, find and copy the **entityID** value.</span></span> <span data-ttu-id="4a280-183">I Azure-portalen under **SAP Business objektet molnet domän och URL: er**, klistra in värdet i den **identifierare** rutan.</span><span class="sxs-lookup"><span data-stu-id="4a280-183">In the Azure portal, under **SAP Business Object Cloud Domain and URLs**, paste the value in the **Identifier** box.</span></span>

    ![Kopiera och klistra in värdet för ID för entiteterna](./media/active-directory-saas-sapboc-tutorial/config4.png)  

11. <span data-ttu-id="4a280-185">Att överföra metadata för service provider (steg 2) i filen som du hämtade från Azure-portalen under **överför identitetsleverantör metadata**väljer **överför**.</span><span class="sxs-lookup"><span data-stu-id="4a280-185">To upload the service provider metadata (Step 2) in the file that you downloaded from the Azure portal, under **Upload Identity Provider metadata**, select **Upload**.</span></span>  

    ![Välj överför under överför identitetsleverantör metadata](./media/active-directory-saas-sapboc-tutorial/config5.png)

12. <span data-ttu-id="4a280-187">I den **användarattribut** väljer användarattribut (steg3) som du vill använda för din implementering.</span><span class="sxs-lookup"><span data-stu-id="4a280-187">In the **User Attribute** list, select the user attribute (Step 3) that you want to use for your implementation.</span></span> <span data-ttu-id="4a280-188">Den här användarattribut mappar till identitetsleverantören.</span><span class="sxs-lookup"><span data-stu-id="4a280-188">This user attribute maps to the identity provider.</span></span> <span data-ttu-id="4a280-189">Om du vill ange ett anpassat attribut på användarens sida, använda den **mappning av anpassade SAML** alternativet.</span><span class="sxs-lookup"><span data-stu-id="4a280-189">To enter a custom attribute on the user's page, use the **Custom SAML Mapping** option.</span></span> <span data-ttu-id="4a280-190">Du kan också välja antingen **e-post** eller **ANVÄNDAR-ID** som användarattribut.</span><span class="sxs-lookup"><span data-stu-id="4a280-190">Or, you can select either **Email** or **USER ID** as the user attribute.</span></span> <span data-ttu-id="4a280-191">Vi valde i vårt exempel **e-post** som vi har mappats användaranspråk för identifierare med den **userprincipalname** attribut i den **användarattribut** avsnitt i Azure portalen.</span><span class="sxs-lookup"><span data-stu-id="4a280-191">In our example, we selected **Email** because we mapped the user identifier claim with the **userprincipalname** attribute in the **User Attributes** section in the Azure portal.</span></span> <span data-ttu-id="4a280-192">Detta ger ett unikt e-post, som skickas till molnapp för SAP Business objekt i varje lyckat SAML-svar.</span><span class="sxs-lookup"><span data-stu-id="4a280-192">This provides a unique user email, which is sent to the SAP Business Object Cloud application in every successful SAML response.</span></span>

    ![Välj användarattribut](./media/active-directory-saas-sapboc-tutorial/config6.png)

13. <span data-ttu-id="4a280-194">Att verifiera kontot med identitetsprovider (steg 4) i den **inloggningen autentiseringsuppgifter (e-post)** ange användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="4a280-194">To verify the account with the identity provider (Step 4), in the **Login Credential (Email)** box, enter the user's email address.</span></span> <span data-ttu-id="4a280-195">Markera **verifiera kontot**.</span><span class="sxs-lookup"><span data-stu-id="4a280-195">Then, select **Verify Account**.</span></span> <span data-ttu-id="4a280-196">Inloggningsuppgifter läggs till användarkontot.</span><span class="sxs-lookup"><span data-stu-id="4a280-196">The system adds sign-in credentials to the user account.</span></span>

    ![Ange e-postadress och välj verifiera konto](./media/active-directory-saas-sapboc-tutorial/config7.png)

14. <span data-ttu-id="4a280-198">Välj den **spara** ikon.</span><span class="sxs-lookup"><span data-stu-id="4a280-198">Select the **Save** icon.</span></span>

    ![Spara ikon](./media/active-directory-saas-sapboc-tutorial/save.png)

> [!TIP]
> <span data-ttu-id="4a280-200">Du kan läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="4a280-200">You can read a concise version of these instructions in the [Azure portal](https://portal.azure.com), while you are setting up your app!</span></span> <span data-ttu-id="4a280-201">När du har lagt till appen genom att välja **Active Directory** > **företagsprogram**, Välj den **enkel inloggning** fliken. Du kan komma åt inbäddade dokumentation i den **Configuration** avsnittet längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="4a280-201">After you add the app by selecting **Active Directory** > **Enterprise Applications**, select the **Single Sign-On** tab. You can access the embedded documentation in the **Configuration** section, at the bottom of the page.</span></span> <span data-ttu-id="4a280-202">Mer information finns i [Azure AD inbäddade dokumentationen]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="4a280-202">For more information, see [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="4a280-203">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4a280-203">Create an Azure AD test user</span></span>
<span data-ttu-id="4a280-204">I det här avsnittet skapar du en användare med namnet Britta Simon i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4a280-204">In this section, you create a test user named Britta Simon in the Azure portal.</span></span>

<span data-ttu-id="4a280-205">Skapa en testanvändare i Azure AD:</span><span class="sxs-lookup"><span data-stu-id="4a280-205">To create a test user in Azure AD:</span></span>

1. <span data-ttu-id="4a280-206">Välj i Azure-portalen på den vänstra menyn **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="4a280-206">In the Azure portal, in the left menu, select **Azure Active Directory**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sapboc-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4a280-208">Om du vill visa en lista över användare, Välj **användare och grupper**, och välj sedan **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="4a280-208">To display the list of users, select **Users and groups**, and then select **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sapboc-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4a280-210">Öppna den **användare** dialogrutan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="4a280-210">To open the **User** dialog box, select **Add**.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sapboc-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4a280-212">I den **användaren** dialogrutan rutan, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="4a280-212">In the **User** dialog box, complete the following steps:</span></span>
 
    1. <span data-ttu-id="4a280-213">I den **namn** ange **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4a280-213">In the **Name** box, enter **BrittaSimon**.</span></span>

    2. <span data-ttu-id="4a280-214">I den **användarnamn** ange e-postadressen för användaren Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4a280-214">In the **User name** box, enter the email address of the user Britta Simon.</span></span>

    3. <span data-ttu-id="4a280-215">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="4a280-215">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    4. <span data-ttu-id="4a280-216">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4a280-216">Select **Create**.</span></span>

        ![Dialogrutan användare](./media/active-directory-saas-sapboc-tutorial/create_aaduser_04.png) 

    ![Skapa Azure AD-användare][100]

### <a name="create-an-sap-business-object-cloud-test-user"></a><span data-ttu-id="4a280-219">Skapa en SAP Business objektet molnet testanvändare</span><span class="sxs-lookup"><span data-stu-id="4a280-219">Create an SAP Business Object Cloud test user</span></span>

<span data-ttu-id="4a280-220">Azure AD-användare måste etableras i SAP Business objektet moln innan de kan logga in till SAP Business objektet moln.</span><span class="sxs-lookup"><span data-stu-id="4a280-220">Azure AD users must be provisioned in SAP Business Object Cloud before they can sign in to SAP Business Object Cloud.</span></span> <span data-ttu-id="4a280-221">I SAP Business objektet moln är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="4a280-221">In SAP Business Object Cloud, provisioning is a manual task.</span></span>

<span data-ttu-id="4a280-222">Att tillhandahålla ett användarkonto:</span><span class="sxs-lookup"><span data-stu-id="4a280-222">To provision a user account:</span></span>

1. <span data-ttu-id="4a280-223">Logga in på webbplatsen SAP Business objektet molnet företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="4a280-223">Sign in to your SAP Business Object Cloud company site as an administrator.</span></span>

2. <span data-ttu-id="4a280-224">Välj **menyn** > **säkerhet** > **användare**.</span><span class="sxs-lookup"><span data-stu-id="4a280-224">Select **Menu** > **Security** > **Users**.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-sapboc-tutorial/user1.png)

3. <span data-ttu-id="4a280-226">På den **användare** sidan om du vill lägga till nya användarinformation, Välj  **+** .</span><span class="sxs-lookup"><span data-stu-id="4a280-226">On the **Users** page, to add new user details, select **+**.</span></span> 

    ![Lägg till användare på sidan](./media/active-directory-saas-sapboc-tutorial/user4.png)

    <span data-ttu-id="4a280-228">Utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="4a280-228">Then, complete the following steps:</span></span>

    1. <span data-ttu-id="4a280-229">I den **ANVÄNDAR-ID** ange användar-ID för användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="4a280-229">In the **USER ID** box, enter the user ID of the user, like **Britta**.</span></span>

    2. <span data-ttu-id="4a280-230">I den **Förnamn** Ange först namnet på användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="4a280-230">In the **FIRST NAME** box, enter the first name of the user, like **Britta**.</span></span>

    3. <span data-ttu-id="4a280-231">I den **efternamn** Ange efternamn för användaren som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="4a280-231">In the **LAST NAME** box, enter the last name of the user, like **Simon**.</span></span>

    4. <span data-ttu-id="4a280-232">I den **VISNINGSNAMN** ange det fullständiga namnet på användaren som **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="4a280-232">In the **DISPLAY NAME** box, enter the full name of the user, like **Britta Simon**.</span></span>

    5. <span data-ttu-id="4a280-233">I den **e-post** ange e-postadressen för användaren som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="4a280-233">In the **E-MAIL** box, enter the email address of the user, like **brittasimon@contoso.com**.</span></span>

    6. <span data-ttu-id="4a280-234">På den **Välj roller** väljer sedan rätt roll för användaren, och välj sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="4a280-234">On the **Select Roles** page, select the appropriate role for the user, and then select **OK**.</span></span>

      ![Välja en roll](./media/active-directory-saas-sapboc-tutorial/user3.png)

    7. <span data-ttu-id="4a280-236">Välj den **spara** ikon.</span><span class="sxs-lookup"><span data-stu-id="4a280-236">Select the **Save** icon.</span></span>    


### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="4a280-237">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="4a280-237">Assign the Azure AD test user</span></span>

<span data-ttu-id="4a280-238">I det här avsnittet tillåter användaren Britta Simon att använda Azure AD enkel inloggning genom att bevilja användarkontoåtkomst till SAP Business objektet molnet.</span><span class="sxs-lookup"><span data-stu-id="4a280-238">In this section, you allow the user Britta Simon to use Azure AD single sign-on by granting the user account access to SAP Business Object Cloud.</span></span>

<span data-ttu-id="4a280-239">Tilldela Britta Simon SAP Business objektet molnet:</span><span class="sxs-lookup"><span data-stu-id="4a280-239">To assign Britta Simon to SAP Business Object Cloud:</span></span>

1. <span data-ttu-id="4a280-240">Öppna vyn program i Azure-portalen och sedan gå till vyn directory.</span><span class="sxs-lookup"><span data-stu-id="4a280-240">In the Azure portal, open the applications view, and then go to the directory view.</span></span> <span data-ttu-id="4a280-241">Välj **företagsprogram**, och välj sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4a280-241">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="4a280-243">Välj i listan med program **SAP Business objektet molnet**.</span><span class="sxs-lookup"><span data-stu-id="4a280-243">In the applications list, select **SAP Business Object Cloud**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_app.png) 

3. <span data-ttu-id="4a280-245">Välj i den vänstra menyn **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4a280-245">In the left menu, select **Users and groups**.</span></span>

    ![Välj användare och grupper][202] 

4. <span data-ttu-id="4a280-247">Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="4a280-247">Select **Add**.</span></span> <span data-ttu-id="4a280-248">Klicka sedan på den **Lägg uppdrag** väljer **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4a280-248">Then, on the **Add Assignment** page, select **Users and groups**.</span></span>

    ![Sidan Lägg till tilldelning][203]

5. <span data-ttu-id="4a280-250">På den **användare och grupper** sida i listan över användare, Välj **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="4a280-250">On the **Users and groups** page, in the list of users, select **Britta Simon**.</span></span>

6. <span data-ttu-id="4a280-251">På den **användare och grupper** väljer **Välj**.</span><span class="sxs-lookup"><span data-stu-id="4a280-251">On the **Users and groups** page, select **Select**.</span></span>

7. <span data-ttu-id="4a280-252">På den **Lägg uppdrag** väljer **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="4a280-252">On the **Add Assignment** page, select **Assign**.</span></span>

![Tilldela rollen][200] 
    
### <a name="test-single-sign-on"></a><span data-ttu-id="4a280-254">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4a280-254">Test single sign-on</span></span>

<span data-ttu-id="4a280-255">I det här avsnittet testa du Azure AD enkel inloggning konfigurationen med hjälp av åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4a280-255">In this section, you test your Azure AD single sign-on configuration by using the access panel.</span></span>

<span data-ttu-id="4a280-256">När du väljer panelen SAP Business objektet molnet på åtkomstpanelen bör du vara loggas in automatiskt i tillämpningsprogrammet SAP Business objektet molnet.</span><span class="sxs-lookup"><span data-stu-id="4a280-256">When you select the SAP Business Object Cloud tile in the access panel, you should be automatically signed in to your SAP Business Object Cloud application.</span></span>

<span data-ttu-id="4a280-257">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4a280-257">For more information about the access panel, see [Introduction to the access panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4a280-258">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4a280-258">Additional resources</span></span>

* [<span data-ttu-id="4a280-259">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4a280-259">List of tutorials on how to integrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4a280-260">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4a280-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


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

