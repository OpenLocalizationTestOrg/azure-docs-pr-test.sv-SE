---
title: "Självstudier: Azure Active Directory-integrering med Proofpoint på begäran | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Proofpoint på begäran."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 773e7f7d-ec31-411b-860d-6a6633335d43
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: b4c8d8c187fc865a905016f04a41843894249f5e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-proofpoint-on-demand"></a><span data-ttu-id="f8c4e-103">Självstudier: Azure Active Directory-integrering med Proofpoint på begäran</span><span class="sxs-lookup"><span data-stu-id="f8c4e-103">Tutorial: Azure Active Directory integration with Proofpoint on Demand</span></span>

<span data-ttu-id="f8c4e-104">I kursen får lära du att integrera Proofpoint på begäran med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="f8c4e-104">In this tutorial, you learn how to integrate Proofpoint on Demand with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f8c4e-105">Integrera Proofpoint på begäran med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f8c4e-105">Integrating Proofpoint on Demand with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f8c4e-106">Du kan styra i Azure AD som har åtkomst till Proofpoint på begäran</span><span class="sxs-lookup"><span data-stu-id="f8c4e-106">You can control in Azure AD who has access to Proofpoint on Demand</span></span>
- <span data-ttu-id="f8c4e-107">Du kan aktivera användarna att automatiskt hämta loggat in på Proofpoint på begäran (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="f8c4e-107">You can enable your users to automatically get signed-on to Proofpoint on Demand (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f8c4e-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f8c4e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f8c4e-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f8c4e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8c4e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f8c4e-110">Prerequisites</span></span>

<span data-ttu-id="f8c4e-111">För att konfigurera Azure AD-integrering med Proofpoint på begäran, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="f8c4e-111">To configure Azure AD integration with Proofpoint on Demand, you need the following items:</span></span>

- <span data-ttu-id="f8c4e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f8c4e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f8c4e-113">En Proofpoint på begäran enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="f8c4e-113">A Proofpoint on Demand single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f8c4e-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f8c4e-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f8c4e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f8c4e-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f8c4e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f8c4e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f8c4e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="f8c4e-118">Scenario description</span></span>
<span data-ttu-id="f8c4e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f8c4e-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="f8c4e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f8c4e-121">Att lägga till Proofpoint på begäran från galleriet</span><span class="sxs-lookup"><span data-stu-id="f8c4e-121">Adding Proofpoint on Demand from the gallery</span></span>
2. <span data-ttu-id="f8c4e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f8c4e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-proofpoint-on-demand-from-the-gallery"></a><span data-ttu-id="f8c4e-123">Att lägga till Proofpoint på begäran från galleriet</span><span class="sxs-lookup"><span data-stu-id="f8c4e-123">Adding Proofpoint on Demand from the gallery</span></span>
<span data-ttu-id="f8c4e-124">För att konfigurera integrering av Proofpoint på begäran till Azure AD, som du behöver lägga till Proofpoint på begäran från galleriet i listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-124">To configure the integration of Proofpoint on Demand into Azure AD, you need to add Proofpoint on Demand from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f8c4e-125">**Utför följande steg för att lägga till Proofpoint på begäran från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="f8c4e-125">**To add Proofpoint on Demand from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f8c4e-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f8c4e-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f8c4e-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="f8c4e-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="f8c4e-133">I sökrutan skriver **Proofpoint på begäran**.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-133">In the search box, type **Proofpoint on Demand**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_search.png)

5. <span data-ttu-id="f8c4e-135">Välj i resultatpanelen **Proofpoint på begäran**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-135">In the results panel, select **Proofpoint on Demand**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f8c4e-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f8c4e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f8c4e-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Proofpoint på begäran baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-138">In this section, you configure and test Azure AD single sign-on with Proofpoint on Demand based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f8c4e-139">Azure AD måste du känna till motsvarande användaren i Proofpoint på begäran till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Proofpoint on Demand is to a user in Azure AD.</span></span> <span data-ttu-id="f8c4e-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Proofpoint på begäran upprättas.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-140">In other words, a link relationship between an Azure AD user and the related user in Proofpoint on Demand needs to be established.</span></span>

<span data-ttu-id="f8c4e-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Proofpoint på begäran.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Proofpoint on Demand.</span></span>

<span data-ttu-id="f8c4e-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Proofpoint på begäran, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="f8c4e-142">To configure and test Azure AD single sign-on with Proofpoint on Demand, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f8c4e-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f8c4e-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f8c4e-145">**[Skapa en Proofpoint på begäran testanvändare](#creating-a-proofpoint-on-demand-test-user)**  – har en motsvarighet för Britta Simon Proofpoint på begäran som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-145">**[Creating a Proofpoint on Demand test user](#creating-a-proofpoint-on-demand-test-user)** - to have a counterpart of Britta Simon in Proofpoint on Demand that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f8c4e-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f8c4e-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f8c4e-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f8c4e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f8c4e-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din Proofpoint på begäran-programmet.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Proofpoint on Demand application.</span></span>

<span data-ttu-id="f8c4e-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Proofpoint på begäran:**</span><span class="sxs-lookup"><span data-stu-id="f8c4e-150">**To configure Azure AD single sign-on with Proofpoint on Demand, perform the following steps:**</span></span>

1. <span data-ttu-id="f8c4e-151">I Azure-portalen på den **Proofpoint på begäran** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-151">In the Azure portal, on the **Proofpoint on Demand** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="f8c4e-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
  
    ![Konfigurera enkel inloggning](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_samlbase.png)

3. <span data-ttu-id="f8c4e-155">På den **Proofpoint på begäran domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f8c4e-155">On the **Proofpoint on Demand Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_url.png)

    <span data-ttu-id="f8c4e-157">a.In den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<hostname>.pphosted.com/ppssamlsp_hostname`</span><span class="sxs-lookup"><span data-stu-id="f8c4e-157">a.In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<hostname>.pphosted.com/ppssamlsp_hostname`</span></span>

    <span data-ttu-id="f8c4e-158">b.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-158">b.</span></span> <span data-ttu-id="f8c4e-159">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<hostname>.pphosted.com/ppssamlsp`</span><span class="sxs-lookup"><span data-stu-id="f8c4e-159">In the **Identifier** textbox, type a URL using the following pattern: `https://<hostname>.pphosted.com/ppssamlsp`</span></span>

    <span data-ttu-id="f8c4e-160">c.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-160">c.</span></span>  <span data-ttu-id="f8c4e-161">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<hostname>.pphosted.com:portnumber/v1/samlauth/samlconsumer`</span><span class="sxs-lookup"><span data-stu-id="f8c4e-161">In the **Reply URL** textbox, type a URL using the following pattern: `https://<hostname>.pphosted.com:portnumber/v1/samlauth/samlconsumer`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="f8c4e-162">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-162">These values are not the real.</span></span> <span data-ttu-id="f8c4e-163">Uppdatera dessa värden med faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-163">Update these values with the actual Identifier, Reply URL and Sign-On URL.</span></span> <span data-ttu-id="f8c4e-164">Kontakta [Proofpoint på begäran klienten supportteamet](https://www.proofpoint.com/us/support-services) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-164">Contact [Proofpoint on Demand Client support team](https://www.proofpoint.com/us/support-services) to get these values.</span></span> 

4. <span data-ttu-id="f8c4e-165">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-165">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_certificate.png) 

5. <span data-ttu-id="f8c4e-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="f8c4e-169">På den **Proofpoint på begäran-konfiguration** klickar du på **konfigurera Proofpoint på begäran** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-169">On the **Proofpoint on Demand Configuration** section, click **Configure Proofpoint on Demand** to open **Configure sign-on** window.</span></span> <span data-ttu-id="f8c4e-170">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="f8c4e-170">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_configure.png) 

7. <span data-ttu-id="f8c4e-172">Konfigurera enkel inloggning på **Proofpoint på begäran** sida, måste du skicka den hämtade **Certificate(Base64)**,**SAML enhets-ID**, och **SAML enkel inloggning Tjänstwebbadress** till [Proofpoint på begäran klienten supportteamet](https://www.proofpoint.com/us/support-services).</span><span class="sxs-lookup"><span data-stu-id="f8c4e-172">To configure single sign-on on **Proofpoint on Demand** side, you need to send the downloaded **Certificate(Base64)**,**SAML Entity ID**, and **SAML Single Sign-On Service URL** to [Proofpoint on Demand Client support team](https://www.proofpoint.com/us/support-services).</span></span>

> [!TIP]
> <span data-ttu-id="f8c4e-173">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="f8c4e-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f8c4e-174">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f8c4e-175">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f8c4e-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f8c4e-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f8c4e-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="f8c4e-177">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="f8c4e-179">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="f8c4e-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f8c4e-180">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f8c4e-182">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-182">These values are not the real.</span></span> <span data-ttu-id="f8c4e-183">Uppdatera dessa värden med den faktiska</span><span class="sxs-lookup"><span data-stu-id="f8c4e-183">Update these values with the actual</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f8c4e-185">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-185">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f8c4e-187">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f8c4e-187">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-proofpoint-ondemand-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f8c4e-189">a.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-189">a.</span></span> <span data-ttu-id="f8c4e-190">I den **namn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-190">In the **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="f8c4e-191">b.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-191">b.</span></span> <span data-ttu-id="f8c4e-192">I den **användarnamn** textruta typ av **e-postadress** av Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-192">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="f8c4e-193">c.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-193">c.</span></span> <span data-ttu-id="f8c4e-194">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-194">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f8c4e-195">d.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-195">d.</span></span> <span data-ttu-id="f8c4e-196">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-196">Click **Create**.</span></span>
 
### <a name="creating-a-proofpoint-on-demand-test-user"></a><span data-ttu-id="f8c4e-197">Skapa en Proofpoint på begäran testanvändare</span><span class="sxs-lookup"><span data-stu-id="f8c4e-197">Creating a Proofpoint on Demand test user</span></span>

<span data-ttu-id="f8c4e-198">I det här avsnittet skapar du en användare som kallas Britta Simon i Proofpoint på begäran.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-198">In this section, you create a user called Britta Simon in Proofpoint on Demand.</span></span> <span data-ttu-id="f8c4e-199">Arbeta med [Proofpoint på begäran klienten supportteamet](https://www.proofpoint.com/us/support-services) att lägga till användare i Proofpoint på begäran-plattformen.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-199">Work with [Proofpoint on Demand Client support team](https://www.proofpoint.com/us/support-services) to add users in the Proofpoint on Demand platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f8c4e-200">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="f8c4e-200">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f8c4e-201">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Proofpoint på begäran.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-201">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Proofpoint on Demand.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="f8c4e-203">**Om du vill tilldela Britta Simon Proofpoint på begäran, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f8c4e-203">**To assign Britta Simon to Proofpoint on Demand, perform the following steps:**</span></span>

1. <span data-ttu-id="f8c4e-204">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-204">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="f8c4e-206">Välj i listan med program **Proofpoint på begäran**.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-206">In the applications list, select **Proofpoint on Demand**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_proofpointondemand_app.png) 

3. <span data-ttu-id="f8c4e-208">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-208">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="f8c4e-210">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-210">Click **Add** button.</span></span> <span data-ttu-id="f8c4e-211">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="f8c4e-213">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-213">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f8c4e-214">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f8c4e-215">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f8c4e-216">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f8c4e-216">Testing single sign-on</span></span>

<span data-ttu-id="f8c4e-217">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-217">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f8c4e-218">När du klickar på den **Proofpoint på begäran** panelen på panelen åtkomst ska vara loggas du automatiskt att din Proofpoint på begäran-programmet.</span><span class="sxs-lookup"><span data-stu-id="f8c4e-218">When you click the **Proofpoint on Demand** tile on the Access Panel, you should be automatically signed on to your Proofpoint on Demand application.</span></span>
<span data-ttu-id="f8c4e-219">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f8c4e-219">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>  

## <a name="additional-resources"></a><span data-ttu-id="f8c4e-220">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f8c4e-220">Additional resources</span></span>

* [<span data-ttu-id="f8c4e-221">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f8c4e-221">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f8c4e-222">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f8c4e-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-proofpoint-ondemand-tutorial/tutorial_general_203.png

