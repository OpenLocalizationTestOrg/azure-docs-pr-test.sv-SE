---
title: "Självstudier: Azure Active Directory-integrering med Picturepark | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Picturepark."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 31c21cd4-9c00-4cad-9538-a13996dc872f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: jeedes
ms.openlocfilehash: 1c009aa1fdd3140a4466cf762b6c9687e74ce4c7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a><span data-ttu-id="ab273-103">Självstudier: Azure Active Directory-integrering med Picturepark</span><span class="sxs-lookup"><span data-stu-id="ab273-103">Tutorial: Azure Active Directory integration with Picturepark</span></span>

<span data-ttu-id="ab273-104">I kursen får lära du att integrera Picturepark med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ab273-104">In this tutorial, you learn how to integrate Picturepark with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ab273-105">Integrera Picturepark med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ab273-105">Integrating Picturepark with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ab273-106">Du kan styra i Azure AD som har åtkomst till Picturepark</span><span class="sxs-lookup"><span data-stu-id="ab273-106">You can control in Azure AD who has access to Picturepark</span></span>
- <span data-ttu-id="ab273-107">Du kan aktivera användarna att automatiskt hämta loggat in på Picturepark (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ab273-107">You can enable your users to automatically get signed-on to Picturepark (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ab273-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ab273-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ab273-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ab273-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab273-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ab273-110">Prerequisites</span></span>

<span data-ttu-id="ab273-111">För att konfigurera Azure AD-integrering med Picturepark, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="ab273-111">To configure Azure AD integration with Picturepark, you need the following items:</span></span>

- <span data-ttu-id="ab273-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ab273-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ab273-113">En Picturepark enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="ab273-113">A Picturepark single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ab273-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ab273-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ab273-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ab273-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ab273-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ab273-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ab273-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ab273-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ab273-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ab273-118">Scenario description</span></span>
<span data-ttu-id="ab273-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ab273-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ab273-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ab273-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ab273-121">Att lägga till Picturepark från galleriet</span><span class="sxs-lookup"><span data-stu-id="ab273-121">Adding Picturepark from the gallery</span></span>
2. <span data-ttu-id="ab273-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ab273-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-picturepark-from-the-gallery"></a><span data-ttu-id="ab273-123">Att lägga till Picturepark från galleriet</span><span class="sxs-lookup"><span data-stu-id="ab273-123">Adding Picturepark from the gallery</span></span>
<span data-ttu-id="ab273-124">Du måste lägga till Picturepark från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Picturepark i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ab273-124">To configure the integration of Picturepark into Azure AD, you need to add Picturepark from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ab273-125">**Utför följande steg för att lägga till Picturepark från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="ab273-125">**To add Picturepark from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ab273-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ab273-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ab273-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ab273-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ab273-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ab273-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="ab273-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ab273-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="ab273-133">I sökrutan skriver **Picturepark**.</span><span class="sxs-lookup"><span data-stu-id="ab273-133">In the search box, type **Picturepark**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_search.png)

5. <span data-ttu-id="ab273-135">Välj i resultatpanelen **Picturepark**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="ab273-135">In the results panel, select **Picturepark**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ab273-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ab273-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ab273-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Picturepark baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ab273-138">In this section, you configure and test Azure AD single sign-on with Picturepark based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ab273-139">Azure AD måste du känna till användaren i Picturepark motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="ab273-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Picturepark is to a user in Azure AD.</span></span> <span data-ttu-id="ab273-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Picturepark upprättas.</span><span class="sxs-lookup"><span data-stu-id="ab273-140">In other words, a link relationship between an Azure AD user and the related user in Picturepark needs to be established.</span></span>

<span data-ttu-id="ab273-141">I Picturepark, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="ab273-141">In Picturepark, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ab273-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Picturepark, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ab273-142">To configure and test Azure AD single sign-on with Picturepark, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ab273-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ab273-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ab273-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ab273-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ab273-145">**[Skapa en testanvändare Picturepark](#creating-a-picturepark-test-user)**  – du har en motsvarighet för Britta Simon i Picturepark som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ab273-145">**[Creating a Picturepark test user](#creating-a-picturepark-test-user)** - to have a counterpart of Britta Simon in Picturepark that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ab273-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ab273-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ab273-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ab273-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ab273-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ab273-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ab273-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Picturepark program.</span><span class="sxs-lookup"><span data-stu-id="ab273-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Picturepark application.</span></span>

<span data-ttu-id="ab273-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Picturepark:**</span><span class="sxs-lookup"><span data-stu-id="ab273-150">**To configure Azure AD single sign-on with Picturepark, perform the following steps:**</span></span>

1. <span data-ttu-id="ab273-151">I Azure-portalen på den **Picturepark** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ab273-151">In the Azure portal, on the **Picturepark** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="ab273-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ab273-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_samlbase.png)

3. <span data-ttu-id="ab273-155">På den **Picturepark domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ab273-155">On the **Picturepark Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_url.png)

    <span data-ttu-id="ab273-157">a.</span><span class="sxs-lookup"><span data-stu-id="ab273-157">a.</span></span> <span data-ttu-id="ab273-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.picturepark.com`</span><span class="sxs-lookup"><span data-stu-id="ab273-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.picturepark.com`</span></span>

    <span data-ttu-id="ab273-159">b.</span><span class="sxs-lookup"><span data-stu-id="ab273-159">b.</span></span> <span data-ttu-id="ab273-160">I den **identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="ab273-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span> 
    
    |  |
    |--|
    | `https://<companyname>.current-picturepark.com`|
    | `https://<companyname>.picturepark.com`|
    | `https://<companyname>.next-picturepark.com`|
    | |

    > [!NOTE] 
    > <span data-ttu-id="ab273-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="ab273-161">These values are not real.</span></span> <span data-ttu-id="ab273-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="ab273-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ab273-163">Kontakta [Picturepark klienten supportteamet](https://picturepark.com/about/contact/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="ab273-163">Contact [Picturepark Client support team](https://picturepark.com/about/contact/) to get these values.</span></span> 
 
4. <span data-ttu-id="ab273-164">På den **SAML-signeringscertifikat** avsnittet, kopiera den **TUMAVTRYCKET** värdet för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="ab273-164">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_certificate.png) 

5. <span data-ttu-id="ab273-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ab273-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-picturepark-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ab273-168">På den **Picturepark Configuration** klickar du på **konfigurera Picturepark** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="ab273-168">On the **Picturepark Configuration** section, click **Configure Picturepark** to open **Configure sign-on** window.</span></span> <span data-ttu-id="ab273-169">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="ab273-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_configure.png) 

7. <span data-ttu-id="ab273-171">Logga in på webbplatsen Picturepark företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="ab273-171">In a different web browser window, log into your Picturepark company site as an administrator.</span></span>

8. <span data-ttu-id="ab273-172">Klicka på i verktygsfältet högst upp **Administrationsverktyg**, och klicka sedan på **hanteringskonsolen**.</span><span class="sxs-lookup"><span data-stu-id="ab273-172">In the toolbar on the top, click **Administrative tools**, and then click **Management Console**.</span></span>
   
    <span data-ttu-id="ab273-173">![Konsolen för hantering av](./media/active-directory-saas-picturepark-tutorial/ic795062.png "Management Console")</span><span class="sxs-lookup"><span data-stu-id="ab273-173">![Management Console](./media/active-directory-saas-picturepark-tutorial/ic795062.png "Management Console")</span></span>

9. <span data-ttu-id="ab273-174">Klicka på **autentisering**, och klicka sedan på **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="ab273-174">Click **Authentication**, and then click **Identity providers**.</span></span>
   
    <span data-ttu-id="ab273-175">![Autentisering](./media/active-directory-saas-picturepark-tutorial/ic795063.png "autentisering")</span><span class="sxs-lookup"><span data-stu-id="ab273-175">![Authentication](./media/active-directory-saas-picturepark-tutorial/ic795063.png "Authentication")</span></span>

10. <span data-ttu-id="ab273-176">I den **identitet providerkonfigurationen** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ab273-176">In the **Identity provider configuration** section, perform the following steps:</span></span>
   
    <span data-ttu-id="ab273-177">![Identitet providerkonfigurationen](./media/active-directory-saas-picturepark-tutorial/ic795064.png "identitet providerkonfigurationen")</span><span class="sxs-lookup"><span data-stu-id="ab273-177">![Identity provider configuration](./media/active-directory-saas-picturepark-tutorial/ic795064.png "Identity provider configuration")</span></span>
   
    <span data-ttu-id="ab273-178">a.</span><span class="sxs-lookup"><span data-stu-id="ab273-178">a.</span></span> <span data-ttu-id="ab273-179">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="ab273-179">Click **Add**.</span></span>
  
    <span data-ttu-id="ab273-180">b.</span><span class="sxs-lookup"><span data-stu-id="ab273-180">b.</span></span> <span data-ttu-id="ab273-181">Ange ett namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ab273-181">Type a name for your configuration.</span></span>
   
    <span data-ttu-id="ab273-182">c.</span><span class="sxs-lookup"><span data-stu-id="ab273-182">c.</span></span> <span data-ttu-id="ab273-183">Välj **anges som standard**.</span><span class="sxs-lookup"><span data-stu-id="ab273-183">Select **Set as default**.</span></span>
   
    <span data-ttu-id="ab273-184">d.</span><span class="sxs-lookup"><span data-stu-id="ab273-184">d.</span></span> <span data-ttu-id="ab273-185">I **Issuer URI** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ab273-185">In **Issuer URI** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="ab273-186">e.</span><span class="sxs-lookup"><span data-stu-id="ab273-186">e.</span></span> <span data-ttu-id="ab273-187">I **betrodda utfärdare USB utskrifts** textruta klistra in värdet för **tumavtrycket** som du har kopierat från **SAML-signeringscertifikat** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ab273-187">In **Trusted Issuer Thumb Print** textbox, paste the value of **Thumbprint** which you have copied from **SAML Signing Certificate** section.</span></span> 

11. <span data-ttu-id="ab273-188">Klicka på **JoinDefaultUsersGroup**.</span><span class="sxs-lookup"><span data-stu-id="ab273-188">Click **JoinDefaultUsersGroup**.</span></span>

12. <span data-ttu-id="ab273-189">Ange den **e-postadress** attribut i den **anspråk** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="ab273-189">To set the **Emailaddress** attribute in the **Claim** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` and click **Save**.</span></span>

      <span data-ttu-id="ab273-190">![Konfigurationen](./media/active-directory-saas-picturepark-tutorial/ic795065.png "konfiguration")</span><span class="sxs-lookup"><span data-stu-id="ab273-190">![Configuration](./media/active-directory-saas-picturepark-tutorial/ic795065.png "Configuration")</span></span>

> [!TIP]
> <span data-ttu-id="ab273-191">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="ab273-191">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ab273-192">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="ab273-192">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ab273-193">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ab273-193">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ab273-194">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ab273-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="ab273-195">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ab273-195">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="ab273-197">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="ab273-197">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ab273-198">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ab273-198">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ab273-200">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ab273-200">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ab273-202">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ab273-202">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ab273-204">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ab273-204">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-picturepark-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ab273-206">a.</span><span class="sxs-lookup"><span data-stu-id="ab273-206">a.</span></span> <span data-ttu-id="ab273-207">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ab273-207">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ab273-208">b.</span><span class="sxs-lookup"><span data-stu-id="ab273-208">b.</span></span> <span data-ttu-id="ab273-209">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ab273-209">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ab273-210">c.</span><span class="sxs-lookup"><span data-stu-id="ab273-210">c.</span></span> <span data-ttu-id="ab273-211">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ab273-211">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ab273-212">d.</span><span class="sxs-lookup"><span data-stu-id="ab273-212">d.</span></span> <span data-ttu-id="ab273-213">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ab273-213">Click **Create**.</span></span>
 
### <a name="creating-a-picturepark-test-user"></a><span data-ttu-id="ab273-214">Skapa en testanvändare Picturepark</span><span class="sxs-lookup"><span data-stu-id="ab273-214">Creating a Picturepark test user</span></span>

<span data-ttu-id="ab273-215">För att aktivera Azure AD-användare att logga in på Picturepark etableras de i Picturepark.</span><span class="sxs-lookup"><span data-stu-id="ab273-215">In order to enable Azure AD users to log into Picturepark, they must be provisioned into Picturepark.</span></span> <span data-ttu-id="ab273-216">När det gäller Picturepark är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="ab273-216">In the case of Picturepark, provisioning is a manual task.</span></span>

<span data-ttu-id="ab273-217">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="ab273-217">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="ab273-218">Logga in på ditt **Picturepark** klient.</span><span class="sxs-lookup"><span data-stu-id="ab273-218">Log in to your **Picturepark** tenant.</span></span>

2. <span data-ttu-id="ab273-219">Klicka på i verktygsfältet högst upp **Administrationsverktyg**, och klicka sedan på **användare**.</span><span class="sxs-lookup"><span data-stu-id="ab273-219">In the toolbar on the top, click **Administrative tools**, and then click **Users**.</span></span>
   
    <span data-ttu-id="ab273-220">![Användare](./media/active-directory-saas-picturepark-tutorial/ic795067.png "användare")</span><span class="sxs-lookup"><span data-stu-id="ab273-220">![Users](./media/active-directory-saas-picturepark-tutorial/ic795067.png "Users")</span></span>

3. <span data-ttu-id="ab273-221">I den **översikt över användare** klickar du på **ny**.</span><span class="sxs-lookup"><span data-stu-id="ab273-221">In the **Users overview** tab, click **New**.</span></span>
   
    <span data-ttu-id="ab273-222">![Användarhantering](./media/active-directory-saas-picturepark-tutorial/ic795068.png "användarhantering")</span><span class="sxs-lookup"><span data-stu-id="ab273-222">![User management](./media/active-directory-saas-picturepark-tutorial/ic795068.png "User management")</span></span>

4. <span data-ttu-id="ab273-223">På den **skapa användare** dialogrutan, utför följande steg på en giltig Azure Active Directory-användare du vill att tillhandahålla:</span><span class="sxs-lookup"><span data-stu-id="ab273-223">On the **Create User** dialog, perform the following steps of a valid Azure Active Directory User you want to provision:</span></span>
   
    <span data-ttu-id="ab273-224">![Skapa användare](./media/active-directory-saas-picturepark-tutorial/ic795069.png "skapa användare")</span><span class="sxs-lookup"><span data-stu-id="ab273-224">![Create User](./media/active-directory-saas-picturepark-tutorial/ic795069.png "Create User")</span></span>
   
    <span data-ttu-id="ab273-225">a.</span><span class="sxs-lookup"><span data-stu-id="ab273-225">a.</span></span> <span data-ttu-id="ab273-226">I den **e-postadress** textruta typ av **e-postadress** användarens  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="ab273-226">In the **Email Address** textbox, type the **email address** of the user **BrittaSimon@contoso.com**.</span></span>  
   
    <span data-ttu-id="ab273-227">b.</span><span class="sxs-lookup"><span data-stu-id="ab273-227">b.</span></span> <span data-ttu-id="ab273-228">I den **lösenord** och **Bekräfta lösenord** textrutor, typen av **lösenord** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ab273-228">In the **Password** and **Confirm Password** textboxes, type the **password** of BrittaSimon.</span></span> 
   
    <span data-ttu-id="ab273-229">c.</span><span class="sxs-lookup"><span data-stu-id="ab273-229">c.</span></span> <span data-ttu-id="ab273-230">I den **Förnamn** textruta typ av **Förnamn** användarens **Britta**.</span><span class="sxs-lookup"><span data-stu-id="ab273-230">In the **First Name** textbox, type the **First Name** of the user **Britta**.</span></span> 
   
    <span data-ttu-id="ab273-231">d.</span><span class="sxs-lookup"><span data-stu-id="ab273-231">d.</span></span> <span data-ttu-id="ab273-232">I den **efternamn** textruta typ av **efternamn** användarens **Simon**.</span><span class="sxs-lookup"><span data-stu-id="ab273-232">In the **Last Name** textbox, type the **Last Name** of the user **Simon**.</span></span>
   
    <span data-ttu-id="ab273-233">e.</span><span class="sxs-lookup"><span data-stu-id="ab273-233">e.</span></span> <span data-ttu-id="ab273-234">I den **företagets** textruta typ av **företagsnamn** för användaren.</span><span class="sxs-lookup"><span data-stu-id="ab273-234">In the **Company** textbox, type the **Company name** of the user.</span></span> 
   
    <span data-ttu-id="ab273-235">f.</span><span class="sxs-lookup"><span data-stu-id="ab273-235">f.</span></span> <span data-ttu-id="ab273-236">I den **land** textruta väljer den **land** för användaren.</span><span class="sxs-lookup"><span data-stu-id="ab273-236">In the **Country** textbox, select the **Country** of the user.</span></span>
  
    <span data-ttu-id="ab273-237">g.</span><span class="sxs-lookup"><span data-stu-id="ab273-237">g.</span></span> <span data-ttu-id="ab273-238">I den **ZIP** textruta typ av **postnummer** orten.</span><span class="sxs-lookup"><span data-stu-id="ab273-238">In the **ZIP** textbox, type the **ZIP code** of the city.</span></span>
   
    <span data-ttu-id="ab273-239">h.</span><span class="sxs-lookup"><span data-stu-id="ab273-239">h.</span></span> <span data-ttu-id="ab273-240">I den **Stad** textruta typ av **Ortnamn** för användaren.</span><span class="sxs-lookup"><span data-stu-id="ab273-240">In the **City** textbox, type the **City name** of the user.</span></span>

    <span data-ttu-id="ab273-241">Jag.</span><span class="sxs-lookup"><span data-stu-id="ab273-241">i.</span></span> <span data-ttu-id="ab273-242">Välj en **språk**.</span><span class="sxs-lookup"><span data-stu-id="ab273-242">Select a **Language**.</span></span>
   
    <span data-ttu-id="ab273-243">j.</span><span class="sxs-lookup"><span data-stu-id="ab273-243">j.</span></span> <span data-ttu-id="ab273-244">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ab273-244">Click **Create**.</span></span>

>[!NOTE]
><span data-ttu-id="ab273-245">Du kan använda något annat Picturepark användarens konto skapas verktyg eller API: er som tillhandahålls av Picturepark att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="ab273-245">You can use any other Picturepark user account creation tools or APIs provided by Picturepark to provision Azure AD user accounts.</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ab273-246">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="ab273-246">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ab273-247">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Picturepark.</span><span class="sxs-lookup"><span data-stu-id="ab273-247">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Picturepark.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="ab273-249">**Om du vill tilldela Picturepark Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ab273-249">**To assign Britta Simon to Picturepark, perform the following steps:**</span></span>

1. <span data-ttu-id="ab273-250">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ab273-250">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ab273-252">Välj i listan med program **Picturepark**.</span><span class="sxs-lookup"><span data-stu-id="ab273-252">In the applications list, select **Picturepark**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_app.png) 

3. <span data-ttu-id="ab273-254">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ab273-254">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="ab273-256">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ab273-256">Click **Add** button.</span></span> <span data-ttu-id="ab273-257">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ab273-257">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="ab273-259">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="ab273-259">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ab273-260">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ab273-260">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ab273-261">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ab273-261">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ab273-262">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ab273-262">Testing single sign-on</span></span>

<span data-ttu-id="ab273-263">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="ab273-263">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="ab273-264">När du klickar på panelen Picturepark på åtkomstpanelen du bör få automatiskt loggat in på ditt Picturepark program.</span><span class="sxs-lookup"><span data-stu-id="ab273-264">When you click the Picturepark tile in the Access Panel, you should get automatically signed-on to your Picturepark application.</span></span> <span data-ttu-id="ab273-265">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ab273-265">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ab273-266">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ab273-266">Additional resources</span></span>

* [<span data-ttu-id="ab273-267">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ab273-267">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ab273-268">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ab273-268">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_203.png

