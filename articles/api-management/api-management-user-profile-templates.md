---
title: "Användarens profilmallar i Azure API Management | Microsoft Docs"
description: "Lär dig hur du anpassar innehållet användarprofil sidor i developer-portalen i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 2e3b73ef-d223-44fe-9280-c3af3fd4a030
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 9a11bd5800068a5725ab2f099043993bff0b28d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="user-profile-templates-in-azure-api-management"></a><span data-ttu-id="232ca-103">Användarens profilmallar i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="232ca-103">User profile templates in Azure API Management</span></span>
<span data-ttu-id="232ca-104">Azure API Management ger dig möjlighet att anpassa innehållet i developer portalens sidor med hjälp av en uppsättning mallar som konfigurerar deras innehåll.</span><span class="sxs-lookup"><span data-stu-id="232ca-104">Azure API Management provides you the ability to customize the content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="232ca-105">Med hjälp av [DotLiquid](http://dotliquidmarkup.org/) syntax och redigeringsprogram, t.ex [DotLiquid för Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), och en angiven uppsättning lokaliserade [String resurser](api-management-template-resources.md#strings), [glyf resurser](api-management-template-resources.md#glyphs), och [sidan kontroller](api-management-page-controls.md), du har stor flexibilitet för att konfigurera innehåll för sidorna som du vill använda dessa mallar.</span><span class="sxs-lookup"><span data-stu-id="232ca-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and the editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility to configure the content of the pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="232ca-106">Mallarna i det här avsnittet kan du anpassa innehållet i användarens profilsidor i developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="232ca-106">The templates in this section allow you to customize the content of the User profile pages in the developer portal.</span></span>  
  
-   [<span data-ttu-id="232ca-107">Profil</span><span class="sxs-lookup"><span data-stu-id="232ca-107">Profile</span></span>](#Profile)  
  
-   [<span data-ttu-id="232ca-108">Prenumerationer</span><span class="sxs-lookup"><span data-stu-id="232ca-108">Subscriptions</span></span>](#Subscriptions)  
  
-   [<span data-ttu-id="232ca-109">Program</span><span class="sxs-lookup"><span data-stu-id="232ca-109">Applications</span></span>](#Applications)  
  
-   [<span data-ttu-id="232ca-110">Uppdatera kontoinformation</span><span class="sxs-lookup"><span data-stu-id="232ca-110">Update account info</span></span>](#UpdateAccountInfo)  
  
> [!NOTE]
>  <span data-ttu-id="232ca-111">Standard exempelmallarna ingår i följande dokumentation, men kan komma att ändras på grund av kontinuerliga förbättringar.</span><span class="sxs-lookup"><span data-stu-id="232ca-111">Sample default templates are included in the following documentation, but are subject to change due to continuous improvements.</span></span> <span data-ttu-id="232ca-112">Du kan visa live standardmallarna i developer-portalen genom att navigera till önskade enskilda mallar.</span><span class="sxs-lookup"><span data-stu-id="232ca-112">You can view the live default templates in the developer portal by navigating to the desired individual templates.</span></span> <span data-ttu-id="232ca-113">Mer information om hur du arbetar med mallar finns [hur du anpassar API Management developer-portalen med hjälp av mallar](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="232ca-113">For more information about working with templates, see [How to customize the API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="232ca-114"><a name="Profile"></a>Profil</span><span class="sxs-lookup"><span data-stu-id="232ca-114"><a name="Profile"></a> Profile</span></span>  
 <span data-ttu-id="232ca-115">Den **profil** mall kan du anpassa avsnittet användarens profil i användarprofilsidan i developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="232ca-115">The **profile** template allows you to customize the user profile section of the user profile page in the developer portal.</span></span>  
  
 <span data-ttu-id="232ca-116">![Användaren profilsida](./media/api-management-user-profile-templates/APIM-User-Profile-Page.png "APIM användarprofilen sida")</span><span class="sxs-lookup"><span data-stu-id="232ca-116">![User Profile Page](./media/api-management-user-profile-templates/APIM-User-Profile-Page.png "APIM User Profile Page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="232ca-117">Standardmall</span><span class="sxs-lookup"><span data-stu-id="232ca-117">Default template</span></span>  
  
```xml  
<div class="pull-right">  
  {% if canChangePassword == true %}  
  <a class="btn btn-default" id="ChangePassword" role="button" href="{{changePasswordUrl}}">{% localized "UserProfile|ButtonLabelChangePassword" %}</a>  
  {% endif %}  
  <a id="changeAccountInfo" href="{{changeNameOrEmailUrl}}" class="btn btn-default">  
    <span class="glyphicon glyphicon-user"></span>  
    <span>{% localized "UserProfile|ButtonLabelChangeAccountInfo" %}</span>  
  </a>  
</div>  
<h2>{% localized "SubscriptionStrings|PageTitleDeveloperProfile" %}</h2>  
<div class="container-fluid">  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="Email">{% localized "UserProfile|TextboxLabelEmail" %}</label>  
    </div>  
    <div class="col-sm-9" id="Email">{{email}}</div>  
  </div>  
  
  {% if isSystemUser != true %}  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="FirstName">{% localized "UserProfile|TextboxLabelEmailFirstName" %}</label>  
    </div>  
    <div class="col-sm-9" id="FirstName">{{FirstName}}</div>  
  </div>  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="LastName">{% localized "UserProfile|TextboxLabelEmailLastName" %}</label>  
    </div>  
    <div class="col-sm-9" id="LastName">{{LastName}}</div>  
  </div>  
  {% else %}  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="CompanyName">{% localized "UserProfile|TextboxLabelOrganizationName" %}</label>  
    </div>  
    <div class="col-sm-9" id="CompanyName">{{CompanyName}}</div>  
  </div>  
  <div class="row">  
    <div class="col-sm-3">  
      <label for="AddresserEmail">{% localized "UserProfile|TextboxLabelNotificationsSenderEmail" %}</label>  
    </div>  
    <div class="col-sm-9" id="AddresserEmail">{{AddresserEmail}}</div>  
  </div>  
  {% endif %}  
  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="232ca-118">Kontroller</span><span class="sxs-lookup"><span data-stu-id="232ca-118">Controls</span></span>  
 <span data-ttu-id="232ca-119">Den här mallen kan inte använda någon [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="232ca-119">This template may not use any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="232ca-120">Datamodell</span><span class="sxs-lookup"><span data-stu-id="232ca-120">Data model</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="232ca-121">Den [profil](#Profile), [program](#Applications), och [prenumerationer](#Subscriptions) mallar delar samma datamodellen och samma malldata tas emot.</span><span class="sxs-lookup"><span data-stu-id="232ca-121">The [Profile](#Profile), [Applications](#Applications), and [Subscriptions](#Subscriptions) templates share the same data model and receive the same template data.</span></span>  
  
|<span data-ttu-id="232ca-122">Egenskap</span><span class="sxs-lookup"><span data-stu-id="232ca-122">Property</span></span>|<span data-ttu-id="232ca-123">Typ</span><span class="sxs-lookup"><span data-stu-id="232ca-123">Type</span></span>|<span data-ttu-id="232ca-124">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="232ca-124">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="232ca-125">Förnamn</span><span class="sxs-lookup"><span data-stu-id="232ca-125">firstName</span></span>|<span data-ttu-id="232ca-126">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-126">string</span></span>|<span data-ttu-id="232ca-127">Den aktuella användaren förnamn.</span><span class="sxs-lookup"><span data-stu-id="232ca-127">First name of the current user.</span></span>|  
|<span data-ttu-id="232ca-128">Efternamn</span><span class="sxs-lookup"><span data-stu-id="232ca-128">lastName</span></span>|<span data-ttu-id="232ca-129">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-129">string</span></span>|<span data-ttu-id="232ca-130">Efternamn för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-130">Last name of the current user.</span></span>|  
|<span data-ttu-id="232ca-131">Företagsnamn</span><span class="sxs-lookup"><span data-stu-id="232ca-131">companyName</span></span>|<span data-ttu-id="232ca-132">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-132">string</span></span>|<span data-ttu-id="232ca-133">Företagsnamn för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-133">The company name of the current user.</span></span>|  
|<span data-ttu-id="232ca-134">addresserEmail</span><span class="sxs-lookup"><span data-stu-id="232ca-134">addresserEmail</span></span>|<span data-ttu-id="232ca-135">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-135">string</span></span>|<span data-ttu-id="232ca-136">E-postadressen för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-136">Email address of the current user.</span></span>|  
|<span data-ttu-id="232ca-137">developersUsageStatisticsLinkk</span><span class="sxs-lookup"><span data-stu-id="232ca-137">developersUsageStatisticsLinkk</span></span>|<span data-ttu-id="232ca-138">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-138">string</span></span>|<span data-ttu-id="232ca-139">Relativ URL för att visa analytics för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-139">Relative URL to view analytics for the current user.</span></span>|  
|<span data-ttu-id="232ca-140">prenumerationer</span><span class="sxs-lookup"><span data-stu-id="232ca-140">subscriptions</span></span>|<span data-ttu-id="232ca-141">Samling av [prenumeration](api-management-template-data-model-reference.md#Subscription) entiteter.</span><span class="sxs-lookup"><span data-stu-id="232ca-141">Collection of [Subscription](api-management-template-data-model-reference.md#Subscription) entities.</span></span>|<span data-ttu-id="232ca-142">Prenumerationer för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-142">The subscriptions for the current user.</span></span>|  
|<span data-ttu-id="232ca-143">program</span><span class="sxs-lookup"><span data-stu-id="232ca-143">applications</span></span>|<span data-ttu-id="232ca-144">Samling av [programmet](api-management-template-data-model-reference.md#Application) entiteter.</span><span class="sxs-lookup"><span data-stu-id="232ca-144">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="232ca-145">Program för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-145">The applications of the current user.</span></span>|  
|<span data-ttu-id="232ca-146">changePasswordUrl</span><span class="sxs-lookup"><span data-stu-id="232ca-146">changePasswordUrl</span></span>|<span data-ttu-id="232ca-147">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-147">string</span></span>|<span data-ttu-id="232ca-148">Relativ URL att ändra den aktuella användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="232ca-148">The relative URL to change the current user's password.</span></span>|  
|<span data-ttu-id="232ca-149">changeNameOrEmailUrl</span><span class="sxs-lookup"><span data-stu-id="232ca-149">changeNameOrEmailUrl</span></span>|<span data-ttu-id="232ca-150">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-150">string</span></span>|<span data-ttu-id="232ca-151">Relativ URL som ändra namn och e-post för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-151">The relative URL to change the name and email for the current user.</span></span>|  
|<span data-ttu-id="232ca-152">canChangePassword</span><span class="sxs-lookup"><span data-stu-id="232ca-152">canChangePassword</span></span>|<span data-ttu-id="232ca-153">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="232ca-153">boolean</span></span>|<span data-ttu-id="232ca-154">Om den aktuella användaren kan ändra sina lösenord.</span><span class="sxs-lookup"><span data-stu-id="232ca-154">Whether the current user can change their password.</span></span>|  
|<span data-ttu-id="232ca-155">isSystemUser</span><span class="sxs-lookup"><span data-stu-id="232ca-155">isSystemUser</span></span>|<span data-ttu-id="232ca-156">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="232ca-156">boolean</span></span>|<span data-ttu-id="232ca-157">Om den aktuella användaren är medlem i någon av inbyggda [grupper](api-management-key-concepts.md#groups).</span><span class="sxs-lookup"><span data-stu-id="232ca-157">Whether the current user is a member of one of the built-in [groups](api-management-key-concepts.md#groups).</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="232ca-158">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="232ca-158">Sample template data</span></span>  
  
```json  
{  
    "firstName": "Administrator",  
    "lastName": "",  
    "companyName": "Contoso",  
    "addresserEmail": "apimgmt-noreply@mail.windowsazure.com",  
    "email": "admin@live.com",  
    "developersUsageStatisticsLink": "/Developer/Analytics",  
    "subscriptions": [  
        {  
            "Id": "57026e30de15d80041070001",  
            "ProductId": "57026e30de15d80041060001",  
            "ProductTitle": "Starter",  
            "ProductDescription": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060001",  
            "State": "Active",  
            "DisplayName": "Starter  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.847",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "b6b2870953d04420a4e02c58f2c08e74",  
            "SecondaryKey": "cfe28d5a1cd04d8abc93f48352076ea5",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070001/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070001/Renew"  
        },  
        {  
            "Id": "57026e30de15d80041070002",  
            "ProductId": "57026e30de15d80041060002",  
            "ProductTitle": "Unlimited",  
            "ProductDescription": "Subscribers have completely unlimited access to the API. Administrator approval is required.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060002",  
            "State": "Active",  
            "DisplayName": "Unlimited  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.923",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "8fe7843c36de4cceb4728e6cae297336",  
            "SecondaryKey": "96c850d217e74acf9b514ff8a5b38551",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070002/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070002/Renew"  
        }  
    ],  
    "applications": [],  
    "changePasswordUrl": "/account/password/change",  
    "changeNameOrEmailUrl": "/account/update",  
    "canChangePassword": false,  
    "isSystemUser": true  
}  
```  
  
##  <span data-ttu-id="232ca-159"><a name="Subscriptions"></a>Prenumerationer</span><span class="sxs-lookup"><span data-stu-id="232ca-159"><a name="Subscriptions"></a> Subscriptions</span></span>  
 <span data-ttu-id="232ca-160">Den **prenumerationer** mall kan du anpassa avsnittet prenumerationer i användarprofilsidan i developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="232ca-160">The **Subscriptions** template allows you to customize the subscriptions section of the user profile page in the developer portal.</span></span>  
  
 <span data-ttu-id="232ca-161">![Användaren prenumerationssidan](./media/api-management-user-profile-templates/APIM-User-Subscription-Page.png "APIM användarsidan prenumeration")</span><span class="sxs-lookup"><span data-stu-id="232ca-161">![User Subscription Page](./media/api-management-user-profile-templates/APIM-User-Subscription-Page.png "APIM User Subscription Page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="232ca-162">Standardmall</span><span class="sxs-lookup"><span data-stu-id="232ca-162">Default template</span></span>  
  
```xml  
<div class="ap-account-subscriptions">  
  <a href="{{developersUsageStatisticsLink}}" id="UsageStatistics" class="btn btn-default pull-right">  
    <span class="glyphicon glyphicon-stats"></span>  
    <span>{% localized "SubscriptionListStrings|WebDevelopersUsageStatisticsLink" %}</span>  
  </a>  
  
  <h2>{% localized "SubscriptionListStrings|WebDevelopersYourSubscriptions" %}</h2>  
  
  <table class="table">  
    <thead>  
      <tr>  
        <th>Subscription details</th>  
        <th>Product</th>  
        <th>{% localized "SubscriptionListStrings|WebDevelopersSubscriptionTableStateHeader" %}</th>  
        <th>Action</th>  
      </tr>  
    </thead>  
    <tbody>  
      {% if subscriptions.size == 0 %}  
      <tr>  
        <td class="text-center" colspan="4">  
          {% localized "CommonResources|NoItemsToDisplay" %}  
        </td>  
      </tr>  
      {% else %}  
      {% for subscription in subscriptions %}  
      <tr id="{{subscription.id}}" {% if subscription.hasExpired %} class="expired" {% endif %}>  
        <td>  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|SubscriptionPropertyLabelName" %}</label>  
            <div class="col-lg-6">  
              {{ subscription.displayName }}  
            </div>  
            <div class="col-lg-2">  
              <a class="btn-link" href="/Subscriptions/{{subscription.id}}/Rename">Rename</a>  
            </div>  
            <div class="clearfix"></div>  
          </div>  
          {% if subscription.isAwaitingApproval %}  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|SubscriptionPropertyLabelRequestedDate" %}</label>  
            <div class="col-lg-6">  
              {{ subscription.createdDate | date:"MM/dd/yyyy" }}  
            </div>  
          </div>  
          {% else %}  
          {% if subscription.isRejected == false %}  
          {% if subscription.startDate %}  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|SubscriptionPropertyLabelStartedDate" %}</label>  
            <div class="col-lg-6">  
              {{ subscription.startDate | date:"MM/dd/yyyy" }}  
            </div>  
          </div>  
          {% endif %}  
  
          <!-- ko with: Developers.Account.Root.account.key('{{subscription.primaryKey}}', '{{subscription.id}}', true) -->  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|WebDevelopersPrimaryKey" %}</label>  
            <div class="col-lg-6">  
              <code data-bind="text: $data.displayKey()" id="primary_{{subscription.id}}"></code>  
            </div>  
            <div class="col-lg-2">  
              <!-- ko if: !requestInProgress() -->  
              <div class="nowrap">  
                <a href="#" class="btn-link" id="togglePrimary_{{subscription.id}}" data-bind="click: toggleKeyDisplay, text: toggleKeyLabel"></a>  
                |  
                <a href="#" class="btn-link" id="regeneratePrimary_{{subscription.id}}" data-bind="click: regenerateKey, text: regenerateKeyLabel"></a>  
              </div>  
              <!-- /ko -->  
              <!-- ko if: requestInProgress() -->  
              <div class="progress progress-striped active">  
                <div class="progress-bar" role="progressbar" aria-valuenow="100" aria-valuemin="0" aria-valuemax="100" style="width: 100%">  
                  <span class="sr-only"></span>  
                </div>  
              </div>  
              <!-- /ko -->  
            </div>  
            <div class="clearfix"></div>  
          </div>  
          <!-- /ko -->  
          <!-- ko with: Developers.Account.Root.account.key('{{subscription.secondaryKey}}', '{{subscription.id}}', false) -->  
          <div class="row">  
            <label class="col-lg-3">{% localized "SubscriptionListStrings|WebDevelopersSecondaryKey" %}</label>  
            <div class="col-lg-6">  
              <code data-bind="text: $data.displayKey()" id="secondary_{{subscription.id}}"></code>  
            </div>  
            <div class="col-lg-2">  
              <div class="nowrap">  
                <a href="#" class="btn-link" id="toggleSecondary_{{subscription.id}}" data-bind="click: toggleKeyDisplay, text: toggleKeyLabel">{% localized "SubscriptionListStrings|ButtonLabelShowKey" %}</a>  
                |  
                <a href="#" class="btn-link" id="regenerateSecondary_{{subscription.id}}" data-bind="click: regenerateKey, text: regenerateKeyLabel">{% localized "SubscriptionListStrings|WebDevelopersRegenerateLink" %}</a>  
              </div>  
            </div>  
            <div class="clearfix"> </div>  
          </div>  
          <!-- /ko -->  
          {% endif %}  
          {% endif %}  
        </td>  
        <td>  
          <a href="{{subscription.productDetailsUrl}}">{{subscription.productTitle}}</a>  
        </td>  
        <td>  
          <strong>  
            {{subscription.state}}  
          </strong>  
        </td>  
        <td>  
          <div class="nowrap">  
            {% if subscription.canBeCancelled %}  
            <subscription-cancel params="{ subscriptionId: '{{subscription.id}}', cancelUrl: '{{subscription.cancelUrl}}' }"></subscription-cancel>  
            {% endif %}  
          </div>  
        </td>  
      </tr>  
      {% endfor %}  
      {% endif %}  
    </tbody>  
  </table>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="232ca-163">Kontroller</span><span class="sxs-lookup"><span data-stu-id="232ca-163">Controls</span></span>  
 <span data-ttu-id="232ca-164">Den här mallen kan du använda följande [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="232ca-164">This template may use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="232ca-165">Avbryt prenumeration</span><span class="sxs-lookup"><span data-stu-id="232ca-165">subscription-cancel</span></span>](api-management-page-controls.md#subscription-cancel)  
  
### <a name="data-model"></a><span data-ttu-id="232ca-166">Datamodell</span><span class="sxs-lookup"><span data-stu-id="232ca-166">Data model</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="232ca-167">Den [profil](#Profile), [program](#Applications), och [prenumerationer](#Subscriptions) mallar delar samma datamodellen och samma malldata tas emot.</span><span class="sxs-lookup"><span data-stu-id="232ca-167">The [Profile](#Profile), [Applications](#Applications), and [Subscriptions](#Subscriptions) templates share the same data model and receive the same template data.</span></span>  
  
|<span data-ttu-id="232ca-168">Egenskap</span><span class="sxs-lookup"><span data-stu-id="232ca-168">Property</span></span>|<span data-ttu-id="232ca-169">Typ</span><span class="sxs-lookup"><span data-stu-id="232ca-169">Type</span></span>|<span data-ttu-id="232ca-170">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="232ca-170">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="232ca-171">Förnamn</span><span class="sxs-lookup"><span data-stu-id="232ca-171">firstName</span></span>|<span data-ttu-id="232ca-172">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-172">string</span></span>|<span data-ttu-id="232ca-173">Den aktuella användaren förnamn.</span><span class="sxs-lookup"><span data-stu-id="232ca-173">First name of the current user.</span></span>|  
|<span data-ttu-id="232ca-174">Efternamn</span><span class="sxs-lookup"><span data-stu-id="232ca-174">lastName</span></span>|<span data-ttu-id="232ca-175">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-175">string</span></span>|<span data-ttu-id="232ca-176">Efternamn för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-176">Last name of the current user.</span></span>|  
|<span data-ttu-id="232ca-177">Företagsnamn</span><span class="sxs-lookup"><span data-stu-id="232ca-177">companyName</span></span>|<span data-ttu-id="232ca-178">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-178">string</span></span>|<span data-ttu-id="232ca-179">Företagsnamn för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-179">The company name of the current user.</span></span>|  
|<span data-ttu-id="232ca-180">addresserEmail</span><span class="sxs-lookup"><span data-stu-id="232ca-180">addresserEmail</span></span>|<span data-ttu-id="232ca-181">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-181">string</span></span>|<span data-ttu-id="232ca-182">E-postadressen för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-182">Email address of the current user.</span></span>|  
|<span data-ttu-id="232ca-183">developersUsageStatisticsLinkk</span><span class="sxs-lookup"><span data-stu-id="232ca-183">developersUsageStatisticsLinkk</span></span>|<span data-ttu-id="232ca-184">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-184">string</span></span>|<span data-ttu-id="232ca-185">Relativ URL för att visa analytics för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-185">Relative URL to view analytics for the current user.</span></span>|  
|<span data-ttu-id="232ca-186">prenumerationer</span><span class="sxs-lookup"><span data-stu-id="232ca-186">subscriptions</span></span>|<span data-ttu-id="232ca-187">Samling av [prenumeration](api-management-template-data-model-reference.md#Subscription) entiteter.</span><span class="sxs-lookup"><span data-stu-id="232ca-187">Collection of [Subscription](api-management-template-data-model-reference.md#Subscription) entities.</span></span>|<span data-ttu-id="232ca-188">Prenumerationer för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-188">The subscriptions for the current user.</span></span>|  
|<span data-ttu-id="232ca-189">program</span><span class="sxs-lookup"><span data-stu-id="232ca-189">applications</span></span>|<span data-ttu-id="232ca-190">Samling av [programmet](api-management-template-data-model-reference.md#Application) entiteter.</span><span class="sxs-lookup"><span data-stu-id="232ca-190">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="232ca-191">Program för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-191">The applications of the current user.</span></span>|  
|<span data-ttu-id="232ca-192">changePasswordUrl</span><span class="sxs-lookup"><span data-stu-id="232ca-192">changePasswordUrl</span></span>|<span data-ttu-id="232ca-193">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-193">string</span></span>|<span data-ttu-id="232ca-194">Relativ URL att ändra den aktuella användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="232ca-194">The relative URL to change the current user's password.</span></span>|  
|<span data-ttu-id="232ca-195">changeNameOrEmailUrl</span><span class="sxs-lookup"><span data-stu-id="232ca-195">changeNameOrEmailUrl</span></span>|<span data-ttu-id="232ca-196">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-196">string</span></span>|<span data-ttu-id="232ca-197">Relativ URL som ändra namn och e-post för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-197">The relative URL to change the name and email for the current user.</span></span>|  
|<span data-ttu-id="232ca-198">canChangePassword</span><span class="sxs-lookup"><span data-stu-id="232ca-198">canChangePassword</span></span>|<span data-ttu-id="232ca-199">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="232ca-199">boolean</span></span>|<span data-ttu-id="232ca-200">Om den aktuella användaren kan ändra sina lösenord.</span><span class="sxs-lookup"><span data-stu-id="232ca-200">Whether the current user can change their password.</span></span>|  
|<span data-ttu-id="232ca-201">isSystemUser</span><span class="sxs-lookup"><span data-stu-id="232ca-201">isSystemUser</span></span>|<span data-ttu-id="232ca-202">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="232ca-202">boolean</span></span>|<span data-ttu-id="232ca-203">Om den aktuella användaren är medlem i någon av inbyggda [grupper](api-management-key-concepts.md#groups).</span><span class="sxs-lookup"><span data-stu-id="232ca-203">Whether the current user is a member of one of the built-in [groups](api-management-key-concepts.md#groups).</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="232ca-204">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="232ca-204">Sample template data</span></span>  
  
```json  
{  
    "firstName": "Administrator",  
    "lastName": "",  
    "companyName": "Contoso",  
    "addresserEmail": "apimgmt-noreply@mail.windowsazure.com",  
    "email": "admin@live.com",  
    "developersUsageStatisticsLink": "/Developer/Analytics",  
    "subscriptions": [  
        {  
            "Id": "57026e30de15d80041070001",  
            "ProductId": "57026e30de15d80041060001",  
            "ProductTitle": "Starter",  
            "ProductDescription": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060001",  
            "State": "Active",  
            "DisplayName": "Starter  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.847",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "b6b2870953d04420a4e02c58f2c08e74",  
            "SecondaryKey": "cfe28d5a1cd04d8abc93f48352076ea5",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070001/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070001/Renew"  
        },  
        {  
            "Id": "57026e30de15d80041070002",  
            "ProductId": "57026e30de15d80041060002",  
            "ProductTitle": "Unlimited",  
            "ProductDescription": "Subscribers have completely unlimited access to the API. Administrator approval is required.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060002",  
            "State": "Active",  
            "DisplayName": "Unlimited  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.923",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "8fe7843c36de4cceb4728e6cae297336",  
            "SecondaryKey": "96c850d217e74acf9b514ff8a5b38551",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070002/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070002/Renew"  
        }  
    ],  
    "applications": [],  
    "changePasswordUrl": "/account/password/change",  
    "changeNameOrEmailUrl": "/account/update",  
    "canChangePassword": false,  
    "isSystemUser": true  
}  
```  
  
##  <span data-ttu-id="232ca-205"><a name="Applications"></a>Program</span><span class="sxs-lookup"><span data-stu-id="232ca-205"><a name="Applications"></a> Applications</span></span>  
 <span data-ttu-id="232ca-206">Den **program** mall kan du anpassa avsnittet prenumerationer i användarprofilsidan i developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="232ca-206">The **Applications** template allows you to customize the subscriptions section of the user profile page in the developer portal.</span></span>  
  
 <span data-ttu-id="232ca-207">![Användarsida konto program](./media/api-management-user-profile-templates/APIM-User-Account-Applications-Page.png "APIM användarkontot Programsidan")</span><span class="sxs-lookup"><span data-stu-id="232ca-207">![User Account Applications Page](./media/api-management-user-profile-templates/APIM-User-Account-Applications-Page.png "APIM User Account Applications Page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="232ca-208">Standardmall</span><span class="sxs-lookup"><span data-stu-id="232ca-208">Default template</span></span>  
  
```xml  
<div class="ap-account-applications">  
  <a id="RegisterApplication" href="/Developer/Applications/Register" class="btn btn-success pull-right">  
    <span class="glyphicon glyphicon-plus"></span>  
    <span>{% localized "ApplicationListStrings|WebDevelopersRegisterAppLink" %}</span>  
  </a>  
  <h2>{% localized "ApplicationListStrings|WebDevelopersYourApplicationsHeader" %}</h2>  
  
  <table class="table">  
    <thead>  
      <tr>  
        <th class="col-md-8">{% localized "ApplicationListStrings|WebDevelopersAppTableNameHeader" %}</th>  
        <th class="col-md-2">{% localized "ApplicationListStrings|WebDevelopersAppTableCategoryHeader" %}</th>  
        <th class="col-md-2" colspan="2">{% localized "ApplicationListStrings|WebDevelopersAppTableStateHeader" %}</th>  
      </tr>  
    </thead>  
    <tbody>  
  
      {% if applications.size == 0 %}  
  
      <tr>  
        <td class="col-md-12 text-center" colspan="4">  
          {% localized "CommonResources|NoItemsToDisplay" %}  
        </td>  
      </tr>  
  
      {% else %}  
  
      {% for app in applications %}  
      <tr>  
        <td class="col-md-8">  
          {{app.title}}  
        </td>  
        <td class="col-md-2">  
          {{app.categoryName}}  
        </td>  
        <td class="col-md-2">  
          <strong>  
            {% case app.state %}  
            {% when ApplicationStateModel.Registered %}  
            {% localized "ApplicationListStrings|WebDevelopersAppNotSubminted" %}  
  
            {% when ApplicationStateModel.Unpublished %}  
            {% localized "ApplicationListStrings|WebDevelopersAppNotPublished" %}  
  
            {% else %}  
            {{ app.state }}  
            {% endcase %}  
          </strong>  
        </td>  
        <td class="col-md-1">  
          <div class="nowrap">  
            {% if app.state != ApplicationStateModel.Submitted and app.state != ApplicationStateModel.Published %}  
            <app-actions params="{ appId: '{{app.id}}' }"></app-actions>  
            {% endif %}  
          </div>  
        </td>  
      </tr>  
      {% endfor %}  
  
      {% endif %}  
    </tbody>  
  </table>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="232ca-209">Kontroller</span><span class="sxs-lookup"><span data-stu-id="232ca-209">Controls</span></span>  
 <span data-ttu-id="232ca-210">Den här mallen kan du använda följande [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="232ca-210">This template may use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="232ca-211">App-åtgärder</span><span class="sxs-lookup"><span data-stu-id="232ca-211">app-actions</span></span>](api-management-page-controls.md#app-actions)  
  
### <a name="data-model"></a><span data-ttu-id="232ca-212">Datamodell</span><span class="sxs-lookup"><span data-stu-id="232ca-212">Data model</span></span>  
  
> [!NOTE]
>  <span data-ttu-id="232ca-213">Den [profil](#Profile), [program](#Applications), och [prenumerationer](#Subscriptions) mallar delar samma datamodellen och samma malldata tas emot.</span><span class="sxs-lookup"><span data-stu-id="232ca-213">The [Profile](#Profile), [Applications](#Applications), and [Subscriptions](#Subscriptions) templates share the same data model and receive the same template data.</span></span>  
  
|<span data-ttu-id="232ca-214">Egenskap</span><span class="sxs-lookup"><span data-stu-id="232ca-214">Property</span></span>|<span data-ttu-id="232ca-215">Typ</span><span class="sxs-lookup"><span data-stu-id="232ca-215">Type</span></span>|<span data-ttu-id="232ca-216">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="232ca-216">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="232ca-217">Förnamn</span><span class="sxs-lookup"><span data-stu-id="232ca-217">firstName</span></span>|<span data-ttu-id="232ca-218">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-218">string</span></span>|<span data-ttu-id="232ca-219">Den aktuella användaren förnamn.</span><span class="sxs-lookup"><span data-stu-id="232ca-219">First name of the current user.</span></span>|  
|<span data-ttu-id="232ca-220">Efternamn</span><span class="sxs-lookup"><span data-stu-id="232ca-220">lastName</span></span>|<span data-ttu-id="232ca-221">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-221">string</span></span>|<span data-ttu-id="232ca-222">Efternamn för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-222">Last name of the current user.</span></span>|  
|<span data-ttu-id="232ca-223">Företagsnamn</span><span class="sxs-lookup"><span data-stu-id="232ca-223">companyName</span></span>|<span data-ttu-id="232ca-224">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-224">string</span></span>|<span data-ttu-id="232ca-225">Företagsnamn för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-225">The company name of the current user.</span></span>|  
|<span data-ttu-id="232ca-226">addresserEmail</span><span class="sxs-lookup"><span data-stu-id="232ca-226">addresserEmail</span></span>|<span data-ttu-id="232ca-227">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-227">string</span></span>|<span data-ttu-id="232ca-228">E-postadressen för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-228">Email address of the current user.</span></span>|  
|<span data-ttu-id="232ca-229">developersUsageStatisticsLinkk</span><span class="sxs-lookup"><span data-stu-id="232ca-229">developersUsageStatisticsLinkk</span></span>|<span data-ttu-id="232ca-230">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-230">string</span></span>|<span data-ttu-id="232ca-231">Relativ URL för att visa analytics för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-231">Relative URL to view analytics for the current user.</span></span>|  
|<span data-ttu-id="232ca-232">prenumerationer</span><span class="sxs-lookup"><span data-stu-id="232ca-232">subscriptions</span></span>|<span data-ttu-id="232ca-233">Samling av [prenumeration](api-management-template-data-model-reference.md#Subscription) entiteter.</span><span class="sxs-lookup"><span data-stu-id="232ca-233">Collection of [Subscription](api-management-template-data-model-reference.md#Subscription) entities.</span></span>|<span data-ttu-id="232ca-234">Prenumerationer för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-234">The subscriptions for the current user.</span></span>|  
|<span data-ttu-id="232ca-235">program</span><span class="sxs-lookup"><span data-stu-id="232ca-235">applications</span></span>|<span data-ttu-id="232ca-236">Samling av [programmet](api-management-template-data-model-reference.md#Application) entiteter.</span><span class="sxs-lookup"><span data-stu-id="232ca-236">Collection of [Application](api-management-template-data-model-reference.md#Application) entities.</span></span>|<span data-ttu-id="232ca-237">Program för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-237">The applications of the current user.</span></span>|  
|<span data-ttu-id="232ca-238">changePasswordUrl</span><span class="sxs-lookup"><span data-stu-id="232ca-238">changePasswordUrl</span></span>|<span data-ttu-id="232ca-239">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-239">string</span></span>|<span data-ttu-id="232ca-240">Relativ URL att ändra den aktuella användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="232ca-240">The relative URL to change the current user's password.</span></span>|  
|<span data-ttu-id="232ca-241">changeNameOrEmailUrl</span><span class="sxs-lookup"><span data-stu-id="232ca-241">changeNameOrEmailUrl</span></span>|<span data-ttu-id="232ca-242">Sträng</span><span class="sxs-lookup"><span data-stu-id="232ca-242">string</span></span>|<span data-ttu-id="232ca-243">Relativ URL som ändra namn och e-post för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="232ca-243">The relative URL to change the name and email for the current user.</span></span>|  
|<span data-ttu-id="232ca-244">canChangePassword</span><span class="sxs-lookup"><span data-stu-id="232ca-244">canChangePassword</span></span>|<span data-ttu-id="232ca-245">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="232ca-245">boolean</span></span>|<span data-ttu-id="232ca-246">Om den aktuella användaren kan ändra sina lösenord.</span><span class="sxs-lookup"><span data-stu-id="232ca-246">Whether the current user can change their password.</span></span>|  
|<span data-ttu-id="232ca-247">isSystemUser</span><span class="sxs-lookup"><span data-stu-id="232ca-247">isSystemUser</span></span>|<span data-ttu-id="232ca-248">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="232ca-248">boolean</span></span>|<span data-ttu-id="232ca-249">Om den aktuella användaren är medlem i någon av inbyggda [grupper](api-management-key-concepts.md#groups).</span><span class="sxs-lookup"><span data-stu-id="232ca-249">Whether the current user is a member of one of the built-in [groups](api-management-key-concepts.md#groups).</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="232ca-250">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="232ca-250">Sample template data</span></span>  
  
```json  
{  
    "firstName": "Administrator",  
    "lastName": "",  
    "companyName": "Contoso",  
    "addresserEmail": "apimgmt-noreply@mail.windowsazure.com",  
    "email": "admin@live.com",  
    "developersUsageStatisticsLink": "/Developer/Analytics",  
    "subscriptions": [  
        {  
            "Id": "57026e30de15d80041070001",  
            "ProductId": "57026e30de15d80041060001",  
            "ProductTitle": "Starter",  
            "ProductDescription": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060001",  
            "State": "Active",  
            "DisplayName": "Starter  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.847",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "b6b2870953d04420a4e02c58f2c08e74",  
            "SecondaryKey": "cfe28d5a1cd04d8abc93f48352076ea5",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070001/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070001/Renew"  
        },  
        {  
            "Id": "57026e30de15d80041070002",  
            "ProductId": "57026e30de15d80041060002",  
            "ProductTitle": "Unlimited",  
            "ProductDescription": "Subscribers have completely unlimited access to the API. Administrator approval is required.",  
            "ProductDetailsUrl": "/Products/57026e30de15d80041060002",  
            "State": "Active",  
            "DisplayName": "Unlimited  (default)",  
            "CreatedDate": "2016-04-04T13:37:52.923",  
            "CanBeCancelled": true,  
            "IsAwaitingApproval": false,  
            "StartDate": null,  
            "ExpirationDate": null,  
            "NotificationDate": null,  
            "PrimaryKey": "8fe7843c36de4cceb4728e6cae297336",  
            "SecondaryKey": "96c850d217e74acf9b514ff8a5b38551",  
            "UserId": 1,  
            "CanBeRenewed": false,  
            "HasExpired": false,  
            "IsRejected": false,  
            "CancelUrl": "/Subscriptions/57026e30de15d80041070002/Cancel",  
            "RenewUrl": "/Subscriptions/57026e30de15d80041070002/Renew"  
        }  
    ],  
    "applications": [],  
    "changePasswordUrl": "/account/password/change",  
    "changeNameOrEmailUrl": "/account/update",  
    "canChangePassword": false,  
    "isSystemUser": true  
}  
```  
  
##  <span data-ttu-id="232ca-251"><a name="UpdateAccountInfo"></a>Uppdatera kontoinformation</span><span class="sxs-lookup"><span data-stu-id="232ca-251"><a name="UpdateAccountInfo"></a> Update account info</span></span>  
 <span data-ttu-id="232ca-252">Den **Uodate kontoinformation** mall kan du anpassa den **uppdatera kontoinformation** sida i developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="232ca-252">The **Uodate account info** template allows you to customize the **Update account information** page in the developer portal.</span></span>  
  
 <span data-ttu-id="232ca-253">![Information om sidan Developer Portal mallar för användarkonton](./media/api-management-user-profile-templates/APIM-User-Account-Info-Page-Developer-Portal-Templates.png "APIM Info sidan Developer Portal mallar för användarkonton")</span><span class="sxs-lookup"><span data-stu-id="232ca-253">![User Account Info Page Developer Portal Templates](./media/api-management-user-profile-templates/APIM-User-Account-Info-Page-Developer-Portal-Templates.png "APIM User Account Info Page Developer Portal Templates")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="232ca-254">Standardmall</span><span class="sxs-lookup"><span data-stu-id="232ca-254">Default template</span></span>  
  
```xml  
<div class="row">  
  <div class="col-sm-6 col-md-6">  
    <div class="form-group">  
      <label for="Email">{% localized "SigninResources|TextboxLabelEmail" %}</label>  
      <input autofocus="autofocus" class="form-control" id="Email" name="Email" type="text" value="{{email}}">  
    </div>  
    <div class="form-group">  
      <label for="FirstName">{% localized "SigninResources|TextboxLabelEmailFirstName" %}</label>  
      <input class="form-control" id="FirstName" name="FirstName" type="text" value="{{firstName}}">  
    </div>  
    <div class="form-group">  
      <label for="LastName">{% localized "SigninResources|TextboxLabelEmailLastName" %}</label>  
      <input class="form-control" id="LastName" name="LastName" type="text" value="{{lastName}}">  
    </div>  
    <div class="form-group">  
      <label for="Password">{% localized "SigninResources|WebAuthenticationSigninPasswordLabel" %}</label>  
      <input class="form-control" id="Password" name="Password" type="password">  
    </div>  
  </div>  
</div>  
  
<button type="submit" class="btn btn-primary" id="UpdateProfile">  
  {% localized "UpdateProfileStrings|ButtonLabelUpdateProfile" %}  
</button>  
<a class="btn btn-default" href="/developer" role="button">  
  {% localized "CommonStrings|ButtonLabelCancel" %}  
</a>  
```  
  
### <a name="controls"></a><span data-ttu-id="232ca-255">Kontroller</span><span class="sxs-lookup"><span data-stu-id="232ca-255">Controls</span></span>  
 <span data-ttu-id="232ca-256">Den här mallen kan inte använda någon [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="232ca-256">This template may not use any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="232ca-257">Datamodell</span><span class="sxs-lookup"><span data-stu-id="232ca-257">Data model</span></span>  
 <span data-ttu-id="232ca-258">[Information om användarkonto](api-management-template-data-model-reference.md#UserAccountInfo) entitet.</span><span class="sxs-lookup"><span data-stu-id="232ca-258">[User account info](api-management-template-data-model-reference.md#UserAccountInfo) entity.</span></span>  
  
### <a name="sample-template-data"></a><span data-ttu-id="232ca-259">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="232ca-259">Sample template data</span></span>  
  
```json  
{  
    "FirstName": "Administrator",  
    "LastName": "",  
    "Email": "admin@live.com",  
    "Password": null,  
    "NameIdentifier": null,  
    "ProviderName": null,  
    "IsBasicAccount": false  
}  
```

## <a name="next-steps"></a><span data-ttu-id="232ca-260">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="232ca-260">Next steps</span></span>
<span data-ttu-id="232ca-261">Mer information om hur du arbetar med mallar finns [hur du anpassar API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="232ca-261">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>