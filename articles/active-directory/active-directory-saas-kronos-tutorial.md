---
title: "Självstudier: Azure Active Directory-integrering med Kronos | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Kronos."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e28d6191-c375-43c6-b2df-22daa88d9939
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: eb61ec0a7d3e992a285b1af3d4a7fbe1feb8d991
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kronos"></a><span data-ttu-id="3fd56-103">Självstudier: Azure Active Directory-integrering med Kronos</span><span class="sxs-lookup"><span data-stu-id="3fd56-103">Tutorial: Azure Active Directory integration with Kronos</span></span>

<span data-ttu-id="3fd56-104">I kursen får lära du att integrera Kronos med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3fd56-104">In this tutorial, you learn how to integrate Kronos with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3fd56-105">Integrera Kronos med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3fd56-105">Integrating Kronos with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3fd56-106">Du kan styra i Azure AD som har åtkomst till Kronos</span><span class="sxs-lookup"><span data-stu-id="3fd56-106">You can control in Azure AD who has access to Kronos</span></span>
- <span data-ttu-id="3fd56-107">Du kan aktivera användarna att automatiskt hämta loggat in på Kronos (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="3fd56-107">You can enable your users to automatically get signed-on to Kronos (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3fd56-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3fd56-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3fd56-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3fd56-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3fd56-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3fd56-110">Prerequisites</span></span>

<span data-ttu-id="3fd56-111">För att konfigurera Azure AD-integrering med Kronos, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="3fd56-111">To configure Azure AD integration with Kronos, you need the following items:</span></span>

- <span data-ttu-id="3fd56-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3fd56-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3fd56-113">En **Kronos arbetsstyrka centrala** SSO aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="3fd56-113">A **Kronos Workforce Central** SSO enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3fd56-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="3fd56-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3fd56-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="3fd56-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3fd56-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="3fd56-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3fd56-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3fd56-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3fd56-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="3fd56-118">Scenario description</span></span>
<span data-ttu-id="3fd56-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="3fd56-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3fd56-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="3fd56-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3fd56-121">Att lägga till Kronos från galleriet</span><span class="sxs-lookup"><span data-stu-id="3fd56-121">Adding Kronos from the gallery</span></span>
2. <span data-ttu-id="3fd56-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3fd56-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kronos-from-the-gallery"></a><span data-ttu-id="3fd56-123">Att lägga till Kronos från galleriet</span><span class="sxs-lookup"><span data-stu-id="3fd56-123">Adding Kronos from the gallery</span></span>
<span data-ttu-id="3fd56-124">Du måste lägga till Kronos från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Kronos i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3fd56-124">To configure the integration of Kronos into Azure AD, you need to add Kronos from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3fd56-125">**Utför följande steg för att lägga till Kronos från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="3fd56-125">**To add Kronos from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3fd56-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3fd56-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3fd56-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3fd56-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3fd56-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3fd56-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="3fd56-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3fd56-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="3fd56-133">I sökrutan skriver **Kronos**.</span><span class="sxs-lookup"><span data-stu-id="3fd56-133">In the search box, type **Kronos**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_search.png)

5. <span data-ttu-id="3fd56-135">Välj i resultatpanelen **Kronos**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="3fd56-135">In the results panel, select **Kronos**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3fd56-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3fd56-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3fd56-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Kronos baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="3fd56-138">In this section, you configure and test Azure AD single sign-on with Kronos based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3fd56-139">Azure AD måste du känna till användaren i Kronos motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="3fd56-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Kronos is to a user in Azure AD.</span></span> <span data-ttu-id="3fd56-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Kronos upprättas.</span><span class="sxs-lookup"><span data-stu-id="3fd56-140">In other words, a link relationship between an Azure AD user and the related user in Kronos needs to be established.</span></span>

<span data-ttu-id="3fd56-141">I Kronos, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="3fd56-141">In Kronos, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3fd56-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Kronos, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="3fd56-142">To configure and test Azure AD single sign-on with Kronos, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3fd56-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3fd56-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3fd56-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3fd56-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3fd56-145">**[Skapa en testanvändare Kronos](#creating-a-kronos-test-user)**  – du har en motsvarighet för Britta Simon i Kronos som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="3fd56-145">**[Creating a Kronos test user](#creating-a-kronos-test-user)** - to have a counterpart of Britta Simon in Kronos that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3fd56-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3fd56-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3fd56-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3fd56-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3fd56-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3fd56-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3fd56-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Kronos program.</span><span class="sxs-lookup"><span data-stu-id="3fd56-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kronos application.</span></span>

<span data-ttu-id="3fd56-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Kronos:**</span><span class="sxs-lookup"><span data-stu-id="3fd56-150">**To configure Azure AD single sign-on with Kronos, perform the following steps:**</span></span>

1. <span data-ttu-id="3fd56-151">I Azure-portalen på den **Kronos** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3fd56-151">In the Azure portal, on the **Kronos** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="3fd56-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3fd56-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_samlbase.png)

3. <span data-ttu-id="3fd56-155">På den **Kronos domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3fd56-155">On the **Kronos Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_url.png)

    <span data-ttu-id="3fd56-157">a.</span><span class="sxs-lookup"><span data-stu-id="3fd56-157">a.</span></span> <span data-ttu-id="3fd56-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<company name>.kronos.net/`</span><span class="sxs-lookup"><span data-stu-id="3fd56-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.kronos.net/`</span></span>

    <span data-ttu-id="3fd56-159">b.</span><span class="sxs-lookup"><span data-stu-id="3fd56-159">b.</span></span> <span data-ttu-id="3fd56-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<company name>.kronos.net/wfc/navigator/logonWithUID`</span><span class="sxs-lookup"><span data-stu-id="3fd56-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.kronos.net/wfc/navigator/logonWithUID`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3fd56-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="3fd56-161">These values are not real.</span></span> <span data-ttu-id="3fd56-162">Uppdatera dessa värden med den faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="3fd56-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="3fd56-163">Kontakta [Kronos supportteam](https://www.kronos.in/contact/en-in/form) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3fd56-163">Contact [Kronos support team](https://www.kronos.in/contact/en-in/form) to get these values.</span></span>
 
4. <span data-ttu-id="3fd56-164">Tillämpningsprogrammet Kronos förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="3fd56-164">Your Kronos application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="3fd56-165">Arbeta med [Kronos supportteam](https://www.kronos.in/contact/en-in/form) först för att identifiera rätt användar-ID, som är mappad till programmet.</span><span class="sxs-lookup"><span data-stu-id="3fd56-165">Work with [Kronos support team](https://www.kronos.in/contact/en-in/form) first to identify the correct user identifier, which is mapped into the application.</span></span> <span data-ttu-id="3fd56-166">Ta även information om attributet, som de vill använda för att mappningen.</span><span class="sxs-lookup"><span data-stu-id="3fd56-166">Also please take the guidance about the attribute, which they want to use for this mapping.</span></span>
 
     <span data-ttu-id="3fd56-167">Microsoft rekommenderar att du använder den **”NameIdentifier”** attribut som användaridentifierare.</span><span class="sxs-lookup"><span data-stu-id="3fd56-167">Microsoft recommends using the **"NameIdentifier"** attribute as user identifier.</span></span> <span data-ttu-id="3fd56-168">Du kan hantera värden för attributen från den **”användarattribut”** avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="3fd56-168">You can manage the values of these attributes from the **"User Attributes"** section on application integration page.</span></span>
     
     <span data-ttu-id="3fd56-169">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="3fd56-169">The following screenshot shows an example for this.</span></span> <span data-ttu-id="3fd56-170">Vi har här mappat den **användar-ID (nameid)** med **ExtractMailPrefix()** funktion **user.userprincipalname**.</span><span class="sxs-lookup"><span data-stu-id="3fd56-170">Here we have mapped the **User Identifier (nameid)** with **ExtractMailPrefix()** function of **user.userprincipalname**.</span></span> <span data-ttu-id="3fd56-171">Detta ger prefixvärdet av e-post för den användare som är den unika användar-ID.</span><span class="sxs-lookup"><span data-stu-id="3fd56-171">This gives the prefix value of email of the user which is the unique User ID.</span></span> <span data-ttu-id="3fd56-172">Detta skickas till programmet Kronos i varje lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="3fd56-172">This is sent to the Kronos application in every successful response.</span></span> 
     
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_attribute.png)

5. <span data-ttu-id="3fd56-174">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="3fd56-174">In the **User Attributes** section on the **Single sign-on** dialog:</span></span>

    <span data-ttu-id="3fd56-175">a.</span><span class="sxs-lookup"><span data-stu-id="3fd56-175">a.</span></span> <span data-ttu-id="3fd56-176">Välj i listrutan användar-ID **ExtractMailPrefix**.</span><span class="sxs-lookup"><span data-stu-id="3fd56-176">In the User Identifier dropdown list, select **ExtractMailPrefix**.</span></span>

    <span data-ttu-id="3fd56-177">b.</span><span class="sxs-lookup"><span data-stu-id="3fd56-177">b.</span></span> <span data-ttu-id="3fd56-178">I den **e** listrutan, Välj **user.userprincipalname**.</span><span class="sxs-lookup"><span data-stu-id="3fd56-178">In the **Mail** dropdown list, select **user.userprincipalname**.</span></span>

6. <span data-ttu-id="3fd56-179">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="3fd56-179">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_certificate.png) 

7. <span data-ttu-id="3fd56-181">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="3fd56-181">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kronos-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="3fd56-183">Konfigurera enkel inloggning på **Kronos** sida, måste du skicka den hämtade **XML-Metadata för** till [Kronos supportteam](https://www.kronos.in/contact/en-in/form).</span><span class="sxs-lookup"><span data-stu-id="3fd56-183">To configure single sign-on on **Kronos** side, you need to send the downloaded **Metadata XML** to [Kronos support team](https://www.kronos.in/contact/en-in/form).</span></span> 

> [!TIP]
> <span data-ttu-id="3fd56-184">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="3fd56-184">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3fd56-185">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="3fd56-185">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3fd56-186">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3fd56-186">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3fd56-187">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="3fd56-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="3fd56-188">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3fd56-188">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="3fd56-190">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="3fd56-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3fd56-191">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3fd56-191">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3fd56-193">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="3fd56-193">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3fd56-195">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3fd56-195">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3fd56-197">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3fd56-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3fd56-199">a.</span><span class="sxs-lookup"><span data-stu-id="3fd56-199">a.</span></span> <span data-ttu-id="3fd56-200">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3fd56-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3fd56-201">b.</span><span class="sxs-lookup"><span data-stu-id="3fd56-201">b.</span></span> <span data-ttu-id="3fd56-202">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3fd56-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3fd56-203">c.</span><span class="sxs-lookup"><span data-stu-id="3fd56-203">c.</span></span> <span data-ttu-id="3fd56-204">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="3fd56-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3fd56-205">d.</span><span class="sxs-lookup"><span data-stu-id="3fd56-205">d.</span></span> <span data-ttu-id="3fd56-206">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3fd56-206">Click **Create**.</span></span>
 
### <a name="creating-a-kronos-test-user"></a><span data-ttu-id="3fd56-207">Skapa en testanvändare Kronos</span><span class="sxs-lookup"><span data-stu-id="3fd56-207">Creating a Kronos test user</span></span>

<span data-ttu-id="3fd56-208">I det här avsnittet skapar du en användare som kallas Britta Simon i Kronos.</span><span class="sxs-lookup"><span data-stu-id="3fd56-208">In this section, you create a user called Britta Simon in Kronos.</span></span> <span data-ttu-id="3fd56-209">Kronos programmet måste alla användare som ska etableras i programmet innan du utför enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3fd56-209">Kronos application needs all the users to be provisioned in the application before doing SSO.</span></span> 

<span data-ttu-id="3fd56-210">Arbeta med den [Kronos supportteam](https://www.kronos.in/contact/en-in/form) att etablera dessa användare till programmet.</span><span class="sxs-lookup"><span data-stu-id="3fd56-210">Work with the [Kronos support team](https://www.kronos.in/contact/en-in/form) to provision all these users into the application.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3fd56-211">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="3fd56-211">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3fd56-212">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Kronos.</span><span class="sxs-lookup"><span data-stu-id="3fd56-212">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kronos.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="3fd56-214">**Om du vill tilldela Kronos Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3fd56-214">**To assign Britta Simon to Kronos, perform the following steps:**</span></span>

1. <span data-ttu-id="3fd56-215">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3fd56-215">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="3fd56-217">Välj i listan med program **Kronos**.</span><span class="sxs-lookup"><span data-stu-id="3fd56-217">In the applications list, select **Kronos**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_app.png) 

3. <span data-ttu-id="3fd56-219">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3fd56-219">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="3fd56-221">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="3fd56-221">Click **Add** button.</span></span> <span data-ttu-id="3fd56-222">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3fd56-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="3fd56-224">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="3fd56-224">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3fd56-225">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3fd56-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3fd56-226">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3fd56-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3fd56-227">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3fd56-227">Testing single sign-on</span></span>

<span data-ttu-id="3fd56-228">I det här avsnittet kan du testa din Azure AD SSO-konfiguration med hjälp av åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="3fd56-228">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="3fd56-229">När du klickar på panelen Kronos på åtkomstpanelen du bör få automatiskt loggat in på ditt Kronos program.</span><span class="sxs-lookup"><span data-stu-id="3fd56-229">When you click the Kronos tile in the Access Panel, you should get automatically signed-on to your Kronos application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3fd56-230">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3fd56-230">Additional resources</span></span>

* [<span data-ttu-id="3fd56-231">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3fd56-231">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3fd56-232">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3fd56-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_203.png

