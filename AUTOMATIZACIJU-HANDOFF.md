# Žema kaina · Atkūrimo automatizacijos (krepšelis / atsiskaitymas / peržiūra)

**Pastatyta:** 2026-09-17 · **Klientas:** zemakaina.lt (UAB Žema kaina; užsakovas Margarium MB) · **ESP:** Omnisend
**Šaltinis:** `AUTOMATION-FLOW-PLANAS.md` (2026-09-11) · **Dizainas:** tęsia `welcome-01..03`
**Statusas (2026-09-22):** maketai, copy, template'ai ir **automatizacijos Omnisende sukurtos** -
visos 5 (13 laiškų) tebėra **išjungtos**, laukia test send ir Luko aktyvavimo.
Pilnas diegimo statusas, template / automation ID ir srautų konfigūracija: **`OMNISEND-DIEGIMAS.md`**.

8 laiškai iš plane numatytų 15. Likę 7 (welcome ×3 jau repo, win-back ×2, post-purchase ×2) nėra šio paketo dalis.

| Failas | ID | Srautas | Delay | Tema |
|---|---|---|---|---|
| `cart-01.html` | C1 | Apleistas krepšelis | +2 val. | Jūsų pasirinkimai liko krepšelyje |
| `cart-02.html` | C2 | Apleistas krepšelis | +26 val. | Gera kaina svarbu. Tinkamas pasirinkimas - taip pat. |
| `cart-03.html` | C3 | Apleistas krepšelis | +74 val. | Grįžkime prie Jūsų krepšelio |
| `checkout-01.html` | CO1 | Apleistas atsiskaitymas | +1 val. | Iki pirkinio liko vienas žingsnis |
| `checkout-02.html` | CO2 | Apleistas atsiskaitymas | +25 val. | Užstrigo pirkimas? Padėsime. |
| `checkout-03.html` | CO3 | Apleistas atsiskaitymas | +73 val. | Dar vienas priminimas apie Jūsų pasirinkimą |
| `product-01.html` | P1 | Produkto peržiūra | +2 val. | Verta pasižiūrėti dar kartą |
| `product-02.html` | P2 | Produkto peržiūra | +26 val. | Padėsime išsirinkti tai, kas pravers |

Laikai lentelėje - nuo atskaitos įvykio, ne nuo ankstesnio laiško. Diegiant Delay žingsniai: C 2 val. → 24 val. → 48 val.; CO 1 val. → 24 val. → 48 val.; P 2 val. → 24 val. Jei neveiklumo laikotarpis jau įskaičiuotas triggeryje, antrą kartą jo kaip Delay nepridėti.

## Struktūra: kur eina dinaminis blokas

Kiekvienas laiškas Omnisende sudedamas iš **trijų** dalių:

```
[logotipas - natyvus Omnisend blokas]
[Custom HTML - omnisend/<vardas>-top.html]
[product_cart_recovery - NATYVUS Omnisend blokas]   <- privalomas, tai srauto funkcija
[Custom HTML - omnisend/<vardas>-bottom.html]
[footer - natyvus Omnisend blokas]
```

Šakninis `<vardas>.html` yra **peržiūros** versija: vietoje natyvaus bloko jame yra brūkšniuotas rėmelis su žyma `OMNISEND · ...` ir dviem tuščiomis prekių kortelėmis. Į Omnisend keliami `omnisend/` fragmentai, ne šis failas.

- C1-C3, CO1-CO3 → `product_cart_recovery` (2 stulpeliai), rodo realų krepšelį / checkout
- P1-P2 → `product_cart_recovery` su peržiūrėtu produktu (Omnisend product abandonment seka)
- Dinaminių blokų **niekada** nešalinti - be jų laiškas netenka prasmės

HTML be logotipo ir be footerio (juos Omnisend prideda natyviai). Jei laiškai kada nors siunčiami ne per Omnisend, reikės pridėti footerį su `[[unsubscribe_link]]`, kaip `welcome-*.html`.

## CTA nuorodos

- C ir CO laiškuose mygtuko `href` = `{{abandonedCheckoutUrl}}` (Omnisend atkūrimo URL; patikrinta gamyboje kitam klientui). Prieš aktyvuojant **išbandyti gyvai** - jei Shopify integracija grąžina checkout atkūrimą, mygtuko tekstas turi derėti su tuo, kur žmogus nukrenta.
- P laiškuose HTML mygtukas veda į `https://zemakaina.lt/collections/all`, o į konkrečią prekę - natyvaus produkto bloko mygtukas. P laiškuose sąmoningai nerašoma „palikote krepšelyje“.

## Nuolaida

Naujų nuolaidų šiose sekose nėra. C3 ir CO3 turi **sąlyginį** 5 € bloką (`ZEMA5`) - tai welcome dovanos priminimas. Rodyti tik gavėjams, kuriems kodas realiai galioja (nauji, dar nepirkę prenumeratoriai). Realizacija: Omnisend split arba atskiras laiško variantas be to bloko. Jei kupono galiojimo / minimalios sumos taisyklės neaiškios - bloką išimti, ne spėlioti.

## Trigger / exit / frequency

| Srautas | Trigger | Filtras | Exit | Frequency | Gavėjai |
|---|---|---|---|---|---|
| Krepšelis | Added product to cart | krepšelis neatnaujintas 2 val. | started checkout, placed order, tuščias krepšelis, unsubscribe | 7 d. | subscribed + non-subscribed |
| Atsiskaitymas | Started checkout | checkout neatnaujintas 1 val. | placed order, netinkamas/tuščias checkout | 7 d. | subscribed + non-subscribed |
| Peržiūra | Viewed product (in stock) | atpažintas kontaktas, prekė neįdėta į krepšelį | added to cart, started checkout, placed order, unsubscribe | 14 d. | tik subscribed |

Sekų tarpusavio logika (iš plano): peržiūra → krepšelis → checkout → pirkimas, kiekvienas vėlesnis etapas stabdo ankstesnio priminimus. Omnisend vieno prioritetų jungiklio neturi - valdyti per Exit Conditions, Skip Contacts ir dažnį, ir **patikrinti bandoma kelione**, nes abipusis slopinimas gali neleisti įeiti į checkout seką po cart. Rekomendacija: ne daugiau kaip vienas pardaviminis automatizacijos laiškas per 24 val.

## Faktai laiškuose (patikrinta 2026-09-17 gyvai)

| Teiginys | Šaltinis |
|---|---|
| Pristatymas per 1-3 darbo dienas nuo užsakymo patvirtinimo | `/policies/shipping-policy` |
| Kurjeriu arba į paštomatą, pagal prekės dydį ir svorį | `/policies/shipping-policy` |
| Pristatymo kaina priklauso nuo būdo, dydžio ir svorio; matoma prieš patvirtinant | `/policies/shipping-policy` |
| Bankinis pavedimas, mokėjimas internetu, atsiskaitymas kurjeriui, lizingas | `/pages/apmokejimo-budai` |
| +370 622 09006, pardavimai@zemakaina.lt | `/pages/contact` |

Nemokamo pristatymo, rezervuoto krepšelio, garantijų trukmės ar dirbtinio skubinimo laiškuose nėra - plane tai eksplicitiškai uždrausta.

## ⚠️ Copy statusas

Copy parašytas **Opus fallback'u**, nes Codex (`lt_copy.py`) atsimušė į ChatGPT prenumeratos usage limitą, galiojantį **iki 2026-09-21 11:49**. Tekstai laikomi `_copy/copy.json`; atsistačius limitui juos galima pergeneruoti per `lt_copy.py` ir perleisti `_build.py` - maketų keisti nereikės. Prieš siuntimą Lukui verta peržiūrėti copy.

Pastaba: esami `welcome-01..03` laiškai turi em brūkšnius (—), kurių agentūros taisyklė neleidžia. Šiuose 8 laiškuose naudojamas paprastas brūkšnys (-). Jei norima suvienodinti, welcome tekstus reikėtų pataisyti atskirai.

## Prieš aktyvuojant

1. Patikrinti realius Shopify → Omnisend įvykius: Added to cart, Started checkout, Viewed product, Placed order.
2. Išbandyti `{{abandonedCheckoutUrl}}` gyvu užsakymu (desktop + mobile).
3. Peržiūrėti, ar Shopify / Omnisend jau nesiunčia savo atkūrimo laiškų - nedubliuoti.
4. Patikrinti ZEMA5 galiojimą ir tinkamumą prieš paliekant sąlyginius blokus.
5. Bandomieji scenarijai: tik peržiūra; peržiūra → cart; cart → checkout; checkout → pirkimas; laukiamas pavedimas; išparduota prekė; atsisakyta prenumerata.
6. Aktyvavimas - tik Lukui patvirtinus.

## Failai

```
clients/zemakaina/automations/
  _build.py          - generuoja HTML + TXT + omnisend/ fragmentus is _copy/copy.json
  _push.py           - ikelia i elaiskai/zema-kaina repo saknini lygi
  _copy/copy.json    - visas copy (subject, preheader, antrastes, CTA)
  <vardas>.html      - perziura
  <vardas>.txt       - plain text
  omnisend/          - top/bottom fragmentai Omnisend Custom HTML blokams
```
