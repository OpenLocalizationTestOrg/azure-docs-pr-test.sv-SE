---
title: "Självstudier: Azure Active Directory-integrering med Domo | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Domo."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 058626e4-73b3-4dc2-86ca-b060d002d70a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2017
ms.author: jeedes
ms.openlocfilehash: 919d2262cf9f14159a13370037301005b5b69da2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-domo"></a><span data-ttu-id="66241-103">Självstudier: Azure Active Directory-integrering med Domo</span><span class="sxs-lookup"><span data-stu-id="66241-103">Tutorial: Azure Active Directory integration with Domo</span></span>

<span data-ttu-id="66241-104">I kursen får lära du att integrera Domo med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="66241-104">In this tutorial, you learn how to integrate Domo with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="66241-105">Integrera Domo med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="66241-105">Integrating Domo with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="66241-106">Du kan styra i Azure AD som har åtkomst till Domo</span><span class="sxs-lookup"><span data-stu-id="66241-106">You can control in Azure AD who has access to Domo</span></span>
- <span data-ttu-id="66241-107">Du kan aktivera användarna att automatiskt hämta loggat in på Domo (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="66241-107">You can enable your users to automatically get signed-on to Domo (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="66241-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="66241-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="66241-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="66241-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66241-110">Krav</span><span class="sxs-lookup"><span data-stu-id="66241-110">Prerequisites</span></span>

<span data-ttu-id="66241-111">För att konfigurera Azure AD-integrering med Domo, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="66241-111">To configure Azure AD integration with Domo, you need the following items:</span></span>

- <span data-ttu-id="66241-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="66241-112">An Azure AD subscription</span></span>
- <span data-ttu-id="66241-113">En Domo enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="66241-113">A Domo single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="66241-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="66241-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="66241-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="66241-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="66241-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="66241-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="66241-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="66241-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="66241-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="66241-118">Scenario description</span></span>
<span data-ttu-id="66241-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="66241-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="66241-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="66241-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="66241-121">Att lägga till Domo från galleriet</span><span class="sxs-lookup"><span data-stu-id="66241-121">Adding Domo from the gallery</span></span>
2. <span data-ttu-id="66241-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="66241-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-domo-from-the-gallery"></a><span data-ttu-id="66241-123">Att lägga till Domo från galleriet</span><span class="sxs-lookup"><span data-stu-id="66241-123">Adding Domo from the gallery</span></span>
<span data-ttu-id="66241-124">Du måste lägga till Domo från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Domo i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="66241-124">To configure the integration of Domo into Azure AD, you need to add Domo from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="66241-125">**Utför följande steg för att lägga till Domo från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="66241-125">**To add Domo from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="66241-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="66241-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="66241-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="66241-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="66241-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="66241-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="66241-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="66241-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="66241-133">I sökrutan skriver **Domo**.</span><span class="sxs-lookup"><span data-stu-id="66241-133">In the search box, type **Domo**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-domo-tutorial/tutorial_domo_search.png)

5. <span data-ttu-id="66241-135">Välj i resultatpanelen **Domo**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="66241-135">In the results panel, select **Domo**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-domo-tutorial/tutorial_domo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="66241-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="66241-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="66241-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Domo baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="66241-138">In this section, you configure and test Azure AD single sign-on with Domo based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="66241-139">Azure AD måste du känna till användaren i Domo motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="66241-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Domo is to a user in Azure AD.</span></span> <span data-ttu-id="66241-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Domo upprättas.</span><span class="sxs-lookup"><span data-stu-id="66241-140">In other words, a link relationship between an Azure AD user and the related user in Domo needs to be established.</span></span>

<span data-ttu-id="66241-141">I Domo, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="66241-141">In Domo, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="66241-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Domo, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="66241-142">To configure and test Azure AD single sign-on with Domo, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="66241-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="66241-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="66241-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="66241-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="66241-145">**[Skapa en testanvändare Domo](#creating-a-domo-test-user)**  – du har en motsvarighet för Britta Simon i Domo som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="66241-145">**[Creating a Domo test user](#creating-a-domo-test-user)** - to have a counterpart of Britta Simon in Domo that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="66241-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="66241-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="66241-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="66241-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="66241-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="66241-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="66241-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Domo program.</span><span class="sxs-lookup"><span data-stu-id="66241-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Domo application.</span></span>

<span data-ttu-id="66241-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Domo:**</span><span class="sxs-lookup"><span data-stu-id="66241-150">**To configure Azure AD single sign-on with Domo, perform the following steps:**</span></span>

1. <span data-ttu-id="66241-151">I Azure-portalen på den **Domo** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="66241-151">In the Azure portal, on the **Domo** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="66241-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="66241-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-domo-tutorial/tutorial_domo_samlbase.png)

3. <span data-ttu-id="66241-155">På den **Domo domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="66241-155">On the **Domo Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-domo-tutorial/tutorial_domo_url.png)

    <span data-ttu-id="66241-157">a.</span><span class="sxs-lookup"><span data-stu-id="66241-157">a.</span></span> <span data-ttu-id="66241-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.domo.com`</span><span class="sxs-lookup"><span data-stu-id="66241-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.domo.com`</span></span>

    <span data-ttu-id="66241-159">b.</span><span class="sxs-lookup"><span data-stu-id="66241-159">b.</span></span> <span data-ttu-id="66241-160">I den **identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="66241-160">In the **Identifier** textbox, type a URL using the following patterns:</span></span>     

    | |
    |--|    
    | `https://<companyname>.domo.com` |
    | `https://<companyname>.beta.domo.com` |
    | `https://<companyname>.demo.domo.com` |
    | `https://<companyname>.dev.domo.com` | 
    | `https://<companyname>.fastage1.domo.com` |       
    | `https://<companyname>.frdev.domo.com` |       
    | `https://<companyname>.gastage.domo.com` |       
    | `https://<companyname>.load.domo.com` |       
    | `https://<companyname>.local.domo.com` |       
    | `https://<companyname>.qa.domo.com` |
    | `https://<companyname>.stage.domo.com` |
    
    > [!NOTE] 
    > <span data-ttu-id="66241-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="66241-161">These values are not real.</span></span> <span data-ttu-id="66241-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="66241-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="66241-163">Kontakta [Domo klienten supportteamet](mailto:support@domo.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="66241-163">Contact [Domo Client support team](mailto:support@domo.com) to get these values.</span></span>

4. <span data-ttu-id="66241-164">Domo program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="66241-164">Domo application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="66241-165">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="66241-165">Configure the following claims for this application.</span></span> <span data-ttu-id="66241-166">Du kan hantera värden för attributen från den ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="66241-166">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="66241-167">Följande skärmbild visar ett exempel på den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="66241-167">The following screenshot shows an example for this configuration.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-domo-tutorial/tutorial_domo_attributes.png)
    
5. <span data-ttu-id="66241-169">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="66241-169">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="66241-170">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="66241-170">Attribute Name</span></span> | <span data-ttu-id="66241-171">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="66241-171">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="66241-172">namn</span><span class="sxs-lookup"><span data-stu-id="66241-172">name</span></span> | <span data-ttu-id="66241-173">User.DisplayName</span><span class="sxs-lookup"><span data-stu-id="66241-173">user.displayname</span></span> |
    | <span data-ttu-id="66241-174">E-post</span><span class="sxs-lookup"><span data-stu-id="66241-174">email</span></span> | <span data-ttu-id="66241-175">User.Mail</span><span class="sxs-lookup"><span data-stu-id="66241-175">user.mail</span></span> |
    
    <span data-ttu-id="66241-176">a.</span><span class="sxs-lookup"><span data-stu-id="66241-176">a.</span></span> <span data-ttu-id="66241-177">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="66241-177">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-domo-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-domo-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="66241-180">b.</span><span class="sxs-lookup"><span data-stu-id="66241-180">b.</span></span> <span data-ttu-id="66241-181">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="66241-181">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="66241-182">c.</span><span class="sxs-lookup"><span data-stu-id="66241-182">c.</span></span> <span data-ttu-id="66241-183">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="66241-183">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="66241-184">d.</span><span class="sxs-lookup"><span data-stu-id="66241-184">d.</span></span> <span data-ttu-id="66241-185">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="66241-185">Click **Ok**.</span></span> 
 
6. <span data-ttu-id="66241-186">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="66241-186">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-domo-tutorial/tutorial_domo_certificate.png) 

7. <span data-ttu-id="66241-188">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="66241-188">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-domo-tutorial/tutorial_general_400.png)


8. <span data-ttu-id="66241-190">På den **Domo Configuration** klickar du på **konfigurera Domo** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="66241-190">On the **Domo Configuration** section, click **Configure Domo** to open **Configure sign-on** window.</span></span> <span data-ttu-id="66241-191">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="66241-191">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>   

   ![Konfigurera enkel inloggning](./media/active-directory-saas-domo-tutorial/tutorial_domo_configure.png) 

9. <span data-ttu-id="66241-193">Konfigurera enkel inloggning på **Domo** sida, måste du skicka den hämtade **certifikat**, **SAML enhets-ID**, **SAML enkel inloggning Tjänstwebbadress** och **Sign-Out URL** till [Domo supportteamet](mailto:support@domo.com).</span><span class="sxs-lookup"><span data-stu-id="66241-193">To configure single sign-on on **Domo** side, you need to send the downloaded **Certificate**, **SAML Entity ID**, the **SAML Single Sign-On Service URL** and the **Sign-Out URL** to [Domo support team](mailto:support@domo.com).</span></span> <span data-ttu-id="66241-194">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="66241-194">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="66241-195">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="66241-195">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="66241-196">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="66241-196">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="66241-197">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="66241-197">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="66241-198">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="66241-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="66241-199">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="66241-199">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="66241-201">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="66241-201">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="66241-202">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="66241-202">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-domo-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="66241-204">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="66241-204">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-domo-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="66241-206">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="66241-206">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-domo-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="66241-208">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="66241-208">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-domo-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="66241-210">a.</span><span class="sxs-lookup"><span data-stu-id="66241-210">a.</span></span> <span data-ttu-id="66241-211">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="66241-211">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="66241-212">b.</span><span class="sxs-lookup"><span data-stu-id="66241-212">b.</span></span> <span data-ttu-id="66241-213">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="66241-213">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="66241-214">c.</span><span class="sxs-lookup"><span data-stu-id="66241-214">c.</span></span> <span data-ttu-id="66241-215">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="66241-215">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="66241-216">d.</span><span class="sxs-lookup"><span data-stu-id="66241-216">d.</span></span> <span data-ttu-id="66241-217">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="66241-217">Click **Create**.</span></span>
 
### <a name="creating-a-domo-test-user"></a><span data-ttu-id="66241-218">Skapa en testanvändare Domo</span><span class="sxs-lookup"><span data-stu-id="66241-218">Creating a Domo test user</span></span>

<span data-ttu-id="66241-219">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Domo.</span><span class="sxs-lookup"><span data-stu-id="66241-219">The objective of this section is to create a user called Britta Simon in Domo.</span></span> <span data-ttu-id="66241-220">Domo stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="66241-220">Domo supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="66241-221">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="66241-221">There is no action item for you in this section.</span></span> <span data-ttu-id="66241-222">En ny användare skapas under ett försök att komma åt Domo om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="66241-222">A new user is created during an attempt to access Domo if it doesn't exist yet.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="66241-223">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="66241-223">Assigning the Azure AD test user</span></span>

<span data-ttu-id="66241-224">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Domo.</span><span class="sxs-lookup"><span data-stu-id="66241-224">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Domo.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="66241-226">**Om du vill tilldela Domo Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="66241-226">**To assign Britta Simon to Domo, perform the following steps:**</span></span>

1. <span data-ttu-id="66241-227">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="66241-227">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="66241-229">Välj i listan med program **Domo**.</span><span class="sxs-lookup"><span data-stu-id="66241-229">In the applications list, select **Domo**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-domo-tutorial/tutorial_domo_app.png) 

3. <span data-ttu-id="66241-231">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="66241-231">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="66241-233">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="66241-233">Click **Add** button.</span></span> <span data-ttu-id="66241-234">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="66241-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="66241-236">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="66241-236">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="66241-237">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="66241-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="66241-238">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="66241-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="66241-239">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="66241-239">Testing single sign-on</span></span>

<span data-ttu-id="66241-240">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="66241-240">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>
<span data-ttu-id="66241-241">När du klickar på panelen Domo på åtkomstpanelen du bör få automatiskt loggat in på ditt Domo program.</span><span class="sxs-lookup"><span data-stu-id="66241-241">When you click the Domo tile in the Access Panel, you should get automatically signed-on to your Domo application.</span></span>

<span data-ttu-id="66241-242">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="66241-242">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="66241-243">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="66241-243">Additional resources</span></span>

* [<span data-ttu-id="66241-244">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="66241-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="66241-245">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="66241-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-domo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-domo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-domo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-domo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-domo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-domo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-domo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-domo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-domo-tutorial/tutorial_general_203.png

