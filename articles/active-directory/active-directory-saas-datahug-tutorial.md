---
title: "Självstudier: Azure Active Directory-integrering med Datahug | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Datahug."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5c0dc1ea-7ff4-4554-b60b-0f2fa9f5abaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: jeedes
ms.openlocfilehash: ec431dd5ccfa53e4b975e46da247704dd1e15c2c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-datahug"></a><span data-ttu-id="e0786-103">Självstudier: Azure Active Directory-integrering med Datahug</span><span class="sxs-lookup"><span data-stu-id="e0786-103">Tutorial: Azure Active Directory integration with Datahug</span></span>

<span data-ttu-id="e0786-104">I kursen får lära du att integrera Datahug med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e0786-104">In this tutorial, you learn how to integrate Datahug with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e0786-105">Integrera Datahug med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e0786-105">Integrating Datahug with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e0786-106">Du kan styra i Azure AD som har åtkomst till Datahug</span><span class="sxs-lookup"><span data-stu-id="e0786-106">You can control in Azure AD who has access to Datahug</span></span>
- <span data-ttu-id="e0786-107">Du kan aktivera användarna att automatiskt hämta loggat in på Datahug (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e0786-107">You can enable your users to automatically get signed-on to Datahug (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e0786-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e0786-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e0786-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns i.</span><span class="sxs-lookup"><span data-stu-id="e0786-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="e0786-110">[Vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e0786-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e0786-111">Krav</span><span class="sxs-lookup"><span data-stu-id="e0786-111">Prerequisites</span></span>

<span data-ttu-id="e0786-112">För att konfigurera Azure AD-integrering med Datahug, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="e0786-112">To configure Azure AD integration with Datahug, you need the following items:</span></span>

- <span data-ttu-id="e0786-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e0786-113">An Azure AD subscription</span></span>
- <span data-ttu-id="e0786-114">En Datahug enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="e0786-114">A Datahug single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e0786-115">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e0786-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e0786-116">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e0786-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e0786-117">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e0786-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e0786-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e0786-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e0786-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e0786-119">Scenario description</span></span>
<span data-ttu-id="e0786-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e0786-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e0786-121">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e0786-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e0786-122">Att lägga till Datahug från galleriet</span><span class="sxs-lookup"><span data-stu-id="e0786-122">Adding Datahug from the gallery</span></span>
2. <span data-ttu-id="e0786-123">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e0786-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-datahug-from-the-gallery"></a><span data-ttu-id="e0786-124">Att lägga till Datahug från galleriet</span><span class="sxs-lookup"><span data-stu-id="e0786-124">Adding Datahug from the gallery</span></span>
<span data-ttu-id="e0786-125">Du måste lägga till Datahug från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Datahug i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e0786-125">To configure the integration of Datahug into Azure AD, you need to add Datahug from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e0786-126">**Utför följande steg för att lägga till Datahug från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="e0786-126">**To add Datahug from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e0786-127">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e0786-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e0786-129">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e0786-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e0786-130">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e0786-130">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="e0786-132">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0786-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="e0786-134">I sökrutan skriver **Datahug**.</span><span class="sxs-lookup"><span data-stu-id="e0786-134">In the search box, type **Datahug**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_search.png)

5. <span data-ttu-id="e0786-136">Välj i resultatpanelen **Datahug**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="e0786-136">In the results panel, select **Datahug**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e0786-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e0786-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e0786-139">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Datahug baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e0786-139">In this section, you configure and test Azure AD single sign-on with Datahug based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e0786-140">Azure AD måste du känna till användaren i Datahug motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="e0786-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Datahug is to a user in Azure AD.</span></span> <span data-ttu-id="e0786-141">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Datahug upprättas.</span><span class="sxs-lookup"><span data-stu-id="e0786-141">In other words, a link relationship between an Azure AD user and the related user in Datahug needs to be established.</span></span>

<span data-ttu-id="e0786-142">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Datahug.</span><span class="sxs-lookup"><span data-stu-id="e0786-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Datahug.</span></span>

<span data-ttu-id="e0786-143">Om du vill konfigurera och testa Azure AD enkel inloggning med Datahug, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e0786-143">To configure and test Azure AD single sign-on with Datahug, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e0786-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e0786-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e0786-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e0786-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e0786-146">**[Skapa en testanvändare Datahug](#creating-a-datahug-test-user)**  – du har en motsvarighet för Britta Simon i Datahug som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="e0786-146">**[Creating a Datahug test user](#creating-a-datahug-test-user)** - to have a counterpart of Britta Simon in Datahug that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e0786-147">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e0786-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e0786-148">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e0786-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e0786-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e0786-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e0786-150">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Datahug program.</span><span class="sxs-lookup"><span data-stu-id="e0786-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Datahug application.</span></span>

<span data-ttu-id="e0786-151">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Datahug:**</span><span class="sxs-lookup"><span data-stu-id="e0786-151">**To configure Azure AD single sign-on with Datahug, perform the following steps:**</span></span>

1. <span data-ttu-id="e0786-152">I Azure-portalen på den **Datahug** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e0786-152">In the Azure portal, on the **Datahug** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="e0786-154">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e0786-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_samlbase.png)

3. <span data-ttu-id="e0786-156">På den **Datahug domän och URL: er** om du vill konfigurera programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="e0786-156">On the **Datahug Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_ur1.png)

    <span data-ttu-id="e0786-158">a.</span><span class="sxs-lookup"><span data-stu-id="e0786-158">a.</span></span> <span data-ttu-id="e0786-159">I den **identifierare** textruta Skriv en URL med följande mönster:`https://apps.datahug.com/identity/<uniqueID>`</span><span class="sxs-lookup"><span data-stu-id="e0786-159">In the **Identifier** textbox, type a URL using the following pattern: `https://apps.datahug.com/identity/<uniqueID>`</span></span>

    <span data-ttu-id="e0786-160">b.</span><span class="sxs-lookup"><span data-stu-id="e0786-160">b.</span></span> <span data-ttu-id="e0786-161">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://apps.datahug.com/identity/<uniqueID>/acs`</span><span class="sxs-lookup"><span data-stu-id="e0786-161">In the **Reply URL** textbox, type a URL using the following pattern: `https://apps.datahug.com/identity/<uniqueID>/acs`</span></span>

4. <span data-ttu-id="e0786-162">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="e0786-162">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="e0786-163">Om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="e0786-163">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_url2.png)

    <span data-ttu-id="e0786-165">I den **inloggnings-URL** textruta Skriv en URL som:`https://apps.datahug.com/`</span><span class="sxs-lookup"><span data-stu-id="e0786-165">In the **Sign-on URL** textbox, type a URL as: `https://apps.datahug.com/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="e0786-166">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="e0786-166">These values are not the real.</span></span> <span data-ttu-id="e0786-167">Uppdatera dessa värden med den faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="e0786-167">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="e0786-168">Här rekommenderar vi att du om du vill använda det unika värdet på strängen i identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="e0786-168">Here we suggest you to use the unique value of string in the Identifier and Reply URL.</span></span> <span data-ttu-id="e0786-169">Kontakta [Datahug klienten supportteamet](http://datahug.com/about/contact-us/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="e0786-169">Contact [Datahug Client support team](http://datahug.com/about/contact-us/) to get these values.</span></span> 

5. <span data-ttu-id="e0786-170">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="e0786-170">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_certificate.png) 

6.  <span data-ttu-id="e0786-172">Kontrollera **”visa avancerade inställningar för signering av certifikat”** och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e0786-172">Check **“Show advanced certificate signing settings”** and perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_cert.png)

    <span data-ttu-id="e0786-174">a.</span><span class="sxs-lookup"><span data-stu-id="e0786-174">a.</span></span> <span data-ttu-id="e0786-175">I **signering alternativet**väljer **signera SAML assertion**.</span><span class="sxs-lookup"><span data-stu-id="e0786-175">In **Signing Option**, select **Sign SAML assertion**.</span></span>
    
    <span data-ttu-id="e0786-176">b.</span><span class="sxs-lookup"><span data-stu-id="e0786-176">b.</span></span> <span data-ttu-id="e0786-177">I **signering algoritmen**väljer **SHA1**.</span><span class="sxs-lookup"><span data-stu-id="e0786-177">In **Signing algorithm**, select **SHA1**.</span></span>
 
7. <span data-ttu-id="e0786-178">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e0786-178">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-datahug-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="e0786-180">På den **Datahug Configuration** klickar du på **konfigurera Datahug** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="e0786-180">On the **Datahug Configuration** section, click **Configure Datahug** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e0786-181">Kopiera den **SAML enhets-ID** och **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="e0786-181">Copy the **SAML Entity ID** and **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_configure.png) 

9. <span data-ttu-id="e0786-183">Konfigurera enkel inloggning på **Datahug** sida, måste du skicka den hämtade **XML-Metadata för**, **SAML enhets-ID** och **SAML enkel inloggning tjänst-URL**  till [Datahug support](http://datahug.com/about/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="e0786-183">To configure single sign-on on **Datahug** side, you need to send the downloaded **Metadata XML**, **SAML Entity ID** and **SAML Single Sign-On Service URL** to [Datahug support](http://datahug.com/about/contact-us/).</span></span> <span data-ttu-id="e0786-184">De konfigurera programmet så att SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="e0786-184">They set this application up to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="e0786-185">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="e0786-185">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e0786-186">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="e0786-186">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e0786-187">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e0786-187">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e0786-188">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e0786-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="e0786-189">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e0786-189">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="e0786-191">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="e0786-191">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e0786-192">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e0786-192">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e0786-194">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e0786-194">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e0786-196">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0786-196">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e0786-198">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e0786-198">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e0786-200">a.</span><span class="sxs-lookup"><span data-stu-id="e0786-200">a.</span></span> <span data-ttu-id="e0786-201">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e0786-201">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e0786-202">b.</span><span class="sxs-lookup"><span data-stu-id="e0786-202">b.</span></span> <span data-ttu-id="e0786-203">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e0786-203">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e0786-204">c.</span><span class="sxs-lookup"><span data-stu-id="e0786-204">c.</span></span> <span data-ttu-id="e0786-205">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e0786-205">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e0786-206">d.</span><span class="sxs-lookup"><span data-stu-id="e0786-206">d.</span></span> <span data-ttu-id="e0786-207">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e0786-207">Click **Create**.</span></span>
 
### <a name="creating-a-datahug-test-user"></a><span data-ttu-id="e0786-208">Skapa en testanvändare Datahug</span><span class="sxs-lookup"><span data-stu-id="e0786-208">Creating a Datahug test user</span></span>

<span data-ttu-id="e0786-209">Om du vill aktivera Azure AD-användare kan logga in på Datahug etableras de i Datahug.</span><span class="sxs-lookup"><span data-stu-id="e0786-209">To enable Azure AD users to log in to Datahug, they must be provisioned into Datahug.</span></span>  
<span data-ttu-id="e0786-210">När Datahug, etablering är en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="e0786-210">When Datahug, provisioning is a manual task.</span></span>

<span data-ttu-id="e0786-211">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="e0786-211">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="e0786-212">Logga in på webbplatsen Datahug företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="e0786-212">Log in to your Datahug company site as an administrator.</span></span>

2. <span data-ttu-id="e0786-213">Hovra över den **kugge** i övre högra hörnet och klicka på **inställningar**</span><span class="sxs-lookup"><span data-stu-id="e0786-213">Hover over the **cog** in the top right-hand corner and click **Settings**</span></span>
   
   ![Lägga till medarbetare](./media/active-directory-saas-datahug-tutorial/1.png)

3. <span data-ttu-id="e0786-215">Välj **personer** och klicka på den **Lägg till användare** fliken</span><span class="sxs-lookup"><span data-stu-id="e0786-215">Choose **People** and click the **Add Users** tab</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-datahug-tutorial/2.png)

4. <span data-ttu-id="e0786-217">Skriv e-postadressen till den person som du vill skapa ett konto för och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e0786-217">Type the email of the person you would like to create an account for and click **Add**.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-datahug-tutorial/3.png)

    > [!NOTE] 
    > <span data-ttu-id="e0786-219">Du kan skicka e-post för registrering till användare genom att välja **skicka välkomstmeddelandet** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="e0786-219">You can send registration mail to user by selecting **Send welcome email** checkbox.</span></span>  
    > <span data-ttu-id="e0786-220">Om du skapar ett konto för Salesforce inte skicka välkomstmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="e0786-220">If you are creating an account for Salesforce do not send the welcome email.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e0786-221">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="e0786-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e0786-222">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Datahug.</span><span class="sxs-lookup"><span data-stu-id="e0786-222">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Datahug.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="e0786-224">**Om du vill tilldela Datahug Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e0786-224">**To assign Britta Simon to Datahug, perform the following steps:**</span></span>

1. <span data-ttu-id="e0786-225">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e0786-225">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e0786-227">Välj i listan med program **Datahug**.</span><span class="sxs-lookup"><span data-stu-id="e0786-227">In the applications list, select **Datahug**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_app.png) 

3. <span data-ttu-id="e0786-229">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e0786-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="e0786-231">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e0786-231">Click **Add** button.</span></span> <span data-ttu-id="e0786-232">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0786-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="e0786-234">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="e0786-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e0786-235">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0786-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e0786-236">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0786-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e0786-237">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e0786-237">Testing single sign-on</span></span>

<span data-ttu-id="e0786-238">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e0786-238">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>
<span data-ttu-id="e0786-239">När du klickar på panelen Datahug på åtkomstpanelen du bör få automatiskt loggat in på ditt Datahug program.</span><span class="sxs-lookup"><span data-stu-id="e0786-239">When you click the Datahug tile in the Access Panel, you should get automatically signed-on to your Datahug application.</span></span> <span data-ttu-id="e0786-240">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="e0786-240">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e0786-241">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e0786-241">Additional resources</span></span>

* [<span data-ttu-id="e0786-242">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e0786-242">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e0786-243">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e0786-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_203.png

