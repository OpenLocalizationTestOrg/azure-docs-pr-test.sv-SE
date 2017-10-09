hello Domain Name System (DNS) är används toolocate resurser på hello internet. Till exempel när du anger en app webbadress i webbläsaren eller på en länk på en webbsida, används DNS tootranslate hello-domän till en IP-adress. hello IP-adressen är till exempel en gatuadress, men det är inte mycket mänsklig eget. Till exempel, är det mycket enklare tooremember ett DNS-namn som **contoso.com** än den som den är tooremember en IP-adress, till exempel 192.168.1.88 eller 2001:0:4137:1f67:24a2:3888:9cce:fea3.

hello DNS-systemet är baserad på *poster*. Poster associera en specifik *namn*, som **contoso.com**, med en IP-adress eller ett annat DNS-namn. När ett program, till exempel en webbläsare, letar efter ett namn i DNS, den söker efter hello-post och använder det pekar hello tooas adress. Om hello värde den punkter toois en IP-adress, använder hello webbläsare det värdet. Om den pekar tooanother DNS-namn, sedan hello programmet har toodo upplösning igen. Slutligen går all namnmatchning ut om en IP-adress.

När du skapar en webbapp i App Service tilldelas automatiskt toohello webbprogrammet ett DNS-namn. Det här namnet tar hello form av  **&lt;yourwebappname&gt;. azurewebsites.net**. Det finns också en virtuell IP-adress för användning när du skapar DNS-poster, så du kan skapa poster som punkt toohello **. azurewebsites.net**, eller peka toohello IP-adress.

> [!NOTE]
> ändrar hello IP-adressen för ditt webbprogram om du tar bort och återskapa ditt webbprogram eller ändra hello läge för App Service-plan för**lediga** när den väl har angetts för**grundläggande**, **delade**, eller **Standard**.
> 
> 

Det finns flera typer av poster med sina egna funktioner och begränsningar, men för web apps vi bara hand om två *A* och *CNAME* poster.

### <a name="address-record-a-record"></a>Adresspost (en post)
En A-post som mappar en domän, **contoso.com** eller **www.contoso.com**, *eller en jokerteckendomän med* som  **\*. contoso.com**, tooan IP-adress. Hello gäller en webbapp i App Service hello antingen virtuella IP-Adressen för hello tjänst eller en specifik IP-adress som du har köpt för ditt webbprogram.

hello huvudsakliga fördelarna med en A-post via en CNAME-post är:

* Du kan mappa en rotdomän som **contoso.com** tooan IP-adress; många registratorer endast tillåta detta med hjälp av A-poster
* Du kan ha en post som använder ett jokertecken som  **\*. contoso.com**, som kan hantera förfrågningar för flera underordnade domäner som **mail.contoso.com**,  **blogs.contoso.com**, eller **www.contso.com**.

> [!NOTE]
> Eftersom en A-post mappas tooa statisk IP-adress, den kan inte automatiskt lösa ändringar toohello IP-adressen för ditt webbprogram. En IP-adress för användning med A-poster har angetts när du konfigurerar inställningar för anpassad domän för ditt webbprogram; Det här värdet kan dock ändra om du tar bort och återskapa ditt webbprogram eller ändra hello App Service-plan läge tooback för**lediga**.
> 
> 

### <a name="alias-record-cname-record"></a>Aliaspost (CNAME-post)
En CNAME-post mappar en *specifika* DNS-namn, exempelvis **mail.contoso.com** eller **www.contoso.com**, tooanother (kanoniska) domännamn. I App Service Web Apps hello fallet hello kanoniska domännamnet är hello  **&lt;yourwebappname >. azurewebsites.net** domännamnet för din webbapp. När du skapat hello CNAME skapas ett alias för hello  **&lt;yourwebappname >. azurewebsites.net** domännamn. hello CNAME-post ska matcha toohello IP-adressen för din  **&lt;yourwebappname >. azurewebsites.net** domännamn automatiskt, så om hello IP-adressen för hello webbprogrammet ändras, behöver du inte tootake någon åtgärd.

> [!NOTE]
> Vissa domän-registratorer endast tillåter att du toomap underdomäner när du använder en CNAME-post som **www.contoso.com**, och inte rot-namn som **contoso.com**. Mer information om CNAME-poster i dokumentationen hello tillhandahålls av din registrator <a href="http://en.wikipedia.org/wiki/CNAME_record">hello Wikipedia transaktionen på CNAME-post</a>, eller hello <a href="http://tools.ietf.org/html/rfc1035">IETF domännamn - implementering och specifikation</a> dokumentet.
> 
> 

### <a name="web-app-dns-specifics"></a>Web app DNS-information
Med en A-post med Web Apps kräver att du toofirst skapar du en hello efter TXT-poster:

* **För hello rotdomän** -A DNS TXT-posten för  **@**  för  **&lt;yourwebappname&gt;. azurewebsites.net**.
* **För en specifik underordnade domän** -en DNS-namnet på  **&lt;underordnade domän >** för**&lt;yourwebappname&gt;. azurewebsites.net**. Till exempel **bloggar** om hello en post för **blogs.contoso.com**.
* **För hello jokertecken sub-dodmains** -A DNS TXT-posten för *** för  **&lt;yourwebappname&gt;. azurewebsites.net**.

Den här TXT-posten är används tooverify som du äger hello-domän som du försöker toouse. Det är dessutom toocreating en A-post som pekar toohello virtuella IP-adressen för ditt webbprogram.

Du kan hitta hello IP-adress och **. azurewebsites.net** namn för ditt webbprogram genom att utföra hello följande steg:

1. Öppna i din webbläsare hello [Azure Portal](https://portal.azure.com).
2. I hello **Web Apps** bladet Klicka hello namnet på ditt webbprogram och välj sedan **anpassade domäner** från hello längst ned på sidan för hello.
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. I hello **anpassade domäner** bladet visas hello virtuell IP-adress. Spara den här informationen som ska användas när du skapar DNS-poster
   
    ![](./media/custom-dns-web-site/virtual-ip-address.png)
   
   > [!NOTE]
   > Du kan inte använda anpassade domännamn med en **lediga** web app och måste uppgradera hello App Service-plan för**delade**, **grundläggande**, **Standard**, eller **Premium** nivå. Mer information om hello App Service plan prisnivåer, inklusive hur toochange hello prisnivå för din webbapp, se [hur tooscale webbappar](../articles/app-service-web/web-sites-scale.md).
   > 
   > 

