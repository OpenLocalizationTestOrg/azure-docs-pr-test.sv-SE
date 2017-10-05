---
title: "Självstudier: Azure Active Directory-integrering med Optimizely | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Optimizely."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28ef03e1-9aad-4301-af97-d94e853edc74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 4d6f6da6bace09fbd6ab105530a1162653675c99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a><span data-ttu-id="194ec-103">Självstudier: Azure Active Directory-integrering med Optimizely</span><span class="sxs-lookup"><span data-stu-id="194ec-103">Tutorial: Azure Active Directory integration with Optimizely</span></span>

<span data-ttu-id="194ec-104">I kursen får lära du att integrera Optimizely med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="194ec-104">In this tutorial, you learn how to integrate Optimizely with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="194ec-105">Integrera Optimizely med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="194ec-105">Integrating Optimizely with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="194ec-106">Du kan styra i Azure AD som har åtkomst till Optimizely</span><span class="sxs-lookup"><span data-stu-id="194ec-106">You can control in Azure AD who has access to Optimizely</span></span>
- <span data-ttu-id="194ec-107">Du kan aktivera användarna att automatiskt hämta loggat in på Optimizely (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="194ec-107">You can enable your users to automatically get signed-on to Optimizely (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="194ec-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="194ec-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="194ec-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="194ec-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="194ec-110">Krav</span><span class="sxs-lookup"><span data-stu-id="194ec-110">Prerequisites</span></span>

<span data-ttu-id="194ec-111">För att konfigurera Azure AD-integrering med Optimizely, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="194ec-111">To configure Azure AD integration with Optimizely, you need the following items:</span></span>

- <span data-ttu-id="194ec-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="194ec-112">An Azure AD subscription</span></span>
- <span data-ttu-id="194ec-113">En Optimizely enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="194ec-113">An Optimizely single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="194ec-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="194ec-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="194ec-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="194ec-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="194ec-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="194ec-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="194ec-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="194ec-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="194ec-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="194ec-118">Scenario description</span></span>
<span data-ttu-id="194ec-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="194ec-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="194ec-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="194ec-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="194ec-121">Att lägga till Optimizely från galleriet</span><span class="sxs-lookup"><span data-stu-id="194ec-121">Adding Optimizely from the gallery</span></span>
2. <span data-ttu-id="194ec-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="194ec-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-optimizely-from-the-gallery"></a><span data-ttu-id="194ec-123">Att lägga till Optimizely från galleriet</span><span class="sxs-lookup"><span data-stu-id="194ec-123">Adding Optimizely from the gallery</span></span>
<span data-ttu-id="194ec-124">Du måste lägga till Optimizely från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Optimizely i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="194ec-124">To configure the integration of Optimizely into Azure AD, you need to add Optimizely from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="194ec-125">**Utför följande steg för att lägga till Optimizely från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="194ec-125">**To add Optimizely from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="194ec-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="194ec-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="194ec-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="194ec-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="194ec-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="194ec-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="194ec-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="194ec-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="194ec-133">I sökrutan skriver **Optimizely**.</span><span class="sxs-lookup"><span data-stu-id="194ec-133">In the search box, type **Optimizely**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_search.png)

5. <span data-ttu-id="194ec-135">Välj i resultatpanelen **Optimizely**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="194ec-135">In the results panel, select **Optimizely**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="194ec-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="194ec-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="194ec-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Optimizely baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="194ec-138">In this section, you configure and test Azure AD single sign-on with Optimizely based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="194ec-139">Azure AD måste du känna till användaren i Optimizely motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="194ec-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Optimizely is to a user in Azure AD.</span></span> <span data-ttu-id="194ec-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Optimizely upprättas.</span><span class="sxs-lookup"><span data-stu-id="194ec-140">In other words, a link relationship between an Azure AD user and the related user in Optimizely needs to be established.</span></span>

<span data-ttu-id="194ec-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Optimizely.</span><span class="sxs-lookup"><span data-stu-id="194ec-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Optimizely.</span></span>

<span data-ttu-id="194ec-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Optimizely, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="194ec-142">To configure and test Azure AD single sign-on with Optimizely, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="194ec-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="194ec-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="194ec-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="194ec-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="194ec-145">**[Skapa en testanvändare Optimizely](#creating-an-optimizely-test-user)**  – du har en motsvarighet för Britta Simon i Optimizely som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="194ec-145">**[Creating an Optimizely test user](#creating-an-optimizely-test-user)** - to have a counterpart of Britta Simon in Optimizely that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="194ec-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="194ec-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="194ec-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="194ec-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="194ec-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="194ec-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="194ec-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Optimizely program.</span><span class="sxs-lookup"><span data-stu-id="194ec-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Optimizely application.</span></span>

<span data-ttu-id="194ec-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Optimizely:**</span><span class="sxs-lookup"><span data-stu-id="194ec-150">**To configure Azure AD single sign-on with Optimizely, perform the following steps:**</span></span>

1. <span data-ttu-id="194ec-151">I Azure-portalen på den **Optimizely** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="194ec-151">In the Azure portal, on the **Optimizely** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="194ec-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="194ec-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_samlbase.png)

3. <span data-ttu-id="194ec-155">På den **Optimizely domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="194ec-155">On the **Optimizely Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_url.png)

    <span data-ttu-id="194ec-157">a.</span><span class="sxs-lookup"><span data-stu-id="194ec-157">a.</span></span> <span data-ttu-id="194ec-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://app.optimizely.net/<instance name>`</span><span class="sxs-lookup"><span data-stu-id="194ec-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://app.optimizely.net/<instance name>`</span></span>

    <span data-ttu-id="194ec-159">b.</span><span class="sxs-lookup"><span data-stu-id="194ec-159">b.</span></span> <span data-ttu-id="194ec-160">I den **identifierare** textruta Skriv en URL med följande mönster:`urn:auth0:optimizely:contoso`</span><span class="sxs-lookup"><span data-stu-id="194ec-160">In the **Identifier** textbox, type a URL using the following pattern:  `urn:auth0:optimizely:contoso`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="194ec-161">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="194ec-161">These values are not the real.</span></span> <span data-ttu-id="194ec-162">Du uppdaterar värdet med det faktiska inloggnings-URL och identifierare som beskrivs senare i självstudierna.</span><span class="sxs-lookup"><span data-stu-id="194ec-162">You will update the value with the actual Sign-on URL and Identifier, which is explained later in the tutorial.</span></span> 

4. <span data-ttu-id="194ec-163">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="194ec-163">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_certificate.png) 

5. <span data-ttu-id="194ec-165">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="194ec-165">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-optimizely-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="194ec-167">På den **Optimizely Configuration** klickar du på **konfigurera Optimizely** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="194ec-167">On the **Optimizely Configuration** section, click **Configure Optimizely** to open **Configure sign-on** window.</span></span> <span data-ttu-id="194ec-168">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="194ec-168">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_configure.png) 

7. <span data-ttu-id="194ec-170">Konfigurera enkel inloggning på **Optimizely** sida, kontakta din Kontoansvariga Optimizely och ange den hämtade **certifikat (Base64)**, och **SAML inloggning tjänst-URL för enkel**.</span><span class="sxs-lookup"><span data-stu-id="194ec-170">To configure single sign-on on **Optimizely** side, contact your Optimizely Account Manager and provide the downloaded **Certificate (Base64)**, and **SAML Single Sign-On Service URL**.</span></span> 

8. <span data-ttu-id="194ec-171">Som svar på din e-post ger Optimizely dig logga på URL: en (SP-initierad SSO) och identifierarvärdena (Service Provider enhets-ID).</span><span class="sxs-lookup"><span data-stu-id="194ec-171">In response to your email, Optimizely provides you with the Sign On URL (SP-initiated SSO) and the Identifier (Service Provider Entity ID) values.</span></span>

    <span data-ttu-id="194ec-172">a.</span><span class="sxs-lookup"><span data-stu-id="194ec-172">a.</span></span> <span data-ttu-id="194ec-173">Kopiera den **SP-initierad SSO URL** tillhandahållna av Optimizely och klistra in i den **logga URL** TextBox-kontroll i **Optimizely domän och URL: er** avsnitt på Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="194ec-173">Copy the **SP-initiated SSO URL** provided by Optimizely, and paste into the **Sign On URL** textbox in **Optimizely Domain and URLs** section on Azure portal</span></span> 

    <span data-ttu-id="194ec-174">b.</span><span class="sxs-lookup"><span data-stu-id="194ec-174">b.</span></span> <span data-ttu-id="194ec-175">Kopiera den **Service Provider enhets-ID** tillhandahållna av Optimizely och klistra in i den **identifierare** TextBox-kontroll i **Optimizely domän och URL: er** avsnitt på Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="194ec-175">Copy the **Service Provider Entity ID** provided by Optimizely, and paste into the **Identifier** textbox in **Optimizely Domain and URLs** section on Azure portal</span></span> 

9. <span data-ttu-id="194ec-176">I ett annat webbläsarfönster inloggning i tillämpningsprogrammet Optimizely.</span><span class="sxs-lookup"><span data-stu-id="194ec-176">In a different browser window, sign-on to your Optimizely application.</span></span>

10. <span data-ttu-id="194ec-177">Klicka på du kontonamn i övre högra hörnet och sedan **kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="194ec-177">Click you account name in the top right corner and then **Account Settings**.</span></span>
   
    ![Azure AD-Single Sign-On](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_09.png)

11. <span data-ttu-id="194ec-179">Markera kryssrutan på fliken konto **aktivera enkel inloggning** under enkel inloggning i den **översikt** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="194ec-179">In the Account tab, check the box **Enable SSO** under Single Sign On in the **Overview** section.</span></span>
   
    ![Azure AD-Single Sign-On](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_10.png)
    
12. <span data-ttu-id="194ec-181">Klicka på **spara**</span><span class="sxs-lookup"><span data-stu-id="194ec-181">Click **Save**</span></span>

> [!TIP]
> <span data-ttu-id="194ec-182">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="194ec-182">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="194ec-183">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="194ec-183">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="194ec-184">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="194ec-184">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="194ec-185">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="194ec-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="194ec-186">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="194ec-186">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="194ec-188">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="194ec-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="194ec-189">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="194ec-189">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="194ec-191">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="194ec-191">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="194ec-193">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="194ec-193">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="194ec-195">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="194ec-195">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="194ec-197">a.</span><span class="sxs-lookup"><span data-stu-id="194ec-197">a.</span></span> <span data-ttu-id="194ec-198">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="194ec-198">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="194ec-199">b.</span><span class="sxs-lookup"><span data-stu-id="194ec-199">b.</span></span> <span data-ttu-id="194ec-200">I den **användarnamn** textruta typ av **e-postadress** av Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="194ec-200">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="194ec-201">c.</span><span class="sxs-lookup"><span data-stu-id="194ec-201">c.</span></span> <span data-ttu-id="194ec-202">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="194ec-202">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="194ec-203">d.</span><span class="sxs-lookup"><span data-stu-id="194ec-203">d.</span></span> <span data-ttu-id="194ec-204">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="194ec-204">Click **Create**.</span></span>
 
### <a name="creating-an-optimizely-test-user"></a><span data-ttu-id="194ec-205">Skapa en testanvändare Optimizely</span><span class="sxs-lookup"><span data-stu-id="194ec-205">Creating an Optimizely test user</span></span>

<span data-ttu-id="194ec-206">I det här avsnittet skapar du en användare som kallas Britta Simon i Optimizely.</span><span class="sxs-lookup"><span data-stu-id="194ec-206">In this section, you create a user called Britta Simon in Optimizely.</span></span>

1. <span data-ttu-id="194ec-207">På startsidan, Välj **medarbetare** fliken.</span><span class="sxs-lookup"><span data-stu-id="194ec-207">On the home page, select **Collaborators** tab.</span></span>

2. <span data-ttu-id="194ec-208">Klicka på Lägg till ny deltagare i projektet, **ny deltagare**.</span><span class="sxs-lookup"><span data-stu-id="194ec-208">To add new collaborator to the project, click **New Collaborator**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_10.png)

3. <span data-ttu-id="194ec-210">Fyll i e-postadressen och tilldela dem en roll.</span><span class="sxs-lookup"><span data-stu-id="194ec-210">Fill in the email address and assign them a role.</span></span> <span data-ttu-id="194ec-211">Klicka på **bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="194ec-211">Click **Invite**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-optimizely-tutorial/create_aaduser_11.png)

4. <span data-ttu-id="194ec-213">De får ett e-inbjudan.</span><span class="sxs-lookup"><span data-stu-id="194ec-213">They receive an email invite.</span></span> <span data-ttu-id="194ec-214">Med den e-postadressen som de behöver logga in på Optimizely.</span><span class="sxs-lookup"><span data-stu-id="194ec-214">Using the email address, they have to log in to Optimizely.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="194ec-215">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="194ec-215">Assigning the Azure AD test user</span></span>

<span data-ttu-id="194ec-216">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Optimizely.</span><span class="sxs-lookup"><span data-stu-id="194ec-216">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Optimizely.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="194ec-218">**Om du vill tilldela Optimizely Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="194ec-218">**To assign Britta Simon to Optimizely, perform the following steps:**</span></span>

1. <span data-ttu-id="194ec-219">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="194ec-219">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="194ec-221">Välj i listan med program **Optimizely**.</span><span class="sxs-lookup"><span data-stu-id="194ec-221">In the applications list, select **Optimizely**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_app.png) 

3. <span data-ttu-id="194ec-223">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="194ec-223">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="194ec-225">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="194ec-225">Click **Add** button.</span></span> <span data-ttu-id="194ec-226">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="194ec-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="194ec-228">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="194ec-228">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="194ec-229">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="194ec-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="194ec-230">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="194ec-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="194ec-231">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="194ec-231">Testing single sign-on</span></span>

<span data-ttu-id="194ec-232">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="194ec-232">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="194ec-233">När du klickar på panelen Optimizely på åtkomstpanelen du bör få automatiskt loggat in på ditt Optimizely program.</span><span class="sxs-lookup"><span data-stu-id="194ec-233">When you click the Optimizely tile in the Access Panel, you should get automatically signed-on to your Optimizely application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="194ec-234">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="194ec-234">Additional resources</span></span>

* [<span data-ttu-id="194ec-235">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="194ec-235">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="194ec-236">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="194ec-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_203.png

