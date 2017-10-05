---
title: "Felsöka SAML-baserade enkel inloggning till program i Azure Active Directory | Microsoft Docs"
description: "Lär dig att felsöka SAML-baserade enkel inloggning till program i Azure Active Directory "
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: asmalser
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: 31447d597296bac57481dc2acb4a95ee3a104161
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a><span data-ttu-id="4330e-103">Felsöka SAML-baserade enkel inloggning till program i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4330e-103">How to debug SAML-based single sign-on to applications in Azure Active Directory</span></span>
<span data-ttu-id="4330e-104">När du felsöker en SAML-baserade programintegration, är det ofta praktiskt att använda ett verktyg som [Fiddler](http://www.telerik.com/fiddler) att se SAML-begäran, SAML-svar och faktiska SAML-token som utfärdas till programmet.</span><span class="sxs-lookup"><span data-stu-id="4330e-104">When debugging a SAML-based application integration, it is often helpful to use a tool like [Fiddler](http://www.telerik.com/fiddler) to see the SAML request, the SAML response, and the actual SAML token that is issued to the application.</span></span> <span data-ttu-id="4330e-105">Genom att undersöka SAML-token kan du se till att alla obligatoriska attribut, användarnamnet i SAML-ämne och utfärdaren URI kommer som förväntat.</span><span class="sxs-lookup"><span data-stu-id="4330e-105">By examining the SAML token, you can ensure that all of the required attributes, the username in the SAML subject, and the issuer URI are coming through as expected.</span></span>

![][1]

<span data-ttu-id="4330e-106">Svaret från Azure AD som innehåller SAML-token är vanligtvis det som sker när en HTTP 302-omdirigering från https://login.windows.net och skickas till den konfigurerade **Reply URL** av programmet.</span><span class="sxs-lookup"><span data-stu-id="4330e-106">The response from Azure AD that contains the SAML token is typically the one that occurs after an HTTP 302 redirect from https://login.windows.net, and is sent to the configured **Reply URL** of the application.</span></span> 

<span data-ttu-id="4330e-107">Du kan visa SAML-token genom att välja den här raden och sedan välja den **kontrollanter > WebForms** fliken i den högra panelen.</span><span class="sxs-lookup"><span data-stu-id="4330e-107">You can view the SAML token by selecting this line and then selecting the **Inspectors > WebForms** tab in the right panel.</span></span> <span data-ttu-id="4330e-108">Därifrån kan du högerklicka på den **SAMLResponse** värdet och välj **skicka till TextWizard**.</span><span class="sxs-lookup"><span data-stu-id="4330e-108">From there, right-click the **SAMLResponse** value and select **Send to TextWizard**.</span></span> <span data-ttu-id="4330e-109">Välj sedan **från Base64** från den **transformera** menyn för att avkoda token och se innehållet.</span><span class="sxs-lookup"><span data-stu-id="4330e-109">Then select **From Base64** from the **Transform** menu to decode the token and see its contents.</span></span>

<span data-ttu-id="4330e-110">**Obs**: Om du vill visa innehållet i den här HTTP-begäran, Fiddler kan du uppmanas att konfigurera dekryptering av HTTPS-trafik som du behöver göra.</span><span class="sxs-lookup"><span data-stu-id="4330e-110">**Note**: To see the contents of this HTTP request, Fiddler may prompt you to configure decryption of HTTPS traffic, which you will need to do.</span></span>

## <a name="related-articles"></a><span data-ttu-id="4330e-111">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="4330e-111">Related Articles</span></span>
* [<span data-ttu-id="4330e-112">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4330e-112">Article Index for Application Management in Azure Active Directory</span></span>](../active-directory-apps-index.md)
* [<span data-ttu-id="4330e-113">Konfigurera enkel inloggning för program som inte ingår i Azure Active Directory-programgalleriet</span><span class="sxs-lookup"><span data-stu-id="4330e-113">Configuring single sign-on to applications that are not in the Azure Active Directory application gallery</span></span>](../active-directory-saas-custom-apps.md)
* [<span data-ttu-id="4330e-114">Anpassa anspråk som utfärdats i SAML-Token för förintegrerade appar</span><span class="sxs-lookup"><span data-stu-id="4330e-114">How to Customize Claims Issued in the SAML Token for Pre-Integrated Apps</span></span>](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ../media/active-directory-saml-debugging/fiddler.png