---
title: aaaChange hello Azure Active Directory-klient i Azure RemoteApp | Microsoft Docs
description: "Lär dig hur toochange hello Azure Active Directory-klient som är associerade med Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 20faf169-6e48-428a-8bdd-f231daff19fa
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d0928b099b7fcfb3ab16077e295d7aaf519c3653
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-azure-active-directory-tenant-in-azure-remoteapp"></a>Ändra hello Azure Active Directory-klient i Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Azure RemoteApp använder Azure Active Directory (AD Azure) tooallow användaråtkomst. hello endast Azure AD-klient som du kan använda i Azure RemoteApp är hello något som är associerade med hello Azure-prenumeration. Du kan visa hello associerade prenumeration på hello **inställningar** sidan hello-portalen. Titta på hello **Directory** kolumn på hello **prenumerationer** fliken.

> [!NOTE]
> För den här ändringen toosucceed först ta bort alla användare från hello befintliga Azure Active Directory-klient från alla Azure RemoteApp-samlingar. toodo detta, gå toohello Azure Portal, gå toohello **Azure RemoteApp** fliken och öppna varje Azure RemoteApp-samling. Gå toohello **användare** fliken och ta bort användare som tillhör tooyour aktuella Azure Active Directory-klient. Upprepa för alla befintliga Azure RemoteApp-samlingar. Utan att göra detta, kommer inte att kunna toocreate eller korrigering samlingar.
> 
> 

Om du vill toouse en annan klient använder du dessa steg toochange hello association med din prenumeration:

1. Ta bort alla Azure AD-användare toowhich i hello portal du har gett åtkomst tooAzure RemoteApp-samlingar. (Se anmärkning av hello ovan anvisningar om hur toodo detta.)
2. Ange ett Microsoft-konto (kallades Live ID) som hello tjänstadministratör. (Inte vet om du redan är hello tjänstadministratör? Du hittar genom att klicka på **Inställningar -> administratörer**.) Här är nu hur du ändrar som:
   
   1. Klicka på hello användare i hello övre högra hörnet och klicka sedan på **Visa min faktura**.
   2. Klicka på hello prenumeration. På hello ny sida, bläddra nedåt och klicka på **redigera prenumerationsinformation** i hello rätt. (Slags hello mellersta nedre högra hörnet om som hjälper dig att hitta den.)
   3. Skriv hello Microsoft-konto för hello-användare som ska vara hello tjänstadministratör
3. Nu logga ut från hello-portalen och logga sedan in igen med hello Microsoft-konto som du angav i föregående steg i hello.
4. Klicka på **App Ny -> tjänster -> Active Directory-katalog > -> anpassat skapa**.
5. Under **Directory**, Välj **Använd befintlig katalog**. Vi toohave toosign utanför hello portal nu, så du **jag är redo toobe utloggad nu**.
6. Logga in hello portalen som global administratör för hello katalog du vill tooadd. (Om du inte redan en global administratör, kommer du att efter en hundratal logga in och logga sedan ut.)
7. Du ombeds när du loggar in om du vill toouse din befintliga AD-klient med din prenumeration. Klicka på **Fortsätt**, och klicka sedan på **logga ut nu**.
8. Logga in igen och gå tillbaka för**Inställningar -> prenumerationer**. Välj din prenumeration och klicka sedan på **redigera katalog**. Välj hello Azure AD-klient som du vill toouse.

Du kan nu använda hello nya Azure AD-klient toocontrol åtkomst toohello Azure-prenumeration och tooconfigure användaråtkomst i Azure RemoteApp.

