En DNS-zon är används toohost hello DNS-poster för en viss domän. toostart som värd för din domän i Azure DNS, behöver du toocreate en DNS-zon för domännamnet. Varje DNS-post för din domän skapas sedan i den här DNS-zonen.

Hello domänen ”contoso.com” kan till exempel innehålla flera DNS-poster, till exempel ”mail.contoso.com” (för en e-postserver) och ”www.contoso.com” (för en webbplats).

När du skapar en DNS-zon i Azure DNS:

* hello namnet på hello zonen måste vara unika inom hello resursgruppen och hello zonen får inte redan finnas. Annars misslyckas hello åtgärden.
* hello samma zonnamn kan återanvändas i en annan resursgrupp eller Azure-prenumeration.
* Där flera zoner delar hello samma namn och varje instans tilldelas olika adresser. Endast en uppsättning adresser kan konfigureras med hello domännamnsregistratorn.

> [!NOTE]
> Du har inte tooown en domain name toocreate en DNS-zon med det domännamnet i Azure DNS. Du behöver dock tooown hello domän tooconfigure hello Azure DNS-namnservrar som hello rätt namnservrar för hello domännamnet med hello domännamnsregistratorn.
> 
> Mer information finns i [delegera en domän tooAzure DNS](../articles/dns/dns-domain-delegation.md).
