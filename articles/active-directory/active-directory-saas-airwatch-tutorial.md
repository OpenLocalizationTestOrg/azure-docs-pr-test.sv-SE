---
title: "Självstudier: Azure Active Directory-integrering med AirWatch | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och AirWatch."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 1996ec97e7c0d94c5606ca43bb5956548f1f3712
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a><span data-ttu-id="622c7-103">Självstudier: Azure Active Directory-integrering med AirWatch</span><span class="sxs-lookup"><span data-stu-id="622c7-103">Tutorial: Azure Active Directory integration with AirWatch</span></span>

<span data-ttu-id="622c7-104">I kursen får lära du att integrera AirWatch med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="622c7-104">In this tutorial, you learn how to integrate AirWatch with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="622c7-105">Integrera AirWatch med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="622c7-105">Integrating AirWatch with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="622c7-106">Du kan styra i Azure AD som har åtkomst till AirWatch</span><span class="sxs-lookup"><span data-stu-id="622c7-106">You can control in Azure AD who has access to AirWatch</span></span>
- <span data-ttu-id="622c7-107">Du kan aktivera användarna att automatiskt hämta loggat in på AirWatch (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="622c7-107">You can enable your users to automatically get signed-on to AirWatch (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="622c7-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="622c7-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="622c7-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="622c7-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="622c7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="622c7-110">Prerequisites</span></span>

<span data-ttu-id="622c7-111">För att konfigurera Azure AD-integrering med AirWatch, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="622c7-111">To configure Azure AD integration with AirWatch, you need the following items:</span></span>

- <span data-ttu-id="622c7-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="622c7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="622c7-113">En AirWatch enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="622c7-113">An AirWatch single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="622c7-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="622c7-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="622c7-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="622c7-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="622c7-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="622c7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="622c7-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="622c7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="622c7-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="622c7-118">Scenario description</span></span>
<span data-ttu-id="622c7-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="622c7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="622c7-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="622c7-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="622c7-121">Att lägga till AirWatch från galleriet</span><span class="sxs-lookup"><span data-stu-id="622c7-121">Adding AirWatch from the gallery</span></span>
2. <span data-ttu-id="622c7-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="622c7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-airwatch-from-the-gallery"></a><span data-ttu-id="622c7-123">Att lägga till AirWatch från galleriet</span><span class="sxs-lookup"><span data-stu-id="622c7-123">Adding AirWatch from the gallery</span></span>
<span data-ttu-id="622c7-124">Du måste lägga till AirWatch från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av AirWatch i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="622c7-124">To configure the integration of AirWatch into Azure AD, you need to add AirWatch from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="622c7-125">**Utför följande steg för att lägga till AirWatch från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="622c7-125">**To add AirWatch from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="622c7-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="622c7-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="622c7-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="622c7-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="622c7-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="622c7-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="622c7-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="622c7-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="622c7-133">I sökrutan skriver **AirWatch**.</span><span class="sxs-lookup"><span data-stu-id="622c7-133">In the search box, type **AirWatch**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_search.png)

5. <span data-ttu-id="622c7-135">Välj i resultatpanelen **AirWatch**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="622c7-135">In the results panel, select **AirWatch**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="622c7-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="622c7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="622c7-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med AirWatch baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="622c7-138">In this section, you configure and test Azure AD single sign-on with AirWatch based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="622c7-139">Azure AD måste du känna till användaren i AirWatch motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="622c7-139">For single sign-on to work, Azure AD needs to know what the counterpart user in AirWatch is to a user in Azure AD.</span></span> <span data-ttu-id="622c7-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i AirWatch upprättas.</span><span class="sxs-lookup"><span data-stu-id="622c7-140">In other words, a link relationship between an Azure AD user and the related user in AirWatch needs to be established.</span></span>

<span data-ttu-id="622c7-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i AirWatch.</span><span class="sxs-lookup"><span data-stu-id="622c7-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in AirWatch.</span></span>

<span data-ttu-id="622c7-142">Om du vill konfigurera och testa Azure AD enkel inloggning med AirWatch, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="622c7-142">To configure and test Azure AD single sign-on with AirWatch, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="622c7-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="622c7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="622c7-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="622c7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="622c7-145">**[Skapa en testanvändare AirWatch](#creating-a-airwatch-test-user)**  – du har en motsvarighet för Britta Simon i AirWatch som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="622c7-145">**[Creating a AirWatch test user](#creating-a-airwatch-test-user)** - to have a counterpart of Britta Simon in AirWatch that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="622c7-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="622c7-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="622c7-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="622c7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="622c7-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="622c7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="622c7-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt AirWatch program.</span><span class="sxs-lookup"><span data-stu-id="622c7-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your AirWatch application.</span></span>

<span data-ttu-id="622c7-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med AirWatch:**</span><span class="sxs-lookup"><span data-stu-id="622c7-150">**To configure Azure AD single sign-on with AirWatch, perform the following steps:**</span></span>

1. <span data-ttu-id="622c7-151">I Azure-portalen på den **AirWatch** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="622c7-151">In the Azure portal, on the **AirWatch** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="622c7-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="622c7-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_samlbase.png)

3. <span data-ttu-id="622c7-155">På den **AirWatch domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="622c7-155">On the **AirWatch Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_url.png)

    <span data-ttu-id="622c7-157">a.</span><span class="sxs-lookup"><span data-stu-id="622c7-157">a.</span></span> <span data-ttu-id="622c7-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`</span><span class="sxs-lookup"><span data-stu-id="622c7-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`</span></span>

    <span data-ttu-id="622c7-159">b.</span><span class="sxs-lookup"><span data-stu-id="622c7-159">b.</span></span> <span data-ttu-id="622c7-160">I den **identifierare** textruta Skriv värdet som`AirWatch`</span><span class="sxs-lookup"><span data-stu-id="622c7-160">In the **Identifier** textbox, type the value as `AirWatch`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="622c7-161">Det här värdet är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="622c7-161">This value is not the real.</span></span> <span data-ttu-id="622c7-162">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="622c7-162">Update this value with the actual Sign-on URL.</span></span> <span data-ttu-id="622c7-163">Kontakta [AirWatch klienten supportteamet](http://www.air-watch.com/company/contact-us/) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="622c7-163">Contact [AirWatch Client support team](http://www.air-watch.com/company/contact-us/) to get this value.</span></span> 
 
4. <span data-ttu-id="622c7-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="622c7-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_certificate.png) 

5. <span data-ttu-id="622c7-166">På den **AirWatch Configuration** klickar du på **konfigurera AirWatch** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="622c7-166">On the **AirWatch Configuration** section, click **Configure AirWatch** to open **Configure sign-on** window.</span></span> <span data-ttu-id="622c7-167">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="622c7-167">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_configure.png) 

6. <span data-ttu-id="622c7-169">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="622c7-169">Click **Save** button.</span></span>

    <span data-ttu-id="622c7-170">![Konfigurera enkel inloggning](./media/active-directory-saas-airwatch-tutorial/tutorial_general_400.png)
<CS></span><span class="sxs-lookup"><span data-stu-id="622c7-170">![Configure Single Sign-On](./media/active-directory-saas-airwatch-tutorial/tutorial_general_400.png)
<CS></span></span>
7. <span data-ttu-id="622c7-171">I en annan webbläsarfönster loggar du in på webbplatsen AirWatch företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="622c7-171">In a different web browser window, log in to your AirWatch company site as an administrator.</span></span>

8. <span data-ttu-id="622c7-172">I det vänstra navigeringsfönstret klickar du på **konton**, och klicka sedan på **administratörer**.</span><span class="sxs-lookup"><span data-stu-id="622c7-172">In the left navigation pane, click **Accounts**, and then click **Administrators**.</span></span>
   
   <span data-ttu-id="622c7-173">![Administratörer](./media/active-directory-saas-airwatch-tutorial/ic791920.png "administratörer")</span><span class="sxs-lookup"><span data-stu-id="622c7-173">![Administrators](./media/active-directory-saas-airwatch-tutorial/ic791920.png "Administrators")</span></span>

9. <span data-ttu-id="622c7-174">Expandera den **inställningar** -menyn och klicka sedan på **Directory Services**.</span><span class="sxs-lookup"><span data-stu-id="622c7-174">Expand the **Settings** menu, and then click **Directory Services**.</span></span>
   
   <span data-ttu-id="622c7-175">![Inställningar för](./media/active-directory-saas-airwatch-tutorial/ic791921.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="622c7-175">![Settings](./media/active-directory-saas-airwatch-tutorial/ic791921.png "Settings")</span></span>

10. <span data-ttu-id="622c7-176">Klicka på den **användare** fliken den **Bas-DN** textruta Skriv ditt domännamn och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="622c7-176">Click the **User** tab, in the **Base DN** textbox, type your domain name, and then click **Save**.</span></span>
   
   <span data-ttu-id="622c7-177">![Användaren](./media/active-directory-saas-airwatch-tutorial/ic791922.png "användare")</span><span class="sxs-lookup"><span data-stu-id="622c7-177">![User](./media/active-directory-saas-airwatch-tutorial/ic791922.png "User")</span></span>

11. <span data-ttu-id="622c7-178">Klicka på den **Server** fliken.</span><span class="sxs-lookup"><span data-stu-id="622c7-178">Click the **Server** tab.</span></span>
   
   <span data-ttu-id="622c7-179">![Server](./media/active-directory-saas-airwatch-tutorial/ic791923.png "Server")</span><span class="sxs-lookup"><span data-stu-id="622c7-179">![Server](./media/active-directory-saas-airwatch-tutorial/ic791923.png "Server")</span></span>

12. <span data-ttu-id="622c7-180">Utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="622c7-180">Perform the following steps:</span></span>
    
    <span data-ttu-id="622c7-181">![Överför](./media/active-directory-saas-airwatch-tutorial/ic791924.png "överför")</span><span class="sxs-lookup"><span data-stu-id="622c7-181">![Upload](./media/active-directory-saas-airwatch-tutorial/ic791924.png "Upload")</span></span>   
    
    <span data-ttu-id="622c7-182">a.</span><span class="sxs-lookup"><span data-stu-id="622c7-182">a.</span></span> <span data-ttu-id="622c7-183">Som **katalogtyp**väljer **ingen**.</span><span class="sxs-lookup"><span data-stu-id="622c7-183">As **Directory Type**, select **None**.</span></span>

    <span data-ttu-id="622c7-184">b.</span><span class="sxs-lookup"><span data-stu-id="622c7-184">b.</span></span> <span data-ttu-id="622c7-185">Välj **använder SAML för autentisering**.</span><span class="sxs-lookup"><span data-stu-id="622c7-185">Select **Use SAML For Authentication**.</span></span>

    <span data-ttu-id="622c7-186">c.</span><span class="sxs-lookup"><span data-stu-id="622c7-186">c.</span></span> <span data-ttu-id="622c7-187">Ladda upp det hämta certifikatet klickar du på **överför**.</span><span class="sxs-lookup"><span data-stu-id="622c7-187">To upload the downloaded certificate, click **Upload**.</span></span>

13. <span data-ttu-id="622c7-188">I den **begära** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="622c7-188">In the **Request** section, perform the following steps:</span></span>
    
    <span data-ttu-id="622c7-189">![Begära](./media/active-directory-saas-airwatch-tutorial/ic791925.png "begäran")</span><span class="sxs-lookup"><span data-stu-id="622c7-189">![Request](./media/active-directory-saas-airwatch-tutorial/ic791925.png "Request")</span></span>  

    <span data-ttu-id="622c7-190">a.</span><span class="sxs-lookup"><span data-stu-id="622c7-190">a.</span></span> <span data-ttu-id="622c7-191">Som **begära bindning av typen**väljer **efter**.</span><span class="sxs-lookup"><span data-stu-id="622c7-191">As **Request Binding Type**, select **POST**.</span></span>

    <span data-ttu-id="622c7-192">b.</span><span class="sxs-lookup"><span data-stu-id="622c7-192">b.</span></span> <span data-ttu-id="622c7-193">I Azure-portalen på den **Konfigurera enkel inloggning på Airwatch** dialogrutan sidan, kopiera den **SAML enkel inloggning Tjänstwebbadress** värdet och klistrar in det i den **identitet providern enkel inloggning URL: en** textruta.</span><span class="sxs-lookup"><span data-stu-id="622c7-193">In the Azure portal, on the **Configure single sign-on at Airwatch** dialog page, copy the **SAML Single Sign-On Service URL** value, and then paste it into the **Identity Provider Single Sign On URL** textbox.</span></span>

    <span data-ttu-id="622c7-194">c.</span><span class="sxs-lookup"><span data-stu-id="622c7-194">c.</span></span> <span data-ttu-id="622c7-195">Som **NameID Format**väljer **e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="622c7-195">As **NameID Format**, select **Email Address**.</span></span>

    <span data-ttu-id="622c7-196">d.</span><span class="sxs-lookup"><span data-stu-id="622c7-196">d.</span></span> <span data-ttu-id="622c7-197">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="622c7-197">Click **Save**.</span></span>

14. <span data-ttu-id="622c7-198">Klicka på den **användaren** fliken igen.</span><span class="sxs-lookup"><span data-stu-id="622c7-198">Click the **User** tab again.</span></span>
    
    <span data-ttu-id="622c7-199">![Användaren](./media/active-directory-saas-airwatch-tutorial/ic791926.png "användare")</span><span class="sxs-lookup"><span data-stu-id="622c7-199">![User](./media/active-directory-saas-airwatch-tutorial/ic791926.png "User")</span></span>

15. <span data-ttu-id="622c7-200">I den **attributet** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="622c7-200">In the **Attribute** section, perform the following steps:</span></span>
    
    <span data-ttu-id="622c7-201">![Attributet](./media/active-directory-saas-airwatch-tutorial/ic791927.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="622c7-201">![Attribute](./media/active-directory-saas-airwatch-tutorial/ic791927.png "Attribute")</span></span>

    <span data-ttu-id="622c7-202">a.</span><span class="sxs-lookup"><span data-stu-id="622c7-202">a.</span></span> <span data-ttu-id="622c7-203">I den **objektidentifierare** textruta typen **http://schemas.microsoft.com/identity/claims/objectidentifier**.</span><span class="sxs-lookup"><span data-stu-id="622c7-203">In the **Object Identifier** textbox, type **http://schemas.microsoft.com/identity/claims/objectidentifier**.</span></span>

    <span data-ttu-id="622c7-204">b.</span><span class="sxs-lookup"><span data-stu-id="622c7-204">b.</span></span> <span data-ttu-id="622c7-205">I den **användarnamn** textruta typen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="622c7-205">In the **Username** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span>

    <span data-ttu-id="622c7-206">c.</span><span class="sxs-lookup"><span data-stu-id="622c7-206">c.</span></span> <span data-ttu-id="622c7-207">I den **visningsnamn** textruta typen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span><span class="sxs-lookup"><span data-stu-id="622c7-207">In the **Display Name** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span></span>

    <span data-ttu-id="622c7-208">d.</span><span class="sxs-lookup"><span data-stu-id="622c7-208">d.</span></span> <span data-ttu-id="622c7-209">I den **Förnamn** textruta typen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span><span class="sxs-lookup"><span data-stu-id="622c7-209">In the **First Name** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span></span>

    <span data-ttu-id="622c7-210">e.</span><span class="sxs-lookup"><span data-stu-id="622c7-210">e.</span></span> <span data-ttu-id="622c7-211">I den **efternamn** textruta typen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span><span class="sxs-lookup"><span data-stu-id="622c7-211">In the **Last Name** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span></span>

    <span data-ttu-id="622c7-212">f.</span><span class="sxs-lookup"><span data-stu-id="622c7-212">f.</span></span> <span data-ttu-id="622c7-213">I den **e-post** textruta typen **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="622c7-213">In the **Email** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span>

    <span data-ttu-id="622c7-214">g.</span><span class="sxs-lookup"><span data-stu-id="622c7-214">g.</span></span> <span data-ttu-id="622c7-215">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="622c7-215">Click **Save**.</span></span>

<CE>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="622c7-216">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="622c7-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="622c7-217">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="622c7-217">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="622c7-219">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="622c7-219">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="622c7-220">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="622c7-220">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="622c7-222">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="622c7-222">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="622c7-224">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="622c7-224">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="622c7-226">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="622c7-226">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-airwatch-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="622c7-228">a.</span><span class="sxs-lookup"><span data-stu-id="622c7-228">a.</span></span> <span data-ttu-id="622c7-229">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="622c7-229">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="622c7-230">b.</span><span class="sxs-lookup"><span data-stu-id="622c7-230">b.</span></span> <span data-ttu-id="622c7-231">I den **användarnamn** textruta typ av **e-postadress** av Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="622c7-231">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="622c7-232">c.</span><span class="sxs-lookup"><span data-stu-id="622c7-232">c.</span></span> <span data-ttu-id="622c7-233">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="622c7-233">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="622c7-234">d.</span><span class="sxs-lookup"><span data-stu-id="622c7-234">d.</span></span> <span data-ttu-id="622c7-235">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="622c7-235">Click **Create**.</span></span>
 
### <a name="creating-a-airwatch-test-user"></a><span data-ttu-id="622c7-236">Skapa en testanvändare AirWatch</span><span class="sxs-lookup"><span data-stu-id="622c7-236">Creating a AirWatch test user</span></span>

<span data-ttu-id="622c7-237">Om du vill aktivera Azure AD-användare kan logga in på AirWatch måste de vara etablerade i till AirWatch.</span><span class="sxs-lookup"><span data-stu-id="622c7-237">To enable Azure AD users to log in to AirWatch, they must be provisioned in to AirWatch.</span></span>

* <span data-ttu-id="622c7-238">När AirWatch, etablering är en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="622c7-238">When AirWatch, provisioning is a manual task.</span></span>

<span data-ttu-id="622c7-239">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="622c7-239">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="622c7-240">Logga in på ditt **AirWatch** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="622c7-240">Log in to your **AirWatch** company site as administrator.</span></span>
2. <span data-ttu-id="622c7-241">I navigeringsfönstret till vänster klickar du på **konton**, och klicka sedan på **användare**.</span><span class="sxs-lookup"><span data-stu-id="622c7-241">In the navigation pane on the left side, click **Accounts**, and then click **Users**.</span></span>
   
   <span data-ttu-id="622c7-242">![Användare](./media/active-directory-saas-airwatch-tutorial/ic791929.png "användare")</span><span class="sxs-lookup"><span data-stu-id="622c7-242">![Users](./media/active-directory-saas-airwatch-tutorial/ic791929.png "Users")</span></span>
3. <span data-ttu-id="622c7-243">I den **användare** -menyn klickar du på **listvyn**, och klicka sedan på **Lägg till \> Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="622c7-243">In the **Users** menu, click **List View**, and then click **Add \> Add User**.</span></span>
   
   <span data-ttu-id="622c7-244">![Lägg till användare](./media/active-directory-saas-airwatch-tutorial/ic791930.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="622c7-244">![Add User](./media/active-directory-saas-airwatch-tutorial/ic791930.png "Add User")</span></span>
4. <span data-ttu-id="622c7-245">På den **Lägg till / redigera användare** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="622c7-245">On the **Add / Edit User** dialog, perform the following steps:</span></span>

   <span data-ttu-id="622c7-246">![Lägg till användare](./media/active-directory-saas-airwatch-tutorial/ic791931.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="622c7-246">![Add User](./media/active-directory-saas-airwatch-tutorial/ic791931.png "Add User")</span></span>   
   1. <span data-ttu-id="622c7-247">Typ av **användarnamn**, **lösenord**, **Bekräfta lösenord**, **Förnamn**, **efternamn**,  **E-postadress** för ett giltigt Azure Active Directory-konto som du vill etablera i relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="622c7-247">Type the **Username**, **Password**, **Confirm Password**, **First Name**, **Last Name**, **Email Address** of a valid Azure Active Directory account you want to provision into the related textboxes.</span></span>
   2. <span data-ttu-id="622c7-248">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="622c7-248">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="622c7-249">Du kan använda något annat AirWatch användarens konto skapas verktyg eller API: er som tillhandahålls av AirWatch att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="622c7-249">You can use any other AirWatch user account creation tools or APIs provided by AirWatch to provision AAD user accounts.</span></span>
>  

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="622c7-250">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="622c7-250">Assigning the Azure AD test user</span></span>

<span data-ttu-id="622c7-251">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till AirWatch.</span><span class="sxs-lookup"><span data-stu-id="622c7-251">In this section, you enable Britta Simon to use Azure single sign-on by granting access to AirWatch.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="622c7-253">**Om du vill tilldela AirWatch Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="622c7-253">**To assign Britta Simon to AirWatch, perform the following steps:**</span></span>

1. <span data-ttu-id="622c7-254">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="622c7-254">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="622c7-256">Välj i listan med program **AirWatch**.</span><span class="sxs-lookup"><span data-stu-id="622c7-256">In the applications list, select **AirWatch**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_app.png) 

3. <span data-ttu-id="622c7-258">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="622c7-258">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="622c7-260">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="622c7-260">Click **Add** button.</span></span> <span data-ttu-id="622c7-261">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="622c7-261">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="622c7-263">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="622c7-263">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="622c7-264">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="622c7-264">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="622c7-265">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="622c7-265">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="622c7-266">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="622c7-266">Testing single sign-on</span></span>

<span data-ttu-id="622c7-267">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="622c7-267">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="622c7-268">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="622c7-268">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="622c7-269">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="622c7-269">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="622c7-270">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="622c7-270">Additional resources</span></span>

* [<span data-ttu-id="622c7-271">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="622c7-271">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="622c7-272">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="622c7-272">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_203.png

