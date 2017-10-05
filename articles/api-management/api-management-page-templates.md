---
title: Sidan mallar i Azure API Management | Microsoft Docs
description: "Lär dig hur du anpassar innehållet i developer portalens sidor med en uppsättning mallar i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: e57df269-1019-4b74-b74d-53155b809d59
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 7f9ef37a694bce786b6acaa428df83f0cb23c2dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="page-templates-in-azure-api-management"></a><span data-ttu-id="35ec1-103">Mallar i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="35ec1-103">Page templates in Azure API Management</span></span>
<span data-ttu-id="35ec1-104">Azure API Management ger dig möjlighet att anpassa innehållet i developer portalens sidor med hjälp av en uppsättning mallar som konfigurerar deras innehåll.</span><span class="sxs-lookup"><span data-stu-id="35ec1-104">Azure API Management provides you the ability to customize the content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="35ec1-105">Med hjälp av [DotLiquid](http://dotliquidmarkup.org/) syntax och redigeringsprogram, t.ex [DotLiquid för Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), och en angiven uppsättning lokaliserade [String resurser](api-management-template-resources.md#strings), [glyf resurser](api-management-template-resources.md#glyphs), och [sidan kontroller](api-management-page-controls.md), du har stor flexibilitet för att konfigurera innehåll för sidorna som du vill använda dessa mallar.</span><span class="sxs-lookup"><span data-stu-id="35ec1-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and the editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility to configure the content of the pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="35ec1-106">Mallarna i det här avsnittet kan du anpassa innehållet i inloggning, logga in och att hitta inte sidan sidor i developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="35ec1-106">The templates in this section allow you to customize the content of the sign in, sign up, and page not found pages in the developer portal.</span></span>  
  
-   [<span data-ttu-id="35ec1-107">Logga in</span><span class="sxs-lookup"><span data-stu-id="35ec1-107">Sign in</span></span>](#SignIn)  
  
-   [<span data-ttu-id="35ec1-108">Registrera sig</span><span class="sxs-lookup"><span data-stu-id="35ec1-108">Sign up</span></span>](#SignUp)  
  
-   [<span data-ttu-id="35ec1-109">Det gick inte att hitta sidan</span><span class="sxs-lookup"><span data-stu-id="35ec1-109">Page not found</span></span>](#PageNotFound)  
  
> [!NOTE]
>  <span data-ttu-id="35ec1-110">Standard exempelmallarna ingår i följande dokumentation, men kan komma att ändras på grund av kontinuerliga förbättringar.</span><span class="sxs-lookup"><span data-stu-id="35ec1-110">Sample default templates are included in the following documentation, but are subject to change due to continuous improvements.</span></span> <span data-ttu-id="35ec1-111">Du kan visa live standardmallarna i developer-portalen genom att navigera till önskade enskilda mallar.</span><span class="sxs-lookup"><span data-stu-id="35ec1-111">You can view the live default templates in the developer portal by navigating to the desired individual templates.</span></span> <span data-ttu-id="35ec1-112">Mer information om hur du arbetar med mallar finns [hur du anpassar API Management developer-portalen med hjälp av mallar](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="35ec1-112">For more information about working with templates, see [How to customize the API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="35ec1-113"><a name="SignIn"></a>Logga in</span><span class="sxs-lookup"><span data-stu-id="35ec1-113"><a name="SignIn"></a> Sign in</span></span>  
 <span data-ttu-id="35ec1-114">Den **inloggning** mall kan du anpassa inloggningssidan i developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="35ec1-114">The **sign in** template allows you to customize the sign in page in the developer portal.</span></span>  
  
 <span data-ttu-id="35ec1-115">![Inloggningssidan i](./media/api-management-page-templates/APIM-Sign-In-Page-Developer-Portal-Templates.png "APIM inloggning sidan Developer Portal mallar")</span><span class="sxs-lookup"><span data-stu-id="35ec1-115">![Sign In Page](./media/api-management-page-templates/APIM-Sign-In-Page-Developer-Portal-Templates.png "APIM Sign In Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="35ec1-116">Standardmall</span><span class="sxs-lookup"><span data-stu-id="35ec1-116">Default template</span></span>  
  
```xml  
<h2 class="text-center">{% localized "SigninStrings|WebAuthenticationSigninTitle" %}</h2>  
{% if registrationEnabled == true %}  
<p class="text-center">{% localized "SigninStrings|WebAuthenticationNotAMember" %}</p>  
{% endif %}  
  
<div class="row center-block ap-idp-container">  
  <div class="col-md-6">  
    {% if registrationEnabled == true %}  
        <p>{% localized "SigninStrings|WebAuthenticationSigininWithPassword" %}</p>  
    <basic-SignIn></basic-SignIn>  
    {% endif %}  
  </div>  
  
    {% if registrationEnabled != true and providers.size == 0 %}  
        {% localized "ProviderInfoStrings|TextboxExternalIdentitiesDisabled" %}  
  {% else %}  
        {% if providers.size > 0 %}  
      <div class="col-md-6">  
            <div class="providers-list">  
                <p class="text-left">  
                {% if registrationEnabled == true %}  
                    {% localized "ProviderInfoStrings|TextboxExternalIdentitiesSigninInvitation" %}  
                {% else %}  
                    {% localized "ProviderInfoStrings|TextboxExternalIdentitiesSigninInvitationPrimary" %}  
                {% endif %}  
                </p>  
        <providers></providers>  
            </div>  
    </div>  
        {% endif %}  
    {% endif %}  
  
  {% if userRegistrationTermsEnabled == true %}  
    <div class="col-md-6">  
        <div id="terms" class="modal" role="dialog" tabindex="-1">  
            <div class="modal-dialog">  
                <div class="modal-content">  
                    <div class="modal-header">  
                        <h4 class="modal-title">{% localized "SigninResources|DialogHeadingTermsOfUse" %}</h4>  
                    </div>  
                    <div class="modal-body break-all">{{userRegistrationTerms}}</div>  
                    <div class="modal-footer">  
                        <button type="button" class="btn btn-default" data-dismiss="modal">{% localized "CommonStrings|ButtonLabelClose" %}</button>  
                    </div>  
                </div>  
            </div>  
        </div>  
        <p>{% localized "SigninResources|TextblockUserRegistrationTermsProvided" %}</p>  
    </div>  
    {% endif %}  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="35ec1-117">Kontroller</span><span class="sxs-lookup"><span data-stu-id="35ec1-117">Controls</span></span>  
 <span data-ttu-id="35ec1-118">Den här mallen kan du använda följande [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="35ec1-118">This template may  use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="35ec1-119">Basic-inloggning</span><span class="sxs-lookup"><span data-stu-id="35ec1-119">basic-signin</span></span>](api-management-page-controls.md#basic-signin)  
  
-   [<span data-ttu-id="35ec1-120">providers</span><span class="sxs-lookup"><span data-stu-id="35ec1-120">providers</span></span>](api-management-page-controls.md#providers)  
  
### <a name="data-model"></a><span data-ttu-id="35ec1-121">Datamodell</span><span class="sxs-lookup"><span data-stu-id="35ec1-121">Data model</span></span>  
 <span data-ttu-id="35ec1-122">[Användarens](api-management-template-data-model-reference.md#UseSignIn) entitet.</span><span class="sxs-lookup"><span data-stu-id="35ec1-122">[User sign in](api-management-template-data-model-reference.md#UseSignIn) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="35ec1-123">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="35ec1-123">Sample template data</span></span>  
  
```json  
{  
    "Email": null,  
    "Password": null,  
    "ReturnUrl": null,  
    "RememberMe": false,  
    "RegistrationEnabled": true,  
    "DelegationEnabled": false,  
    "DelegationUrl": null,  
    "SsoSignUpUrl": null,  
    "AuxServiceUrl": "https://manage.windowsazure.com/#Workspaces/ApiManagementExtension/service/contoso5/dashboard",  
    "Providers": [  
        {  
            "Properties": {  
                "AuthenticationType": "Aad",  
                "Caption": "Azure Active Directory"  
            },  
            "AuthenticationType": "Aad",  
            "Caption": "Azure Active Directory"  
        }  
    ],  
    "UserRegistrationTerms": null,  
    "UserRegistrationTermsEnabled": false  
}  
```  
  
##  <span data-ttu-id="35ec1-124"><a name="SignUp"></a>Registrera sig</span><span class="sxs-lookup"><span data-stu-id="35ec1-124"><a name="SignUp"></a> Sign up</span></span>  
 <span data-ttu-id="35ec1-125">Den **registrering** mall kan du anpassa sidan registrering i developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="35ec1-125">The **sign up** template allows you to customize the sign up page in the developer portal.</span></span>  
  
 <span data-ttu-id="35ec1-126">![Inloggningssidan](./media/api-management-page-templates/APIM-Sign-Up-Page-Developer-Portal-Templates.png "APIM logga in sidan Developer Portal mallar")</span><span class="sxs-lookup"><span data-stu-id="35ec1-126">![Sign Up Page](./media/api-management-page-templates/APIM-Sign-Up-Page-Developer-Portal-Templates.png "APIM Sign Up Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="35ec1-127">Standardmall</span><span class="sxs-lookup"><span data-stu-id="35ec1-127">Default template</span></span>  
  
```xml  
<h2 class="text-center">{% localized "SignupStrings|PageTitleSignup" %}</h2>  
<p class="text-center">  
  {% localized "SignupStrings|WebAuthenticationAlreadyAMember" %} <a href="/signin">{% localized "SignupStrings|WebAuthenticationSigninNow" %}</a>  
</p>  
  
<div class="row center-block ap-idp-container">  
  <div class="col-md-6">  
    <p>{% localized "SignupStrings|WebAuthenticationCreateNewAccount" %}</p>  
    <sign-up></sign-up>  
  </div>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="35ec1-128">Kontroller</span><span class="sxs-lookup"><span data-stu-id="35ec1-128">Controls</span></span>  
 <span data-ttu-id="35ec1-129">Den här mallen kan du använda följande [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="35ec1-129">This template may  use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="35ec1-130">registrering</span><span class="sxs-lookup"><span data-stu-id="35ec1-130">sign-up</span></span>](api-management-page-controls.md#sign-up)  
  
### <a name="data-model"></a><span data-ttu-id="35ec1-131">Datamodell</span><span class="sxs-lookup"><span data-stu-id="35ec1-131">Data model</span></span>  
 <span data-ttu-id="35ec1-132">[Användaren loggar in](api-management-template-data-model-reference.md#UserSignUp) entitet.</span><span class="sxs-lookup"><span data-stu-id="35ec1-132">[User sign up](api-management-template-data-model-reference.md#UserSignUp) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="35ec1-133">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="35ec1-133">Sample template data</span></span>  
  
```json  
{  
    "PasswordConfirm": null,  
    "Password": null,  
    "PasswordVerdictLevel": 0,  
    "UserRegistrationTerms": null,  
    "UserRegistrationTermsOptions": 0,  
    "ConsentAccepted": false,  
    "Email": null,  
    "FirstName": null,  
    "LastName": null,  
    "UserData": null,  
    "NameIdentifier": null,  
    "ProviderName": null  
}  
```  
  
##  <span data-ttu-id="35ec1-134"><a name="PageNotFound"></a>Det gick inte att hitta sidan</span><span class="sxs-lookup"><span data-stu-id="35ec1-134"><a name="PageNotFound"></a> Page not found</span></span>  
 <span data-ttu-id="35ec1-135">Den **sidan inte att hitta** mall kan du anpassa sidan inte att hitta sidan i developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="35ec1-135">The **page not found** template allows you to customize the page not found page in the developer portal.</span></span>  
  
 <span data-ttu-id="35ec1-136">![Gick inte att hitta sidan](./media/api-management-page-templates/APIM-Not-Found-Page-Developer-Portal-Templates.png "APIM gick inte att hitta sidan Developer Portal mallar")</span><span class="sxs-lookup"><span data-stu-id="35ec1-136">![Not Found Page](./media/api-management-page-templates/APIM-Not-Found-Page-Developer-Portal-Templates.png "APIM Not Found Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="35ec1-137">Standardmall</span><span class="sxs-lookup"><span data-stu-id="35ec1-137">Default template</span></span>  
  
```xml  
<h2>{% localized "NotFoundStrings|PageTitleNotFound" %}</h2>  
  
<h3>{% localized "NotFoundStrings|TitlePotentialCause" %}</h3>  
<ul>  
  <li>{% localized "NotFoundStrings|TextblockPotentialCauseOldLink" %}</li>  
  <li>{% localized "NotFoundStrings|TextblockPotentialCauseMisspelledUrl" %}</li>  
</ul>  
  
<h3>{% localized "NotFoundStrings|TitlePotentialSolution" %}</h3>  
<ul>  
  <li>{% localized "NotFoundStrings|TextblockPotentialSolutionRetype" %}</li>  
  <li>  
    {% capture textPotentialSolutionStartOver %}{% localized "NotFoundStrings|TextblockPotentialSolutionStartOver" %}{% endcapture %}  
    {% capture homeLink %}<a href="/">{% localized "NotFoundStrings|LinkLabelHomePage" %}</a>{% endcapture %}  
    {% assign replaceString = '{0}' %}  
  
    {{ textPotentialSolutionStartOver | replace : replaceString, homeLink }}  
  </li>  
</ul>  
  
<p>  
  {% capture textReportProblem %}{% localized "NotFoundStrings|TextReportProblem" %}{% endcapture %}  
  {% capture emailLink %}<a href="mailto:apimgmt@microsoft.com" target="_self" title="API Management Support">{% localized "NotFoundStrings|LinkLabelSendUsEmail" %}</a>{% endcapture %}  
  {% assign replaceString = '{0}' %}  
  
  {{ textReportProblem | replace : replaceString, emailLink }}  
</p>  
```  
  
### <a name="controls"></a><span data-ttu-id="35ec1-138">Kontroller</span><span class="sxs-lookup"><span data-stu-id="35ec1-138">Controls</span></span>  
 <span data-ttu-id="35ec1-139">Den här mallen kan inte använda någon [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="35ec1-139">This template may  not use any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="35ec1-140">Datamodell</span><span class="sxs-lookup"><span data-stu-id="35ec1-140">Data model</span></span>  
  
|<span data-ttu-id="35ec1-141">Egenskap</span><span class="sxs-lookup"><span data-stu-id="35ec1-141">Property</span></span>|<span data-ttu-id="35ec1-142">Typ</span><span class="sxs-lookup"><span data-stu-id="35ec1-142">Type</span></span>|<span data-ttu-id="35ec1-143">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="35ec1-143">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="35ec1-144">referenceCode</span><span class="sxs-lookup"><span data-stu-id="35ec1-144">referenceCode</span></span>|<span data-ttu-id="35ec1-145">Sträng</span><span class="sxs-lookup"><span data-stu-id="35ec1-145">string</span></span>|<span data-ttu-id="35ec1-146">Koden genereras om den här sidan visas som ett resultat av ett internt fel.</span><span class="sxs-lookup"><span data-stu-id="35ec1-146">Code generated if this page was displayed as the result of an internal error.</span></span>|  
|<span data-ttu-id="35ec1-147">Felkod</span><span class="sxs-lookup"><span data-stu-id="35ec1-147">errorCode</span></span>|<span data-ttu-id="35ec1-148">Sträng</span><span class="sxs-lookup"><span data-stu-id="35ec1-148">string</span></span>|<span data-ttu-id="35ec1-149">Koden genereras om den här sidan visas som ett resultat av ett internt fel.</span><span class="sxs-lookup"><span data-stu-id="35ec1-149">Code generated if this page was displayed as the result of an internal error.</span></span>|  
|<span data-ttu-id="35ec1-150">emailBody</span><span class="sxs-lookup"><span data-stu-id="35ec1-150">emailBody</span></span>|<span data-ttu-id="35ec1-151">Sträng</span><span class="sxs-lookup"><span data-stu-id="35ec1-151">string</span></span>|<span data-ttu-id="35ec1-152">E-brödtext genereras om den här sidan visas som ett resultat av ett internt fel.</span><span class="sxs-lookup"><span data-stu-id="35ec1-152">Email body generated if this page was displayed as the result of an internal error.</span></span>|  
|<span data-ttu-id="35ec1-153">requestedUrl</span><span class="sxs-lookup"><span data-stu-id="35ec1-153">requestedUrl</span></span>|<span data-ttu-id="35ec1-154">Sträng</span><span class="sxs-lookup"><span data-stu-id="35ec1-154">string</span></span>|<span data-ttu-id="35ec1-155">Den URL som begärts när sidan inte hittades.</span><span class="sxs-lookup"><span data-stu-id="35ec1-155">The URL requested when the page was not found.</span></span>|  
|<span data-ttu-id="35ec1-156">referrerUrl</span><span class="sxs-lookup"><span data-stu-id="35ec1-156">referrerUrl</span></span>|<span data-ttu-id="35ec1-157">Sträng</span><span class="sxs-lookup"><span data-stu-id="35ec1-157">string</span></span>|<span data-ttu-id="35ec1-158">Referent URL till begärd URL.</span><span class="sxs-lookup"><span data-stu-id="35ec1-158">The referrer URL to the requested URL.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="35ec1-159">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="35ec1-159">Sample template data</span></span>  
  
```json  
{  
    "referenceCode": null,  
    "errorCode": null,  
    "emailBody": null,  
    "requestedUrl": "https://contoso5.portal.azure-api.net:443/NotFoundPage?startEditTemplate=NotFoundPage",  
    "referrerUrl": "https://contoso5.portal.azure-api.net/signup?startEditTemplate=SignUpTemplate"  
}  
```

## <a name="next-steps"></a><span data-ttu-id="35ec1-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="35ec1-160">Next steps</span></span>
<span data-ttu-id="35ec1-161">Mer information om hur du arbetar med mallar finns [hur du anpassar API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="35ec1-161">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>