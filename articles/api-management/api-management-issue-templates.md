---
title: aaaIssue mallar i Azure API Management | Microsoft Docs
description: "Lär dig hur toocustomize hello innehåll hello problemet sidor i hello developer-portalen i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 47da4bb2-426e-4e53-8fa7-214ee2e3ab37
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: e12902e52c164f73902a97f15ea550790dfecf1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="issue-templates-in-azure-api-management"></a><span data-ttu-id="9c0e3-103">Utfärda mallar i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="9c0e3-103">Issue templates in Azure API Management</span></span>
<span data-ttu-id="9c0e3-104">Azure API Management ger du hello möjlighet toocustomize hello innehållet i developer portalens sidor med hjälp av en uppsättning mallar som konfigurerar deras innehåll.</span><span class="sxs-lookup"><span data-stu-id="9c0e3-104">Azure API Management provides you hello ability toocustomize hello content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="9c0e3-105">Med hjälp av [DotLiquid](http://dotliquidmarkup.org/) syntax och hello redigeringsprogram, t.ex [DotLiquid för Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), och en angiven uppsättning lokaliserade [String resurser](api-management-template-resources.md#strings), [ Glyf resurser](api-management-template-resources.md#glyphs), och [sidan kontroller](api-management-page-controls.md), du har stor flexibilitet tooconfigure hello innehåll hello sidor som du vill använda dessa mallar.</span><span class="sxs-lookup"><span data-stu-id="9c0e3-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and hello editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility tooconfigure hello content of hello pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="9c0e3-106">hello mallar i det här avsnittet kan du toocustomize hello innehåll hello problemet sidor i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="9c0e3-106">hello templates in this section allow you toocustomize hello content of hello Issue pages in hello developer portal.</span></span>  
  
-   [<span data-ttu-id="9c0e3-107">Problemet lista</span><span class="sxs-lookup"><span data-stu-id="9c0e3-107">Issue list</span></span>](#IssueList)  
  
> [!NOTE]
>  <span data-ttu-id="9c0e3-108">Standard exempelmallarna ingår i hello följande dokumentation, men är ämne toochange på grund av toocontinuous förbättringar.</span><span class="sxs-lookup"><span data-stu-id="9c0e3-108">Sample default templates are included in hello following documentation, but are subject toochange due toocontinuous improvements.</span></span> <span data-ttu-id="9c0e3-109">Du kan visa hello live standardmallarna i hello developer-portalen genom att gå toohello önskad enskilda mallar.</span><span class="sxs-lookup"><span data-stu-id="9c0e3-109">You can view hello live default templates in hello developer portal by navigating toohello desired individual templates.</span></span> <span data-ttu-id="9c0e3-110">Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="9c0e3-110">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>  
  
##  <span data-ttu-id="9c0e3-111"><a name="IssueList"></a>Problemet lista</span><span class="sxs-lookup"><span data-stu-id="9c0e3-111"><a name="IssueList"></a> Issue list</span></span>  
 <span data-ttu-id="9c0e3-112">Hej **problemet listan** mall kan du toocustomize hello brödtext hello problemet sidan i hello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="9c0e3-112">hello **Issue list** template allows you toocustomize hello body of hello issue list page in hello developer portal.</span></span>  
  
 <span data-ttu-id="9c0e3-113">![Utfärda listan Developer-portalen](./media/api-management-issue-templates/APIM-Issue-List-Developer-Portal.png "APIM problemet listan Developer-portalen")</span><span class="sxs-lookup"><span data-stu-id="9c0e3-113">![Issue List Developer Portal](./media/api-management-issue-templates/APIM-Issue-List-Developer-Portal.png "APIM Issue List Developer Portal")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="9c0e3-114">Standardmall</span><span class="sxs-lookup"><span data-stu-id="9c0e3-114">Default template</span></span>  
  
```xml  
<div class="row">  
  <div class="col-md-9">  
    <h2>{% localized "IssuesStrings|WebIssuesIndexTitle" %}</h2>  
  </div>  
</div>  
<div class="row">  
  <div class="col-md-12">  
    {% if issues.size > 0 %}  
    <ul class="list-unstyled">  
      {% capture reportedBy %}{% localized "IssuesStrings|WebIssuesStatusReportedBy" %}{% endcapture %}  
      {% assign replaceString0 = '{0}' %}  
      {% assign replaceString1 = '{1}' %}  
      {% for issue in issues %}  
      <li>  
        <h3>  
          <a href="/issues/{{issue.id}}">{{issue.title}}</a>  
        </h3>  
        <p>{{issue.description}}</p>  
        <em>  
          {% capture state %}{{issue.issueState}}{% endcapture %}  
          {% capture devName %}{{issue.subscriptionDeveloperName}}{% endcapture %}  
          {% capture str1 %}{{ reportedBy | replace : replaceString0, state }}{% endcapture %}  
          {{ str1 | replace : replaceString1, devName }}  
          <span class="UtcDateElement">{{ issue.reportedOn | date: "r" }}</span>  
        </em>  
      </li>  
      {% endfor %}  
    </ul>  
    <paging-control></paging-control>  
    {% else %}  
    {% localized "CommonResources|NoItemsToDisplay" %}  
    {% endif %}  
    {% if canReportIssue %}  
    <a class="btn btn-primary" id="createIssue" href="/Issues/Create">{% localized "IssuesStrings|WebIssuesReportIssueButton" %}</a>  
    {% elsif isAuthenticated %}  
    <hr />  
    <p>{% localized "IssuesStrings|WebIssuesNoActiveSubscriptions" %}</p>  
    {% else %}  
    <hr />  
    <p>  
      {% capture signIntext %}{% localized "IssuesStrings|WebIssuesNotSignin" %}{% endcapture %}  
      {% capture link %}<a href="/signin">{% localized "IssuesStrings|WebIssuesSignIn" %}</a>{% endcapture %}  
      {{ signIntext | replace : replaceString0, link }}  
    </p>  
    {% endif %}  
  </div>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="9c0e3-115">Kontroller</span><span class="sxs-lookup"><span data-stu-id="9c0e3-115">Controls</span></span>  
 <span data-ttu-id="9c0e3-116">Hej `Issue list` mall kan använda följande hello [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="9c0e3-116">hello `Issue list` template may use hello following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="9c0e3-117">växlingsfil-kontroll</span><span class="sxs-lookup"><span data-stu-id="9c0e3-117">paging-control</span></span>](api-management-page-controls.md#paging-control)  
  
### <a name="data-model"></a><span data-ttu-id="9c0e3-118">Datamodell</span><span class="sxs-lookup"><span data-stu-id="9c0e3-118">Data model</span></span>  
  
|<span data-ttu-id="9c0e3-119">Egenskap</span><span class="sxs-lookup"><span data-stu-id="9c0e3-119">Property</span></span>|<span data-ttu-id="9c0e3-120">Typ</span><span class="sxs-lookup"><span data-stu-id="9c0e3-120">Type</span></span>|<span data-ttu-id="9c0e3-121">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="9c0e3-121">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="9c0e3-122">Problem</span><span class="sxs-lookup"><span data-stu-id="9c0e3-122">Issues</span></span>|<span data-ttu-id="9c0e3-123">Samling av [problemet](api-management-template-data-model-reference.md#Issue) entiteter.</span><span class="sxs-lookup"><span data-stu-id="9c0e3-123">Collection of [Issue](api-management-template-data-model-reference.md#Issue) entities.</span></span>|<span data-ttu-id="9c0e3-124">hello problem synliga toohello aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="9c0e3-124">hello issues visible toohello current user.</span></span>|  
|<span data-ttu-id="9c0e3-125">Sidindelning</span><span class="sxs-lookup"><span data-stu-id="9c0e3-125">Paging</span></span>|<span data-ttu-id="9c0e3-126">[Växling](api-management-template-data-model-reference.md#Paging) entitet.</span><span class="sxs-lookup"><span data-stu-id="9c0e3-126">[Paging](api-management-template-data-model-reference.md#Paging) entity.</span></span>|<span data-ttu-id="9c0e3-127">hello sidindelning information för hello program samling.</span><span class="sxs-lookup"><span data-stu-id="9c0e3-127">hello paging information for hello applications collection.</span></span>|  
|<span data-ttu-id="9c0e3-128">IsAuthenticated</span><span class="sxs-lookup"><span data-stu-id="9c0e3-128">IsAuthenticated</span></span>|<span data-ttu-id="9c0e3-129">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="9c0e3-129">boolean</span></span>|<span data-ttu-id="9c0e3-130">Om hello aktuell användare är inloggad toohello developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="9c0e3-130">Whether hello current user is signed-in toohello developer portal.</span></span>|  
|<span data-ttu-id="9c0e3-131">CanReportIssues</span><span class="sxs-lookup"><span data-stu-id="9c0e3-131">CanReportIssues</span></span>|<span data-ttu-id="9c0e3-132">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="9c0e3-132">boolean</span></span>|<span data-ttu-id="9c0e3-133">Om hello aktuell användare har behörighet toofile ett problem.</span><span class="sxs-lookup"><span data-stu-id="9c0e3-133">Whether hello current user has permissions toofile an issue.</span></span>|  
|<span data-ttu-id="9c0e3-134">Söka</span><span class="sxs-lookup"><span data-stu-id="9c0e3-134">Search</span></span>|<span data-ttu-id="9c0e3-135">Sträng</span><span class="sxs-lookup"><span data-stu-id="9c0e3-135">string</span></span>|<span data-ttu-id="9c0e3-136">Den här egenskapen är inaktuell och bör inte användas.</span><span class="sxs-lookup"><span data-stu-id="9c0e3-136">This property is deprecated and should not be used.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="9c0e3-137">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="9c0e3-137">Sample template data</span></span>  
  
```json  
{  
    "Issues": [  
        {  
            "Id": "5702b68bb16653124c8f9ba7",  
            "ApiId": "570275f1b16653124c8f9ba3",  
            "Title": "I couldn't figure out how tooconnect my application toohello API",  
            "Description": "I'm having trouble connecting my application toohello backend API.",  
            "SubscriptionDeveloperName": "Clayton",  
            "IssueState": "Proposed",  
            "ReportedOn": "2016-04-04T18:46:35.64",  
            "Comments": null,  
            "Attachments": null,  
            "Services": null  
        }  
    ],  
    "Paging": {  
        "Page": 1,  
        "PageSize": 10,  
        "TotalItemCount": 1,  
        "ShowAll": false,  
        "PageCount": 1  
    },  
    "IsAuthenticated": true,  
    "CanReportIssue": true,  
    "Search": null  
}  
```

## <a name="next-steps"></a><span data-ttu-id="9c0e3-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9c0e3-138">Next steps</span></span>
<span data-ttu-id="9c0e3-139">Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="9c0e3-139">For more information about working with templates, see [How toocustomize hello API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>
