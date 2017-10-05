---
title: "Självstudier: Azure Active Directory-integrering med MOVEit överföring - integrering med Azure AD | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och MOVEit överföringen - integrering med Azure AD."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: d35aceb9be2d0ff49f86a00cc84f5deb198d88f0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a><span data-ttu-id="0f7c9-103">Självstudier: Azure Active Directory-integrering med MOVEit överföring - Azure AD-integrering</span><span class="sxs-lookup"><span data-stu-id="0f7c9-103">Tutorial: Azure Active Directory integration with MOVEit Transfer - Azure AD integration</span></span>

<span data-ttu-id="0f7c9-104">I kursen får lära du att integrera MOVEit överföring - Azure AD-integrering med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="0f7c9-104">In this tutorial, you learn how to integrate MOVEit Transfer - Azure AD integration with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0f7c9-105">Integrera MOVEit överföring - Azure AD-integrering med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="0f7c9-105">Integrating MOVEit Transfer - Azure AD integration with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0f7c9-106">Du kan styra i Azure AD som har åtkomst till MOVEit överföring - integrering med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-106">You can control in Azure AD who has access to MOVEit Transfer - Azure AD integration.</span></span>
- <span data-ttu-id="0f7c9-107">Du kan aktivera användarna att automatiskt hämta inloggade MOVEit - överföring av Azure AD-integrering (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-107">You can enable your users to automatically get signed-on to MOVEit Transfer - Azure AD integration (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="0f7c9-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="0f7c9-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0f7c9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0f7c9-110">Krav</span><span class="sxs-lookup"><span data-stu-id="0f7c9-110">Prerequisites</span></span>

<span data-ttu-id="0f7c9-111">Om du vill konfigurera Azure AD-integrering med MOVEit överföring - Azure AD-integrering, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="0f7c9-111">To configure Azure AD integration with MOVEit Transfer - Azure AD integration, you need the following items:</span></span>

- <span data-ttu-id="0f7c9-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0f7c9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0f7c9-113">En MOVEit överföring - Azure AD-integrering enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="0f7c9-113">A MOVEit Transfer - Azure AD integration single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0f7c9-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0f7c9-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="0f7c9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0f7c9-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0f7c9-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0f7c9-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0f7c9-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="0f7c9-118">Scenario description</span></span>
<span data-ttu-id="0f7c9-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0f7c9-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="0f7c9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0f7c9-121">Lägga till MOVEit överföring - integrering med Azure AD från galleriet</span><span class="sxs-lookup"><span data-stu-id="0f7c9-121">Adding MOVEit Transfer - Azure AD integration from the gallery</span></span>
2. <span data-ttu-id="0f7c9-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0f7c9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-moveit-transfer---azure-ad-integration-from-the-gallery"></a><span data-ttu-id="0f7c9-123">Lägga till MOVEit överföring - integrering med Azure AD från galleriet</span><span class="sxs-lookup"><span data-stu-id="0f7c9-123">Adding MOVEit Transfer - Azure AD integration from the gallery</span></span>
<span data-ttu-id="0f7c9-124">Du måste lägga till MOVEit överföring - Azure AD-integrering från galleriet i din lista över hanterade SaaS-appar för att konfigurera MOVEit överföring - integrering med Azure AD till Azure AD-integrering.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-124">To configure the integration of MOVEit Transfer - Azure AD integration into Azure AD, you need to add MOVEit Transfer - Azure AD integration from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0f7c9-125">**Utför följande steg för att lägga till MOVEit överföring - integrering med Azure AD från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="0f7c9-125">**To add MOVEit Transfer - Azure AD integration from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0f7c9-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="0f7c9-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0f7c9-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="0f7c9-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="0f7c9-133">I sökrutan skriver **MOVEit överföring - integrering med Azure AD**väljer **MOVEit överföring - integrering med Azure AD** resultatet-panelen klickar **Lägg till** för att lägga till den programmet.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-133">In the search box, type **MOVEit Transfer - Azure AD integration**, select **MOVEit Transfer - Azure AD integration** from result panel then click **Add** button to add the application.</span></span>

    ![MOVEit överföring - Azure AD-integrering i resultat-listan](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="0f7c9-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0f7c9-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="0f7c9-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med MOVEit överföring - integrering med Azure AD utifrån en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-136">In this section, you configure and test Azure AD single sign-on with MOVEit Transfer - Azure AD integration based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0f7c9-137">Azure AD måste du känna till användaren i MOVEit överföring - integrering med Azure AD motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-137">For single sign-on to work, Azure AD needs to know what the counterpart user in MOVEit Transfer - Azure AD integration is to a user in Azure AD.</span></span> <span data-ttu-id="0f7c9-138">Med andra ord en länk förhållandet mellan en Azure AD-användare och relaterade användaren i MOVEit överföring - Azure AD-integrering måste upprättas.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-138">In other words, a link relationship between an Azure AD user and the related user in MOVEit Transfer - Azure AD integration needs to be established.</span></span>

<span data-ttu-id="0f7c9-139">I MOVEit överföring - Azure AD-integrering, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-139">In MOVEit Transfer - Azure AD integration, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="0f7c9-140">Om du vill konfigurera och testa Azure AD enkel inloggning med MOVEit överföring - Azure AD-integrering, måste du slutföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="0f7c9-140">To configure and test Azure AD single sign-on with MOVEit Transfer - Azure AD integration, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0f7c9-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0f7c9-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0f7c9-143">**[Skapa en MOVEit överföring - Azure AD-integrering testanvändare](#create-a-moveit-transfer---azure-ad-integration-test-user)**  – har en motsvarighet för Britta Simon MOVEit överföring - integrering med Azure AD som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-143">**[Create a MOVEit Transfer - Azure AD integration test user](#create-a-moveit-transfer---azure-ad-integration-test-user)** - to have a counterpart of Britta Simon in MOVEit Transfer - Azure AD integration that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0f7c9-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0f7c9-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="0f7c9-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0f7c9-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="0f7c9-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din MOVEit överföring - program för Azure AD-integrering.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your MOVEit Transfer - Azure AD integration application.</span></span>

<span data-ttu-id="0f7c9-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med MOVEit överföring - Azure AD-integrering:**</span><span class="sxs-lookup"><span data-stu-id="0f7c9-148">**To configure Azure AD single sign-on with MOVEit Transfer - Azure AD integration, perform the following steps:**</span></span>

1. <span data-ttu-id="0f7c9-149">I Azure-portalen på den **MOVEit överföring - integrering med Azure AD** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-149">In the Azure portal, on the **MOVEit Transfer - Azure AD integration** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="0f7c9-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_samlbase.png)

3. <span data-ttu-id="0f7c9-153">På den **MOVEit överföring - URL: er och integrering med Azure AD Domain** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0f7c9-153">On the **MOVEit Transfer - Azure AD integration Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_url.png)

    <span data-ttu-id="0f7c9-155">a.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-155">a.</span></span> <span data-ttu-id="0f7c9-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://contoso.com`</span><span class="sxs-lookup"><span data-stu-id="0f7c9-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://contoso.com`</span></span>

    <span data-ttu-id="0f7c9-157">b.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-157">b.</span></span> <span data-ttu-id="0f7c9-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://contoso.com/<tenatid>`</span><span class="sxs-lookup"><span data-stu-id="0f7c9-158">In the **Identifier** textbox, type a URL using the following pattern: `https://contoso.com/<tenatid>`</span></span>

    <span data-ttu-id="0f7c9-159">c.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-159">c.</span></span> <span data-ttu-id="0f7c9-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`</span><span class="sxs-lookup"><span data-stu-id="0f7c9-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`</span></span>    
     
    > [!NOTE] 
    > <span data-ttu-id="0f7c9-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-161">These values are not real.</span></span> <span data-ttu-id="0f7c9-162">Uppdatera dessa värden med den faktiska identifierare Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-162">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="0f7c9-163">Du kan se dessa värden senare i **URL för tjänstmetadata providern** avsnitt eller kontakta [MOVEit överföring - supportteamet för Azure AD-integrering klienten](https://community.ipswitch.com/s/support) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-163">You can refer these values later in **Service Provider Metadata URL** section or contact [MOVEit Transfer - Azure AD integration Client support team](https://community.ipswitch.com/s/support) to get these values.</span></span>

4. <span data-ttu-id="0f7c9-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_certificate.png) 

5. <span data-ttu-id="0f7c9-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="0f7c9-168">Logga in på din MOVEit överföring klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-168">Sign on to your MOVEit Transfer tenant as an administrator.</span></span>

7. <span data-ttu-id="0f7c9-169">I det vänstra navigeringsfönstret klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-169">On the left navigation pane, click **Settings**.</span></span>

    ![Inställningar för avsnittet på App-sida](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_000.png)

8. <span data-ttu-id="0f7c9-171">Klicka på **enkel inloggning** länk som är under **principer -> användaren Auth**.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-171">Click **Single Signon** link, which is under **Security Policies -> User Auth**.</span></span>

    ![Säkerhet principer på App-sida](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_001.png)

9. <span data-ttu-id="0f7c9-173">Klicka på länken Metadata-URL för att hämta Metadatadokumentet.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-173">Click the Metadata URL link to download the metadata document.</span></span>

    ![URL för tjänstmetadata Provider](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
    * <span data-ttu-id="0f7c9-175">Kontrollera **ID för entiteterna** matchar **identifierare** i den **MOVEit överföring - URL: er och integrering med Azure AD Domain** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-175">Verify **entityID** matches **Identifier** in the **MOVEit Transfer - Azure AD integration Domain and URLs** section .</span></span>
    * <span data-ttu-id="0f7c9-176">Kontrollera **AssertionConsumerService** -URL: en matchar **REPLY URL** i den **MOVEit överföring - URL: er och integrering med Azure AD Domain** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-176">Verify **AssertionConsumerService** Location URL matches **REPLY URL** in the **MOVEit Transfer - Azure AD integration Domain and URLs** section.</span></span>
    
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_007.png)

10. <span data-ttu-id="0f7c9-178">Klicka på **lägga till identitetsleverantör** för att lägga till en ny Provider för federerad identitet.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-178">Click **Add Identity Provider** button to add a new Federated Identity Provider.</span></span>

    ![Lägg till identitetsleverantören.](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_003.png)

11. <span data-ttu-id="0f7c9-180">Klicka på **Bläddra...**  för att välja metadatafilen som du laddade ned från Azure-portalen, klicka på **lägga till identitetsleverantör** att ladda upp den hämta filen.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-180">Click **Browse...** to select the metadata file which you downloaded from Azure portal, then click **Add Identity Provider** to upload the downloaded file.</span></span>

    ![SAML-identitetsprovider](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_004.png)

12. <span data-ttu-id="0f7c9-182">Välj ”**Ja**” som **aktiverat** i den **redigera federerad identitet providerinställningar...**  och klickar på **spara**.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-182">Select "**Yes**" as **Enabled** in the **Edit Federated Identity Provider Settings...** page and click **Save**.</span></span>

    ![Federerade identiteter providerinställningar](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_005.png)

13. <span data-ttu-id="0f7c9-184">I den **redigera federerad identitet providern användarinställningar** utför följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="0f7c9-184">In the **Edit Federated Identity Provider User Settings** page, perform the following actions:</span></span>
    
    ![Redigera inställningar för federerad identitet-Provider](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    <span data-ttu-id="0f7c9-186">a.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-186">a.</span></span> <span data-ttu-id="0f7c9-187">Välj **SAML NameID** som **inloggningsnamnet**.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-187">Select **SAML NameID** as **Login name**.</span></span>
    
    <span data-ttu-id="0f7c9-188">b.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-188">b.</span></span> <span data-ttu-id="0f7c9-189">Välj **andra** som **fullständiga namn** och i den **attributnamn** textruta placera värdet: `http://schemas.microsoft.com/identity/claims/displayname`.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-189">Select **Other** as **Full name** and in the **Attribute name** textbox put the value: `http://schemas.microsoft.com/identity/claims/displayname`.</span></span>
    
    <span data-ttu-id="0f7c9-190">c.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-190">c.</span></span> <span data-ttu-id="0f7c9-191">Välj **andra** som **e-post** och i den **attributnamn** textruta placera värdet: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-191">Select **Other** as **Email** and in the **Attribute name** textbox put the value: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
    
    <span data-ttu-id="0f7c9-192">d.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-192">d.</span></span> <span data-ttu-id="0f7c9-193">Välj **Ja** som **skapa automatiskt konto på inloggning**.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-193">Select **Yes** as **Auto-create account on signon**.</span></span>
    
    <span data-ttu-id="0f7c9-194">e.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-194">e.</span></span> <span data-ttu-id="0f7c9-195">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-195">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="0f7c9-196">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="0f7c9-196">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0f7c9-197">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-197">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0f7c9-198">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0f7c9-198">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="0f7c9-199">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="0f7c9-199">Create an Azure AD test user</span></span>

<span data-ttu-id="0f7c9-200">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-200">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="0f7c9-202">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="0f7c9-202">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0f7c9-203">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-203">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="0f7c9-205">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-205">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="0f7c9-207">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-207">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="0f7c9-209">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0f7c9-209">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_04.png)

    <span data-ttu-id="0f7c9-211">a.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-211">a.</span></span> <span data-ttu-id="0f7c9-212">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-212">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0f7c9-213">b.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-213">b.</span></span> <span data-ttu-id="0f7c9-214">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-214">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="0f7c9-215">c.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-215">c.</span></span> <span data-ttu-id="0f7c9-216">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-216">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="0f7c9-217">d.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-217">d.</span></span> <span data-ttu-id="0f7c9-218">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-218">Click **Create**.</span></span>
 
### <a name="create-a-moveit-transfer---azure-ad-integration-test-user"></a><span data-ttu-id="0f7c9-219">Skapa en MOVEit överföring - testanvändare i Azure AD-integrering</span><span class="sxs-lookup"><span data-stu-id="0f7c9-219">Create a MOVEit Transfer - Azure AD integration test user</span></span>

<span data-ttu-id="0f7c9-220">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i MOVEit överföring - integrering med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-220">The objective of this section is to create a user called Britta Simon in MOVEit Transfer - Azure AD integration.</span></span> <span data-ttu-id="0f7c9-221">MOVEit överföring - integrering med Azure AD stöder just-in-time-allokering som du har aktiverat.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-221">MOVEit Transfer - Azure AD integration supports just-in-time provisioning, which you have enabled.</span></span> <span data-ttu-id="0f7c9-222">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-222">There is no action item for you in this section.</span></span> <span data-ttu-id="0f7c9-223">En ny användare skapas under ett försök att komma åt MOVEit överföring - Azure AD-integrering om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-223">A new user is created during an attempt to access MOVEit Transfer - Azure AD integration if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="0f7c9-224">Om du behöver skapa en användare manuellt, måste du kontakta den [MOVEit överföring - supportteamet för Azure AD-integrering klienten](https://community.ipswitch.com/s/support).</span><span class="sxs-lookup"><span data-stu-id="0f7c9-224">If you need to create a user manually, you need to contact the [MOVEit Transfer - Azure AD integration Client support team](https://community.ipswitch.com/s/support).</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="0f7c9-225">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="0f7c9-225">Assign the Azure AD test user</span></span>

<span data-ttu-id="0f7c9-226">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till MOVEit överföring - integrering med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-226">In this section, you enable Britta Simon to use Azure single sign-on by granting access to MOVEit Transfer - Azure AD integration.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="0f7c9-228">**Utför följande steg om du vill tilldela Britta Simon MOVEit - överföring av Azure AD-integrering:**</span><span class="sxs-lookup"><span data-stu-id="0f7c9-228">**To assign Britta Simon to MOVEit Transfer - Azure AD integration, perform the following steps:**</span></span>

1. <span data-ttu-id="0f7c9-229">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-229">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="0f7c9-231">Välj i listan med program **MOVEit överföring - integrering med Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-231">In the applications list, select **MOVEit Transfer - Azure AD integration**.</span></span>

    ![MOVEit överföringen - Azure AD-integrering länken i listan med program](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_app.png)  

3. <span data-ttu-id="0f7c9-233">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="0f7c9-235">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-235">Click **Add** button.</span></span> <span data-ttu-id="0f7c9-236">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="0f7c9-238">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0f7c9-239">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0f7c9-240">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="0f7c9-241">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0f7c9-241">Test single sign-on</span></span>

<span data-ttu-id="0f7c9-242">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-242">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="0f7c9-243">När du klickar på MOVEit överföringen - panelen Azure AD-integrering i panelen åtkomst du ska hämta automatiskt loggat in på ditt MOVEit överföring - program för Azure AD-integrering.</span><span class="sxs-lookup"><span data-stu-id="0f7c9-243">When you click the MOVEit Transfer - Azure AD integration tile in the Access Panel, you should get automatically signed-on to your MOVEit Transfer - Azure AD integration application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="0f7c9-244">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="0f7c9-244">Additional resources</span></span>

* [<span data-ttu-id="0f7c9-245">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0f7c9-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0f7c9-246">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0f7c9-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_203.png

