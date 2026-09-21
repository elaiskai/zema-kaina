# Žema kaina · Omnisend diegimas (5 srautai / 13 laiškų)

**Data:** 2026-09-21 · **Paskyra:** brandID `67e11c161a6c374b809601a7` · **Repo SHA:** `b0a8fe88f5bb0f81cacc84b05965f5f123a95a7c`

## Statusas

| Etapas | Būklė |
|---|---|
| 13 laiškų kaip Omnisend `email-templates` | ✅ sukurta per API (visi 201) |
| 5 automatizacijos (trigger + delay + exit) | ⛔ **užblokuota** - žr. žemiau |
| Aktyvavimas | ⏸️ nedaryta, laukia Luko |

## ⛔ Blokatorius: nepatvirtintas siuntėjo domenas

`POST /api/automations` grąžina:

```
409 email-unverified-domain
"Email uses a sender domain that is no longer verified."
```

Patikrinta `_sender_test.py` su trimis variantais - **klaida ta pati visais atvejais**:

| Bandytas `senderEmail` | Rezultatas |
|---|---|
| (praleistas, paskyros numatytasis) | 409 |
| `communications@67e11c161a6c374b809601a7.soundest.email` | 409 |
| `pardavimai@zemakaina.lt` | 409 |

Vadinasi tai **paskyros lygio blokas**, ne payload klaida: kol `zemakaina.lt` siuntėjo
domenas Omnisende nepatvirtintas, naujų automatizacijų sukurti negalima. Senosios
(2025 m.) automatizacijos paskyroje išlikusios, nes buvo sukurtos dar galiojant
autentifikacijai; visos 7 - išjungtos.

**Ką reikia padaryti (Omnisend UI, klientas + Lukas):** Settings → Sender domains →
`zemakaina.lt` → pakartoti DNS patikrą (SPF / DKIM / DMARC įrašai). Paskutinės išsiųstos
kampanijos ėjo nuo `pardavimai@zemakaina.lt`, tad įrašai kažkada veikė - greičiausiai
pasikeitė arba dingo DNS zonoje.

**Kai domenas patvirtintas, užtenka vienos komandos** (template'ai nebekuriami iš naujo,
jie paimami iš `_omnisend_ids.json`):

```
python3 _omnisend_push_flows.py automation all
```

## Sukurti template'ai

| Laiškas | Template ID | Struktūra |
|---|---|---|
| welcome-01 | `6ab14639fe9daa7b1825732f` | vienas Custom HTML (savas logo + footeris) |
| welcome-02 | `6ab1463afe9daa7b18257334` | vienas Custom HTML |
| welcome-03 | `6ab1463ada0f3ee429c11e0b` | vienas Custom HTML |
| cart-01 | `6ab145c8da0f3ee429c11d50` | logo / HTML / `product_cart_recovery` / HTML / footer / badge |
| cart-02 | `6ab145c8fe9daa7b18257283` | tas pats |
| cart-03 | `6ab145c9fe9daa7b182572c7` | tas pats |
| checkout-01 | `6ab1463ada0f3ee429c11e57` | tas pats |
| checkout-02 | `6ab1463bda0f3ee429c11e9b` | tas pats |
| checkout-03 | `6ab1463bda0f3ee429c11ee3` | tas pats |
| product-01 | `6ab1463bda0f3ee429c11f04` | tas pats, `product_cart_recovery` su 1 preke |
| product-02 | `6ab1463cfe9daa7b18257355` | tas pats |
| winback-01 | `6ab1463cfe9daa7b1825735a` | vienas Custom HTML |
| winback-02 | `6ab1463cfe9daa7b18257363` | vienas Custom HTML |

Patikra: `python3 _verify_templates.py` → 13/13, visos sekcijos vietose,
**0 `raw.githubusercontent` nuorodų, 0 `&nbsp;`**.

## Srautų konfigūracija (paruošta, dar nesukurta)

Visi `origin: api`, nes būtent taip įvykius paduoda esama šios paskyros integracija.

| Srautas | Pavadinimas | Trigger | Laikai | Exit | Re-entry |
|---|---|---|---|---|---|
| welcome | Sveiki atvykę · eLaiškai LT (2026-09) | subscribed to marketing (Email, first_subscription) | +20 min / +23 val. 40 min / +48 val. | placed order | `once` |
| cart | Apleistas krepšelis · eLaiškai LT (2026-09) | added product to cart, neveiklumas 2 val. | 0 / +24 val. / +48 val. | placed order, started checkout | 7 d. |
| checkout | Apleistas atsiskaitymas · eLaiškai LT (2026-09) | started checkout, neveiklumas 1 val. | 0 / +24 val. / +48 val. | placed order | 7 d. |
| product | Peržiūrėta prekė · eLaiškai LT (2026-09) | viewed product (inStock) | +2 val. / +24 val. | added to cart, started checkout, placed order | 14 d. |
| winback | Sugrįžimas · eLaiškai LT (2026-09) | placed order | +60 d. / +7 d. | placed order | 90 d. |

Neveiklumo laikas cart / checkout srautuose sėdi **triggeryje**, todėl prieš pirmą laišką
Delay bloko nėra (kitaip laukimas susidėtų dvigubai).

Gavėjai: cart / checkout - `nonSubscribed` (atkūrimas), welcome / product / winback - tik `subscribed`.

## Ką dar reikia nuspręsti / padaryti

1. **Siuntėjo domenas** - blokatorius, žr. aukščiau. Po patvirtinimo dar perjungti siuntėją
   į `pardavimai@zemakaina.lt` (skripte laikinai bendrinis `soundest.email` adresas).
2. **Footerio nevienodumas** - welcome ir win-back turi savo tamsų HTML footerį su
   kategorijų navigacija, adresu ir atsisakymo nuoroda; cart / checkout / product naudoja
   natyvų violetinį Omnisend footerį. Abu variantai teisiškai tvarkingi, bet prenumeratoriui
   matosi skirtumas - reikia Luko sprendimo, kurį palikti visiems.
3. **`{{abandonedCheckoutUrl}}`** - prieš aktyvuojant išbandyti gyvai (Shopify atkūrimo URL).
4. **5 € kodas (`ZEMA5`)** - sąlyginis blokas cart-03 ir checkout-03; rodyti tik tinkamiems
   naujiems prenumeratoriams (Omnisend split arba atskiras variantas be bloko).
5. **Copy** - visas parašytas 09-17 Opus fallback'u, nes Codex limitas galiojo iki 09-21 11:49.
   Limitas jau pasibaigęs, tad norint galima pergeneruoti per `lt_copy.py` ir perleisti `_build.py`.
6. **Seni flow'ai** - paskyroje yra 7 senos išjungtos automatizacijos (`Welcome No.2/No.3`,
   `Abandoned Cart No.2`, `Customer Reactivation`, `Apleista prekė`, `Sveiki atvykę`,
   `Apleistas krepšelis`). Įjungiant naujas - senas ištrinti arba palikti išjungtas.
7. **`.omnisend_key`** - API raktas laikomas šiame aplanke (į repo nekeliamas). Ištrinti, kai Lukas pasakys.

## Kas pataisyta ruošiant

- **Em / en brūkšniai** welcome ir win-back tekstuose pakeisti į paprastą brūkšnį
  (agentūros taisyklė) - 15 failų repo, `_fix_dashes.py`. ⚠️ `welcome-*-desktop.jpg` /
  `-mobile.jpg` peržiūros yra iš senos versijos, tad jose brūkšniai dar seni.
- **Assetai** persukti iš `raw.githubusercontent.com/...main` į
  `cdn.jsdelivr.net/gh/elaiskai/zema-kaina@<SHA>` - visi 50 URL patikrinti, 200 (`_cdn_check.py`).
- **`&nbsp;` / `&rarr;`** pakeisti į numerinius `&#160;` / `&#8594;` (Omnisend nukerta `&`
  named entity'uose ir atvaizduoja `bsp;`).

## Skriptai

| Failas | Paskirtis |
|---|---|
| `_om.py` | Omnisend API klientas (tik GET + POST) |
| `_probe.py` | paskyros žvalgyba (automatizacijos, kampanijos, template'ai) |
| `_omnisend_push_flows.py` | `dry` / `templates [all\|<srautas>]` / `automation [all\|<srautas>]` / `verify` |
| `_verify_templates.py` | patikra, ar 13 template'ų paskyroje ir ar struktūra teisinga |
| `_cdn_check.py` | visų jsDelivr assetų 200 patikra |
| `_sender_test.py` | 409 priežasties izoliavimas (3 siuntėjo variantai) |
| `_fix_dashes.py` | em/en brūkšnių taisymas welcome + win-back failuose |
| `_omnisend_ids.json` | sukurtų template'ų ir automatizacijų ID |
