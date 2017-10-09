>[!NOTE]
>För resurser som inte har åtgärdats, kan du begära hello kvoter toobe aktiveras genom att öppna ett supportärende. Gör **inte** skapa ytterligare Azure Media Services-konton i ett försök tooobtain högre gränser.

| Resurs | Standardgräns | 
| --- | --- | 
| Azure Media Services-konton (AMS) i en enskild prenumeration | 25 (fast) |
| Mediereserverade enheter per AMS-konto |25 (S1, S2)<br/>10 (S3) <sup>(1)</sup> | 
| Jobb per AMS-konto | 50,000<sup>(2)</sup> |
| Länkade uppgifter per jobb | 30 (fast) |
| Tillgångar per AMS-konto | 1,000,000|
| Tillgångar per uppgift | 50 |
| Tillgångar per jobb | 100 |
| Unik positionerare som är associerad med en tillgång vid ett tillfälle | 5<sup>(4)</sup> |
| Livekanaler per AMS-konto |5|
| Program i stoppat tillstånd per kanal |50|
| Program i körningstillstånd per kanal |3|
| Strömmande slutpunkter i körningstillstånd per AMS-konto|2|
| Strömningsenheter per slutpunkt för direktuppspelning |10 |
| Lagringskonton | 1 000<sup>(5)</sup> (fast) |
| Principer | 1 000 000<sup>(6)</sup> |
| Filstorlek| I vissa fall att det finns en gräns på hello maximal filstorlek som stöds för bearbetning i Media Services. <sup>7</sup> |
  
<sup>1</sup> S3 RU:er är inte tillgängliga i västra Indien. Hej max RU gränser återställs om hello kunden ändrar hello typ (till exempel från S2 tooS1). 

<sup>2</sup> Det här värdet innefattar jobb i kö och avslutade, aktiva och avbrutna jobb. Det innefattar inte borttagna jobb. Du kan ta bort hello gamla jobb med hjälp av **IJob.Delete** eller hello **ta bort** HTTP-begäran.

Startar 1 April 2017 raderas alla jobb poster i ditt konto som är äldre än 90 dagar automatiskt, tillsammans med dess associerade aktiviteten poster, även om hello Totalt antal poster som är lägre än hello maximala kvoten. Om du behöver tooarchive hello projektaktivitet/information du kan använda hello kod beskrivs [här](../articles/media-services/media-services-dotnet-manage-entities.md).

<sup>3</sup> när du gör en begäran toolist jobbet entiteter kan högst 1 000 returneras per begäran. Om du behöver tookeep reda på alla skickade jobb kan du använda upp/skip enligt beskrivningen i [OData system frågealternativ](http://msdn.microsoft.com/library/gg309461.aspx).

<sup>4</sup> Positionerare är inte utformade för att hantera åtkomstkontroll per användare. toogive olika åtkomst rättigheter tooindividual användare använda Digital Rights Management (DRM) lösningar. Mer information finns i [det här](../articles/media-services/media-services-content-protection-overview.md) avsnittet.

<sup>5</sup> hello storage-konton måste vara från hello samma Azure-prenumeration.

<sup>6</sup> Det finns en gräns på 1 000 000 principer för olika AMS-principer (till exempel för positionerarprincipen eller ContentKeyAuthorizationPolicy). 

>[!NOTE]
> Du bör använda hello samma princip-ID om du alltid använder hello samma dagar / åtkomstbehörigheter / osv. Mer information och ett exempel finns i [det här](../articles/media-services/media-services-dotnet-manage-entities.md#limit-access-policies) avsnittet.

<sup>7</sup>om du överför innehåll tooan tillgångar i Azure Media Services med hello avsiktshantering tooprocess den med något av hello media processorer i vår tjänst (d.v.s. kodare som Media Encoder Standard och Media Encoder Premium arbetsflöde eller analys som står inför detektor), bör sedan du vara medveten om hello begränsningen på hello maximala storlek. 

Från och med den 15 maj 2017 hello maximala storleken som stöds för en enda blob är 195 TB - med filen largers än den här gränsen, uppgiften misslyckas. Vi arbetar en korrigering tooaddress den här gränsen. Dessutom hello begränsningen på hello maxstorleken för hello tillgången är som följer.

| Mediereserverad enhet | Maximal inkommande storlek (GB)| 
| --- | --- | 
|S1 | 325|
|S2 | 640|
|S3 | 260|
