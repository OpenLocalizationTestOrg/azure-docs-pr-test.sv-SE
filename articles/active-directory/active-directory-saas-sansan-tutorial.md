---
title: "Självstudier: Azure Active Directory-integrering med Sansan | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Sansan."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f653a0f2-c44a-4670-b936-68c136b578ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: e1a9653d5feea910308cefabdbdfe3a6af44bbe4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sansan"></a><span data-ttu-id="bbd24-103">Självstudier: Azure Active Directory-integrering med Sansan</span><span class="sxs-lookup"><span data-stu-id="bbd24-103">Tutorial: Azure Active Directory integration with Sansan</span></span>

<span data-ttu-id="bbd24-104">I kursen får lära du att integrera Sansan med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="bbd24-104">In this tutorial, you learn how to integrate Sansan with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bbd24-105">Integrera Sansan med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="bbd24-105">Integrating Sansan with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="bbd24-106">Du kan styra i Azure AD som har åtkomst till Sansan</span><span class="sxs-lookup"><span data-stu-id="bbd24-106">You can control in Azure AD who has access to Sansan</span></span>
- <span data-ttu-id="bbd24-107">Du kan aktivera användarna att automatiskt hämta loggat in på Sansan (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="bbd24-107">You can enable your users to automatically get signed-on to Sansan (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bbd24-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bbd24-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="bbd24-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bbd24-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bbd24-110">Krav</span><span class="sxs-lookup"><span data-stu-id="bbd24-110">Prerequisites</span></span>

<span data-ttu-id="bbd24-111">För att konfigurera Azure AD-integrering med Sansan, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="bbd24-111">To configure Azure AD integration with Sansan, you need the following items:</span></span>

- <span data-ttu-id="bbd24-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="bbd24-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bbd24-113">En Sansan enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="bbd24-113">A Sansan single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bbd24-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="bbd24-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bbd24-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="bbd24-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bbd24-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="bbd24-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bbd24-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bbd24-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bbd24-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="bbd24-118">Scenario description</span></span>
<span data-ttu-id="bbd24-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="bbd24-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bbd24-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="bbd24-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bbd24-121">Att lägga till Sansan från galleriet</span><span class="sxs-lookup"><span data-stu-id="bbd24-121">Adding Sansan from the gallery</span></span>
2. <span data-ttu-id="bbd24-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bbd24-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sansan-from-the-gallery"></a><span data-ttu-id="bbd24-123">Att lägga till Sansan från galleriet</span><span class="sxs-lookup"><span data-stu-id="bbd24-123">Adding Sansan from the gallery</span></span>
<span data-ttu-id="bbd24-124">Du måste lägga till Sansan från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Sansan i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bbd24-124">To configure the integration of Sansan into Azure AD, you need to add Sansan from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="bbd24-125">**Utför följande steg för att lägga till Sansan från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="bbd24-125">**To add Sansan from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="bbd24-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="bbd24-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bbd24-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="bbd24-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="bbd24-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="bbd24-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="bbd24-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bbd24-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="bbd24-133">I sökrutan skriver **Sansan**.</span><span class="sxs-lookup"><span data-stu-id="bbd24-133">In the search box, type **Sansan**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_search.png)

5. <span data-ttu-id="bbd24-135">Välj i resultatpanelen **Sansan**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="bbd24-135">In the results panel, select **Sansan**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bbd24-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bbd24-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bbd24-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Sansan baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="bbd24-138">In this section, you configure and test Azure AD single sign-on with Sansan based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="bbd24-139">Azure AD måste du känna till användaren i Sansan motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="bbd24-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Sansan is to a user in Azure AD.</span></span> <span data-ttu-id="bbd24-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Sansan upprättas.</span><span class="sxs-lookup"><span data-stu-id="bbd24-140">In other words, a link relationship between an Azure AD user and the related user in Sansan needs to be established.</span></span>

<span data-ttu-id="bbd24-141">I Sansan, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="bbd24-141">In Sansan, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="bbd24-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Sansan, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="bbd24-142">To configure and test Azure AD single sign-on with Sansan, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="bbd24-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="bbd24-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="bbd24-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bbd24-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bbd24-145">**[Skapa en testanvändare Sansan](#creating-a-sansan-test-user)**  – du har en motsvarighet för Britta Simon i Sansan som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="bbd24-145">**[Creating a Sansan test user](#creating-a-sansan-test-user)** - to have a counterpart of Britta Simon in Sansan that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="bbd24-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bbd24-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bbd24-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="bbd24-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bbd24-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bbd24-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bbd24-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Sansan program.</span><span class="sxs-lookup"><span data-stu-id="bbd24-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Sansan application.</span></span>

<span data-ttu-id="bbd24-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Sansan:**</span><span class="sxs-lookup"><span data-stu-id="bbd24-150">**To configure Azure AD single sign-on with Sansan, perform the following steps:**</span></span>

1. <span data-ttu-id="bbd24-151">I Azure-portalen på den **Sansan** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="bbd24-151">In the Azure portal, on the **Sansan** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="bbd24-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bbd24-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_samlbase.png)

3. <span data-ttu-id="bbd24-155">På den **Sansan domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="bbd24-155">On the **Sansan Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_url.png)

    <span data-ttu-id="bbd24-157">a.</span><span class="sxs-lookup"><span data-stu-id="bbd24-157">a.</span></span> <span data-ttu-id="bbd24-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="bbd24-158">In the **Sign-on URL** textbox, type a URL using the following patterns:</span></span> 
    
    | <span data-ttu-id="bbd24-159">Miljö</span><span class="sxs-lookup"><span data-stu-id="bbd24-159">Environment</span></span> | <span data-ttu-id="bbd24-160">URL: EN</span><span class="sxs-lookup"><span data-stu-id="bbd24-160">URL</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="bbd24-161">Dator</span><span class="sxs-lookup"><span data-stu-id="bbd24-161">PC web</span></span> |`https://ap.sansan.com/v/saml2/<company name>/acs` |
    | <span data-ttu-id="bbd24-162">Inbyggda mobila appen</span><span class="sxs-lookup"><span data-stu-id="bbd24-162">Native Mobile app</span></span> |`https://internal.api.sansan.com/saml2/<company name>/acs` |
    | <span data-ttu-id="bbd24-163">Inställningar för mobila webbläsare</span><span class="sxs-lookup"><span data-stu-id="bbd24-163">Mobile browser settings</span></span> |`https://ap.sansan.com/s/saml2/<company name>/acs` |  

    <span data-ttu-id="bbd24-164">b.</span><span class="sxs-lookup"><span data-stu-id="bbd24-164">b.</span></span> <span data-ttu-id="bbd24-165">I den **identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="bbd24-165">In the **Identifier** textbox, type a URL using the following patterns:</span></span>
    | <span data-ttu-id="bbd24-166">Miljö</span><span class="sxs-lookup"><span data-stu-id="bbd24-166">Environment</span></span>             | <span data-ttu-id="bbd24-167">URL: EN</span><span class="sxs-lookup"><span data-stu-id="bbd24-167">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="bbd24-168">Dator</span><span class="sxs-lookup"><span data-stu-id="bbd24-168">PC web</span></span>                  | `https://ap.sansan.com/v/saml2/<company name>`|
    | <span data-ttu-id="bbd24-169">Inbyggda mobila appen</span><span class="sxs-lookup"><span data-stu-id="bbd24-169">Native Mobile app</span></span>       | `https://internal.api.sansan.com/saml2/<company name>` |
    | <span data-ttu-id="bbd24-170">Inställningar för mobila webbläsare</span><span class="sxs-lookup"><span data-stu-id="bbd24-170">Mobile browser settings</span></span> | `https://ap.sansan.com/s/saml2/<company name>` |

    > [!NOTE] 
    > <span data-ttu-id="bbd24-171">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="bbd24-171">These values are not real.</span></span> <span data-ttu-id="bbd24-172">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="bbd24-172">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="bbd24-173">Kontakta [Sansan klienten supportteamet](https://www.sansan.com/form/contact) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="bbd24-173">Contact [Sansan Client support team](https://www.sansan.com/form/contact) to get these values.</span></span> 

4. <span data-ttu-id="bbd24-174">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="bbd24-174">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_certificate.png) 

5. <span data-ttu-id="bbd24-176">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="bbd24-176">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sansan-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bbd24-178">På den **Sansan Configuration** klickar du på **konfigurera Sansan** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="bbd24-178">On the **Sansan Configuration** section, click **Configure Sansan** to open **Configure sign-on** window.</span></span> <span data-ttu-id="bbd24-179">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="bbd24-179">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_configure.png) 

7. <span data-ttu-id="bbd24-181">Konfigurera enkel inloggning på **Sansan** sida, måste du skicka den hämtade **certifikat**, **Sign-Out URL**, **SAML enhets-ID**, och **SAML enkel inloggning Tjänstwebbadress** till [Sansan supportteamet](https://www.sansan.com/form/contact).</span><span class="sxs-lookup"><span data-stu-id="bbd24-181">To configure single sign-on on **Sansan** side, you need to send the downloaded **Certificate**, **Sign-Out URL**, **SAML Entity ID**, and **SAML Single Sign-On Service URL** to [Sansan support team](https://www.sansan.com/form/contact).</span></span> <span data-ttu-id="bbd24-182">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="bbd24-182">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

>[!NOTE]
><span data-ttu-id="bbd24-183">PC webbläsarinställningen fungerar även för mobila appar och mobila webbläsare tillsammans med dator.</span><span class="sxs-lookup"><span data-stu-id="bbd24-183">PC browser setting also work for Mobile app and Mobile browser along with PC web.</span></span>  

> [!TIP]
> <span data-ttu-id="bbd24-184">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="bbd24-184">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="bbd24-185">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="bbd24-185">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="bbd24-186">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bbd24-186">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bbd24-187">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="bbd24-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="bbd24-188">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bbd24-188">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="bbd24-190">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="bbd24-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="bbd24-191">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="bbd24-191">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bbd24-193">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="bbd24-193">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bbd24-195">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bbd24-195">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bbd24-197">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="bbd24-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bbd24-199">a.</span><span class="sxs-lookup"><span data-stu-id="bbd24-199">a.</span></span> <span data-ttu-id="bbd24-200">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bbd24-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bbd24-201">b.</span><span class="sxs-lookup"><span data-stu-id="bbd24-201">b.</span></span> <span data-ttu-id="bbd24-202">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bbd24-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bbd24-203">c.</span><span class="sxs-lookup"><span data-stu-id="bbd24-203">c.</span></span> <span data-ttu-id="bbd24-204">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="bbd24-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="bbd24-205">d.</span><span class="sxs-lookup"><span data-stu-id="bbd24-205">d.</span></span> <span data-ttu-id="bbd24-206">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="bbd24-206">Click **Create**.</span></span>
 
### <a name="creating-a-sansan-test-user"></a><span data-ttu-id="bbd24-207">Skapa en testanvändare Sansan</span><span class="sxs-lookup"><span data-stu-id="bbd24-207">Creating a Sansan test user</span></span>

<span data-ttu-id="bbd24-208">I det här avsnittet skapar du en användare som kallas Britta Simon i SanSan.</span><span class="sxs-lookup"><span data-stu-id="bbd24-208">In this section, you create a user called Britta Simon in SanSan.</span></span> <span data-ttu-id="bbd24-209">SanSan programmet måste användaren som ska etableras i programmet innan du utför enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bbd24-209">SanSan application needs the user to be provisioned in the application before doing SSO.</span></span> 

>[!NOTE]
><span data-ttu-id="bbd24-210">Om du behöver skapa en användare manuellt eller batch-användare, måste du kontakta den [Sansan supportteamet](https://www.sansan.com/form/contact).</span><span class="sxs-lookup"><span data-stu-id="bbd24-210">If you need to create a user manually or batch of users, you need to contact the [Sansan support team](https://www.sansan.com/form/contact).</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="bbd24-211">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="bbd24-211">Assigning the Azure AD test user</span></span>

<span data-ttu-id="bbd24-212">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Sansan.</span><span class="sxs-lookup"><span data-stu-id="bbd24-212">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Sansan.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="bbd24-214">**Om du vill tilldela Sansan Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="bbd24-214">**To assign Britta Simon to Sansan, perform the following steps:**</span></span>

1. <span data-ttu-id="bbd24-215">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="bbd24-215">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="bbd24-217">Välj i listan med program **Sansan**.</span><span class="sxs-lookup"><span data-stu-id="bbd24-217">In the applications list, select **Sansan**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_app.png) 

3. <span data-ttu-id="bbd24-219">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="bbd24-219">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="bbd24-221">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="bbd24-221">Click **Add** button.</span></span> <span data-ttu-id="bbd24-222">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bbd24-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="bbd24-224">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="bbd24-224">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="bbd24-225">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bbd24-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bbd24-226">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bbd24-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bbd24-227">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bbd24-227">Testing single sign-on</span></span>

<span data-ttu-id="bbd24-228">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="bbd24-228">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="bbd24-229">När du klickar på panelen Sansan på åtkomstpanelen du bör få automatiskt loggat in på ditt Sansan program.</span><span class="sxs-lookup"><span data-stu-id="bbd24-229">When you click the Sansan tile in the Access Panel, you should get automatically signed-on to your Sansan application.</span></span>
<span data-ttu-id="bbd24-230">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bbd24-230">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bbd24-231">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="bbd24-231">Additional resources</span></span>

* [<span data-ttu-id="bbd24-232">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bbd24-232">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bbd24-233">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bbd24-233">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_203.png

