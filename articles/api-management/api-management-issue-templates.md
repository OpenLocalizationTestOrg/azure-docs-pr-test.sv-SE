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
# <a name="issue-templates-in-azure-api-management"></a>Utfärda mallar i Azure API Management
Azure API Management ger du hello möjlighet toocustomize hello innehållet i developer portalens sidor med hjälp av en uppsättning mallar som konfigurerar deras innehåll. Med hjälp av [DotLiquid](http://dotliquidmarkup.org/) syntax och hello redigeringsprogram, t.ex [DotLiquid för Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), och en angiven uppsättning lokaliserade [String resurser](api-management-template-resources.md#strings), [ Glyf resurser](api-management-template-resources.md#glyphs), och [sidan kontroller](api-management-page-controls.md), du har stor flexibilitet tooconfigure hello innehåll hello sidor som du vill använda dessa mallar.  
  
 hello mallar i det här avsnittet kan du toocustomize hello innehåll hello problemet sidor i hello developer-portalen.  
  
-   [Problemet lista](#IssueList)  
  
> [!NOTE]
>  Standard exempelmallarna ingår i hello följande dokumentation, men är ämne toochange på grund av toocontinuous förbättringar. Du kan visa hello live standardmallarna i hello developer-portalen genom att gå toohello önskad enskilda mallar. Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).  
  
##  <a name="IssueList"></a>Problemet lista  
 Hej **problemet listan** mall kan du toocustomize hello brödtext hello problemet sidan i hello developer-portalen.  
  
 ![Utfärda listan Developer-portalen](./media/api-management-issue-templates/APIM-Issue-List-Developer-Portal.png "APIM problemet listan Developer-portalen")  
  
### <a name="default-template"></a>Standardmall  
  
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
  
### <a name="controls"></a>Kontroller  
 Hej `Issue list` mall kan använda följande hello [sidan kontroller](api-management-page-controls.md).  
  
-   [växlingsfil-kontroll](api-management-page-controls.md#paging-control)  
  
### <a name="data-model"></a>Datamodell  
  
|Egenskap|Typ|Beskrivning|  
|--------------|----------|-----------------|  
|Problem|Samling av [problemet](api-management-template-data-model-reference.md#Issue) entiteter.|hello problem synliga toohello aktuella användaren.|  
|Sidindelning|[Växling](api-management-template-data-model-reference.md#Paging) entitet.|hello sidindelning information för hello program samling.|  
|IsAuthenticated|Booleskt värde|Om hello aktuell användare är inloggad toohello developer-portalen.|  
|CanReportIssues|Booleskt värde|Om hello aktuell användare har behörighet toofile ett problem.|  
|Söka|Sträng|Den här egenskapen är inaktuell och bör inte användas.|  
  
### <a name="sample-template-data"></a>Mallen exempeldata  
  
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

## <a name="next-steps"></a>Nästa steg
Mer information om hur du arbetar med mallar finns [hur toocustomize hello API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).
