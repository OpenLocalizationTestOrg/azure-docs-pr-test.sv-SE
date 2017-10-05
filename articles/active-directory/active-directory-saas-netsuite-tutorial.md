---
title: "Självstudier: Azure Active Directory-integrering med Netsuite | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Netsuite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dafa0864-aef2-4f5e-9eac-770504688ef4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 4a19ab310212b93a53495a6fc6c25c77dfb82e79
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-netsuite"></a><span data-ttu-id="b1e9d-103">Självstudier: Azure Active Directory-integrering med Netsuite</span><span class="sxs-lookup"><span data-stu-id="b1e9d-103">Tutorial: Azure Active Directory integration with Netsuite</span></span>

<span data-ttu-id="b1e9d-104">I kursen får lära du att integrera Netsuite med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b1e9d-104">In this tutorial, you learn how to integrate Netsuite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b1e9d-105">Integrera Netsuite med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b1e9d-105">Integrating Netsuite with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b1e9d-106">Du kan styra i Azure AD som har åtkomst till Netsuite</span><span class="sxs-lookup"><span data-stu-id="b1e9d-106">You can control in Azure AD who has access to Netsuite</span></span>
- <span data-ttu-id="b1e9d-107">Du kan aktivera användarna att automatiskt hämta loggat in på Netsuite (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b1e9d-107">You can enable your users to automatically get signed-on to Netsuite (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b1e9d-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b1e9d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b1e9d-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b1e9d-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b1e9d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b1e9d-110">Prerequisites</span></span>

<span data-ttu-id="b1e9d-111">För att konfigurera Azure AD-integrering med Netsuite, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="b1e9d-111">To configure Azure AD integration with Netsuite, you need the following items:</span></span>

- <span data-ttu-id="b1e9d-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b1e9d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b1e9d-113">En Netsuite enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="b1e9d-113">A Netsuite single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b1e9d-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b1e9d-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b1e9d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b1e9d-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b1e9d-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b1e9d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b1e9d-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b1e9d-118">Scenario description</span></span>
<span data-ttu-id="b1e9d-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b1e9d-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b1e9d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b1e9d-121">Att lägga till Netsuite från galleriet</span><span class="sxs-lookup"><span data-stu-id="b1e9d-121">Adding Netsuite from the gallery</span></span>
2. <span data-ttu-id="b1e9d-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b1e9d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-netsuite-from-the-gallery"></a><span data-ttu-id="b1e9d-123">Att lägga till Netsuite från galleriet</span><span class="sxs-lookup"><span data-stu-id="b1e9d-123">Adding Netsuite from the gallery</span></span>
<span data-ttu-id="b1e9d-124">Du måste lägga till Netsuite från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Netsuite i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-124">To configure the integration of Netsuite into Azure AD, you need to add Netsuite from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b1e9d-125">**Utför följande steg för att lägga till Netsuite från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="b1e9d-125">**To add Netsuite from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b1e9d-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b1e9d-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b1e9d-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="b1e9d-131">Klicka på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-131">Click **New application** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="b1e9d-133">I sökrutan skriver **Netsuite**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-133">In the search box, type **Netsuite**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_search.png)

5. <span data-ttu-id="b1e9d-135">Välj i resultatpanelen **Netsuite**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-135">In the results panel, select **Netsuite**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b1e9d-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b1e9d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b1e9d-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Netsuite baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-138">In this section, you configure and test Azure AD single sign-on with Netsuite based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b1e9d-139">Azure AD måste du känna till användaren i Netsuite motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Netsuite is to a user in Azure AD.</span></span> <span data-ttu-id="b1e9d-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Netsuite upprättas.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-140">In other words, a link relationship between an Azure AD user and the related user in Netsuite needs to be established.</span></span>

<span data-ttu-id="b1e9d-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Netsuite.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Netsuite.</span></span>

<span data-ttu-id="b1e9d-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Netsuite, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b1e9d-142">To configure and test Azure AD single sign-on with Netsuite, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b1e9d-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b1e9d-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b1e9d-145">**[Skapa en testanvändare Netsuite](#creating-a-netsuite-test-user)**  – du har en motsvarighet för Britta Simon i Netsuite som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-145">**[Creating a Netsuite test user](#creating-a-netsuite-test-user)** - to have a counterpart of Britta Simon in Netsuite that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b1e9d-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b1e9d-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b1e9d-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b1e9d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b1e9d-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Netsuite program.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Netsuite application.</span></span>

<span data-ttu-id="b1e9d-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Netsuite:**</span><span class="sxs-lookup"><span data-stu-id="b1e9d-150">**To configure Azure AD single sign-on with Netsuite, perform the following steps:**</span></span>

1. <span data-ttu-id="b1e9d-151">I Azure-portalen på den **Netsuite** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-151">In the Azure portal, on the **Netsuite** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="b1e9d-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_samlbase.png)

3. <span data-ttu-id="b1e9d-155">På den **Netsuite domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b1e9d-155">On the **Netsuite Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_url.png)

    <span data-ttu-id="b1e9d-157">I den **Reply URL** textruta Skriv en URL med följande mönster: `https://<tenant-name>.netsuite.com/saml2/acs` `https://<tenant-name>.na1.netsuite.com/saml2/acs` `https://<tenant-name>.na2.netsuite.com/saml2/acs` `https://<tenant-name>.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs``https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`</span><span class="sxs-lookup"><span data-stu-id="b1e9d-157">In the **Reply URL** textbox, type a URL using the following pattern:   `https://<tenant-name>.netsuite.com/saml2/acs` `https://<tenant-name>.na1.netsuite.com/saml2/acs` `https://<tenant-name>.na2.netsuite.com/saml2/acs` `https://<tenant-name>.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs` `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b1e9d-158">Det här värdet är inte verkliga värde.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-158">This value is not real value.</span></span> <span data-ttu-id="b1e9d-159">Uppdatera värdet med det faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-159">Update the value with the actual Reply URL.</span></span> <span data-ttu-id="b1e9d-160">Kontakta [Netsuite supportteamet](http://www.netsuite.com/portal/services/support.shtml) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-160">Contact [Netsuite support team](http://www.netsuite.com/portal/services/support.shtml) to get this value.</span></span>
 
4. <span data-ttu-id="b1e9d-161">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_certificate.png) 

5. <span data-ttu-id="b1e9d-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b1e9d-165">På den **Netsuite Configuration** klickar du på **konfigurera Netsuite** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-165">On the **Netsuite Configuration** section, click **Configure Netsuite** to open **Configure sign-on** window.</span></span> <span data-ttu-id="b1e9d-166">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="b1e9d-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_configure.png) 

7. <span data-ttu-id="b1e9d-168">Öppna en ny flik i webbläsaren och logga in på webbplatsen Netsuite företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-168">Open a new tab in your browser, and sign into your Netsuite company site as an administrator.</span></span>

8. <span data-ttu-id="b1e9d-169">I verktygsfältet högst upp på sidan, klickar du på **installationsprogrammet**, klicka på **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-169">In the toolbar at the top of the page, click **Setup**, then click **Setup Manager**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

9. <span data-ttu-id="b1e9d-171">Från den **installationsaktiviteter** väljer **integrering**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-171">From the **Setup Tasks** list, select **Integration**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-integration.png)

10. <span data-ttu-id="b1e9d-173">I den **hantera autentisering** klickar du på **SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-173">In the **Manage Authentication** section, click **SAML Single Sign-on**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-saml.png)

11. <span data-ttu-id="b1e9d-175">På den **SAML installationsprogrammet** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b1e9d-175">On the **SAML Setup** page, perform the following steps:</span></span>
   
    <span data-ttu-id="b1e9d-176">a.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-176">a.</span></span> <span data-ttu-id="b1e9d-177">Kopiera den **SAML inloggning tjänst-URL för enkel** värde från **Snabbreferens** avsnitt i **konfigurera inloggning** och klistrar in det i den **identitet providern inloggningssidan** i Netsuite.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-177">Copy the **SAML Single Sign-On Service URL** value from **Quick Reference** section of **Configure sign-on** and paste it into the **Identity Provider Login Page** field in Netsuite.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png)
  
    <span data-ttu-id="b1e9d-179">b.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-179">b.</span></span> <span data-ttu-id="b1e9d-180">Välj i Netsuite, **primära autentiseringsmetod**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-180">In Netsuite, select **Primary Authentication Method**.</span></span>

    <span data-ttu-id="b1e9d-181">c.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-181">c.</span></span> <span data-ttu-id="b1e9d-182">För fältet **SAMLV2 identitet providern Metadata**väljer **överför IDP metadatafil**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-182">For the field labeled **SAMLV2 Identity Provider Metadata**, select **Upload IDP Metadata File**.</span></span> <span data-ttu-id="b1e9d-183">Klicka på **Bläddra** att överföra metadatafilen som du hämtade från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-183">Then click **Browse** to upload the metadata file that you downloaded from Azure portal.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png)

    <span data-ttu-id="b1e9d-185">d.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-185">d.</span></span> <span data-ttu-id="b1e9d-186">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-186">Click **Submit**.</span></span>

12. <span data-ttu-id="b1e9d-187">I Azure AD, klickar du på **visa och redigera andra användarattribut** kryssrutan och Lägg till attribut.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-187">In Azure AD, Click on **View and edit all other user attributes** check-box and add attribute.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-attributes.png)

13. <span data-ttu-id="b1e9d-189">För den **attributnamn** anger i `account`.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-189">For the **Attribute Name** field, type in `account`.</span></span> <span data-ttu-id="b1e9d-190">För den **attributvärdet** anger i din Netsuite konto-ID. Det här värdet är konstant och ändra med kontot.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-190">For the **Attribute Value** field, type in your Netsuite account ID.This value is constant and change with account.</span></span> <span data-ttu-id="b1e9d-191">Instruktioner om hur du hittar konto-ID finns nedan:</span><span class="sxs-lookup"><span data-stu-id="b1e9d-191">Instructions on how to find your account ID are included below:</span></span>

      ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-add-attribute.png)

    <span data-ttu-id="b1e9d-193">a.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-193">a.</span></span> <span data-ttu-id="b1e9d-194">Klicka på Netsuite, **installationsprogrammet** på menyn övre navigeringsfältet.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-194">In Netsuite, click **Setup** from the top navigation menu.</span></span>

    <span data-ttu-id="b1e9d-195">b.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-195">b.</span></span> <span data-ttu-id="b1e9d-196">Klicka på under den **installationsaktiviteter** avsnitt i den vänstra navigeringsfönstret menyn och väljer den **integrering** avsnittet och klicka på **Web Services-inställningar**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-196">Then click under the **Setup Tasks** section of the left navigation menu, select the **Integration** section, and click **Web Services Preferences**.</span></span>

    <span data-ttu-id="b1e9d-197">c.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-197">c.</span></span> <span data-ttu-id="b1e9d-198">Kopiera Netsuite konto-ID och klistrar in det i den **attributvärdet** i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-198">Copy your Netsuite Account ID and paste it into the **Attribute Value** field in Azure AD.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-account-id.png)

14. <span data-ttu-id="b1e9d-200">Innan användarna kan utföra enkel inloggning till Netsuite, måste de först tilldelas behörighet i Netsuite.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-200">Before users can perform single sign-on into Netsuite, they must first be assigned the appropriate permissions in Netsuite.</span></span> <span data-ttu-id="b1e9d-201">Följ anvisningarna nedan för att tilldela dessa behörigheter.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-201">Follow the instructions below to assign these permissions.</span></span>

    <span data-ttu-id="b1e9d-202">a.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-202">a.</span></span> <span data-ttu-id="b1e9d-203">Klicka på menyn övre navigeringsfältet **installationsprogrammet**, klicka på **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-203">On the top navigation menu, click **Setup**, then click **Setup Manager**.</span></span>
      
      ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    <span data-ttu-id="b1e9d-205">b.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-205">b.</span></span> <span data-ttu-id="b1e9d-206">Välj på den vänstra navigeringsmenyn **användare och roller för**, klicka på **hantera roller**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-206">On the left navigation menu, select **Users/Roles**, then click **Manage Roles**.</span></span>
      
      ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-manage-roles.png)

    <span data-ttu-id="b1e9d-208">c.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-208">c.</span></span> <span data-ttu-id="b1e9d-209">Klicka på **ny roll**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-209">Click **New Role**.</span></span>

    <span data-ttu-id="b1e9d-210">d.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-210">d.</span></span> <span data-ttu-id="b1e9d-211">Ange en **namn** för din nya rollen och välj den **enkel inloggning endast** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-211">Type in a **Name** for your new role, and select the **Single Sign-On Only** checkbox.</span></span>
      
      ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-new-role.png)

    <span data-ttu-id="b1e9d-213">e.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-213">e.</span></span> <span data-ttu-id="b1e9d-214">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-214">Click **Save**.</span></span>

    <span data-ttu-id="b1e9d-215">f.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-215">f.</span></span> <span data-ttu-id="b1e9d-216">Klicka på menyn högst upp **behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-216">In the menu on the top, click **Permissions**.</span></span> <span data-ttu-id="b1e9d-217">Klicka på **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-217">Then click **Setup**.</span></span>
      
       ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-sso.png)

    <span data-ttu-id="b1e9d-219">g.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-219">g.</span></span> <span data-ttu-id="b1e9d-220">Välj **ställa in SAM Single Sign-on**, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-220">Select **Set Up SAM Single Sign-on**, and then click **Add**.</span></span>

    <span data-ttu-id="b1e9d-221">h.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-221">h.</span></span> <span data-ttu-id="b1e9d-222">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-222">Click **Save**.</span></span>

    <span data-ttu-id="b1e9d-223">Jag.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-223">i.</span></span> <span data-ttu-id="b1e9d-224">Klicka på menyn övre navigeringsfältet **installationsprogrammet**, klicka på **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-224">On the top navigation menu, click **Setup**, then click **Setup Manager**.</span></span>
      
       ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-setup.png)

    <span data-ttu-id="b1e9d-226">j.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-226">j.</span></span> <span data-ttu-id="b1e9d-227">Välj på den vänstra navigeringsmenyn **användare och roller för**, klicka på **hantera användare**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-227">On the left navigation menu, select **Users/Roles**, then click **Manage Users**.</span></span>
      
       ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-manage-users.png)

    <span data-ttu-id="b1e9d-229">k.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-229">k.</span></span> <span data-ttu-id="b1e9d-230">Välj en testanvändare.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-230">Select a test user.</span></span> <span data-ttu-id="b1e9d-231">Klicka på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-231">Then click **Edit**.</span></span>
      
       ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-edit-user.png)

    <span data-ttu-id="b1e9d-233">l.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-233">l.</span></span> <span data-ttu-id="b1e9d-234">Markera den roll som du har skapat och klicka på dialogrutan roller **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-234">On the Roles dialog, select the role that you have created and click **Add**.</span></span>
      
       ![Konfigurera enkel inloggning](./media/active-directory-saas-Netsuite-tutorial/ns-add-role.png)

    <span data-ttu-id="b1e9d-236">m.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-236">m.</span></span> <span data-ttu-id="b1e9d-237">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-237">Click **Save**.</span></span>
    
> [!TIP]
> <span data-ttu-id="b1e9d-238">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="b1e9d-238">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b1e9d-239">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-239">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b1e9d-240">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b1e9d-240">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b1e9d-241">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b1e9d-241">Creating an Azure AD test user</span></span>
<span data-ttu-id="b1e9d-242">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-242">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="b1e9d-244">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="b1e9d-244">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b1e9d-245">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-245">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_01.png) 

2.  <span data-ttu-id="b1e9d-247">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-247">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b1e9d-249">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-249">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b1e9d-251">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b1e9d-251">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-netsuite-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b1e9d-253">a.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-253">a.</span></span> <span data-ttu-id="b1e9d-254">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-254">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b1e9d-255">b.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-255">b.</span></span> <span data-ttu-id="b1e9d-256">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-256">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b1e9d-257">c.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-257">c.</span></span> <span data-ttu-id="b1e9d-258">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-258">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b1e9d-259">d.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-259">d.</span></span> <span data-ttu-id="b1e9d-260">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-260">Click **Create**.</span></span> 

### <a name="creating-a-netsuite-test-user"></a><span data-ttu-id="b1e9d-261">Skapa en testanvändare Netsuite</span><span class="sxs-lookup"><span data-stu-id="b1e9d-261">Creating a Netsuite test user</span></span>

<span data-ttu-id="b1e9d-262">I det här avsnittet skapas en användare som kallas Britta Simon i Netsuite.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-262">In this section, a user called Britta Simon is created in Netsuite.</span></span> <span data-ttu-id="b1e9d-263">Netsuite stöder just-in-time-allokering som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-263">Netsuite supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="b1e9d-264">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-264">There is no action item for you in this section.</span></span> <span data-ttu-id="b1e9d-265">Om en användare inte redan finns i Netsuite, skapas en ny när du försöker komma åt Netsuite.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-265">If a user doesn't already exist in Netsuite, a new one is created when you attempt to access Netsuite.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b1e9d-266">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="b1e9d-266">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b1e9d-267">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Netsuite.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-267">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Netsuite.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="b1e9d-269">**Om du vill tilldela Netsuite Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b1e9d-269">**To assign Britta Simon to Netsuite, perform the following steps:**</span></span>

1. <span data-ttu-id="b1e9d-270">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-270">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b1e9d-272">Välj i listan med program **Netsuite**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-272">In the applications list, select **Netsuite**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-netsuite-tutorial/tutorial_netsuite_app.png) 

3. <span data-ttu-id="b1e9d-274">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-274">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="b1e9d-276">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-276">Click **Add** button.</span></span> <span data-ttu-id="b1e9d-277">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-277">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="b1e9d-279">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-279">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b1e9d-280">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-280">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b1e9d-281">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-281">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b1e9d-282">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b1e9d-282">Testing single sign-on</span></span>

<span data-ttu-id="b1e9d-283">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-283">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b1e9d-284">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen på [https://myapps.microsoft.com](https://myapps.microsoft.com/), logga in på kontot test och på **Netsuite**.</span><span class="sxs-lookup"><span data-stu-id="b1e9d-284">To test your single sign-on settings, open the Access Panel at [https://myapps.microsoft.com](https://myapps.microsoft.com/), sign into the test account, and click **Netsuite**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1e9d-285">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b1e9d-285">Additional resources</span></span>

* [<span data-ttu-id="b1e9d-286">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b1e9d-286">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b1e9d-287">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b1e9d-287">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="b1e9d-288">Konfigurera Användaretablering</span><span class="sxs-lookup"><span data-stu-id="b1e9d-288">Configure User Provisioning</span></span>](active-directory-saas-netsuite-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-netsuite-tutorial/tutorial_general_203.png

