---
title: API-mallar i Azure API Management | Microsoft Docs
description: "Lär dig hur du anpassar innehållet i API-sidor i developer-portalen i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 3642fd09-ba98-4358-93a6-c48ab0500431
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 3802868470f0f74cd1f895a00195259861ea16f8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="api-templates-in-azure-api-management"></a><span data-ttu-id="2a77a-103">API-mallar i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="2a77a-103">API templates in Azure API Management</span></span>
<span data-ttu-id="2a77a-104">Azure API Management ger dig möjlighet att anpassa innehållet i developer portalens sidor med hjälp av en uppsättning mallar som konfigurerar deras innehåll.</span><span class="sxs-lookup"><span data-stu-id="2a77a-104">Azure API Management provides you the ability to customize the content of developer portal pages using a set of templates that configure their content.</span></span> <span data-ttu-id="2a77a-105">Med hjälp av [DotLiquid](http://dotliquidmarkup.org/) syntax och redigeringsprogram, t.ex [DotLiquid för Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), och en angiven uppsättning lokaliserade [String resurser](api-management-template-resources.md#strings), [glyf resurser](api-management-template-resources.md#glyphs), och [sidan kontroller](api-management-page-controls.md), du har stor flexibilitet för att konfigurera innehåll för sidorna som du vill använda dessa mallar.</span><span class="sxs-lookup"><span data-stu-id="2a77a-105">Using [DotLiquid](http://dotliquidmarkup.org/) syntax and the editor of your choice, such as [DotLiquid for Designers](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers), and a provided set of localized [String resources](api-management-template-resources.md#strings), [Glyph resources](api-management-template-resources.md#glyphs), and [Page controls](api-management-page-controls.md), you have great flexibility to configure the content of the pages as you see fit using these templates.</span></span>  
  
 <span data-ttu-id="2a77a-106">Mallarna i det här avsnittet kan du anpassa innehållet i API-sidor i developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="2a77a-106">The templates in this section allow you to customize the content of the API pages in the developer portal.</span></span>  
  
-   [<span data-ttu-id="2a77a-107">API-lista</span><span class="sxs-lookup"><span data-stu-id="2a77a-107">API list</span></span>](#APIList)  
-   [<span data-ttu-id="2a77a-108">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="2a77a-108">Operation</span></span>](#Product)  
-   [<span data-ttu-id="2a77a-109">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="2a77a-109">Code samples</span></span>](#CodeSamples)  
    -   [<span data-ttu-id="2a77a-110">CURL</span><span class="sxs-lookup"><span data-stu-id="2a77a-110">Curl</span></span>](#Curl)  
    -   [<span data-ttu-id="2a77a-111">C#</span><span class="sxs-lookup"><span data-stu-id="2a77a-111">C#</span></span>](#CSharp)  
    -   [<span data-ttu-id="2a77a-112">Java</span><span class="sxs-lookup"><span data-stu-id="2a77a-112">Java</span></span>](#Stub)  
    -   [<span data-ttu-id="2a77a-113">JavaScript</span><span class="sxs-lookup"><span data-stu-id="2a77a-113">JavaScript</span></span>](#JavaScript)  
    -   [<span data-ttu-id="2a77a-114">Objective C</span><span class="sxs-lookup"><span data-stu-id="2a77a-114">Objective C</span></span>](#ObjectiveC)  
    -   [<span data-ttu-id="2a77a-115">PHP</span><span class="sxs-lookup"><span data-stu-id="2a77a-115">PHP</span></span>](#PHP)  
    -   [<span data-ttu-id="2a77a-116">Python</span><span class="sxs-lookup"><span data-stu-id="2a77a-116">Python</span></span>](#Python)  
    -   [<span data-ttu-id="2a77a-117">Ruby</span><span class="sxs-lookup"><span data-stu-id="2a77a-117">Ruby</span></span>](#Ruby)  

> [!NOTE]
>  <span data-ttu-id="2a77a-118">Standard exempelmallarna ingår i följande dokumentation, men kan komma att ändras på grund av kontinuerliga förbättringar.</span><span class="sxs-lookup"><span data-stu-id="2a77a-118">Sample default templates are included in the following documentation, but are subject to change due to continuous improvements.</span></span> <span data-ttu-id="2a77a-119">Du kan visa live standardmallarna i developer-portalen genom att navigera till önskade enskilda mallar.</span><span class="sxs-lookup"><span data-stu-id="2a77a-119">You can view the live default templates in the developer portal by navigating to the desired individual templates.</span></span> <span data-ttu-id="2a77a-120">Mer information om hur du arbetar med mallar finns [hur du anpassar API Management developer-portalen med hjälp av mallar](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span><span class="sxs-lookup"><span data-stu-id="2a77a-120">For more information about working with templates, see [How to customize the API Management developer portal using templates](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).</span></span>  
  
##  <span data-ttu-id="2a77a-121"><a name="APIList"></a>API-lista</span><span class="sxs-lookup"><span data-stu-id="2a77a-121"><a name="APIList"></a> API list</span></span>  
 <span data-ttu-id="2a77a-122">Den **API listan** mall kan du anpassa brödtexten i sidan API: N i developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="2a77a-122">The **API list** template allows you to customize the body of the API list page in the developer portal.</span></span>  
  
 <span data-ttu-id="2a77a-123">![Developer Portal API listan](./media/api-management-api-templates/APIM-Developer-Portal-Templates-API-List.png "APIM Developer Portal API mallistan")</span><span class="sxs-lookup"><span data-stu-id="2a77a-123">![Developer Portal API List](./media/api-management-api-templates/APIM-Developer-Portal-Templates-API-List.png "APIM Developer Portal Templates API List")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="2a77a-124">Standardmall</span><span class="sxs-lookup"><span data-stu-id="2a77a-124">Default template</span></span>  
  
```xml  
<search-control></search-control>  
<div class="row">  
    <div class="col-md-9">  
        <h2>{% localized "ApisStrings|PageTitleApis" %}</h2>  
    </div>  
</div>  
<div class="row">  
    <div class="col-md-12">  
    {% if apis.size > 0 %}  
    <ol class="list-unstyled">  
    {% for api in apis %}  
        <li>  
        <h3>  
          <a href="/docs/services/{{api.id}}">{{api.name}}</a>  
        </h3>  
       {{api.description}}  
      </li>  
    {% endfor %}  
    </ol>  
    <paging-control></paging-control>  
    {% else %}  
    {% localized "CommonResources|NoItemsToDisplay" %}  
    {% endif %}  
    </div>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="2a77a-125">Kontroller</span><span class="sxs-lookup"><span data-stu-id="2a77a-125">Controls</span></span>  
 <span data-ttu-id="2a77a-126">Den `API list` mall kan du använda följande [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="2a77a-126">The `API list` template may use the following [page controls](api-management-page-controls.md).</span></span>  
  
-   [<span data-ttu-id="2a77a-127">växlingsfil-kontroll</span><span class="sxs-lookup"><span data-stu-id="2a77a-127">paging-control</span></span>](api-management-page-controls.md#paging-control)  
  
-   [<span data-ttu-id="2a77a-128">Sök-kontroll</span><span class="sxs-lookup"><span data-stu-id="2a77a-128">search-control</span></span>](api-management-page-controls.md#search-control)  
  
### <a name="data-model"></a><span data-ttu-id="2a77a-129">Datamodell</span><span class="sxs-lookup"><span data-stu-id="2a77a-129">Data model</span></span>  
  
|<span data-ttu-id="2a77a-130">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2a77a-130">Property</span></span>|<span data-ttu-id="2a77a-131">Typ</span><span class="sxs-lookup"><span data-stu-id="2a77a-131">Type</span></span>|<span data-ttu-id="2a77a-132">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2a77a-132">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="2a77a-133">API: er</span><span class="sxs-lookup"><span data-stu-id="2a77a-133">apis</span></span>|<span data-ttu-id="2a77a-134">Samling av [API-översikten](api-management-template-data-model-reference.md#APISummary) entiteter.</span><span class="sxs-lookup"><span data-stu-id="2a77a-134">Collection of [API summary](api-management-template-data-model-reference.md#APISummary) entities.</span></span>|<span data-ttu-id="2a77a-135">API: erna synliga för den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="2a77a-135">The APIs visible to the current user.</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="2a77a-136">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="2a77a-136">Sample template data</span></span>  
  
```json  
{  
    "apis": [  
        {  
            "id": "570275f1b16653124c8f9ba3",  
            "name": "Basic Calculator",  
            "description": "Arithmetics is just a call away!"  
        },  
        {  
            "id": "57026e30de15d80041040001",  
            "name": "Echo API",  
            "description": null  
        }  
    ],  
    "pageTitle": "APIs"  
}  
```  
  
##  <span data-ttu-id="2a77a-137"><a name="Product"></a>Åtgärden</span><span class="sxs-lookup"><span data-stu-id="2a77a-137"><a name="Product"></a> Operation</span></span>  
 <span data-ttu-id="2a77a-138">Den **åtgärden** mall kan du anpassa brödtexten i sidan åtgärden i developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="2a77a-138">The **Operation** template allows you to customize the body of the operation page in the developer portal.</span></span>  
  
 <span data-ttu-id="2a77a-139">![Developer Portal åtgärden sidan](./media/api-management-api-templates/APIM-Developer-Portal-templates-Operation-page.png "APIM Developer-portalen mallar åtgärden sida")</span><span class="sxs-lookup"><span data-stu-id="2a77a-139">![Developer Portal Operation page](./media/api-management-api-templates/APIM-Developer-Portal-templates-Operation-page.png "APIM Developer Portal templates Operation page")</span></span>  
  
### <a name="default-template"></a><span data-ttu-id="2a77a-140">Standardmall</span><span class="sxs-lookup"><span data-stu-id="2a77a-140">Default template</span></span>  
  
```xml  
<h2>{{api.name}}</h2>  
<p>{{api.description }}</p>  
  
<div class="panel">  
    <h3>{{operation.name}}</h3>  
    <p>{{operation.description }}</p>  
    <a class="btn btn-primary" href="{{consoleUrl}}" id="btnOpenConsole" role="button">  
        Try it  
    </a>  
</div>  
  
<h4>{% localized "Documentation|SectionHeadingRequestUrl" %}</h4>  
<div class="panel">  
    <div class="panel-body">  
        <label>{{ sampleUrl | escape }}</label>  
    </div>  
</div>  
  
{% if operation.request %}  
    {% if operation.request.parameters.size > 0 %}  
        <h4>{% localized "Documentation|SectionHeadingRequestParameters" %}</h4>  
  
        <div class="panel">  
            {% for parameter in operation.request.parameters %}  
                 <div class="row panel-body">  
                    <div class="col-md-3">  
                        <label>{{parameter.name}}</label>  
                        {% unless parameter.required %}  
                            <span class="text-muted">({% localized "Documentation|FormLabelSubtextOptional" %})</span>  
                        {% endunless %}  
                    </div>  
                    <div class="col-md-1">  
                        {{parameter.typeName}}  
                    </div>  
                    <div class="col-md-8">  
                        {{parameter.description}}  
                    </div>  
                 </div>  
            {% endfor %}  
        </div>  
    {% endif %}  
  
    {% if operation.request.headers.size > 0 %}  
        <h4>{% localized "Documentation|SectionHeadingRequestHeaders" %}</h4>  
        <div class="panel">  
            {% for header in operation.request.headers %}  
                 <div class="row panel-body">  
                    <div class="col-md-3">  
                        <label>{{header.name}}</label>  
                        {%unless header.required %}  
                            <span class="text-muted">({% localized "Documentation|FormLabelSubtextOptional" %})</span>  
                        {% endunless %}  
                    </div>  
                    <div class="col-md-1">  
                        {{header.typeName}}  
                    </div>  
                    <div class="col-md-8">  
                        {{header.description}}  
                    </div>  
                 </div>  
            {% endfor %}  
        </div>  
    {% endif %}  
  
    {% if operation.request.description or operation.request.representations.size > 0 %}  
        <h4>{% localized "Documentation|SectionHeadingRequestBody" %}</h4>  
        <div class="panel">  
            {% if operation.request.description %}  
                <p>{{operation.request.description }}</p>     
            {% endif %}  
  
            {% if operation.request.representations.size > 0 %}  
                <div role="tabpanel">  
                    <ul class="nav nav-tabs" role="tablist">  
                        {% for representation in operation.request.representations %}  
                            <li role="presentation" {% if forloop.first %}class="active"{% endif %}>  
                                <a href="#requesttab{{forloop.index}}" role="tab" data-toggle="tab">  
                                    {{representation.contentType}}  
                                </a>  
                            </li>  
                        {% endfor %}  
                    </ul>  
                    <div class="tab-content tab-content-boxed">  
                        {% for representation in operation.request.representations %}  
                            <div id="requesttab{{forloop.index}}" role="tabpanel" class="tab-pane snippet{% if forloop.first %} active{% endif %}">  
  
                                {% if representation.sample or representation.schema %}   
                                <div role="tabpanel">  
                                    {% if representation.sample and representation.schema %}   
                                    <ul class="nav nav-tabs-borderless" role="tablist">  
                                        <li role="presentation" class="active">  
                                            <a href="#requesttab{{forloop.index}}sample" role="tab" data-toggle="tab">Sample</a>  
                                        </li>  
                                        <li role="presentation">  
                                            <a href="#requesttab{{forloop.index}}schema" role="tab" data-toggle="tab">Schema</a>  
                                        </li>  
                                    </ul>  
                                    {% endif %}  
  
                                    <div class="tab-content">  
                                        {% if representation.sample %}  
                                        <div id="requesttab{{forloop.index}}sample" role="tabpanel" class="tab-pane snippet active">  
                                            <pre><code class="{{representation.Brush}}">{{ representation.sample | escape }}</code></pre>  
                                        </div>  
                                        {% endif %}  
  
                                        {% if representation.schema %}  
                                        <div id="requesttab{{forloop.index}}schema" role="tabpanel" class="tab-pane snippet">  
                                            <pre><code class="{{representation.Brush}}">{{ representation.schema | escape }}</code></pre>  
                                        </div>  
                                        {% endif %}  
                                    </div>  
                                </div>  
                                {% endif %}  
                            </div>  
                        {% endfor %}  
                    </div>  
                </div>  
            {% endif %}  
  
            <div class="clearfix"></div>  
        </div>  
    {% endif %}  
{% endif %}  
  
{% if operation.responses.size > 0 %}  
    {% for response in operation.responses %}  
        {% if response.description or response.representations.size > 0 %}  
            <h4>{% localized "Documentation|SectionHeadingResponse" %} {{response.statusCode}}</h4>  
  
            <div class="panel">  
                {% if response.description %}  
                    <p>{{ response.description }}</p>  
                {% endif %}  
  
                {% if response.representations.size > 0 %}  
                    <div role="tabpanel">  
                        <ul class="nav nav-tabs" role="tablist">  
                            {% for representation in response.representations %}  
                                <li role="presentation" {% if forloop.first %}class="active"{% endif %}>  
                                    <a href="#response{{response.statusCode}}tab{{forloop.index}}" role="tab" data-toggle="tab">  
                                        {{representation.contentType}}  
                                    </a>  
                                </li>  
                            {% endfor %}  
                        </ul>  
                        <div class="tab-content tab-content-boxed">  
                            {% for representation in response.representations %}  
                                <div id="response{{response.statusCode}}tab{{forloop.index}}" role="tabpanel" class="tab-pane snippet{% if forloop.first %} active{% endif %}">  
  
                                    {% if representation.sample or representation.schema %}  
                                    <div role="tabpanel">  
  
                                        {% if representation.sample and representation.schema %}  
                                        <ul class="nav nav-tabs-borderless" role="tablist">  
                                            <li role="presentation" class="active">  
                                                <a href="#response{{response.statusCode}}tab{{forloop.index}}sample" role="tab" data-toggle="tab">  
                                                    Sample  
                                                </a>  
                                            </li>  
                                            <li role="presentation">  
                                                <a href="#response{{response.statusCode}}tab{{forloop.index}}schema" role="tab" data-toggle="tab">  
                                                    Schema  
                                                </a>  
                                            </li>  
                                        </ul>  
                                        {% endif %}  
  
                                        <div class="tab-content">  
                                            {% if representation.sample %}  
                                            <div id="response{{response.statusCode}}tab{{forloop.index}}sample" role="tabpanel" class="tab-pane snippet active">  
                                                <pre><code class="{{representation.Brush}}">{{ representation.sample | escape }}</code></pre>  
                                            </div>  
                                            {% endif %}  
  
                                            {% if representation.schema %}  
                                            <div id="response{{response.statusCode}}tab{{forloop.index}}schema" role="tabpanel" class="tab-pane snippet">  
                                                <pre><code class="{{representation.Brush}}">{{ representation.schema | escape }}</code></pre>  
                                            </div>  
                                            {% endif %}  
                                        </div>  
                                    </div>  
                                    {% endif %}  
                                </div>  
                            {% endfor %}  
                        </div>  
                    </div>  
                {% endif %}  
  
                <div class="clearfix"></div>  
            </div>  
  
        {% endif %}  
    {% endfor %}  
{% endif %}  
  
<h4>{% localized "Documentation|SectionHeadingCodeSamples" %}</h4>  
<div role="tabpanel">  
    <ul class="nav nav-tabs" role="tablist">  
        {% for sample in samples %}  
            <li role="presentation" {% if forloop.first %}class="active"{% endif %}>  
                <a href="#{{sample.brush}}" aria-controls="{{sample.brush}}" role="tab" data-toggle="tab">  
                    {{sample.title}}  
                </a>  
            </li>  
        {% endfor %}  
    </ul>  
    <div class="tab-content tab-content-boxed" title="{% localized "Documentation|TooltipTextDoubleClickToSelectAll=""" %}">  
        {% for sample in samples %}  
            <div role="tabpanel" class="tab-pane tab-content-boxed {% if forloop.first %} active{% endif %} snippet snippet-resizable" id="{{sample.brush}}" >  
                <pre><code class="{{sample.brush}}">{% partial sample.template for sample in samples %}</code></pre>  
            </div>  
        {% endfor %}  
    </div>  
    <div class="clearfix"></div>  
</div>  
```  
  
### <a name="controls"></a><span data-ttu-id="2a77a-141">Kontroller</span><span class="sxs-lookup"><span data-stu-id="2a77a-141">Controls</span></span>  
 <span data-ttu-id="2a77a-142">Den `Operation` mallen kan inte användas för någon [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="2a77a-142">The `Operation` template does not allow the use of any [page controls](api-management-page-controls.md).</span></span>  
  
### <a name="data-model"></a><span data-ttu-id="2a77a-143">Datamodell</span><span class="sxs-lookup"><span data-stu-id="2a77a-143">Data model</span></span>  
  
|<span data-ttu-id="2a77a-144">Egenskap</span><span class="sxs-lookup"><span data-stu-id="2a77a-144">Property</span></span>|<span data-ttu-id="2a77a-145">Typ</span><span class="sxs-lookup"><span data-stu-id="2a77a-145">Type</span></span>|<span data-ttu-id="2a77a-146">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2a77a-146">Description</span></span>|  
|--------------|----------|-----------------|  
|<span data-ttu-id="2a77a-147">ApiId</span><span class="sxs-lookup"><span data-stu-id="2a77a-147">apiId</span></span>|<span data-ttu-id="2a77a-148">Sträng</span><span class="sxs-lookup"><span data-stu-id="2a77a-148">string</span></span>|<span data-ttu-id="2a77a-149">Id för den aktuella API.</span><span class="sxs-lookup"><span data-stu-id="2a77a-149">The id of the current API.</span></span>|  
|<span data-ttu-id="2a77a-150">apiName</span><span class="sxs-lookup"><span data-stu-id="2a77a-150">apiName</span></span>|<span data-ttu-id="2a77a-151">Sträng</span><span class="sxs-lookup"><span data-stu-id="2a77a-151">string</span></span>|<span data-ttu-id="2a77a-152">Namnet på API: et.</span><span class="sxs-lookup"><span data-stu-id="2a77a-152">The name of the API.</span></span>|  
|<span data-ttu-id="2a77a-153">apiDescription</span><span class="sxs-lookup"><span data-stu-id="2a77a-153">apiDescription</span></span>|<span data-ttu-id="2a77a-154">Sträng</span><span class="sxs-lookup"><span data-stu-id="2a77a-154">string</span></span>|<span data-ttu-id="2a77a-155">En beskrivning av API: et.</span><span class="sxs-lookup"><span data-stu-id="2a77a-155">A description of the API.</span></span>|  
|<span data-ttu-id="2a77a-156">api</span><span class="sxs-lookup"><span data-stu-id="2a77a-156">api</span></span>|<span data-ttu-id="2a77a-157">[API-översikten](api-management-template-data-model-reference.md#APISummary) entitet.</span><span class="sxs-lookup"><span data-stu-id="2a77a-157">[API summary](api-management-template-data-model-reference.md#APISummary) entity.</span></span>|<span data-ttu-id="2a77a-158">Den aktuella API.</span><span class="sxs-lookup"><span data-stu-id="2a77a-158">The current API.</span></span>|  
|<span data-ttu-id="2a77a-159">åtgärden</span><span class="sxs-lookup"><span data-stu-id="2a77a-159">operation</span></span>|[<span data-ttu-id="2a77a-160">Åtgärd</span><span class="sxs-lookup"><span data-stu-id="2a77a-160">Operation</span></span>](api-management-template-data-model-reference.md#Operation)|<span data-ttu-id="2a77a-161">För närvarande visas igen.</span><span class="sxs-lookup"><span data-stu-id="2a77a-161">The currently displayed operation.</span></span>|  
|<span data-ttu-id="2a77a-162">sampleUrl</span><span class="sxs-lookup"><span data-stu-id="2a77a-162">sampleUrl</span></span>|<span data-ttu-id="2a77a-163">Sträng</span><span class="sxs-lookup"><span data-stu-id="2a77a-163">string</span></span>|<span data-ttu-id="2a77a-164">URL till den aktuella åtgärden.</span><span class="sxs-lookup"><span data-stu-id="2a77a-164">The URL for the current operation.</span></span>|  
|<span data-ttu-id="2a77a-165">operationMenu</span><span class="sxs-lookup"><span data-stu-id="2a77a-165">operationMenu</span></span>|[<span data-ttu-id="2a77a-166">Åtgärd-menyn</span><span class="sxs-lookup"><span data-stu-id="2a77a-166">Operation menu</span></span>](api-management-template-data-model-reference.md#Menu)|<span data-ttu-id="2a77a-167">En meny med åtgärder för detta API.</span><span class="sxs-lookup"><span data-stu-id="2a77a-167">A menu of operations for this API.</span></span>|  
|<span data-ttu-id="2a77a-168">consoleUrl</span><span class="sxs-lookup"><span data-stu-id="2a77a-168">consoleUrl</span></span>|<span data-ttu-id="2a77a-169">URI: N</span><span class="sxs-lookup"><span data-stu-id="2a77a-169">URI</span></span>|<span data-ttu-id="2a77a-170">URI för den **prova** knappen.</span><span class="sxs-lookup"><span data-stu-id="2a77a-170">The URI for the **Try it** button.</span></span>|  
|<span data-ttu-id="2a77a-171">Exempel</span><span class="sxs-lookup"><span data-stu-id="2a77a-171">samples</span></span>|<span data-ttu-id="2a77a-172">Samling av [kodexemplet](api-management-template-data-model-reference.md#Sample) entiteter.</span><span class="sxs-lookup"><span data-stu-id="2a77a-172">Collection of [Code sample](api-management-template-data-model-reference.md#Sample) entities.</span></span>|<span data-ttu-id="2a77a-173">Kodexempel för den aktuella åtgärden...</span><span class="sxs-lookup"><span data-stu-id="2a77a-173">The code samples for the current operation..</span></span>|  
  
### <a name="sample-template-data"></a><span data-ttu-id="2a77a-174">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="2a77a-174">Sample template data</span></span>  
  
```json  
{  
    "apiId": "570275f1b16653124c8f9ba3",  
    "apiName": "Basic Calculator",  
    "apiDescription": "Arithmetics is just a call away!",  
    "api": {  
        "id": "570275f1b16653124c8f9ba3",  
        "name": "Basic Calculator",  
        "description": "Arithmetics is just a call away!"  
    },  
    "operation": {  
        "id": "570275f2b1665305c811cf49",  
        "name": "Add two integers",  
        "description": "Produces a sum of two numbers.",  
        "scheme": "https",  
        "uriTemplate": "calc/add?a={a}&b={b}",  
        "host": "sdcontoso5.azure-api.net",  
        "httpMethod": "GET",  
        "request": {  
            "description": null,  
            "headers": [  
                {  
                    "name": "Ocp-Apim-Subscription-Key",  
                    "description": "Subscription key which provides access to this API. Found in your <a href='/developer'>Profile</a>.",  
                    "value": "{subscription key}",  
                    "typeName": "string",  
                    "options": null,  
                    "required": true,  
                    "readonly": false  
                }  
            ],  
            "parameters": [  
                {  
                    "name": "a",  
                    "description": "First operand. Default value is <code>51</code>.",  
                    "value": "51",  
                    "options": [  
                        "51"  
                    ],  
                    "required": true,  
                    "kind": 1,  
                    "typeName": null  
                },  
                {  
                    "name": "b",  
                    "description": "Second operand. Default value is <code>49</code>.",  
                    "value": "49",  
                    "options": [  
                        "49"  
                    ],  
                    "required": true,  
                    "kind": 1,  
                    "typeName": null  
                }  
            ],  
            "representations": []  
        },  
        "responses": []  
    },  
    "sampleUrl": "https://sdcontoso5.azure-api.net/calc/add?a={a}&b={b}",  
    "operationMenu": {  
        "ApiId": "570275f1b16653124c8f9ba3",  
        "CurrentOperationId": "570275f2b1665305c811cf49",  
        "Action": "Operation",  
        "MenuItems": [  
            {  
                "Id": "570275f2b1665305c811cf49",  
                "Title": "Add two integers",  
                "HttpMethod": "GET"  
            },  
            {  
                "Id": "570275f2b1665305c811cf4c",  
                "Title": "Divide two integers",  
                "HttpMethod": "GET"  
            },  
            {  
                "Id": "570275f2b1665305c811cf4b",  
                "Title": "Multiply two integers",  
                "HttpMethod": "GET"  
            },  
            {  
                "Id": "570275f2b1665305c811cf4a",  
                "Title": "Subtract two integers",  
                "HttpMethod": "GET"  
            }  
        ]  
    },  
    "consoleUrl": "/docs/services/570275f1b16653124c8f9ba3/operations/570275f2b1665305c811cf49/console",  
    "samples": [  
        {  
            "title": "Curl",  
            "snippet": null,  
            "brush": "plain",  
            "template": "DocumentationSamplesCurl",  
            "body": "{body}",  
            "method": "GET",  
            "scheme": "https",  
            "path": "/calc/add?a={a}&b={b}",  
            "query": "",  
            "host": "sdcontoso5.azure-api.net",  
            "headers": [  
                {  
                    "name": "Ocp-Apim-Subscription-Key",  
                    "description": "Subscription key which provides access to this API. Found in your <a href='/developer'>Profile</a>.",  
                    "value": "{subscription key}",  
                    "typeName": "string",  
                    "options": null,  
                    "required": true,  
                    "readonly": false  
                }  
            ],  
            "parameters": []  
        },  
        {  
            "title": "C#",  
            "snippet": null,  
            "brush": "csharp",  
            "template": "DocumentationSamplesCsharp",  
            "body": "{body}",  
            "method": "GET",  
            "scheme": "https",  
            "path": "/calc/add?a={a}&b={b}",  
            "query": "",  
            "host": "sdcontoso5.azure-api.net",  
            "headers": [  
                {  
                    "name": "Ocp-Apim-Subscription-Key",  
                    "description": "Subscription key which provides access to this API. Found in your <a href='/developer'>Profile</a>.",  
                    "value": "{subscription key}",  
                    "typeName": "string",  
                    "options": null,  
                    "required": true,  
                    "readonly": false  
                }  
            ],  
            "parameters": []  
        },  
        {  
            "title": "Java",  
            "snippet": null,  
            "brush": "java",  
            "template": "DocumentationSamplesJava",  
            "body": "{body}",  
            "method": "GET",  
            "scheme": "https",  
            "path": "/calc/add?a={a}&b={b}",  
            "query": "",  
            "host": "sdcontoso5.azure-api.net",  
            "headers": [  
                {  
                    "name": "Ocp-Apim-Subscription-Key",  
                    "description": "Subscription key which provides access to this API. Found in your <a href='/developer'>Profile</a>.",  
                    "value": "{subscription key}",  
                    "typeName": "string",  
                    "options": null,  
                    "required": true,  
                    "readonly": false  
                }  
            ],  
            "parameters": []  
        },  
        {  
            "title": "JavaScript",  
            "snippet": null,  
            "brush": "xml",  
            "template": "DocumentationSamplesJs",  
            "body": "{body}",  
            "method": "GET",  
            "scheme": "https",  
            "path": "/calc/add?a={a}&b={b}",  
            "query": "",  
            "host": "sdcontoso5.azure-api.net",  
            "headers": [  
                {  
                    "name": "Ocp-Apim-Subscription-Key",  
                    "description": "Subscription key which provides access to this API. Found in your <a href='/developer'>Profile</a>.",  
                    "value": "{subscription key}",  
                    "typeName": "string",  
                    "options": null,  
                    "required": true,  
                    "readonly": false  
                }  
            ],  
            "parameters": []  
        },  
        {  
            "title": "ObjC",  
            "snippet": null,  
            "brush": "objc",  
            "template": "DocumentationSamplesObjc",  
            "body": "{body}",  
            "method": "GET",  
            "scheme": "https",  
            "path": "/calc/add?a={a}&b={b}",  
            "query": "",  
            "host": "sdcontoso5.azure-api.net",  
            "headers": [  
                {  
                    "name": "Ocp-Apim-Subscription-Key",  
                    "description": "Subscription key which provides access to this API. Found in your <a href='/developer'>Profile</a>.",  
                    "value": "{subscription key}",  
                    "typeName": "string",  
                    "options": null,  
                    "required": true,  
                    "readonly": false  
                }  
            ],  
            "parameters": []  
        },  
        {  
            "title": "PHP",  
            "snippet": null,  
            "brush": "php",  
            "template": "DocumentationSamplesPhp",  
            "body": "{body}",  
            "method": "GET",  
            "scheme": "https",  
            "path": "/calc/add?a={a}&b={b}",  
            "query": "",  
            "host": "sdcontoso5.azure-api.net",  
            "headers": [  
                {  
                    "name": "Ocp-Apim-Subscription-Key",  
                    "description": "Subscription key which provides access to this API. Found in your <a href='/developer'>Profile</a>.",  
                    "value": "{subscription key}",  
                    "typeName": "string",  
                    "options": null,  
                    "required": true,  
                    "readonly": false  
                }  
            ],  
            "parameters": []  
        },  
        {  
            "title": "Python",  
            "snippet": null,  
            "brush": "python",  
            "template": "DocumentationSamplesPython",  
            "body": "{body}",  
            "method": "GET",  
            "scheme": "https",  
            "path": "/calc/add?a={a}&b={b}",  
            "query": "",  
            "host": "sdcontoso5.azure-api.net",  
            "headers": [  
                {  
                    "name": "Ocp-Apim-Subscription-Key",  
                    "description": "Subscription key which provides access to this API. Found in your <a href='/developer'>Profile</a>.",  
                    "value": "{subscription key}",  
                    "typeName": "string",  
                    "options": null,  
                    "required": true,  
                    "readonly": false  
                }  
            ],  
            "parameters": []  
        },  
        {  
            "title": "Ruby",  
            "snippet": null,  
            "brush": "ruby",  
            "template": "DocumentationSamplesRuby",  
            "body": "{body}",  
            "method": "GET",  
            "scheme": "https",  
            "path": "/calc/add?a={a}&b={b}",  
            "query": "",  
            "host": "sdcontoso5.azure-api.net",  
            "headers": [  
                {  
                    "name": "Ocp-Apim-Subscription-Key",  
                    "description": "Subscription key which provides access to this API. Found in your <a href='/developer'>Profile</a>.",  
                    "value": "{subscription key}",  
                    "typeName": "string",  
                    "options": null,  
                    "required": true,  
                    "readonly": false  
                }  
            ],  
            "parameters": []  
        }  
    ]  
}  
```  
  
##  <span data-ttu-id="2a77a-175"><a name="CodeSamples"></a>Kodexempel</span><span class="sxs-lookup"><span data-stu-id="2a77a-175"><a name="CodeSamples"></a> Code samples</span></span>  
 <span data-ttu-id="2a77a-176">Följande mallar kan du anpassa brödtext enskilda kodexempel på sidan igen.</span><span class="sxs-lookup"><span data-stu-id="2a77a-176">The following templates allow you to customize the body of the individual code samples on the operation page.</span></span>  
  
 <span data-ttu-id="2a77a-177">![Developer Portal mallar kodexempel](./media/api-management-api-templates/APIM-Developer-Portal-Templates-Code-samples.png "APIM Developer Portal mallar kodexempel")</span><span class="sxs-lookup"><span data-stu-id="2a77a-177">![Developer Portal Templates Code samples](./media/api-management-api-templates/APIM-Developer-Portal-Templates-Code-samples.png "APIM Developer Portal Templates Code samples")</span></span>  
  
-   [<span data-ttu-id="2a77a-178">CURL</span><span class="sxs-lookup"><span data-stu-id="2a77a-178">Curl</span></span>](#Curl)  
  
-   [<span data-ttu-id="2a77a-179">C#</span><span class="sxs-lookup"><span data-stu-id="2a77a-179">C#</span></span>](#CSharp)  
  
-   [<span data-ttu-id="2a77a-180">Java</span><span class="sxs-lookup"><span data-stu-id="2a77a-180">Java</span></span>](#Stub)  
  
-   [<span data-ttu-id="2a77a-181">JavaScript</span><span class="sxs-lookup"><span data-stu-id="2a77a-181">JavaScript</span></span>](#JavaScript)  
  
-   [<span data-ttu-id="2a77a-182">Objective C</span><span class="sxs-lookup"><span data-stu-id="2a77a-182">Objective C</span></span>](#ObjectiveC)  
  
-   [<span data-ttu-id="2a77a-183">PHP</span><span class="sxs-lookup"><span data-stu-id="2a77a-183">PHP</span></span>](#PHP)  
  
-   [<span data-ttu-id="2a77a-184">Python</span><span class="sxs-lookup"><span data-stu-id="2a77a-184">Python</span></span>](#Python)  
  
-   [<span data-ttu-id="2a77a-185">Ruby</span><span class="sxs-lookup"><span data-stu-id="2a77a-185">Ruby</span></span>](#Ruby)  
  
###  <span data-ttu-id="2a77a-186"><a name="Curl"></a>CURL</span><span class="sxs-lookup"><span data-stu-id="2a77a-186"><a name="Curl"></a> Curl</span></span>  
 <span data-ttu-id="2a77a-187">Den **DocumentationSamplesCurl** mall kan du anpassa detta kodexempel i avsnittet kod prov på sidan igen.</span><span class="sxs-lookup"><span data-stu-id="2a77a-187">The **DocumentationSamplesCurl** template allows you to customize that code sample in the code samples section of the operation page.</span></span>  
  
#### <a name="default-template"></a><span data-ttu-id="2a77a-188">Standardmall</span><span class="sxs-lookup"><span data-stu-id="2a77a-188">Default template</span></span>  
  
```xml  
@ECHO OFF  
  
curl -v -X {{method}} "{{scheme}}://{{host}}{{path}}{{query | escape }}"  
{% for header in headers -%}  
-H "{{ header.name }}: {{ header.value }}"  
{% endfor -%}  
{% if body -%}   
--data-ascii "{{ body | replace:'"','^"' }}"   
{% endif -%}  
  
```  
  
#### <a name="controls"></a><span data-ttu-id="2a77a-189">Kontroller</span><span class="sxs-lookup"><span data-stu-id="2a77a-189">Controls</span></span>  
 <span data-ttu-id="2a77a-190">Koden exempelmallarna tillåter inte användning av någon [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="2a77a-190">The code sample templates do not allow the use of any [page controls](api-management-page-controls.md).</span></span>  
  
#### <a name="data-model"></a><span data-ttu-id="2a77a-191">Datamodell</span><span class="sxs-lookup"><span data-stu-id="2a77a-191">Data model</span></span>  
 <span data-ttu-id="2a77a-192">[Kodexempel](api-management-template-data-model-reference.md#Sample) entitet.</span><span class="sxs-lookup"><span data-stu-id="2a77a-192">[Code sample](api-management-template-data-model-reference.md#Sample) entity.</span></span>  
  
#### <a name="sample-template-data"></a><span data-ttu-id="2a77a-193">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="2a77a-193">Sample template data</span></span>  
  
```json  
{  
    "title": "Curl",  
    "snippet": null,  
    "brush": "plain",  
    "template": "DocumentationSamplesCurl",  
    "body": "{body}",  
    "method": "GET",  
    "scheme": "https",  
    "path": "/calc/add?a={a}&b={b}",  
    "query": "",  
    "host": "sdcontoso5.azure-api.net",  
    "headers": [  
        {  
            "name": "Ocp-Apim-Subscription-Key",  
            "description": "Subscription key which provides access to this API. Found in your <a href='/developer'>Profile</a>.",  
            "value": "{subscription key}",  
            "typeName": "string",  
            "options": null,  
            "required": true,  
            "readonly": false  
        }  
    ],  
    "parameters": []  
}  
```  
  
###  <span data-ttu-id="2a77a-194"><a name="CSharp"></a>C#</span><span class="sxs-lookup"><span data-stu-id="2a77a-194"><a name="CSharp"></a> C#</span></span>  
 <span data-ttu-id="2a77a-195">Den **DocumentationSamplesCsharp** mall kan du anpassa detta kodexempel i avsnittet kod prov på sidan igen.</span><span class="sxs-lookup"><span data-stu-id="2a77a-195">The **DocumentationSamplesCsharp** template allows you to customize that code sample in the code samples section of the operation page.</span></span>  
  
#### <a name="default-template"></a><span data-ttu-id="2a77a-196">Standardmall</span><span class="sxs-lookup"><span data-stu-id="2a77a-196">Default template</span></span>  
  
```xml  
using System;  
using System.Net.Http.Headers;  
using System.Text;  
using System.Net.Http;  
using System.Web;  
  
namespace CSHttpClientSample  
{  
    static class Program  
    {  
        static void Main()  
        {  
            MakeRequest();  
            Console.WriteLine("Hit ENTER to exit...");  
            Console.ReadLine();  
        }  
  
        static async void MakeRequest()  
        {  
            var client = new HttpClient();  
            var queryString = HttpUtility.ParseQueryString(string.Empty);  
  
{% if headers.size > 0 -%}  
            // Request headers  
{% for header in headers -%}  
{% case header.Name -%}  
{% when "Accept"%}  
            client.DefaultRequestHeaders.Accept.Add(MediaTypeWithQualityHeaderValue.Parse("{{header.value}}"));  
{% when "Accept-Charset" -%}  
            client.DefaultRequestHeaders.AcceptCharset.Add(StringWithQualityHeaderValue.Parse("{{header.value}}"));  
{% when "Accept-Encoding" -%}  
            client.DefaultRequestHeaders.AcceptEncoding.Add(StringWithQualityHeaderValue.Parse("{{header.value}}"));  
{% when "Accept-Language" -%}  
            client.DefaultRequestHeaders.AcceptLanguage.Add(StringWithQualityHeaderValue.Parse("{{header.value}}"));  
{% when "Cache-Control" -%}  
            client.DefaultRequestHeaders.CacheControl = CacheControlHeaderValue.Parse("{{header.value}}");  
{% when "Connection" -%}  
            client.DefaultRequestHeaders.Connection.Add("{{header.value}}");  
{% when "Date" -%}  
            client.DefaultRequestHeaders.Date = DateTimeOffset.Parse("{{header.value}}");  
{% when "Expect" -%}  
            client.DefaultRequestHeaders.Expect.Add(NameValueWithParametersHeaderValue.Parse("{{header.value}}"));  
{% when "If-Match" -%}  
            client.DefaultRequestHeaders.IfMatch.Add(EntityTagHeaderValue.Parse("{{header.value}}"));  
{% when "If-Modified-Since" -%}  
            client.DefaultRequestHeaders.IfModifiedSince = DateTimeOffset.Parse("{{header.value}}");  
{% when "If-None-Match" -%}  
            client.DefaultRequestHeaders.IfNoneMatch.Add(EntityTagHeaderValue.Parse("{{header.value}}"));  
{% when "If-Range" -%}  
            client.DefaultRequestHeaders.IfRange = RangeConditionHeaderValue.Parse("{{header.value}}");  
{% when "If-Unmodified-Since" -%}  
            client.DefaultRequestHeaders.IfUnmodifiedSince = DateTimeOffset.Parse("{{header.value}}");  
{% when "Max-Forwards" -%}  
            client.DefaultRequestHeaders.MaxForwards = int.Parse("{{header.value}}");  
{% when "Pragma" -%}  
            client.DefaultRequestHeaders.Pragma.Add(NameValueHeaderValue.Parse("{{header.value}}"));  
{% when "Range" -%}  
            client.DefaultRequestHeaders.Range = RangeHeaderValue.Parse("{{header.value}}");  
{% when "Referer" -%}  
            client.DefaultRequestHeaders.Referrer = new Uri("{{header.value}}");  
{% when "TE" -%}  
            client.DefaultRequestHeaders.TE.Add(TransferCodingWithQualityHeaderValue.Parse("{{header.value}}"));  
{% when "Transfer-Encoding" -%}  
            client.DefaultRequestHeaders.TransferEncoding.Add(TransferCodingHeaderValue.Parse("{{header.value}}"));  
{% when "Upgrade" -%}  
            client.DefaultRequestHeaders.Upgrade.Add(ProductHeaderValue.Parse("{{header.value}}"));  
{% when "User-Agent" -%}  
            client.DefaultRequestHeaders.UserAgent.Add(ProductInfoHeaderValue.Parse("{{header.value}}"));  
{% when "Via" -%}                
            client.DefaultRequestHeaders.Via.Add(ViaHeaderValue.Parse("{{header.value}}"));  
{% when "Warning" -%}  
            client.DefaultRequestHeaders.Warning.Add(WarningHeaderValue.Parse("{{header.value}}"));  
{% when "Content-Type" -%}  
{% else -%}  
            client.DefaultRequestHeaders.Add("{{header.Name}}", "{{header.value}}");  
{% endcase -%}  
{% endfor -%}  
{% endif -%}  
  
{% if parameters.size > 0 -%}  
            // Request parameters  
{% for parameter in parameters -%}  
            queryString["{{parameter.Name}}"] = "{{parameter.Value}}";  
{% endfor -%}  
{% endif -%}  
            var uri = "{{scheme}}://{{host}}{{path}}{% if path contains '?' %}&{% else %}?{% endif %}" + queryString;  
  
{% case method -%}  
  
{% when "POST" -%}  
            HttpResponseMessage response;  
  
            // Request body  
            byte[] byteData = Encoding.UTF8.GetBytes("{{ body | replace:'"','\"'}}");  
  
            using (var content = new ByteArrayContent(byteData))  
            {  
{% if body -%}  
               content.Headers.ContentType = new MediaTypeHeaderValue("< your content type, i.e. application/json >");  
{% endif -%}  
               response = await client.PostAsync(uri, content);  
            }  
  
{% when "GET" -%}  
            var response = await client.GetAsync(uri);  
{% when "DELETE" -%}  
            var response = await client.DeleteAsync(uri);  
{% when "PUT" -%}  
            HttpResponseMessage response;  
  
            // Request body  
            byte[] byteData = Encoding.UTF8.GetBytes("{{ body | replace:'"','\"'}}");  
  
            using (var content = new ByteArrayContent(byteData))  
            {  
{% if body -%}  
                content.Headers.ContentType = new MediaTypeHeaderValue("< your content type, i.e. application/json >");  
{% endif -%}  
                response = await client.PutAsync(uri, content);  
            }  
{% when "HEAD" -%}  
            var response = await client.SendAsync(new HttpRequestMessage(HttpMethod.Head, uri));  
{% when "OPTIONS" -%}  
            var response = await client.SendAsync(new HttpRequestMessage(HttpMethod.Options, uri));  
{% when "TRACE" -%}  
            var response = await client.SendAsync(new HttpRequestMessage(HttpMethod.Trace, uri));  
  
            if (response.Content != null)  
            {  
                var responseString = await response.Content.ReadAsStringAsync();  
                Console.WriteLine(responseString);      
            }  
{% endcase -%}  
        }  
    }  
}     
```  
  
#### <a name="controls"></a><span data-ttu-id="2a77a-197">Kontroller</span><span class="sxs-lookup"><span data-stu-id="2a77a-197">Controls</span></span>  
 <span data-ttu-id="2a77a-198">Koden exempelmallarna tillåter inte användning av någon [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="2a77a-198">The code sample templates do not allow the use of any [page controls](api-management-page-controls.md).</span></span>  
  
#### <a name="data-model"></a><span data-ttu-id="2a77a-199">Datamodell</span><span class="sxs-lookup"><span data-stu-id="2a77a-199">Data model</span></span>  
 <span data-ttu-id="2a77a-200">[Kodexempel](api-management-template-data-model-reference.md#Sample) entitet.</span><span class="sxs-lookup"><span data-stu-id="2a77a-200">[Code sample](api-management-template-data-model-reference.md#Sample) entity.</span></span>  
  
#### <a name="sample-template-data"></a><span data-ttu-id="2a77a-201">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="2a77a-201">Sample template data</span></span>  
  
```json  
{  
    "title": "C#",  
    "snippet": null,  
    "brush": "csharp",  
    "template": "DocumentationSamplesCsharp",  
    "body": "{body}",  
    "method": "GET",  
    "scheme": "https",  
    "path": "/calc/add?a={a}&b={b}",  
    "query": "",  
    "host": "sdcontoso5.azure-api.net",  
    "headers": [  
        {  
            "name": "Ocp-Apim-Subscription-Key",  
            "description": "Subscription key which provides access to this API. Found in your <a href='/developer'>Profile</a>.",  
            "value": "{subscription key}",  
            "typeName": "string",  
            "options": null,  
            "required": true,  
            "readonly": false  
        }  
    ],  
    "parameters": []  
}  
```  
  
###  <span data-ttu-id="2a77a-202"><a name="Stub"></a>Java</span><span class="sxs-lookup"><span data-stu-id="2a77a-202"><a name="Stub"></a> Java</span></span>  
 <span data-ttu-id="2a77a-203">Den **DocumentationSamplesJava** mall kan du anpassa detta kodexempel i avsnittet kod prov på sidan igen.</span><span class="sxs-lookup"><span data-stu-id="2a77a-203">The **DocumentationSamplesJava** template allows you to customize that code sample in the code samples section of the operation page.</span></span>  
  
#### <a name="default-template"></a><span data-ttu-id="2a77a-204">Standardmall</span><span class="sxs-lookup"><span data-stu-id="2a77a-204">Default template</span></span>  
  
```xml  
// // This sample uses the Apache HTTP client from HTTP Components (http://hc.apache.org/httpcomponents-client-ga/)  
import java.net.URI;  
import org.apache.http.HttpEntity;  
import org.apache.http.HttpResponse;  
import org.apache.http.client.HttpClient;  
import org.apache.http.client.methods.HttpGet;  
import org.apache.http.client.utils.URIBuilder;  
import org.apache.http.impl.client.HttpClients;  
import org.apache.http.util.EntityUtils;  
  
public class JavaSample   
{  
    public static void main(String[] args)   
    {  
        HttpClient httpclient = HttpClients.createDefault();  
  
        try  
        {  
            URIBuilder builder = new URIBuilder("{{scheme}}://{{host}}{{path}}");  
  
{% if parameters.size > 0 -%}  
{% for parameter in parameters -%}  
            builder.setParameter("{{parameter.name}}", "{{parameter.value}}");  
{% endfor -%}  
{% endif -%}  
  
            URI uri = builder.build();  
            Http{{ method | downcase | capitalize }} request = new Http{{ method | downcase | capitalize }}(uri);  
{% for header in headers -%}  
            request.setHeader("{{header.Name}}", "{{header.value}}");  
{% endfor %}  
  
{% if body -%}  
            // Request body  
            StringEntity reqEntity = new StringEntity("{{ body | replace:'"','\"' }}");  
            request.setEntity(reqEntity);  
{% endif -%}  
  
            HttpResponse response = httpclient.execute(request);  
            HttpEntity entity = response.getEntity();  
  
            if (entity != null)   
            {  
                System.out.println(EntityUtils.toString(entity));  
            }  
        }  
        catch (Exception e)  
        {  
            System.out.println(e.getMessage());  
        }  
    }  
}  
  
```  
  
#### <a name="controls"></a><span data-ttu-id="2a77a-205">Kontroller</span><span class="sxs-lookup"><span data-stu-id="2a77a-205">Controls</span></span>  
 <span data-ttu-id="2a77a-206">Koden exempelmallarna tillåter inte användning av någon [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="2a77a-206">The code sample templates do not allow the use of any [page controls](api-management-page-controls.md).</span></span>  
  
#### <a name="data-model"></a><span data-ttu-id="2a77a-207">Datamodell</span><span class="sxs-lookup"><span data-stu-id="2a77a-207">Data model</span></span>  
 <span data-ttu-id="2a77a-208">[Kodexempel](api-management-template-data-model-reference.md#Sample) entitet.</span><span class="sxs-lookup"><span data-stu-id="2a77a-208">[Code sample](api-management-template-data-model-reference.md#Sample) entity.</span></span>  
  
#### <a name="sample-template-data"></a><span data-ttu-id="2a77a-209">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="2a77a-209">Sample template data</span></span>  
  
```json  
{  
    "title": "Java",  
    "snippet": null,  
    "brush": "java",  
    "template": "DocumentationSamplesJava",  
    "body": "{body}",  
    "method": "GET",  
    "scheme": "https",  
    "path": "/calc/add?a={a}&b={b}",  
    "query": "",  
    "host": "sdcontoso5.azure-api.net",  
    "headers": [  
        {  
            "name": "Ocp-Apim-Subscription-Key",  
            "description": "Subscription key which provides access to this API. Found in your <a href='/developer'>Profile</a>.",  
            "value": "{subscription key}",  
            "typeName": "string",  
            "options": null,  
            "required": true,  
            "readonly": false  
        }  
    ],  
    "parameters": []  
}  
```  
  
###  <span data-ttu-id="2a77a-210"><a name="JavaScript"></a>JavaScript</span><span class="sxs-lookup"><span data-stu-id="2a77a-210"><a name="JavaScript"></a> JavaScript</span></span>  
 <span data-ttu-id="2a77a-211">Den **DocumentationSamplesJs** mall kan du anpassa detta kodexempel i avsnittet kod prov på sidan igen.</span><span class="sxs-lookup"><span data-stu-id="2a77a-211">The **DocumentationSamplesJs** template allows you to customize that code sample in the code samples section of the operation page.</span></span>  
  
#### <a name="default-template"></a><span data-ttu-id="2a77a-212">Standardmall</span><span class="sxs-lookup"><span data-stu-id="2a77a-212">Default template</span></span>  
  
```xml  
<!DOCTYPE html>  
<html>  
<head>  
    <title>JSSample</title>  
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>  
</head>  
<body>  
  
<script type="text/javascript">  
    $(function() {  
        var params = {  
{% if parameters.size > 0 -%}  
            // Request parameters  
{% for parameter in parameters -%}  
            "{{parameter.name}}": "{{parameter.value}}",  
{% endfor -%}  
{% endif -%}  
        };  
  
        $.ajax({  
            url: "{{scheme}}://{{host}}{{path}}{% if path contains '?' %}&{% else %}?{% endif %}" + $.param(params),  
{% if headers.size > 0 -%}  
            beforeSend: function(xhrObj){  
                // Request headers  
{% for header in headers -%}  
                xhrObj.setRequestHeader("{{header.name}}","{{header.value}}");  
{% endfor -%}  
            },  
{% endif -%}  
            type: "{{method}}",  
{% if body -%}  
            // Request body  
            data: "{{ body | replace:'"','\"' }}",  
{% endif -%}  
        })  
        .done(function(data) {  
            alert("success");  
        })  
        .fail(function() {  
            alert("error");  
        });  
    });  
</script>  
</body>  
</html>  
  
```  
  
#### <a name="controls"></a><span data-ttu-id="2a77a-213">Kontroller</span><span class="sxs-lookup"><span data-stu-id="2a77a-213">Controls</span></span>  
 <span data-ttu-id="2a77a-214">Koden exempelmallarna tillåter inte användning av någon [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="2a77a-214">The code sample templates do not allow the use of any [page controls](api-management-page-controls.md).</span></span>  
  
#### <a name="data-model"></a><span data-ttu-id="2a77a-215">Datamodell</span><span class="sxs-lookup"><span data-stu-id="2a77a-215">Data model</span></span>  
 <span data-ttu-id="2a77a-216">[Kodexempel](api-management-template-data-model-reference.md#Sample) entitet.</span><span class="sxs-lookup"><span data-stu-id="2a77a-216">[Code sample](api-management-template-data-model-reference.md#Sample) entity.</span></span>  
  
#### <a name="sample-template-data"></a><span data-ttu-id="2a77a-217">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="2a77a-217">Sample template data</span></span>  
  
```json  
{  
    "title": "JavaScript",  
    "snippet": null,  
    "brush": "xml",  
    "template": "DocumentationSamplesJs",  
    "body": "{body}",  
    "method": "GET",  
    "scheme": "https",  
    "path": "/calc/add?a={a}&b={b}",  
    "query": "",  
    "host": "sdcontoso5.azure-api.net",  
    "headers": [  
        {  
            "name": "Ocp-Apim-Subscription-Key",  
            "description": "Subscription key which provides access to this API. Found in your <a href='/developer'>Profile</a>.",  
            "value": "{subscription key}",  
            "typeName": "string",  
            "options": null,  
            "required": true,  
            "readonly": false  
        }  
    ],  
    "parameters": []  
}  
```  
  
###  <span data-ttu-id="2a77a-218"><a name="ObjectiveC"></a>Objective C</span><span class="sxs-lookup"><span data-stu-id="2a77a-218"><a name="ObjectiveC"></a> Objective C</span></span>  
 <span data-ttu-id="2a77a-219">Den **DocumentationSamplesObjc** mall kan du anpassa detta kodexempel i avsnittet kod prov på sidan igen.</span><span class="sxs-lookup"><span data-stu-id="2a77a-219">The **DocumentationSamplesObjc** template allows you to customize that code sample in the code samples section of the operation page.</span></span>  
  
#### <a name="default-template"></a><span data-ttu-id="2a77a-220">Standardmall</span><span class="sxs-lookup"><span data-stu-id="2a77a-220">Default template</span></span>  
  
```xml  
#import <Foundation/Foundation.h>  
  
int main(int argc, const char * argv[])  
{  
    NSAutoreleasePool * pool = [[NSAutoreleasePool alloc] init];  
  
    NSString* path = @"{{scheme}}://{{host}}{{path}}";  
    NSArray* array = @[  
                         // Request parameters  
                         @"entities=true",  
{% if parameters.size > 0 -%}  
{% for parameter in parameters -%}  
                         @"{{parameter.name}}={{parameter.value}}",  
{% endfor -%}  
{% endif -%}  
                      ];  
  
    NSString* string = [array componentsJoinedByString:@"&"];  
    path = [path stringByAppendingFormat:@"?%@", string];  
  
    NSLog(@"%@", path);  
  
    NSMutableURLRequest* _request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:path]];  
    [_request setHTTPMethod:@"{{method}}"];  
{% if headers.size > 0 -%}  
    // Request headers  
{% for header in headers -%}  
    [_request setValue:@"{{header.value}}" forHTTPHeaderField:@"{{header.name}}"];  
{% endfor -%}  
{% endif -%}  
{% if body -%}  
    // Request body  
    [_request setHTTPBody:[@"{{ body | replace:'"','\"' }}" dataUsingEncoding:NSUTF8StringEncoding]];  
{% endif -%}  
  
    NSURLResponse *response = nil;  
    NSError *error = nil;  
    NSData* _connectionData = [NSURLConnection sendSynchronousRequest:_request returningResponse:&response error:&error];  
  
    if (nil != error)  
    {  
        NSLog(@"Error: %@", error);  
    }  
    else  
    {  
        NSError* error = nil;  
        NSMutableDictionary* json = nil;  
        NSString* dataString = [[NSString alloc] initWithData:_connectionData encoding:NSUTF8StringEncoding];  
        NSLog(@"%@", dataString);  
  
        if (nil != _connectionData)  
        {  
            json = [NSJSONSerialization JSONObjectWithData:_connectionData options:NSJSONReadingMutableContainers error:&error];  
        }  
  
        if (error || !json)  
        {  
            NSLog(@"Could not parse loaded json with error:%@", error);  
        }  
  
        NSLog(@"%@", json);  
        _connectionData = nil;  
    }  
  
    [pool drain];  
  
    return 0;  
}  
  
```  
  
#### <a name="controls"></a><span data-ttu-id="2a77a-221">Kontroller</span><span class="sxs-lookup"><span data-stu-id="2a77a-221">Controls</span></span>  
 <span data-ttu-id="2a77a-222">Koden exempelmallarna tillåter inte användning av någon [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="2a77a-222">The code sample templates do not allow the use of any [page controls](api-management-page-controls.md).</span></span>  
  
#### <a name="data-model"></a><span data-ttu-id="2a77a-223">Datamodell</span><span class="sxs-lookup"><span data-stu-id="2a77a-223">Data model</span></span>  
 <span data-ttu-id="2a77a-224">[Kodexempel](api-management-template-data-model-reference.md#Sample) entitet.</span><span class="sxs-lookup"><span data-stu-id="2a77a-224">[Code sample](api-management-template-data-model-reference.md#Sample) entity.</span></span>  
  
#### <a name="sample-template-data"></a><span data-ttu-id="2a77a-225">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="2a77a-225">Sample template data</span></span>  
  
```json  
{  
    "title": "ObjC",  
    "snippet": null,  
    "brush": "objc",  
    "template": "DocumentationSamplesObjc",  
    "body": "{body}",  
    "method": "GET",  
    "scheme": "https",  
    "path": "/calc/add?a={a}&b={b}",  
    "query": "",  
    "host": "sdcontoso5.azure-api.net",  
    "headers": [  
        {  
            "name": "Ocp-Apim-Subscription-Key",  
            "description": "Subscription key which provides access to this API. Found in your <a href='/developer'>Profile</a>.",  
            "value": "{subscription key}",  
            "typeName": "string",  
            "options": null,  
            "required": true,  
            "readonly": false  
        }  
    ],  
    "parameters": []  
}  
```  
  
###  <span data-ttu-id="2a77a-226"><a name="PHP"></a>PHP</span><span class="sxs-lookup"><span data-stu-id="2a77a-226"><a name="PHP"></a> PHP</span></span>  
 <span data-ttu-id="2a77a-227">Den **DocumentationSamplesPhp** mall kan du anpassa detta kodexempel i avsnittet kod prov på sidan igen.</span><span class="sxs-lookup"><span data-stu-id="2a77a-227">The **DocumentationSamplesPhp** template allows you to customize that code sample in the code samples section of the operation page.</span></span>  
  
#### <a name="default-template"></a><span data-ttu-id="2a77a-228">Standardmall</span><span class="sxs-lookup"><span data-stu-id="2a77a-228">Default template</span></span>  
  
```xml  
<?php  
// This sample uses the Apache HTTP client from HTTP Components (http://hc.apache.org/httpcomponents-client-ga/)  
require_once 'HTTP/Request2.php';  
  
$request = new Http_Request2('{{scheme}}://{{host}}{{path}}');  
$url = $request->getUrl();  
  
{% if headers.size > 0 -%}  
$headers = array(  
    // Request headers  
{% for header in headers -%}  
    '{{header.name}}' => '{{header.value}}',  
{% endfor -%}  
);  
  
$request->setHeader($headers);  
{% endif -%}  
  
{% if parameters.size > 0 -%}  
$parameters = array(  
    // Request parameters  
{% for parameter in parameters -%}  
    '{{parameter.name}}' => '{{parameter.value}}',  
{% endfor -%}  
);  
  
$url->setQueryVariables($parameters);  
{% endif -%}  
  
$request->setMethod(HTTP_Request2::METHOD_{{method}});  
  
{% if body -%}  
// Request body  
$request->setBody("{{ body | replace:'"','\"' }}");  
{% endif -%}  
  
try  
{  
    $response = $request->send();  
    echo $response->getBody();  
}  
catch (HttpException $ex)  
{  
    echo $ex;  
}  
  
?>  
```  
  
#### <a name="controls"></a><span data-ttu-id="2a77a-229">Kontroller</span><span class="sxs-lookup"><span data-stu-id="2a77a-229">Controls</span></span>  
 <span data-ttu-id="2a77a-230">Koden exempelmallarna tillåter inte användning av någon [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="2a77a-230">The code sample templates do not allow the use of any [page controls](api-management-page-controls.md).</span></span>  
  
#### <a name="data-model"></a><span data-ttu-id="2a77a-231">Datamodell</span><span class="sxs-lookup"><span data-stu-id="2a77a-231">Data model</span></span>  
 <span data-ttu-id="2a77a-232">[Kodexempel](api-management-template-data-model-reference.md#Sample) entitet.</span><span class="sxs-lookup"><span data-stu-id="2a77a-232">[Code sample](api-management-template-data-model-reference.md#Sample) entity.</span></span>  
  
#### <a name="sample-template-data"></a><span data-ttu-id="2a77a-233">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="2a77a-233">Sample template data</span></span>  
  
```json  
{  
    "title": "PHP",  
    "snippet": null,  
    "brush": "php",  
    "template": "DocumentationSamplesPhp",  
    "body": "{body}",  
    "method": "GET",  
    "scheme": "https",  
    "path": "/calc/add?a={a}&b={b}",  
    "query": "",  
    "host": "sdcontoso5.azure-api.net",  
    "headers": [  
        {  
            "name": "Ocp-Apim-Subscription-Key",  
            "description": "Subscription key which provides access to this API. Found in your <a href='/developer'>Profile</a>.",  
            "value": "{subscription key}",  
            "typeName": "string",  
            "options": null,  
            "required": true,  
            "readonly": false  
        }  
    ],  
    "parameters": []  
}  
```  
  
###  <span data-ttu-id="2a77a-234"><a name="Python"></a>Python</span><span class="sxs-lookup"><span data-stu-id="2a77a-234"><a name="Python"></a> Python</span></span>  
 <span data-ttu-id="2a77a-235">Den **DocumentationSamplesPython** mall kan du anpassa detta kodexempel i avsnittet kod prov på sidan igen.</span><span class="sxs-lookup"><span data-stu-id="2a77a-235">The **DocumentationSamplesPython** template allows you to customize that code sample in the code samples section of the operation page.</span></span>  
  
#### <a name="default-template"></a><span data-ttu-id="2a77a-236">Standardmall</span><span class="sxs-lookup"><span data-stu-id="2a77a-236">Default template</span></span>  
  
```xml  
########### Python 2.7 #############  
import httplib, urllib, base64  
  
headers = {  
{% if headers.size > 0 -%}  
    # Request headers  
{% for header in headers -%}  
    '{{header.name}}': '{{header.value}}',  
{% endfor -%}  
{% endif -%}  
}  
  
params = urllib.urlencode({  
{% if parameters.size > 0 -%}  
    # Request parameters  
{% for parameter in parameters -%}  
    '{{parameter.name}}': '{{parameter.value}}',  
{% endfor -%}  
{% endif -%}  
})  
  
try:  
{% case scheme -%}  
{% when "http" -%}  
    conn = httplib.HTTPConnection('{{host}}')  
{% when "https" -%}  
    conn = httplib.HTTPSConnection('{{host}}')  
{% endcase -%}  
    conn.request("{{method}}", "{{path}}{% if path contains '?' %}&{% else %}?{% endif %}%s" % params{% if body %}, "{{ body | replace:'"','\"' }}"{% endif %}, headers)  
    response = conn.getresponse()  
    data = response.read()  
    print(data)  
    conn.close()  
except Exception as e:  
    print("[Errno {0}] {1}".format(e.errno, e.strerror))  
  
####################################  
  
########### Python 3.2 #############  
import http.client, urllib.request, urllib.parse, urllib.error, base64  
  
headers = {  
{% if headers.size > 0 -%}  
    # Request headers  
{% for header in headers -%}  
    '{{header.name}}': '{{header.value}}',  
{% endfor -%}  
{% endif -%}  
}  
  
params = urllib.parse.urlencode({  
{% if parameters.size > 0 -%}  
    # Request parameters  
{% for parameter in parameters -%}  
    '{{parameter.name}}': '{{parameter.value}}',  
{% endfor -%}  
{% endif -%}  
})  
  
try:  
{% case scheme -%}  
{% when "http" -%}  
    conn = http.client.HTTPConnection('{{host}}')  
{% when "https" -%}  
    conn = http.client.HTTPSConnection('{{host}}')  
{% endcase -%}  
    conn.request("{{method}}", "{{path}}{% if path contains '?' %}&{% else %}?{% endif %}%s" % params{% if body %}, "{{ body | replace:'"','\"' }}"{% endif %}, headers)  
    response = conn.getresponse()  
    data = response.read()  
    print(data)  
    conn.close()  
except Exception as e:  
    print("[Errno {0}] {1}".format(e.errno, e.strerror))  
  
####################################  
```  
  
#### <a name="controls"></a><span data-ttu-id="2a77a-237">Kontroller</span><span class="sxs-lookup"><span data-stu-id="2a77a-237">Controls</span></span>  
 <span data-ttu-id="2a77a-238">Koden exempelmallarna tillåter inte användning av någon [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="2a77a-238">The code sample templates do not allow the use of any [page controls](api-management-page-controls.md).</span></span>  
  
#### <a name="data-model"></a><span data-ttu-id="2a77a-239">Datamodell</span><span class="sxs-lookup"><span data-stu-id="2a77a-239">Data model</span></span>  
 <span data-ttu-id="2a77a-240">[Kodexempel](api-management-template-data-model-reference.md#Sample) entitet.</span><span class="sxs-lookup"><span data-stu-id="2a77a-240">[Code sample](api-management-template-data-model-reference.md#Sample) entity.</span></span>  
  
#### <a name="sample-template-data"></a><span data-ttu-id="2a77a-241">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="2a77a-241">Sample template data</span></span>  
  
```json  
{  
    "title": "Python",  
    "snippet": null,  
    "brush": "python",  
    "template": "DocumentationSamplesPython",  
    "body": "{body}",  
    "method": "GET",  
    "scheme": "https",  
    "path": "/calc/add?a={a}&b={b}",  
    "query": "",  
    "host": "sdcontoso5.azure-api.net",  
    "headers": [  
        {  
            "name": "Ocp-Apim-Subscription-Key",  
            "description": "Subscription key which provides access to this API. Found in your <a href='/developer'>Profile</a>.",  
            "value": "{subscription key}",  
            "typeName": "string",  
            "options": null,  
            "required": true,  
            "readonly": false  
        }  
    ],  
    "parameters": []  
}  
```  
  
###  <span data-ttu-id="2a77a-242"><a name="Ruby"></a>Ruby</span><span class="sxs-lookup"><span data-stu-id="2a77a-242"><a name="Ruby"></a> Ruby</span></span>  
 <span data-ttu-id="2a77a-243">Den **DocumentationSamplesRuby** mall kan du anpassa detta kodexempel i avsnittet kod prov på sidan igen.</span><span class="sxs-lookup"><span data-stu-id="2a77a-243">The **DocumentationSamplesRuby** template allows you to customize that code sample in the code samples section of the operation page.</span></span>  
  
#### <a name="default-template"></a><span data-ttu-id="2a77a-244">Standardmall</span><span class="sxs-lookup"><span data-stu-id="2a77a-244">Default template</span></span>  
  
```xml  
require 'net/http'  
  
uri = URI('{{scheme}}://{{host}}{{path}}')  
uri.query = URI.encode_www_form({  
{% if parameters.size > 0 -%}  
    # Request parameters  
{% for parameter in parameters -%}  
    '{{parameter.name}}' => '{{parameter.value}}'{% unless forloop.last %},{% endunless %}  
{% endfor -%}  
{% endif -%}  
})  
  
request = Net::HTTP::{{ method | downcase | capitalize }}.new(uri.request_uri)  
{% for header in headers -%}  
# Request headers  
request['{{header.name}}'] = '{{header.value}}'  
{% endfor -%}  
{% if body -%}  
# Request body  
request.body = "{{ body | replace:'"','\"' }}"  
{% endif -%}  
  
response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|  
    http.request(request)  
end  
  
puts response.body  
  
```  
  
#### <a name="controls"></a><span data-ttu-id="2a77a-245">Kontroller</span><span class="sxs-lookup"><span data-stu-id="2a77a-245">Controls</span></span>  
 <span data-ttu-id="2a77a-246">Koden exempelmallarna tillåter inte användning av någon [sidan kontroller](api-management-page-controls.md).</span><span class="sxs-lookup"><span data-stu-id="2a77a-246">The code sample templates do not allow the use of any [page controls](api-management-page-controls.md).</span></span>  
  
#### <a name="data-model"></a><span data-ttu-id="2a77a-247">Datamodell</span><span class="sxs-lookup"><span data-stu-id="2a77a-247">Data model</span></span>  
 <span data-ttu-id="2a77a-248">[Kodexempel](api-management-template-data-model-reference.md#Sample) entitet.</span><span class="sxs-lookup"><span data-stu-id="2a77a-248">[Code sample](api-management-template-data-model-reference.md#Sample) entity.</span></span>  
  
#### <a name="sample-template-data"></a><span data-ttu-id="2a77a-249">Mallen exempeldata</span><span class="sxs-lookup"><span data-stu-id="2a77a-249">Sample template data</span></span>  
  
```json  
{  
    "title": "Ruby",  
    "snippet": null,  
    "brush": "ruby",  
    "template": "DocumentationSamplesRuby",  
    "body": "{body}",  
    "method": "GET",  
    "scheme": "https",  
    "path": "/calc/add?a={a}&b={b}",  
    "query": "",  
    "host": "sdcontoso5.azure-api.net",  
    "headers": [  
        {  
            "name": "Ocp-Apim-Subscription-Key",  
            "description": "Subscription key which provides access to this API. Found in your <a href='/developer'>Profile</a>.",  
            "value": "{subscription key}",  
            "typeName": "string",  
            "options": null,  
            "required": true,  
            "readonly": false  
        }  
    ],  
    "parameters": []  
}  
```

## <a name="next-steps"></a><span data-ttu-id="2a77a-250">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2a77a-250">Next steps</span></span>
<span data-ttu-id="2a77a-251">Mer information om hur du arbetar med mallar finns [hur du anpassar API Management developer-portalen med hjälp av mallar](api-management-developer-portal-templates.md).</span><span class="sxs-lookup"><span data-stu-id="2a77a-251">For more information about working with templates, see [How to customize the API Management developer portal using templates](api-management-developer-portal-templates.md).</span></span>