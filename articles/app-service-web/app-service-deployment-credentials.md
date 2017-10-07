---
title: "aaaAzure autentiseringsuppgifter för distribution av App Service | Microsoft Docs"
description: "Lär dig hur toouse hello Azure App Service-autentiseringsuppgifter för distribution."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/05/2016
ms.author: dariagrigoriu
ms.openlocfilehash: d6f9f5cc1b62a17c42643266f4c9490f827c63f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Konfigurera autentiseringsuppgifter för distribution för Azure App Service
[Azure Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714) stöder två typer av autentiseringsuppgifter för [lokal Git-distribution](app-service-deploy-local-git.md) och [FTP/S distribution](app-service-deploy-ftp.md). Dessa är inte hello samma som din Azure Active Directory-autentiseringsuppgifter.

* **Användarnivå autentiseringsuppgifter**: en uppsättning autentiseringsuppgifter för hello hela Azure-konto. Det kan vara används toodeploy tooApp Service för en app i en prenumeration som hello Azure-konto har behörigheten tooaccess. Dessa är hello autentiseringsuppgifter standarduppsättning som du konfigurerar i **Apptjänster** > **&lt;appnamn >** > **distributionsbehörigheterna**. Detta är också hello standarduppsättning som är anslutna i hello portal GUI (till exempel hello **översikt** och **egenskaper** för din app [resursbladet](../azure-resource-manager/resource-group-portal.md#manage-resources)).

    > [!NOTE]
    > När du delegerar åtkomst tooAzure resurser via rollbaserad åtkomstkontroll (RBAC) eller medadministratör behörigheter använda varje Azure användare som får åtkomst tooan app hans/hennes personliga användarnivå autentiseringsuppgifter förrän åtkomst har återkallats. Dessa autentiseringsuppgifter för distribution bör inte delas med andra Azure-användare.
    >
    >

* **App-nivå autentiseringsuppgifter**: en uppsättning autentiseringsuppgifter för varje app. Det kan vara används toodeploy toothat app. hello autentiseringsuppgifter publiceringsprofil för varje app ska genereras automatiskt vid appen skapas och hittas i hello app. Du kan inte manuellt konfigurera hello autentiseringsuppgifter, men kan du återställa dem till en app när som helst.

    > [!NOTE]
    > I ordning toogive någon åtkomst till toothese autentiseringsuppgifter via rollbaserad åtkomstkontroll (RBAC), måste du toomake dem deltagare eller högre på hello Web App. Läsare tillåts inte toopublish och därför kan inte komma åt autentiseringsuppgifterna.
    >
    >

## <a name="userscope"></a>Ange och återställa användarnivå autentiseringsuppgifter

Du kan konfigurera autentiseringsuppgifterna användarnivå i alla appar [resursbladet](../azure-resource-manager/resource-group-portal.md#manage-resources). Oavsett vilken app du konfigurera dessa autentiseringsuppgifter, det gäller tooall appar och för alla prenumerationer i ditt Azure-konto. 

tooconfigure inloggningsinformation användarnivå:

1. I hello [Azure-portalen](https://portal.azure.com), klicka på Apptjänst >  **&lt;any_app >** > **distributionsbehörigheterna**.

    > [!NOTE]
    > Du måste ha minst en app innan du kan komma åt hello distributionsblad autentiseringsuppgifter i hello-portalen. Men med hello [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md), kan du konfigurera användarnivå autentiseringsuppgifter utan en befintlig app.

2. Konfigurera hello användarnamn och lösenord och klicka sedan på **spara**.

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

När du har angett dina autentiseringsuppgifter för distribution, kan du hitta hello *Git* användarnamn för distribution i din app **översikt**,

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

och och *FTP* användarnamn för distribution i din app **egenskaper**.

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> Azure visar inte lösenordet på användarnivå distribution. Det går inte att hämta om du glömmer hello lösenord. Du kan dock återställa dina autentiseringsuppgifter genom att följa hello stegen i det här avsnittet.
>
>  

## <a name="appscope"></a>Hämta och Återställ autentiseringsuppgifterna för app-nivå
För varje app i App Service app-nivå autentiseringsuppgifterna lagras i hello XML Publicera profil.

tooget hello app-nivå autentiseringsuppgifter:

1. I hello [Azure-portalen](https://portal.azure.com), klicka på Apptjänst >  **&lt;any_app >** > **översikt**.

2. Klicka på **... Flera** > **Get publiceringsprofil**, och börjar hämtas för en. PublishSettings-filen.

    ![](./media/app-service-deployment-credentials/publish_profile_get.png)

3. Öppna hello. PublishSettings-filen och hitta hello `<publishProfile>` -tagg med hello attributet `publishMethod="FTP"`. Sedan, dess `userName` och `password` attribut.
Dessa är hello appnivå autentiseringsuppgifter.

    ![](./media/app-service-deployment-credentials/publish_profile_editor.png)

    Liknande toohello användarnivå autentiseringsuppgifter hello FTP distribution användarnamnet är i hello-format för `<app_name>\<username>`, och användarnamn för hello Git-distribution är bara `<username>` utan föregående hello `<app_name>\`.

tooreset hello app-nivå autentiseringsuppgifter:

1. I hello [Azure-portalen](https://portal.azure.com), klicka på Apptjänst >  **&lt;any_app >** > **översikt**.

2. Klicka på **... Flera** > **Återställ publiceringsprofil**. Klicka på **Ja** tooconfirm hello återställa.

    hello reset-åtgärden upphäver någon tidigare hämtade. PublishSettings-filer.

## <a name="next-steps"></a>Nästa steg

Ta reda på hur toouse dessa autentiseringsuppgifter toodeploy appen från [lokala Git](app-service-deploy-local-git.md) eller med hjälp av [FTP/S](app-service-deploy-ftp.md).
