---
title: aaaConnectors i hello Azure AD Synchronization Service Manager UI | Microsoft Docs
description: "Förstå hello kopplingar fliken i hello Synchronization Service Manager för Azure AD Connect."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 60f1d979-8e6d-4460-aaab-747fffedfc1e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0969630313178b1e299385b1289360c8f787cb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-connectors-with-hello-azure-ad-connect-sync-service-manager"></a>Med hjälp av anslutningar med hello Azure AD Connect Sync Service Manager

![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

hello är kopplingar används toomanage alla system hello Synkroniseringsmotorn är ansluten till.

## <a name="connector-actions"></a>Åtgärder för kopplingen
| Åtgärd | Kommentar |
| --- | --- |
| Skapa |Använd inte. Använda hello installationsguiden för att ansluta tooadditional AD-skogar. |
| Egenskaper |Används för domän- och organisationsenhetsfiltrering. |
| [Ta bort](#delete) |Använda tooeither hello data i hello connector utrymme eller toodelete anslutning tooa skog tas bort. |
| [Konfigurera körningsprofiler](#configure-run-profiles) |Förutom domän filtrering ingenting tooconfigure här. Du kan använda den här åtgärden körningsprofiler toosee som redan har konfigurerats. |
| Kör |Använda toostart som ett tillfälligt körning av en profil. |
| Stoppa |Stoppar en koppling som körs på en profil. |
| Exportera koppling |Använd inte. |
| Importera koppling |Använd inte. |
| Uppdatera anslutningen |Använd inte. |
| Uppdatera Schema |Uppdaterar hello cachelagrade schemat. Det är prioriterade toouse hello alternativ i installationsguiden för hello i stället eftersom som också uppdateringar synkroniseras regler. |
| [Söka Anslutarplats](#search-connector-space) |Toofind objekt som används och för[följer ett objekt och dess data via hello system](#follow-an-object-and-its-data-through-the-system). |

### <a name="delete"></a>Ta bort
hello borttagningsåtgärden används för två olika saker.  
![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

Hej alternativet **ta bort anslutningsplatsen endast** tar bort alla data, men behålla hello konfiguration.

Hej alternativet **ta bort kopplingen och connector** tar bort hello data och hello konfiguration. Det här alternativet används när du inte vill att tooconnect tooa skog längre.

Båda alternativen synkronisera alla objekt och uppdatera hello metaversum-objekt. Den här åtgärden är en tidskrävande åtgärd.

### <a name="configure-run-profiles"></a>Konfigurera körningsprofiler
Det här alternativet kan du toosee hello körning av profiler som konfigurerats för en koppling.

![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a>Söka Anslutarplats
hello Sökåtgärd connector utrymme är användbara toofind objekt och felsökning av dataproblem med.

![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

Starta genom att välja en **omfång**. Du kan söka baserat på data (RDN DN-fästpunkt, underträd) eller tillstånd hello-objekt (alla andra alternativ).  
![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)  
Om du till exempel göra en underträd sökning får du alla objekt i en Organisationsenhet.  
![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png)  
Från den här rutnät som du kan välja ett objekt, Välj **egenskaper**, och [följa den](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) från hello anslutningsplatsen källa, via hello metaversum och toohello mål anslutningsplatsen.

### <a name="changing-hello-ad-ds-account-password"></a>Ändra lösenordet för hello AD DS-konto
Om du ändrar lösenordet för hello hello synkroniseringstjänsten kommer inte längre att kunna tooimport och exportera ändringar tooon lokala AD.   Du kan se hello följande:

- hello importera och exportera steg för hello AD-anslutningen misslyckas med felet ”inga-start-autentiseringsuppgifter”.
- Under Windows Loggboken hello programmets händelselogg innehåller ett fel med händelse-ID 6000 och meddelandet ”hello management agent” contoso.com ”kunde inte toorun eftersom hello autentiseringsuppgifter var ogiltiga”.

tooresolve hello utfärda update hello AD DS-användarkonto hello följande:


1. Starta hello Synchronization Service Manager (START → synkroniseringstjänsten).
</br>![Synkronisering av Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)
2. Gå toohello **kopplingar** fliken.
3. Välj hello AD-koppling som är konfigurerade toouse hello AD DS-konto.
4. Välj under åtgärder, **egenskaper**.
5. I hello popup-fönstret, väljer du Anslut tooActive Directory-skogen:
6. hello skogsnamnet anger hello motsvarande lokala AD.
7. hello användarnamn Anger hello AD DS-konto som används för synkronisering.
8. Ange hello nytt lösenord för hello AD DS-konto i hello lösenordsrutan ![Azure AD Connect Sync kryptering nyckeln Utility](media/active-directory-aadconnectsync-encryption-key/key6.png)
9. Klicka på OK toosave hello nya lösenord och starta om hello synkroniseringstjänsten tooremove hello gamla lösenord från cacheminnet.



## <a name="next-steps"></a>Nästa steg
Mer information om hello [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md) konfiguration.

Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
