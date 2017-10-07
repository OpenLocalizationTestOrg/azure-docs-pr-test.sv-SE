---
title: "aaaManaging enheter med hjälp av hello Azure portal – preview | Microsoft Docs"
description: "Lär dig hur toouse hello Azure portal toomanage enheter."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: a39d14e4ce8bb79f0223a9de40d5f1259a869927
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-devices-using-hello-azure-portal---preview"></a>Hantera enheter med hjälp av hello Azure-portalen – förhandsgranskning

>[!NOTE]
>Den här funktionen är för närvarande i förhandsversion. Förbereda toorevert eller ta bort alla ändringar. hello-funktionen är tillgänglig i alla Azure Active Directory (Azure AD)-prenumeration under förhandsversion. När hello funktionen blir allmänt tillgänglig, kan vissa aspekter av hello funktionen kräver en Azure Active Directory premium-prenumeration.


Med hantering av enheter i Azure Active Directory (Azure AD), kan du se till att dina användare har åtkomst till dina resurser från enheter som uppfyller dina krav för säkerhet och efterlevnad. 

Det här avsnittet:

- Förutsätter att du är bekant med hello [introduktion toodevice management i Azure Active Directory](device-management-introduction.md)

- Ger dig med information om att hantera dina enheter med hjälp av hello Azure-portalen


toomanage enheter i hello Azure-portalen, måste tooclick **enheter** i hello **hantera** avsnitt i hello hello **Azure Active Directory** bladet.

![Hantera en enhet i Intune](./media/device-management-azure-portal/11.png)




## <a name="configure-device-settings"></a>Konfigurera inställningar för enheter

toomanage dina enheter med hjälp av hello Azure-portalen, de behöver toobe registrerad eller anslutna tooAzure AD. Du kan finjustera hello process för att registrera och ansluta enheter genom att konfigurera hello Enhetsinställningar som en administratör.

![Hantera en enhet i Intune](./media/device-management-azure-portal/22.png)


hello inställningsbladet för enheten kan du tooconfigure:

- **Användare kan ansluta enheter tooAzure AD** – denna inställningar kan du tooselect hello användare som kan ansluta enheter tooAzure AD. hello standardvärdet är **alla**.

- **Ytterligare lokala administratörer på Azure AD-anslutna enheter** -du kan välja hello användare som beviljas lokal administratörsbehörighet på en enhet. Användare som läggs till här läggs toohello *Enhetsadministratörer* roll i Azure AD. Globala administratörer i Azure AD och enhetsägare beviljas lokal administratörsbehörighet som standard. Det här alternativet är en premium edition funktioner som är tillgängliga via produkter, till exempel Azure AD Premium eller hello Enterprise Mobility Suite (EMS). 

- **Användarna kan registrera sina enheter med Azure AD** -du behöver tooconfigure toobe den här inställningen tooallow enheter som registrerats med Azure AD. Om du väljer **ingen**, enheter är inte tillåtna tooregister när de inte Azure AD ansluten eller hybrid anslutna Azure AD. Registrering med Microsoft Intune eller hantering av mobilenheter (MDM) för Office 365 kräver registrering. Om du har konfigurerat någon av dessa tjänster **alla** är markerad och **NONE** är inte tillgänglig...

- **Kräv Multi-Factor Authentication toojoin enheter** -du kan välja om användare som är nödvändiga tooprovide en andra autentiseringsmetod factor toojoin deras enhet tooAzure AD. hello standardvärdet är **nr**. Vi rekommenderar att kräva multifaktorautentisering vid registrering av en enhet. Innan du aktiverar multifaktorautentisering för den här tjänsten måste du kontrollera att multifaktorautentisering har konfigurerats för hello användare som registrerar sina enheter. Mer information om olika Azure Multi-Factor authentication-tjänster finns [komma igång med Azure Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication-get-started.md). 

- **Högsta antal enheter** -den här inställningen aktiverar tooselect hello maximalt antal enheter som en användare kan ha i Azure AD. Om användarna når den här kvoten, de är inte kan tooadd fler enheter förrän ett eller flera av hello befintliga enheter har tagits bort. hello enheten citattecken räknas för alla enheter som är Azure AD ansluten eller Azure AD som registrerade idag. hello standardvärdet är **20**.

- **Användarna kan synkronisera AppData och inställningar mellan enheter** -som standard är inställningen för**NONE**. Välja specifika användare eller grupper eller alla ger hello användarens inställningar och app data toosync över deras Windows 10-enheter. Mer information om hur synkroniseringen fungerar i Windows 10.
Det här alternativet är en premium-funktioner som är tillgängliga via produkter, till exempel Azure AD Premium eller hello Enterprise Mobility Suite (EMS).
 
    ![Hantera en enhet i Intune](./media/device-management-azure-portal/21.png)




## <a name="locate-devices"></a>Hitta enheter

Som en administratör har i hello Azure-portalen du två alternativ toolocate registrerade och anslutna enheter:

- **Alla enheter** i hello **hantera** avsnitt i hello **enheter** bladet  

    ![Alla enheter](./media/device-management-azure-portal/41.png)


- **Enheter** i hello **hantera** avsnitt i en **användaren** bladet
 
    ![Alla enheter](./media/device-management-azure-portal/43.png)



Med båda alternativen kan du visa tooa som:


- Låter dig toosearch för enheter med hello visningsnamn som filter.

- Ger detaljerad översikt över registrerade och domänanslutna enheter

- Aktiverar tooperform vanliga hanteringsuppgifter för enhet
   

![Alla enheter](./media/device-management-azure-portal/51.png)


## <a name="device-management-tasks"></a>Enhetens hanteringsuppgifter

Som administratör kan du hantera hello registrerade eller anslutna enheter. Det här avsnittet ger information om vanliga hanteringsuppgifter för enheten.


**Hantera en enhet i Intune** -om du är administratör Intune kan du hantera enheter som har markerats som **Microsoft Intune**. En administratör kan se ytterligare enheter 

![Hantera en enhet i Intune](./media/device-management-azure-portal/31.png)


**Aktivera / inaktivera en enhet i Azure AD**

tooenable eller inaktivera en enhet måste toobe en global administratör i Azure AD. Inaktivera en enhet förhindrar att en enhet från att komma åt resurserna i Azure AD.  toodisable hello enhet som du kan klicka *...* Klicka på hello enhet för ytterligare information.

 
![Hantera en enhet i Intune](./media/device-management-azure-portal/33.png)

Inaktivera en enhet ändras hello tillstånd i hello **AKTIVERAD** kolumn för**nr**.

![Inaktivera en enhet](./media/device-management-azure-portal/32.png)


**Ta bort en Azure AD-enhet** -toodelete en enhet måste toobe en global administratör i Azure AD.  
Tar bort en enhet:
 
- Förhindrar att en enhet från att komma åt resurserna i Azure AD 

- Tar bort all information som är bifogade toohello enhet, till exempel, BitLocker-nycklar för Windows-enheter  

- Representerar en oåterkalleligt aktivitet och rekommenderas inte om det inte krävs

Om en enhet hanteras av en annan hanterare (t.ex. Microsoft Intune), kontrollerar du att hello enheten har rensats / dragits tillbaka innan du tar bort hello enheten i Azure AD.

Du kan antingen välja ”...” toodelete hello enhet eller klicka på hello enhet för ytterligare information
 
![Ta bort en enhet](./media/device-management-azure-portal/34.png)


**Visa eller kopiera enhets-ID** -du kan använda en enhet ID tooverify hello enhetsinformation ID på hello enhet eller med hjälp av PowerShell vid felsökning. tooaccess hello kopia klickar du på hello enhet.

![Visa en enhets-ID](./media/device-management-azure-portal/35.png)
  

**Visa eller kopiera BitLocker-nycklar** -om du är administratör kan du visa och kopiera hello BitLocker-nycklar toohelp användare toorecover sina krypterade enheten. Nycklarna är bara tillgängliga för Windows-enheter som är krypterade och har sina nycklar lagras i Azure AD. Du kan kopiera dessa nycklar vid åtkomst till information om hello enhet.
 
![Visa BitLocker-nycklar](./media/device-management-azure-portal/36.png)



## <a name="audit-logs"></a>Granskningsloggar


hello enheten aktiviteter är tillgängliga via hello aktivitetsloggar. Detta omfattar aktiviteter som utlöses av hello registreringstjänsten för enheten eller hello användare:

- Skapa en enhet och lägga till ägare/användare på hello-enhet

- Ändrar toodevice inställningar

- Enheten åtgärder, till exempel ta bort eller uppdatera en enhet
 
Din inmatning punkt toohello granska data är **granskningsloggar** i hello **aktiviteten** avsnitt i hello **enheter* bladet.

![Granskningsloggar](./media/device-management-azure-portal/61.png)


En granskningslogg har en standardlistvy som visar:

- hello datum och tid för hello förekomst

- hello mål

- Hej initieraren / aktören (som) för en aktivitet

- hello-aktivitet (vad)

![Granskningsloggar](./media/device-management-azure-portal/63.png)

Du kan anpassa hello listvyn genom att klicka på **kolumner** i hello-verktygsfältet.
 
![Granskningsloggar](./media/device-management-azure-portal/64.png)


toonarrow ned hello rapporterade data tooa nivå som fungerar för dig, kan du filtrera hello granskningsdata med hello följande fält:

- Catergory
- Resurstyp för aktivitet
- Aktivitet
- Datumintervall
- mål
- Initierad av (aktören)

Dessutom toohello filter, du kan söka efter specifika poster.

![Granskningsloggar](./media/device-management-azure-portal/65.png)

## <a name="next-steps"></a>Nästa steg

* [Introduktion toodevice management i Azure Active Directory](device-management-introduction.md)



