---
title: "Azure hanterade program CredentialsCombo gränssnittselement | Microsoft Docs"
description: "Beskriver Microsoft.Compute.CredentialsCombo UI-element för hanterade program i Azure"
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
ms.openlocfilehash: 254f383ee6f7cb9f7051fa135d85319a22c3c369
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="microsoftcomputecredentialscombo-ui-element"></a><span data-ttu-id="329bd-103">Microsoft.Compute.CredentialsCombo UI-element</span><span class="sxs-lookup"><span data-stu-id="329bd-103">Microsoft.Compute.CredentialsCombo UI element</span></span>
<span data-ttu-id="329bd-104">En grupp av kontroller med inbyggda verifiering för Windows och Linux-lösenord och offentliga SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="329bd-104">A group of controls with built-in validation for Windows and Linux passwords and SSH public keys.</span></span> <span data-ttu-id="329bd-105">Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="329bd-105">You use this element when [creating an Azure Managed Application](managed-application-publishing.md).</span></span>

## <a name="ui-sample"></a><span data-ttu-id="329bd-106">UI-exempel</span><span class="sxs-lookup"><span data-stu-id="329bd-106">UI sample</span></span>
![Microsoft.Compute.CredentialsCombo](./media/managed-application-elements/microsoft.compute.credentialscombo.png)

## <a name="schema"></a><span data-ttu-id="329bd-108">Schemat</span><span class="sxs-lookup"><span data-stu-id="329bd-108">Schema</span></span>
<span data-ttu-id="329bd-109">Om `osPlatform` är **Windows**, används följande schema:</span><span class="sxs-lookup"><span data-stu-id="329bd-109">If `osPlatform` is **Windows**, then the following schema is used:</span></span>
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
    "customValidationMessage": "The password must contain at least 8 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false
  },
  "osPlatform": "Windows",
  "visible": true
}
```

<span data-ttu-id="329bd-110">Om `osPlatform` är **Linux**, används följande schema:</span><span class="sxs-lookup"><span data-stu-id="329bd-110">If `osPlatform` is **Linux**, then the following schema is used:</span></span>
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
    "customValidationMessage": "The password must contain at least 8 characters, with at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false,
    "hidePassword": false
  },
  "osPlatform": "Linux",
  "visible": true
}
```

## <a name="remarks"></a><span data-ttu-id="329bd-111">Kommentarer</span><span class="sxs-lookup"><span data-stu-id="329bd-111">Remarks</span></span>
- <span data-ttu-id="329bd-112">`osPlatform`måste anges, och kan vara antingen **Windows** eller **Linux**.</span><span class="sxs-lookup"><span data-stu-id="329bd-112">`osPlatform` must be specified, and can be either **Windows** or **Linux**.</span></span>
- <span data-ttu-id="329bd-113">Om `constraints.required` är inställd på **SANT**, lösenord eller SSH textrutor för offentlig nyckel måste innehålla värden som ska valideras.</span><span class="sxs-lookup"><span data-stu-id="329bd-113">If `constraints.required` is set to **true**, then the password or SSH public key text boxes must contain values to validate successfully.</span></span> <span data-ttu-id="329bd-114">Standardvärdet är **SANT**.</span><span class="sxs-lookup"><span data-stu-id="329bd-114">The default value is **true**.</span></span>
- <span data-ttu-id="329bd-115">Om `options.hideConfirmation` är inställd på **SANT**, sedan den andra textrutan för att bekräfta användarens lösenord är dolt.</span><span class="sxs-lookup"><span data-stu-id="329bd-115">If `options.hideConfirmation` is set to **true**, then the second text box for confirming the user's password is hidden.</span></span> <span data-ttu-id="329bd-116">Standardvärdet är **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="329bd-116">The default value is **false**.</span></span>
- <span data-ttu-id="329bd-117">Om `options.hidePassword` är inställd på **SANT**, och sedan på alternativet för att använda en lösenordsautentisering är dolt.</span><span class="sxs-lookup"><span data-stu-id="329bd-117">If `options.hidePassword` is set to **true**, then the option to use password authentication is hidden.</span></span> <span data-ttu-id="329bd-118">Det kan användas endast när `osPlatform` är **Linux**.</span><span class="sxs-lookup"><span data-stu-id="329bd-118">It can be used only when `osPlatform` is **Linux**.</span></span> <span data-ttu-id="329bd-119">Standardvärdet är **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="329bd-119">The default value is **false**.</span></span>
- <span data-ttu-id="329bd-120">Ytterligare begränsningar på tillåtna lösenord kan implementeras med hjälp av den `customPasswordRegex` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="329bd-120">Additional constraints on the allowed passwords can be implemented by using the `customPasswordRegex` property.</span></span> <span data-ttu-id="329bd-121">Strängen i `customValidationMessage` visas när ett lösenord inte anpassad verifiering.</span><span class="sxs-lookup"><span data-stu-id="329bd-121">The string in `customValidationMessage` is displayed when a password fails custom validation.</span></span> <span data-ttu-id="329bd-122">Standardvärdet för båda egenskaperna är **null**.</span><span class="sxs-lookup"><span data-stu-id="329bd-122">The default value for both properties is **null**.</span></span>

## <a name="sample-output"></a><span data-ttu-id="329bd-123">Exempel på utdata</span><span class="sxs-lookup"><span data-stu-id="329bd-123">Sample output</span></span>
<span data-ttu-id="329bd-124">Om `osPlatform` är **Windows**, eller användaren har angett ett lösenord i stället för en offentlig SSH-nyckel och sedan följande utdata förväntas:</span><span class="sxs-lookup"><span data-stu-id="329bd-124">If `osPlatform` is **Windows**, or the user provided a password instead of an SSH public key, then the following output is expected:</span></span>

```json
{
  "authenticationType": "password",
  "password": "p4ssw0rd",
}
```

<span data-ttu-id="329bd-125">Om användaren har angett en offentlig SSH-nyckel, förväntat i följande utdata:</span><span class="sxs-lookup"><span data-stu-id="329bd-125">If the user provided an SSH public key, then the following output is expected:</span></span>
```json
{
  "authenticationType": "sshPublicKey",
  "sshPublicKey": "AAAAB3NzaC1yc2EAAAABIwAAAIEA1on8gxCGJJWSRT4uOrR13mUaUk0hRf4RzxSZ1zRbYYFw8pfGesIFoEuVth4HKyF8k1y4mRUnYHP1XNMNMJl1JcEArC2asV8sHf6zSPVffozZ5TT4SfsUu/iKy9lUcCfXzwre4WWZSXXcPff+EHtWshahu3WzBdnGxm5Xoi89zcE=",
}
```

## <a name="next-steps"></a><span data-ttu-id="329bd-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="329bd-126">Next steps</span></span>
* <span data-ttu-id="329bd-127">En introduktion till hanterade program, se [översikt över Azure Managed Application](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="329bd-127">For an introduction to managed applications, see [Azure Managed Application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="329bd-128">En introduktion till att skapa UI-definitioner, se [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).</span><span class="sxs-lookup"><span data-stu-id="329bd-128">For an introduction to creating UI definitions, see [Getting started with CreateUiDefinition](managed-application-createuidefinition-overview.md).</span></span>
* <span data-ttu-id="329bd-129">En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).</span><span class="sxs-lookup"><span data-stu-id="329bd-129">For a description of common properties in UI elements, see [CreateUiDefinition elements](managed-application-createuidefinition-elements.md).</span></span>
