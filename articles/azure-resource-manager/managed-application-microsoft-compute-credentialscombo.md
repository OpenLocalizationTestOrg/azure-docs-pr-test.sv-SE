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
# <a name="microsoftcomputecredentialscombo-ui-element"></a>Microsoft.Compute.CredentialsCombo UI-element
En grupp av kontroller med inbyggda verifiering för Windows och Linux-lösenord och offentliga SSH-nycklar. Du använder det här elementet när [att skapa ett Azure hanterade program](managed-application-publishing.md).

## <a name="ui-sample"></a>UI-exempel
![Microsoft.Compute.CredentialsCombo](./media/managed-application-elements/microsoft.compute.credentialscombo.png)

## <a name="schema"></a>Schemat
Om `osPlatform` är **Windows**, används följande schema:
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

Om `osPlatform` är **Linux**, används följande schema:
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

## <a name="remarks"></a>Kommentarer
- `osPlatform`måste anges, och kan vara antingen **Windows** eller **Linux**.
- Om `constraints.required` är inställd på **SANT**, lösenord eller SSH textrutor för offentlig nyckel måste innehålla värden som ska valideras. Standardvärdet är **SANT**.
- Om `options.hideConfirmation` är inställd på **SANT**, sedan den andra textrutan för att bekräfta användarens lösenord är dolt. Standardvärdet är **FALSKT**.
- Om `options.hidePassword` är inställd på **SANT**, och sedan på alternativet för att använda en lösenordsautentisering är dolt. Det kan användas endast när `osPlatform` är **Linux**. Standardvärdet är **FALSKT**.
- Ytterligare begränsningar på tillåtna lösenord kan implementeras med hjälp av den `customPasswordRegex` egenskapen. Strängen i `customValidationMessage` visas när ett lösenord inte anpassad verifiering. Standardvärdet för båda egenskaperna är **null**.

## <a name="sample-output"></a>Exempel på utdata
Om `osPlatform` är **Windows**, eller användaren har angett ett lösenord i stället för en offentlig SSH-nyckel och sedan följande utdata förväntas:

```json
{
  "authenticationType": "password",
  "password": "p4ssw0rd",
}
```

Om användaren har angett en offentlig SSH-nyckel, förväntat i följande utdata:
```json
{
  "authenticationType": "sshPublicKey",
  "sshPublicKey": "AAAAB3NzaC1yc2EAAAABIwAAAIEA1on8gxCGJJWSRT4uOrR13mUaUk0hRf4RzxSZ1zRbYYFw8pfGesIFoEuVth4HKyF8k1y4mRUnYHP1XNMNMJl1JcEArC2asV8sHf6zSPVffozZ5TT4SfsUu/iKy9lUcCfXzwre4WWZSXXcPff+EHtWshahu3WzBdnGxm5Xoi89zcE=",
}
```

## <a name="next-steps"></a>Nästa steg
* En introduktion till hanterade program, se [översikt över Azure Managed Application](managed-application-overview.md).
* En introduktion till att skapa UI-definitioner, se [komma igång med CreateUiDefinition](managed-application-createuidefinition-overview.md).
* En beskrivning av gemensamma egenskaper i UI-element, se [CreateUiDefinition element](managed-application-createuidefinition-elements.md).
