---
title: "Självstudier: Azure Active Directory-integrering med TeamSeer | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och TeamSeer."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6ec4806f-fe0f-4ed7-8cfa-32d1c840433f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 2a5e8f6d1443681c43db95da5cef0b7f2ef92291
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamseer"></a><span data-ttu-id="0bdc1-103">Självstudier: Azure Active Directory-integrering med TeamSeer</span><span class="sxs-lookup"><span data-stu-id="0bdc1-103">Tutorial: Azure Active Directory integration with TeamSeer</span></span>

<span data-ttu-id="0bdc1-104">I kursen får lära du att integrera TeamSeer med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="0bdc1-104">In this tutorial, you learn how to integrate TeamSeer with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0bdc1-105">Integrera TeamSeer med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="0bdc1-105">Integrating TeamSeer with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0bdc1-106">Du kan styra i Azure AD som har åtkomst till TeamSeer</span><span class="sxs-lookup"><span data-stu-id="0bdc1-106">You can control in Azure AD who has access to TeamSeer</span></span>
- <span data-ttu-id="0bdc1-107">Du kan aktivera användarna att automatiskt hämta loggat in på TeamSeer (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="0bdc1-107">You can enable your users to automatically get signed-on to TeamSeer (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0bdc1-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0bdc1-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="0bdc1-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0bdc1-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0bdc1-110">Krav</span><span class="sxs-lookup"><span data-stu-id="0bdc1-110">Prerequisites</span></span>

<span data-ttu-id="0bdc1-111">För att konfigurera Azure AD-integrering med TeamSeer, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="0bdc1-111">To configure Azure AD integration with TeamSeer, you need the following items:</span></span>

- <span data-ttu-id="0bdc1-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0bdc1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0bdc1-113">En TeamSeer enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="0bdc1-113">A TeamSeer single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0bdc1-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0bdc1-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="0bdc1-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0bdc1-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0bdc1-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0bdc1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0bdc1-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="0bdc1-118">Scenario description</span></span>
<span data-ttu-id="0bdc1-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0bdc1-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="0bdc1-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0bdc1-121">Att lägga till TeamSeer från galleriet</span><span class="sxs-lookup"><span data-stu-id="0bdc1-121">Adding TeamSeer from the gallery</span></span>
2. <span data-ttu-id="0bdc1-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0bdc1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-teamseer-from-the-gallery"></a><span data-ttu-id="0bdc1-123">Att lägga till TeamSeer från galleriet</span><span class="sxs-lookup"><span data-stu-id="0bdc1-123">Adding TeamSeer from the gallery</span></span>
<span data-ttu-id="0bdc1-124">Du måste lägga till TeamSeer från galleriet i listan över hanterade SaaS-appar för att konfigurera TeamSeer i till Azure AD-integrering.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-124">To configure the integration of TeamSeer in to Azure AD, you need to add TeamSeer from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0bdc1-125">**Utför följande steg för att lägga till TeamSeer från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="0bdc1-125">**To add TeamSeer from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0bdc1-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0bdc1-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0bdc1-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="0bdc1-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="0bdc1-133">I sökrutan skriver **TeamSeer**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-133">In the search box, type **TeamSeer**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_search.png)

5. <span data-ttu-id="0bdc1-135">Välj i resultatpanelen **TeamSeer**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-135">In the results panel, select **TeamSeer**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0bdc1-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0bdc1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0bdc1-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med TeamSeer baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-138">In this section, you configure and test Azure AD single sign-on with TeamSeer based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0bdc1-139">Azure AD måste du känna till användaren i TeamSeer motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-139">For single sign-on to work, Azure AD needs to know what the counterpart user in TeamSeer is to a user in Azure AD.</span></span> <span data-ttu-id="0bdc1-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i TeamSeer upprättas.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-140">In other words, a link relationship between an Azure AD user and the related user in TeamSeer needs to be established.</span></span>

<span data-ttu-id="0bdc1-141">I TeamSeer, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-141">In TeamSeer, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="0bdc1-142">Om du vill konfigurera och testa Azure AD enkel inloggning med TeamSeer, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="0bdc1-142">To configure and test Azure AD single sign-on with TeamSeer, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0bdc1-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0bdc1-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0bdc1-145">**[Skapa en testanvändare TeamSeer](#creating-a-teamseer-test-user)**  – du har en motsvarighet för Britta Simon i TeamSeer som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-145">**[Creating a TeamSeer test user](#creating-a-teamseer-test-user)** - to have a counterpart of Britta Simon in TeamSeer that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0bdc1-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0bdc1-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0bdc1-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0bdc1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0bdc1-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt TeamSeer program.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TeamSeer application.</span></span>

<span data-ttu-id="0bdc1-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med TeamSeer:**</span><span class="sxs-lookup"><span data-stu-id="0bdc1-150">**To configure Azure AD single sign-on with TeamSeer, perform the following steps:**</span></span>

1. <span data-ttu-id="0bdc1-151">I Azure-portalen på den **TeamSeer** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-151">In the Azure portal, on the **TeamSeer** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="0bdc1-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_samlbase.png)

3. <span data-ttu-id="0bdc1-155">På den **TeamSeer domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0bdc1-155">On the **TeamSeer Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_url.png)

     <span data-ttu-id="0bdc1-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://www.teamseer.com/<companyid>`</span><span class="sxs-lookup"><span data-stu-id="0bdc1-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://www.teamseer.com/<companyid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0bdc1-158">Värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-158">The value is not real.</span></span> <span data-ttu-id="0bdc1-159">Uppdatera värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="0bdc1-160">Kontakta [TeamSeer klienten supportteamet](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) värdet hämtas.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-160">Contact [TeamSeer Client support team](http://pages.theaccessgroup.com/solutions_business-suite_absence-management_contact.html) to get the value.</span></span> 
 
4. <span data-ttu-id="0bdc1-161">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_certificate.png) 

5. <span data-ttu-id="0bdc1-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamseer-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0bdc1-165">På den **TeamSeer Configuration** klickar du på **konfigurera TeamSeer** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-165">On the **TeamSeer Configuration** section, click **Configure TeamSeer** to open **Configure sign-on** window.</span></span> <span data-ttu-id="0bdc1-166">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="0bdc1-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_configure.png)

7. <span data-ttu-id="0bdc1-168">I en annan webbläsarfönster loggar du in på webbplatsen TeamSeer företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-168">In a different web browser window, log in to your TeamSeer company site as an administrator.</span></span>

8. <span data-ttu-id="0bdc1-169">Gå till **HR Admin**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-169">Go to **HR Admin**.</span></span>
   
    <span data-ttu-id="0bdc1-170">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789634.png "HR Admin")</span><span class="sxs-lookup"><span data-stu-id="0bdc1-170">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789634.png "HR Admin")</span></span>

9. <span data-ttu-id="0bdc1-171">Klicka på **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-171">Click **Setup**.</span></span>
   
    <span data-ttu-id="0bdc1-172">![Installationsprogrammet](./media/active-directory-saas-teamseer-tutorial/ic789635.png "installationen")</span><span class="sxs-lookup"><span data-stu-id="0bdc1-172">![Setup](./media/active-directory-saas-teamseer-tutorial/ic789635.png "Setup")</span></span>

10. <span data-ttu-id="0bdc1-173">Klicka på **ställa in information om SAML provider**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-173">Click **Set up SAML provider details**.</span></span>
   
    <span data-ttu-id="0bdc1-174">![Inställningar för SAML](./media/active-directory-saas-teamseer-tutorial/ic789636.png "SAML-inställningar")</span><span class="sxs-lookup"><span data-stu-id="0bdc1-174">![SAML Settings](./media/active-directory-saas-teamseer-tutorial/ic789636.png "SAML Settings")</span></span>

11. <span data-ttu-id="0bdc1-175">Utför följande steg i avsnittet SAML-providern information:</span><span class="sxs-lookup"><span data-stu-id="0bdc1-175">In the SAML provider details section, perform the following steps:</span></span>
   
    <span data-ttu-id="0bdc1-176">![Inställningar för SAML](./media/active-directory-saas-teamseer-tutorial/ic789637.png "SAML-inställningar")</span><span class="sxs-lookup"><span data-stu-id="0bdc1-176">![SAML Settings](./media/active-directory-saas-teamseer-tutorial/ic789637.png "SAML Settings")</span></span>   

    <span data-ttu-id="0bdc1-177">a.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-177">a.</span></span> <span data-ttu-id="0bdc1-178">Klistra in den **inloggning tjänst-URL för enkel** i ett värde till den **URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-178">Paste the **Single Sign-On Service URL** value in to the **URL** textbox.</span></span>
          
    <span data-ttu-id="0bdc1-179">b.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-179">b.</span></span> <span data-ttu-id="0bdc1-180">Öppna din Base64-kodade certifikatet i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **IdP offentliga certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-180">Open your base-64 encoded certificate in notepad, copy the content of it in to your clipboard, and then paste it to the **IdP Public Certificate** textbox.</span></span>

12. <span data-ttu-id="0bdc1-181">Utför följande steg för att slutföra konfigurationen av SAML-providern:</span><span class="sxs-lookup"><span data-stu-id="0bdc1-181">To complete the SAML provider configuration, perform the following steps:</span></span>
    
    <span data-ttu-id="0bdc1-182">![Inställningar för SAML](./media/active-directory-saas-teamseer-tutorial/ic789638.png "SAML-inställningar")</span><span class="sxs-lookup"><span data-stu-id="0bdc1-182">![SAML Settings](./media/active-directory-saas-teamseer-tutorial/ic789638.png "SAML Settings")</span></span> 

    <span data-ttu-id="0bdc1-183">a.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-183">a.</span></span> <span data-ttu-id="0bdc1-184">I den **testa e-postadresser**, Skriv test användarens e-postadress.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-184">In the **Test Email Addresses**, type the test user’s email address.</span></span> 
  
    <span data-ttu-id="0bdc1-185">b.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-185">b.</span></span> <span data-ttu-id="0bdc1-186">I den **utfärdaren** textruta skriver utfärdar-URL för tjänstleverantören.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-186">In the **Issuer** textbox, type the Issuer URL of the service provider.</span></span> 
  
    <span data-ttu-id="0bdc1-187">c.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-187">c.</span></span> <span data-ttu-id="0bdc1-188">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-188">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="0bdc1-189">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="0bdc1-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0bdc1-190">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0bdc1-191">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0bdc1-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0bdc1-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="0bdc1-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="0bdc1-193">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="0bdc1-195">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="0bdc1-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0bdc1-196">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0bdc1-198">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0bdc1-200">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0bdc1-202">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0bdc1-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamseer-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0bdc1-204">a.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-204">a.</span></span> <span data-ttu-id="0bdc1-205">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0bdc1-206">b.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-206">b.</span></span> <span data-ttu-id="0bdc1-207">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0bdc1-208">c.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-208">c.</span></span> <span data-ttu-id="0bdc1-209">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0bdc1-210">d.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-210">d.</span></span> <span data-ttu-id="0bdc1-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-211">Click **Create**.</span></span>
 
### <a name="creating-a-teamseer-test-user"></a><span data-ttu-id="0bdc1-212">Skapa en testanvändare TeamSeer</span><span class="sxs-lookup"><span data-stu-id="0bdc1-212">Creating a TeamSeer test user</span></span>

<span data-ttu-id="0bdc1-213">Om du vill aktivera Azure AD-användare kan logga in på TeamSeer måste de vara etablerade i till ShiftPlanning.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-213">To enable Azure AD users to log in to TeamSeer, they must be provisioned in to ShiftPlanning.</span></span> <span data-ttu-id="0bdc1-214">När det gäller TeamSeer är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-214">In the case of TeamSeer, provisioning is a manual task.</span></span>

<span data-ttu-id="0bdc1-215">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="0bdc1-215">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="0bdc1-216">Logga in på ditt **TeamSeer** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-216">Log in to your **TeamSeer** company site as an administrator.</span></span>

2. <span data-ttu-id="0bdc1-217">Utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0bdc1-217">Perform the following steps:</span></span>
   
    <span data-ttu-id="0bdc1-218">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789640.png "HR Admin")</span><span class="sxs-lookup"><span data-stu-id="0bdc1-218">![HR Admin](./media/active-directory-saas-teamseer-tutorial/ic789640.png "HR Admin")</span></span>  
 
    <span data-ttu-id="0bdc1-219">a.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-219">a.</span></span> <span data-ttu-id="0bdc1-220">Gå till **HR Admin \> användare**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-220">Go to **HR Admin \> Users**.</span></span>
  
    <span data-ttu-id="0bdc1-221">b.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-221">b.</span></span> <span data-ttu-id="0bdc1-222">Klicka på **kör guiden Ny användare**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-222">Click **Run the New User wizard**.</span></span>

3. <span data-ttu-id="0bdc1-223">I den **användarinformation** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="0bdc1-223">In the **User Details** section, perform the following steps:</span></span>
   
    <span data-ttu-id="0bdc1-224">![Användarinformation](./media/active-directory-saas-teamseer-tutorial/ic789641.png "användarinformation")</span><span class="sxs-lookup"><span data-stu-id="0bdc1-224">![User Details](./media/active-directory-saas-teamseer-tutorial/ic789641.png "User Details")</span></span>

    <span data-ttu-id="0bdc1-225">a.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-225">a.</span></span> <span data-ttu-id="0bdc1-226">Typ av **Förnamn**, **efternamn**, **användarnamn (e-postadress)** av en giltig AAD-konto som du vill etablera i till relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-226">Type the **First Name**, **Surname**, **User name (Email address)** of a valid AAD account you want to provision in to the related textboxes.</span></span>
  
    <span data-ttu-id="0bdc1-227">b.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-227">b.</span></span> <span data-ttu-id="0bdc1-228">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-228">Click **Next**.</span></span>

4. <span data-ttu-id="0bdc1-229">Följ den på skärmen för att lägga till en ny användare och klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-229">Follow the on-screen instructions for adding a new user, and click **Finish**.</span></span>

>[!NOTE]
><span data-ttu-id="0bdc1-230">Du kan använda något annat TeamSeer användarens konto skapas verktyg eller API: er som tillhandahålls av TeamSeer att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-230">You can use any other TeamSeer user account creation tools or APIs provided by TeamSeer to provision Azure AD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="0bdc1-231">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="0bdc1-231">Assigning the Azure AD test user</span></span>

<span data-ttu-id="0bdc1-232">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till TeamSeer.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-232">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TeamSeer.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="0bdc1-234">**Om du vill tilldela TeamSeer Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0bdc1-234">**To assign Britta Simon to TeamSeer, perform the following steps:**</span></span>

1. <span data-ttu-id="0bdc1-235">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-235">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="0bdc1-237">Välj i listan med program **TeamSeer**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-237">In the applications list, select **TeamSeer**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamseer-tutorial/tutorial_teamseer_app.png) 

3. <span data-ttu-id="0bdc1-239">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-239">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="0bdc1-241">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-241">Click **Add** button.</span></span> <span data-ttu-id="0bdc1-242">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="0bdc1-244">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-244">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0bdc1-245">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0bdc1-246">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0bdc1-247">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0bdc1-247">Testing single sign-on</span></span>

<span data-ttu-id="0bdc1-248">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0bdc1-248">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="0bdc1-249">Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0bdc1-249">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0bdc1-250">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="0bdc1-250">Additional resources</span></span>

* [<span data-ttu-id="0bdc1-251">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0bdc1-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0bdc1-252">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0bdc1-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamseer-tutorial/tutorial_general_203.png

