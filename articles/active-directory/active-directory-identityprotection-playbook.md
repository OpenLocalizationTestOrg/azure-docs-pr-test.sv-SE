---
title: aaaAzure Active Directory-identitetsskydd playbook | Microsoft Docs
description: "Lär dig hur Azure AD Identity Protection kan du toolimit hello möjligheten för en angripare tooexploit en komprometterad identitet eller enheten och toosecure en identitet eller en enhet som tidigare var misstänkt eller kända toobe komprometteras."
services: active-directory
keywords: "Azure active directory identitetsskydd, cloud app discovery, hantera program, säkerhet, risk, risknivå, säkerhetsproblem och säkerhetsprincip"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 60836abf-f0e9-459d-b344-8e06b8341d25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 6252bc133fc0c0f84800ee245a04bbf62d4cd25b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-identity-protection-playbook"></a>Azure Active Directory-identitetsskydd playbook
Den här playbook hjälper dig att:

* Fyll i data i hello identitetsskydd miljö genom simulering riskhändelser och säkerhetsproblem
* Konfigurera principer för risk-baserad villkorlig åtkomst och testa hello effekten av dessa principer

## <a name="simulating-risk-events"></a>Simulera riskhändelser
Det här avsnittet innehåller steg för att simulera hello följande risker händelsetyper:

* Inloggningar från anonyma IP-adresser (enkel)
* Inloggningar från okända platser (måttlig)
* Omöjlig resa tooatypical platser (svårt)

Andra riskhändelser kan simuleras på ett säkert sätt.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Inloggningar från anonyma IP-adresser
Den här typen av risk händelse identifierar användare som har loggat in från en IP-adress har identifierats som en anonym proxyserver IP-adress. Dessa proxyservrar är som används av användare som vill toohide sina enhetens IP-adress och kan användas för skadliga åtgärder.

**toosimulate loggar in från en anonym IP utföra hello följande**:

1. Hämta hello [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en).
2. Använd hello Tor Browser, navigera för[https://myapps.microsoft.com](https://myapps.microsoft.com).   
3. Ange hello autentiseringsuppgifter på hello-konto som du vill tooappear i hello **inloggningar från anonyma IP-adresser** rapporten.

hello-inloggning kommer att visas på instrumentpanelen för hello identitetsskydd inom 5 minuter. 

### <a name="sign-ins-from-unfamiliar-locations"></a>Inloggningar från okända platser
hello okända platser risken är en mekanism för realtid inloggning utvärdering som överväger tidigare inloggning platser (IP, latitud / longitud och ASN) toodetermine nya / okända platser. hello lagras tidigare IP-adresser, latitud / longitud och ASN: er för en användare så att dessa toobe bekant platser. En inloggning plats anses vet om hello inloggning plats inte matchar någon av hello befintliga bekant platser.

Azure Active Directory identitetsskydd:  

* har en inledande learning-period på 14 dagar då inte flaggas några nya platser som okända platser.
* ignorerar inloggningar från bekant enheter och platser som är geografiskt nära tooan befintlig bekant plats.

toosimulate okända platser, har du toosign i från en plats och en enhet som hello-kontot inte har loggat in från innan. 

**toosimulate loggar in från en okänd plats, utföra hello följande**:

1. Välj ett konto som har minst en 14 dagar inloggning historik. 
2. Gör något:
   
   a. När du använder en VPN-anslutning kan navigera för[https://myapps.microsoft.com](https://myapps.microsoft.com) och ange hello autentiseringsuppgifter på hello-konto som du vill toosimulate hello risk händelse för.
   
   b. Be en kollega i en annan plats toosign med hello kontots autentiseringsuppgifter (rekommenderas inte).

hello-inloggning kommer att visas på instrumentpanelen för hello identitetsskydd inom 5 minuter.

### <a name="impossible-travel-tooatypical-location"></a>Omöjlig resa tooatypical plats
Det är svårt att simulera hello omöjligt att resa villkoret eftersom hello algoritmen använder machine learning tooweed ut FALSKT positiva identifieringar som omöjligt att resa från bekant enheter eller inloggningar från VPN-anslutningar som används av andra användare i hello directory. Dessutom kräver hello algoritmen en historik 3 too14 dagar för hello användaren innan den börjar skapa riskhändelser.

**toosimulate en omöjlig resa tooatypical plats, utföra hello följande**:

1. Använd din standard-webbläsare, navigera för[https://myapps.microsoft.com](https://myapps.microsoft.com).  
2. Ange hello autentiseringsuppgifterna för hello-konto som du vill toogenerate händelsen omöjligt att resa risken för.
3. Ändra din användaragent. Du kan ändra användaragent i Internet Explorer från utvecklingsverktyg eller ändra din användaragent i Firefox eller Chrome med en användaragent switcher tillägg.
4. Ändra IP-adress. Du kan ändra din IP-adress med hjälp av en VPN-anslutning, Tor-tillägg eller startas en ny dator i Azure i olika datacenter.
5. Logga in för[https://myapps.microsoft.com](https://myapps.microsoft.com) med hello samma autentiseringsuppgifter som tidigare och inom några minuter efter hello tidigare inloggning.

hello-inloggning kommer att visas i instrumentpanelen för hello identitetsskydd inom 2-4 timmar.<br>
På grund av hello komplexa maskininlärning modeller som ingår, ökar risken för den inte får tas upp.<br> Du kanske vill tooreplicate stegen för flera Azure AD-konton.

## <a name="simulating-vulnerabilities"></a>Simulera säkerhetsrisker
Säkerhetsrisker är svagheter i en Azure AD-miljö som kan utnyttjas av en felaktig aktören. För närvarande exponeras 3 typer av säkerhetsproblem i Azure AD Identity Protection som utnyttjar andra funktioner i Azure AD. Dessa problem visas på hello identitetsskydd instrumentpanel automatiskt när dessa funktioner har ställts in.

* Azure AD [Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
* Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).
* Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md). 

## <a name="user-compromise-risk"></a>Användaren har komprometterats risk
**tootest användaren risken för angrepp, utföra hello följande**:

1. Logga in för[https://portal.azure.com](https://portal.azure.com) med globala administratörsbehörigheter för din klient.
2. Navigera för**identitetsskydd**. 
3. På hello huvudsakliga **Azure AD Identity Protection** bladet, klickar du på **inställningar**. 
4. På hello **portalinställningar** bladet under **säkerhetsregler**, klickar du på **användaren risken för angrepp**. 
5. På hello **logga in Risk** bladet aktivera **aktivera regeln** inaktiverat och klicka sedan på **spara** inställningar.
6. För ett givet användarkonto simulera en okänd platser eller anonym IP-händelsen för risk. Detta kommer höjer hello användaren risknivå för användaren för**medel**.
7. Vänta en stund och kontrollera att användarnivå för dina användare är **medel**.
8. Gå toohello **portalinställningar** bladet.
9. På hello **användaren risken för angrepp** bladet under **aktivera regeln**väljer **på** . 
10. Välj något av följande alternativ för hello:
    
    a. tooblock, Välj **medel** under **Block logga in**.
    
    b. tooenforce säker lösenordsändring väljer **medel** under **kräver Multi-Factor authentication**.
11. Klicka på **Spara**.
12. Nu kan du testa risk-baserad villkorlig åtkomst genom att logga in med hjälp av en användare med en ökad risk för nivå. Om hello användaren risken är medel, beroende på hello konfigurationen av din princip är din inloggning är blockeras antingen eller tvingas du toochange ditt lösenord. 
    <br><br>
    ![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
    <br>

## <a name="sign-in-risk"></a>Logga in risk
**tootest en inloggning risk, utför hello följande steg:**

1. Logga in för[https://portal.azure.com ](https://portal.azure.com) med globala administratörsbehörigheter för din klient.
2. Navigera för**identitetsskydd**.
3. På hello huvudsakliga **Azure AD Identity Protection** bladet, klickar du på **inställningar**. 
4. På hello **portalinställningar** bladet under **säkerhetsregler**, klickar du på **logga in risk**.
5. På hello ** logga in Risk ** bladet väljer **på** under **aktivera regeln**. 
6. Välj något av följande alternativ för hello:
   
   a. tooblock, Välj **medel** under **Block logga in**
   
   b. tooenforce säker lösenordsändring väljer **medel** under **kräver Multi-Factor authentication**.
7. tooblock, Välj medel under Block logga in.
8. tooenforce multifaktorautentisering, Välj **medel** under **kräver Multi-Factor authentication**.
9. Klicka på **Spara**.
10. Nu kan du testa risk-baserad villkorlig åtkomst genom att simulera hello okända platser eller anonym IP riskerar händelser eftersom de båda **medel** riskerar händelser.


![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")


## <a name="see-also"></a>Se även
* [Azure Active Directory Identity Protection](active-directory-identityprotection.md)

