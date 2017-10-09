---
title: "aaaAzure hanterade program CredentialsCombo gränssnittselement | Microsoft Docs"
description: "Beskriver hello Microsoft.Compute.CredentialsCombo UI-element för hanterade program i Azure"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/12/2017
ms.author: tabrezm;tomfitz
ms.openlocfilehash: d44a3929ebb7a5ff78b72f9eaeb6e52b098e266f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoftcomputecredentialscombo-ui-element"></a><span data-ttu-id="8a3a5-103">Microsoft.Compute.CredentialsCombo UI-element</span><span class="sxs-lookup"><span data-stu-id="8a3a5-103">Microsoft.Compute.CredentialsCombo UI element</span></span>
<span data-ttu-id="8a3a5-104">En grupp av kontroller med inbyggda verifiering för Windows och Linux-lösenord och offentliga SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="8a3a5-104">A group of controls with built-in validation for Windows and Linux passwords and SSH public keys.</span></span> <span data-ttu-id="8a3a5-105">Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="8a3a5-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="8a3a5-106">UI-exempel</span><span class="sxs-lookup"><span data-stu-id="8a3a5-106">UI sample</span></span>
![Microsoft.Compute.CredentialsCombo](./media/managed-application-elements/microsoft.compute.credentialscombo.png)

## <a name="schema"></a><span data-ttu-id="8a3a5-108">Schemat</span><span class="sxs-lookup"><span data-stu-id="8a3a5-108">Schema</span></span>
<span data-ttu-id="8a3a5-109">Om `osPlatform` är **Windows**, hello följande schema används:</span><span class="sxs-lookup"><span data-stu-id="8a3a5-109">If `osPlatform` is **Windows**, then hello following schema is used:</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.CredentialsCombo",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": {
    "password": ""
  },
  "constraints": {
    "required": true,
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{8,}$",
    "customValidationMessage": "hello password must contain at least 8 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false
  },
  "osPlatform": "Windows",
  "visible": true
}
```

<span data-ttu-id="8a3a5-110">Om `osPlatform` är **Linux**, hello följande schema används:</span><span class="sxs-lookup"><span data-stu-id="8a3a5-110">If `osPlatform` is **Linux**, then hello following schema is used:</span></span>
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.CredentialsCombo",
  "label": {
    "authenticationType": "Authentication type",
    "password": "Password",
    "confirmPassword": "Confirm password",
    "sshPublicKey": "SSH public key"
  },
  "toolTip": {
    "authenticationType": "",
    "password": "",
    "sshPublicKey": ""
  },
  "constraints": {
    "required": true,
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{8,}$",
    "customValidationMessage": "hello password must contain at least 8 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false,
    "hidePassword": false
  },
  "osPlatform": "Linux",
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="8a3a5-111">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="8a3a5-111">Remarks</span></span>
- <span data-ttu-id="8a3a5-112">`osPlatform`måste anges, och kan vara antingen **Windows** eller **Linux**.</span><span class="sxs-lookup"><span data-stu-id="8a3a5-112">`osPlatform` must be specified, and can be either **Windows** or **Linux**.</span></span>
- <span data-ttu-id="8a3a5-113">Om `constraints.required` har angetts för**SANT**, hello lösenord eller SSH textrutor för offentlig nyckel måste innehålla värden toovalidate har.</span><span class="sxs-lookup"><span data-stu-id="8a3a5-113">If `constraints.required` is set too**true**, then hello password or SSH public key text boxes must contain values toovalidate successfully.</span></span> <span data-ttu-id="8a3a5-114">hello standardvärdet är **SANT**.</span><span class="sxs-lookup"><span data-stu-id="8a3a5-114">hello default value is **true**.</span></span>
- <span data-ttu-id="8a3a5-115">Om `options.hideConfirmation` har angetts för**SANT**, och sedan hello andra textrutan för att bekräfta hello användarens lösenord är dolt.</span><span class="sxs-lookup"><span data-stu-id="8a3a5-115">If `options.hideConfirmation` is set too**true**, then hello second text box for confirming hello user's password is hidden.</span></span> <span data-ttu-id="8a3a5-116">hello standardvärdet är **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="8a3a5-116">hello default value is **false**.</span></span>
- <span data-ttu-id="8a3a5-117">Om `options.hidePassword` har angetts för**SANT**, och sedan hello alternativet toouse lösenordsautentisering är dolt.</span><span class="sxs-lookup"><span data-stu-id="8a3a5-117">If `options.hidePassword` is set too**true**, then hello option toouse password authentication is hidden.</span></span> <span data-ttu-id="8a3a5-118">Det kan användas endast när `osPlatform` är **Linux**.</span><span class="sxs-lookup"><span data-stu-id="8a3a5-118">It can be used only when `osPlatform` is **Linux**.</span></span> <span data-ttu-id="8a3a5-119">Standardvärdet är **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="8a3a5-119">The default value is **false**.</span></span>
- <span data-ttu-id="8a3a5-120">Ytterligare begränsningar på hello tillåtna lösenord kan implementeras med hjälp av hello `customPasswordRegex` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="8a3a5-120">Additional constraints on hello allowed passwords can be implemented by using hello `customPasswordRegex` property.</span></span> <span data-ttu-id="8a3a5-121">Hej sträng i `customValidationMessage` visas när ett lösenord inte anpassad verifiering.</span><span class="sxs-lookup"><span data-stu-id="8a3a5-121">hello string in `customValidationMessage` is displayed when a password fails custom validation.</span></span> <span data-ttu-id="8a3a5-122">hello standardvärdet för båda egenskaperna är **null**.</span><span class="sxs-lookup"><span data-stu-id="8a3a5-122">hello default value for both properties is **null**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="8a3a5-123">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="8a3a5-123">Sample output</span></span>
<span data-ttu-id="8a3a5-124">Om `osPlatform` är **Windows**, eller hello användaren har angett ett lösenord i stället för en offentlig SSH-nyckel och sedan hello följande utdata förväntas:</span><span class="sxs-lookup"><span data-stu-id="8a3a5-124">If `osPlatform` is **Windows**, or hello user provided a password instead of an SSH public key, then hello following output is expected:</span></span>

```json
{
  "authenticationType": "password",
  "password": "p4ssw0rd",
}
```

<span data-ttu-id="8a3a5-125">Om hello användaren har angett en offentlig SSH-nyckel, sedan förväntas hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="8a3a5-125">If hello user provided an SSH public key, then hello following output is expected:</span></span>
```json
{
  "authenticationType": "sshPublicKey",
  "sshPublicKey": "AAAAB3NzaC1yc2EAAAABIwAAAIEA1on8gxCGJJWSRT4uOrR13mUaUk0hRf4RzxSZ1zRbYYFw8pfGesIFoEuVth4HKyF8k1y4mRUnYHP1XNMNMJl1JcEArC2asV8sHf6zSPVffozZ5TT4SfsUu/iKy9lUcCfXzwre4WWZSXXcPff+EHtWshahu3WzBdnGxm5Xoi89zcE=",
}
```

## <a name="next-steps"></a><span data-ttu-id="8a3a5-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8a3a5-126">Next steps</span></span>
* <span data-ttu-id="8a3a5-127">En introduktion toomanaged program, se [översikt över Azure Managed Application](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8a3a5-127">For an introduction toomanaged applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="8a3a5-128">En introduktion toocreating UI definitioner finns [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8a3a5-128">For an introduction toocreating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="8a3a5-129">En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="8a3a5-129">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
