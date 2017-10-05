---
title: "Självstudier: Azure Active Directory-integrering med Egnyte | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Egnyte."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 62d01333b61e73c83588d2d1701c0c300df4ab1c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-egnyte"></a><span data-ttu-id="3e571-103">Självstudier: Azure Active Directory-integrering med Egnyte</span><span class="sxs-lookup"><span data-stu-id="3e571-103">Tutorial: Azure Active Directory integration with Egnyte</span></span>

<span data-ttu-id="3e571-104">I kursen får lära du att integrera Egnyte med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3e571-104">In this tutorial, you learn how to integrate Egnyte with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3e571-105">Integrera Egnyte med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3e571-105">Integrating Egnyte with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3e571-106">Du kan styra i Azure AD som har åtkomst till Egnyte</span><span class="sxs-lookup"><span data-stu-id="3e571-106">You can control in Azure AD who has access to Egnyte</span></span>
- <span data-ttu-id="3e571-107">Du kan aktivera användarna att automatiskt hämta loggat in på Egnyte (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="3e571-107">You can enable your users to automatically get signed-on to Egnyte (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3e571-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3e571-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3e571-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3e571-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3e571-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3e571-110">Prerequisites</span></span>

<span data-ttu-id="3e571-111">För att konfigurera Azure AD-integrering med Egnyte, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="3e571-111">To configure Azure AD integration with Egnyte, you need the following items:</span></span>

- <span data-ttu-id="3e571-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3e571-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3e571-113">En Egnyte enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="3e571-113">An Egnyte single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3e571-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="3e571-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3e571-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="3e571-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3e571-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="3e571-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3e571-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3e571-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3e571-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="3e571-118">Scenario description</span></span>
<span data-ttu-id="3e571-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="3e571-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3e571-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="3e571-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3e571-121">Att lägga till Egnyte från galleriet</span><span class="sxs-lookup"><span data-stu-id="3e571-121">Adding Egnyte from the gallery</span></span>
2. <span data-ttu-id="3e571-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3e571-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-egnyte-from-the-gallery"></a><span data-ttu-id="3e571-123">Att lägga till Egnyte från galleriet</span><span class="sxs-lookup"><span data-stu-id="3e571-123">Adding Egnyte from the gallery</span></span>
<span data-ttu-id="3e571-124">Du måste lägga till Egnyte från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Egnyte i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3e571-124">To configure the integration of Egnyte into Azure AD, you need to add Egnyte from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3e571-125">**Utför följande steg för att lägga till Egnyte från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="3e571-125">**To add Egnyte from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3e571-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3e571-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3e571-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3e571-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3e571-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3e571-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="3e571-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3e571-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="3e571-133">I sökrutan skriver **Egnyte**.</span><span class="sxs-lookup"><span data-stu-id="3e571-133">In the search box, type **Egnyte**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_search.png)

5. <span data-ttu-id="3e571-135">Välj i resultatpanelen **Egnyte**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="3e571-135">In the results panel, select **Egnyte**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3e571-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3e571-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3e571-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Egnyte baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="3e571-138">In this section, you configure and test Azure AD single sign-on with Egnyte based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3e571-139">Azure AD måste du känna till användaren i Egnyte motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="3e571-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Egnyte is to a user in Azure AD.</span></span> <span data-ttu-id="3e571-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Egnyte upprättas.</span><span class="sxs-lookup"><span data-stu-id="3e571-140">In other words, a link relationship between an Azure AD user and the related user in Egnyte needs to be established.</span></span>

<span data-ttu-id="3e571-141">I Egnyte, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="3e571-141">In Egnyte, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3e571-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Egnyte, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="3e571-142">To configure and test Azure AD single sign-on with Egnyte, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3e571-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3e571-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3e571-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3e571-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3e571-145">**[Skapa en testanvändare Egnyte](#creating-an-egnyte-test-user)**  – du har en motsvarighet för Britta Simon i Egnyte som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="3e571-145">**[Creating an Egnyte test user](#creating-an-egnyte-test-user)** - to have a counterpart of Britta Simon in Egnyte that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3e571-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3e571-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3e571-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3e571-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3e571-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3e571-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3e571-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Egnyte program.</span><span class="sxs-lookup"><span data-stu-id="3e571-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Egnyte application.</span></span>

<span data-ttu-id="3e571-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Egnyte:**</span><span class="sxs-lookup"><span data-stu-id="3e571-150">**To configure Azure AD single sign-on with Egnyte, perform the following steps:**</span></span>

1. <span data-ttu-id="3e571-151">I Azure-portalen på den **Egnyte** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3e571-151">In the Azure portal, on the **Egnyte** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="3e571-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3e571-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_samlbase.png)

3. <span data-ttu-id="3e571-155">På den **Egnyte domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3e571-155">On the **Egnyte Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_url.png)

    <span data-ttu-id="3e571-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.egnyte.com`</span><span class="sxs-lookup"><span data-stu-id="3e571-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.egnyte.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3e571-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="3e571-158">This value is not real.</span></span> <span data-ttu-id="3e571-159">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="3e571-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="3e571-160">Kontakta [Egnyte klienten supportteamet](https://www.egnyte.com/corp/contact_egnyte.html) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="3e571-160">Contact [Egnyte Client support team](https://www.egnyte.com/corp/contact_egnyte.html) to get this value.</span></span> 
 
4. <span data-ttu-id="3e571-161">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="3e571-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_certificate.png) 

5. <span data-ttu-id="3e571-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="3e571-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-egnyte-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3e571-165">På den **Egnyte Configuration** klickar du på **konfigurera Egnyte** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="3e571-165">On the **Egnyte Configuration** section, click **Configure Egnyte** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3e571-166">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="3e571-166">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_configure.png) 

7. <span data-ttu-id="3e571-168">I en annan webbläsarfönster loggar du in på webbplatsen Egnyte företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="3e571-168">In a different web browser window, log in to your Egnyte company site as an administrator.</span></span>

8. <span data-ttu-id="3e571-169">Klicka på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="3e571-169">Click **Settings**.</span></span>
   
   <span data-ttu-id="3e571-170">![Inställningar för](./media/active-directory-saas-egnyte-tutorial/ic787819.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="3e571-170">![Settings](./media/active-directory-saas-egnyte-tutorial/ic787819.png "Settings")</span></span>

9. <span data-ttu-id="3e571-171">I menyn klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="3e571-171">In the menu, click **Settings**.</span></span>

   <span data-ttu-id="3e571-172">![Inställningar för](./media/active-directory-saas-egnyte-tutorial/ic787820.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="3e571-172">![Settings](./media/active-directory-saas-egnyte-tutorial/ic787820.png "Settings")</span></span>

10. <span data-ttu-id="3e571-173">Klicka på den **Configuration** fliken och klicka sedan på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="3e571-173">Click the **Configuration** tab, and then click **Security**.</span></span>

    <span data-ttu-id="3e571-174">![Säkerhet](./media/active-directory-saas-egnyte-tutorial/ic787821.png "säkerhet")</span><span class="sxs-lookup"><span data-stu-id="3e571-174">![Security](./media/active-directory-saas-egnyte-tutorial/ic787821.png "Security")</span></span>

11. <span data-ttu-id="3e571-175">I den **autentisering för enkel inloggning** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3e571-175">In the **Single Sign-On Authentication** section, perform the following steps:</span></span>

    <span data-ttu-id="3e571-176">![Enkel inloggning för autentisering](./media/active-directory-saas-egnyte-tutorial/ic787822.png "enkel inloggning för autentisering")</span><span class="sxs-lookup"><span data-stu-id="3e571-176">![Single Sign On Authentication](./media/active-directory-saas-egnyte-tutorial/ic787822.png "Single Sign On Authentication")</span></span>   
    
    <span data-ttu-id="3e571-177">a.</span><span class="sxs-lookup"><span data-stu-id="3e571-177">a.</span></span> <span data-ttu-id="3e571-178">Som **autentisering för enkel inloggning**väljer **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="3e571-178">As **Single sign-on authentication**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="3e571-179">b.</span><span class="sxs-lookup"><span data-stu-id="3e571-179">b.</span></span> <span data-ttu-id="3e571-180">Som **identitetsleverantör**väljer **AzureAD**.</span><span class="sxs-lookup"><span data-stu-id="3e571-180">As **Identity provider**, select **AzureAD**.</span></span>
   
    <span data-ttu-id="3e571-181">c.</span><span class="sxs-lookup"><span data-stu-id="3e571-181">c.</span></span> <span data-ttu-id="3e571-182">Klistra in **SAML enkel inloggning Tjänstwebbadress** kopieras från Azure-portalen i den **identitet providern inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="3e571-182">Paste **SAML Single Sign-On Service URL** copied from Azure portal into the **Identity provider login URL** textbox.</span></span>
   
    <span data-ttu-id="3e571-183">d.</span><span class="sxs-lookup"><span data-stu-id="3e571-183">d.</span></span> <span data-ttu-id="3e571-184">Klistra in **SAML enhets-ID** som du har kopierat från Azure-portalen i den **identitet providern enhets-ID** textruta.</span><span class="sxs-lookup"><span data-stu-id="3e571-184">Paste **SAML Entity ID** which you have copied from Azure portal into the **Identity provider entity ID** textbox.</span></span>
      
    <span data-ttu-id="3e571-185">e.</span><span class="sxs-lookup"><span data-stu-id="3e571-185">e.</span></span> <span data-ttu-id="3e571-186">Öppna din Base64-kodade certifikatet i anteckningar som hämtas från Azure-portalen, kopiera innehållet i den till Urklipp och klistra in den till den **providern identitetscertifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="3e571-186">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **Identity provider certificate** textbox.</span></span>
   
    <span data-ttu-id="3e571-187">f.</span><span class="sxs-lookup"><span data-stu-id="3e571-187">f.</span></span> <span data-ttu-id="3e571-188">Som **standard mappning**väljer **e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="3e571-188">As **Default user mapping**, select **Email address**.</span></span>
   
    <span data-ttu-id="3e571-189">g.</span><span class="sxs-lookup"><span data-stu-id="3e571-189">g.</span></span> <span data-ttu-id="3e571-190">Som **använder domänspecifika utfärdaren värdet**väljer **inaktiveras**.</span><span class="sxs-lookup"><span data-stu-id="3e571-190">As **Use domain-specific issuer value**, select **disabled**.</span></span>
   
    <span data-ttu-id="3e571-191">h.</span><span class="sxs-lookup"><span data-stu-id="3e571-191">h.</span></span> <span data-ttu-id="3e571-192">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="3e571-192">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="3e571-193">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="3e571-193">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3e571-194">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="3e571-194">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3e571-195">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3e571-195">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3e571-196">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="3e571-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="3e571-197">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3e571-197">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="3e571-199">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="3e571-199">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3e571-200">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3e571-200">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3e571-202">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="3e571-202">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3e571-204">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3e571-204">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3e571-206">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3e571-206">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-egnyte-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3e571-208">a.</span><span class="sxs-lookup"><span data-stu-id="3e571-208">a.</span></span> <span data-ttu-id="3e571-209">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3e571-209">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3e571-210">b.</span><span class="sxs-lookup"><span data-stu-id="3e571-210">b.</span></span> <span data-ttu-id="3e571-211">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3e571-211">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3e571-212">c.</span><span class="sxs-lookup"><span data-stu-id="3e571-212">c.</span></span> <span data-ttu-id="3e571-213">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="3e571-213">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3e571-214">d.</span><span class="sxs-lookup"><span data-stu-id="3e571-214">d.</span></span> <span data-ttu-id="3e571-215">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3e571-215">Click **Create**.</span></span>
 
### <a name="creating-an-egnyte-test-user"></a><span data-ttu-id="3e571-216">Skapa en testanvändare Egnyte</span><span class="sxs-lookup"><span data-stu-id="3e571-216">Creating an Egnyte test user</span></span>

<span data-ttu-id="3e571-217">Om du vill aktivera Azure AD-användare kan logga in på Egnyte etableras de i Egnyte.</span><span class="sxs-lookup"><span data-stu-id="3e571-217">To enable Azure AD users to log in to Egnyte, they must be provisioned into Egnyte.</span></span> <span data-ttu-id="3e571-218">När det gäller Egnyte är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3e571-218">In the case of Egnyte, provisioning is a manual task.</span></span>

<span data-ttu-id="3e571-219">**Utför följande steg för att etablera en användarkonton:**</span><span class="sxs-lookup"><span data-stu-id="3e571-219">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="3e571-220">Logga in på ditt **Egnyte** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="3e571-220">Log in to your **Egnyte** company site as administrator.</span></span>

2. <span data-ttu-id="3e571-221">Gå till **inställningar \> användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3e571-221">Go to **Settings \> Users & Groups**.</span></span>

3. <span data-ttu-id="3e571-222">Klicka på **Lägg till nya användare**, och välj sedan typ av användare som du vill lägga till.</span><span class="sxs-lookup"><span data-stu-id="3e571-222">Click **Add New User**, and then select the type of user you want to add.</span></span>
   
   <span data-ttu-id="3e571-223">![Användare](./media/active-directory-saas-egnyte-tutorial/ic787824.png "användare")</span><span class="sxs-lookup"><span data-stu-id="3e571-223">![Users](./media/active-directory-saas-egnyte-tutorial/ic787824.png "Users")</span></span>

4. <span data-ttu-id="3e571-224">I den **nya standardanvändare** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3e571-224">In the **New Standard User** section, perform the following steps:</span></span>
   
   <span data-ttu-id="3e571-225">![Nya standardanvändare](./media/active-directory-saas-egnyte-tutorial/ic787825.png "ny vanlig användare")</span><span class="sxs-lookup"><span data-stu-id="3e571-225">![New Standard User](./media/active-directory-saas-egnyte-tutorial/ic787825.png "New Standard User")</span></span>   

   <span data-ttu-id="3e571-226">a.</span><span class="sxs-lookup"><span data-stu-id="3e571-226">a.</span></span> <span data-ttu-id="3e571-227">Typ av **e-post**, **användarnamn**, och annan information om ett giltigt Azure Active Directory-konto som du vill etablera.</span><span class="sxs-lookup"><span data-stu-id="3e571-227">Type the **Email**, **Username**, and other details of a valid Azure Active Directory account you want to provision.</span></span>
   
   <span data-ttu-id="3e571-228">b.</span><span class="sxs-lookup"><span data-stu-id="3e571-228">b.</span></span> <span data-ttu-id="3e571-229">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="3e571-229">Click **Save**.</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="3e571-230">Azure Active Directory kontoinnehavaren får ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="3e571-230">The Azure Active Directory account holder will receive a notification email.</span></span>
    >

>[!NOTE]
><span data-ttu-id="3e571-231">Du kan använda något annat Egnyte användarens konto skapas verktyg eller API: er som tillhandahålls av Egnyte att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="3e571-231">You can use any other Egnyte user account creation tools or APIs provided by Egnyte to provision AAD user accounts.</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3e571-232">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="3e571-232">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3e571-233">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Egnyte.</span><span class="sxs-lookup"><span data-stu-id="3e571-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Egnyte.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="3e571-235">**Om du vill tilldela Egnyte Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3e571-235">**To assign Britta Simon to Egnyte, perform the following steps:**</span></span>

1. <span data-ttu-id="3e571-236">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3e571-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="3e571-238">Välj i listan med program **Egnyte**.</span><span class="sxs-lookup"><span data-stu-id="3e571-238">In the applications list, select **Egnyte**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-egnyte-tutorial/tutorial_egnyte_app.png) 

3. <span data-ttu-id="3e571-240">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3e571-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="3e571-242">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="3e571-242">Click **Add** button.</span></span> <span data-ttu-id="3e571-243">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3e571-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="3e571-245">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="3e571-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3e571-246">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3e571-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3e571-247">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3e571-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3e571-248">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3e571-248">Testing single sign-on</span></span>

<span data-ttu-id="3e571-249">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="3e571-249">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3e571-250">När du klickar på panelen Egnyte på åtkomstpanelen du bör få automatiskt loggat in på ditt Egnyte program.</span><span class="sxs-lookup"><span data-stu-id="3e571-250">When you click the Egnyte tile in the Access Panel, you should get automatically signed-on to your Egnyte application.</span></span>
<span data-ttu-id="3e571-251">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3e571-251">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="3e571-252">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3e571-252">Additional resources</span></span>

* [<span data-ttu-id="3e571-253">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3e571-253">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3e571-254">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3e571-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-egnyte-tutorial/tutorial_general_203.png

