---
title: "Självstudier: Azure Active Directory-integrering med Halogen programvara | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Halogen programvaran och Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2ca2298d-9a0c-4f14-925c-fa23f2659d28
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: e09fa93038965e4880a23002bac6917ad2a077f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a><span data-ttu-id="339c5-103">Självstudier: Azure Active Directory-integrering med Halogen programvara</span><span class="sxs-lookup"><span data-stu-id="339c5-103">Tutorial: Azure Active Directory integration with Halogen Software</span></span>

<span data-ttu-id="339c5-104">I kursen får lära du att integrera Halogen programvara med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="339c5-104">In this tutorial, you learn how to integrate Halogen Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="339c5-105">Integrera Halogen programvara med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="339c5-105">Integrating Halogen Software with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="339c5-106">Du kan styra i Azure AD som har åtkomst till Halogen programvara</span><span class="sxs-lookup"><span data-stu-id="339c5-106">You can control in Azure AD who has access to Halogen Software</span></span>
- <span data-ttu-id="339c5-107">Du kan aktivera användarna att automatiskt hämta loggat in på Halogen programvara (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="339c5-107">You can enable your users to automatically get signed-on to Halogen Software (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="339c5-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="339c5-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="339c5-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="339c5-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="339c5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="339c5-110">Prerequisites</span></span>

<span data-ttu-id="339c5-111">För att konfigurera Azure AD-integrering med Halogen programvara, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="339c5-111">To configure Azure AD integration with Halogen Software, you need the following items:</span></span>

- <span data-ttu-id="339c5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="339c5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="339c5-113">En Halogen programvara enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="339c5-113">A Halogen Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="339c5-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="339c5-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="339c5-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="339c5-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="339c5-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="339c5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="339c5-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="339c5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="339c5-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="339c5-118">Scenario description</span></span>

<span data-ttu-id="339c5-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="339c5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="339c5-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="339c5-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="339c5-121">Lägga till Halogen programvara från galleriet</span><span class="sxs-lookup"><span data-stu-id="339c5-121">Adding Halogen Software from the gallery</span></span>
2. <span data-ttu-id="339c5-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="339c5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-halogen-software-from-the-gallery"></a><span data-ttu-id="339c5-123">Lägga till Halogen programvara från galleriet</span><span class="sxs-lookup"><span data-stu-id="339c5-123">Adding Halogen Software from the gallery</span></span>

<span data-ttu-id="339c5-124">Du måste lägga till Halogen programvara från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Halogen programvara i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="339c5-124">To configure the integration of Halogen Software into Azure AD, you need to add Halogen Software from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="339c5-125">**Utför följande steg för att lägga till Halogen programvara från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="339c5-125">**To add Halogen Software from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="339c5-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="339c5-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="339c5-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="339c5-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="339c5-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="339c5-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="339c5-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="339c5-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="339c5-133">I sökrutan skriver **Halogen programvara**.</span><span class="sxs-lookup"><span data-stu-id="339c5-133">In the search box, type **Halogen Software**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_search.png)

5. <span data-ttu-id="339c5-135">Välj i resultatpanelen **Halogen programvara**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="339c5-135">In the results panel, select **Halogen Software**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="339c5-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="339c5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="339c5-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Halogen programvara baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="339c5-138">In this section, you configure and test Azure AD single sign-on with Halogen Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="339c5-139">Azure AD måste du känna till motsvarande användaren i Halogen programvara till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="339c5-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Halogen Software is to a user in Azure AD.</span></span> <span data-ttu-id="339c5-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Halogen programvara upprättas.</span><span class="sxs-lookup"><span data-stu-id="339c5-140">In other words, a link relationship between an Azure AD user and the related user in Halogen Software needs to be established.</span></span>

<span data-ttu-id="339c5-141">I Halogen programvara, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="339c5-141">In Halogen Software, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="339c5-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Halogen programvara, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="339c5-142">To configure and test Azure AD single sign-on with Halogen Software, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="339c5-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="339c5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="339c5-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="339c5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="339c5-145">**[Skapa en testanvändare Halogen programvara](#creating-a-halogen-software-test-user)**  – du har en motsvarighet för Britta Simon Halogen programvara som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="339c5-145">**[Creating a Halogen Software test user](#creating-a-halogen-software-test-user)** - to have a counterpart of Britta Simon in Halogen Software that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="339c5-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="339c5-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="339c5-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="339c5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="339c5-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="339c5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="339c5-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i Halogen programmet.</span><span class="sxs-lookup"><span data-stu-id="339c5-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Halogen Software application.</span></span>

<span data-ttu-id="339c5-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Halogen programvara:**</span><span class="sxs-lookup"><span data-stu-id="339c5-150">**To configure Azure AD single sign-on with Halogen Software, perform the following steps:**</span></span>

1. <span data-ttu-id="339c5-151">I Azure-portalen på den **Halogen programvara** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="339c5-151">In the Azure portal, on the **Halogen Software** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="339c5-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="339c5-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_samlbase.png)

3. <span data-ttu-id="339c5-155">På den **Halogen programvara domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="339c5-155">On the **Halogen Software Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_url.png)

    <span data-ttu-id="339c5-157">a.</span><span class="sxs-lookup"><span data-stu-id="339c5-157">a.</span></span> <span data-ttu-id="339c5-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://global.hgncloud.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="339c5-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://global.hgncloud.com/<companyname>`</span></span>

    <span data-ttu-id="339c5-159">b.</span><span class="sxs-lookup"><span data-stu-id="339c5-159">b.</span></span> <span data-ttu-id="339c5-160">I den **identifierare** textruta Skriv en URL med följande mönster: `https://global.halogensoftware.com/<companyname>`,`https://global.hgncloud.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="339c5-160">In the **Identifier** textbox, type a URL using the following pattern: `https://global.halogensoftware.com/<companyname>`, `https://global.hgncloud.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="339c5-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="339c5-161">These values are not real.</span></span> <span data-ttu-id="339c5-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="339c5-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="339c5-163">Kontakta [Halogen Programvaruklienten supportteamet](https://support.halogensoftware.com/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="339c5-163">Contact [Halogen Software Client support team](https://support.halogensoftware.com/) to get these values.</span></span> 
 


4. <span data-ttu-id="339c5-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="339c5-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_certificate.png) 

5. <span data-ttu-id="339c5-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="339c5-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-halogen-software-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="339c5-168">I ett annat webbläsarfönster inloggning till din **Halogen programvara** program som administratör.</span><span class="sxs-lookup"><span data-stu-id="339c5-168">In a different browser window, sign-on to your **Halogen Software** application as an administrator.</span></span>

7. <span data-ttu-id="339c5-169">Klicka på den **alternativ** fliken.</span><span class="sxs-lookup"><span data-stu-id="339c5-169">Click the **Options** tab.</span></span> 
   
    ![Vad är Azure AD Connect?][12]

8. <span data-ttu-id="339c5-171">I det vänstra navigeringsfönstret klickar du på **SAML-konfiguration**.</span><span class="sxs-lookup"><span data-stu-id="339c5-171">In the left navigation pane, click **SAML Configuration**.</span></span> 
   
    ![Vad är Azure AD Connect?][13]

9. <span data-ttu-id="339c5-173">På den **SAML-konfiguration** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="339c5-173">On the **SAML Configuration** page, perform the following steps:</span></span> 

    ![Vad är Azure AD Connect?][14]

     <span data-ttu-id="339c5-175">a.</span><span class="sxs-lookup"><span data-stu-id="339c5-175">a.</span></span> <span data-ttu-id="339c5-176">Som **Unik identifierare**väljer **NameID**.</span><span class="sxs-lookup"><span data-stu-id="339c5-176">As **Unique Identifier**, select **NameID**.</span></span>

     <span data-ttu-id="339c5-177">b.</span><span class="sxs-lookup"><span data-stu-id="339c5-177">b.</span></span> <span data-ttu-id="339c5-178">Som **unika identifierare Maps till**väljer **användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="339c5-178">As **Unique Identifier Maps To**, select **Username**.</span></span>
  
     <span data-ttu-id="339c5-179">c.</span><span class="sxs-lookup"><span data-stu-id="339c5-179">c.</span></span> <span data-ttu-id="339c5-180">Om du vill överföra din hämtade metadatafil klickar du på **Bläddra** att välja filen och sedan **överför filen**.</span><span class="sxs-lookup"><span data-stu-id="339c5-180">To upload your downloaded metadata file, click **Browse** to select the file, and then **Upload File**.</span></span>
 
     <span data-ttu-id="339c5-181">d.</span><span class="sxs-lookup"><span data-stu-id="339c5-181">d.</span></span> <span data-ttu-id="339c5-182">Testa konfigurationen genom att klicka på **kör testa**.</span><span class="sxs-lookup"><span data-stu-id="339c5-182">To test the configuration, click **Run Test**.</span></span> 
    
    >[!NOTE]
    ><span data-ttu-id="339c5-183">Du måste vänta tills meddelandet ”*SAML-testet har slutförts. Stäng det här fönstret*”.</span><span class="sxs-lookup"><span data-stu-id="339c5-183">You need to wait for the message "*The SAML test is complete. Please close this window*".</span></span> <span data-ttu-id="339c5-184">Stäng öppna webbläsarfönstret.</span><span class="sxs-lookup"><span data-stu-id="339c5-184">Then, close the opened browser window.</span></span> <span data-ttu-id="339c5-185">Den **aktivera SAML** kryssrutan är endast aktiverad om testet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="339c5-185">The **Enable SAML** checkbox is only enabled if the test has been completed.</span></span> 
     
     <span data-ttu-id="339c5-186">e.</span><span class="sxs-lookup"><span data-stu-id="339c5-186">e.</span></span> <span data-ttu-id="339c5-187">Välj **aktivera SAML**.</span><span class="sxs-lookup"><span data-stu-id="339c5-187">Select **Enable SAML**.</span></span>
    
     <span data-ttu-id="339c5-188">f.</span><span class="sxs-lookup"><span data-stu-id="339c5-188">f.</span></span> <span data-ttu-id="339c5-189">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="339c5-189">Click **Save Changes**.</span></span> 

> [!TIP]
> <span data-ttu-id="339c5-190">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="339c5-190">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="339c5-191">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="339c5-191">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="339c5-192">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="339c5-192">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="339c5-193">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="339c5-193">Creating an Azure AD test user</span></span>

<span data-ttu-id="339c5-194">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="339c5-194">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="339c5-196">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="339c5-196">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="339c5-197">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="339c5-197">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="339c5-199">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="339c5-199">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="339c5-201">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="339c5-201">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="339c5-203">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="339c5-203">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="339c5-205">a.</span><span class="sxs-lookup"><span data-stu-id="339c5-205">a.</span></span> <span data-ttu-id="339c5-206">I den **namn** textruta typnamn som **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="339c5-206">In the **Name** textbox, type name as **BrittaSimon**.</span></span>

    <span data-ttu-id="339c5-207">b.</span><span class="sxs-lookup"><span data-stu-id="339c5-207">b.</span></span> <span data-ttu-id="339c5-208">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="339c5-208">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="339c5-209">c.</span><span class="sxs-lookup"><span data-stu-id="339c5-209">c.</span></span> <span data-ttu-id="339c5-210">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="339c5-210">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="339c5-211">d.</span><span class="sxs-lookup"><span data-stu-id="339c5-211">d.</span></span> <span data-ttu-id="339c5-212">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="339c5-212">Click **Create**.</span></span>
 
### <a name="creating-a-halogen-software-test-user"></a><span data-ttu-id="339c5-213">Skapa en testanvändare Halogen programvara</span><span class="sxs-lookup"><span data-stu-id="339c5-213">Creating a Halogen Software test user</span></span>

<span data-ttu-id="339c5-214">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon Halogen programvaran.</span><span class="sxs-lookup"><span data-stu-id="339c5-214">The objective of this section is to create a user called Britta Simon in Halogen Software.</span></span>

<span data-ttu-id="339c5-215">**Utför följande steg för att skapa en användare som kallas Britta Simon Halogen programvaran:**</span><span class="sxs-lookup"><span data-stu-id="339c5-215">**To create a user called Britta Simon in Halogen Software, perform the following steps:**</span></span>

1. <span data-ttu-id="339c5-216">Logga in på ditt **Halogen programvara** program som administratör.</span><span class="sxs-lookup"><span data-stu-id="339c5-216">Sign on to your **Halogen Software** application as an administrator.</span></span>

2. <span data-ttu-id="339c5-217">Klicka på den **användaren Center** fliken och klicka sedan på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="339c5-217">Click the **User Center** tab, and then click **Create User**.</span></span>
   
    ![Vad är Azure AD Connect?][300]  

3. <span data-ttu-id="339c5-219">På den **ny användare** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="339c5-219">On the **New User** dialog page, perform the following steps:</span></span>
   
    ![Vad är Azure AD Connect?][301]

    <span data-ttu-id="339c5-221">a.</span><span class="sxs-lookup"><span data-stu-id="339c5-221">a.</span></span> <span data-ttu-id="339c5-222">I den **Förnamn** textruta första typnamnet för användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="339c5-222">In the **First Name** textbox, type first name of the user like **Britta**.</span></span>
    
    <span data-ttu-id="339c5-223">b.</span><span class="sxs-lookup"><span data-stu-id="339c5-223">b.</span></span> <span data-ttu-id="339c5-224">I den **efternamn** textruta Skriv Efternamn för användaren som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="339c5-224">In the **Last Name** textbox, type last name of the user like **Simon**.</span></span> 

    <span data-ttu-id="339c5-225">c.</span><span class="sxs-lookup"><span data-stu-id="339c5-225">c.</span></span> <span data-ttu-id="339c5-226">I den **användarnamn** textruta typen **Britta Simon**, användarnamnet som Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="339c5-226">In the **Username** textbox, type **Britta Simon**, the user name as in the Azure portal.</span></span>

    <span data-ttu-id="339c5-227">d.</span><span class="sxs-lookup"><span data-stu-id="339c5-227">d.</span></span> <span data-ttu-id="339c5-228">I den **lösenord** textruta, ange ett lösenord för Britta.</span><span class="sxs-lookup"><span data-stu-id="339c5-228">In the **Password** textbox, type a password for Britta.</span></span>
    
    <span data-ttu-id="339c5-229">e.</span><span class="sxs-lookup"><span data-stu-id="339c5-229">e.</span></span> <span data-ttu-id="339c5-230">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="339c5-230">Click **Save**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="339c5-231">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="339c5-231">Assigning the Azure AD test user</span></span>

<span data-ttu-id="339c5-232">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Halogen programvara.</span><span class="sxs-lookup"><span data-stu-id="339c5-232">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Halogen Software.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="339c5-234">**Om du vill tilldela Halogen programvara Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="339c5-234">**To assign Britta Simon to Halogen Software, perform the following steps:**</span></span>

1. <span data-ttu-id="339c5-235">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="339c5-235">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="339c5-237">Välj i listan med program **Halogen programvara**.</span><span class="sxs-lookup"><span data-stu-id="339c5-237">In the applications list, select **Halogen Software**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_app.png) 

3. <span data-ttu-id="339c5-239">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="339c5-239">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="339c5-241">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="339c5-241">Click **Add** button.</span></span> <span data-ttu-id="339c5-242">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="339c5-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="339c5-244">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="339c5-244">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="339c5-245">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="339c5-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="339c5-246">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="339c5-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="339c5-247">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="339c5-247">Testing single sign-on</span></span>

<span data-ttu-id="339c5-248">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="339c5-248">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="339c5-249">När du klickar på panelen Halogen programvara på åtkomstpanelen du bör få automatiskt loggat in på Halogen programmet.</span><span class="sxs-lookup"><span data-stu-id="339c5-249">When you click the Halogen Software tile in the Access Panel, you should get automatically signed-on to your Halogen Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="339c5-250">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="339c5-250">Additional resources</span></span>

* [<span data-ttu-id="339c5-251">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="339c5-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="339c5-252">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="339c5-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_04.png

[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png

[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png

[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png

[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_203.png

[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png

[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png
