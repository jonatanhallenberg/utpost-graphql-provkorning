# Beslutsdokument: API-stil

*Ett av teamets sex beslutsdokument. Skrivs i M4, hålls levande. Samma mall som `testing.md` och `databas.md`.*

**Datum:**
**Beslut:** *(en eller två meningar: ersätter GraphQL REST, kompletterar det, eller stannar ni på REST för vissa delar? Vilka?)*

## Bakgrund
*Skuld 6 med era egna siffror. Hur många anrop gör startsidan i React-klienten, hur många byte kommer tillbaka, och hur mycket av det används? Samma sak för turlistan. Mät i nätverksfliken eller med `curl … | wc -c`.*

| Vy | REST: anrop | REST: byte | GraphQL: anrop | GraphQL: byte | Databasfrågor (API-loggen) |
|---|---|---|---|---|---|
| Startsidan | | | | | |
| Turlistan | | | | | |

## Schemat i korthet
*Vilka typer och vilka ingångar i `Query` och `Mutation` har ni i v1? Vad heter fälten, och varför heter de inte som kolumnerna? Vad har ni medvetet lämnat utanför schemat (t.ex. `password_hash`)?*

## Vad som går via GraphQL och vad som stannar i REST
*Endpoint för endpoint eller vy för vy. Inloggning? Bilduppladdning? Health? Motivera varje rad.*

## Alternativ vi jämförde
*Minst två riktiga alternativ, t.ex. "laga REST-endpointsen" (smalare svar, en sammansatt endpoint för startsidan) och "GraphQL för allt". Vad talar för och emot vart och ett?*

## Konsekvenser
*Vad kostar valet? Tänk på: cachning, felhantering i klienten (200 med `errors`), N+1 i resolvers (åtgärdas i M5), två sätt att prata med API:et under migreringen, typer som skrivs för hand tills codegen finns.*

**Skrivet av:**
