# Žema kaina

[Atverti visą automatizacijų planą](AUTOMATION-FLOW-PLANAS.md)

6 automatizacijos, 15 laiškų. Welcome pasiūlymas — fiksuota **5 € nuolaida**.

| Seka | Laiškų skaičius |
|---|---:|
| Welcome | 3 |
| Win-back | 2 |
| Post-purchase | 2 |
| Product abandonment | 2 |
| Abandoned checkout | 3 |
| Abandoned cart | 3 |

Plane pateikti siuntimo laikai, temos, turinys, CTA ir sekų tarpusavio taisyklės. Tai planas, o ne aktyvuota Omnisend automatizacija.

Welcome nuolaidos kodas: **ZEMA5** (5 €). Taikymo sąlygas ir kodo galiojimą reikia tikrinti parduotuvėje.


## Welcome laiškai ×3

Aktualios versijos, atnaujinta 2026-09-16. Pirmame ir antrame laiškuose — po šešias kategorijas. Trečiame — šviesus hero su produktais, pagalba renkantis ir nuolaidos priminimas.

| Laiškas | HTML su HTTPS assetais | Vietinis HTML | Desktop JPG |
|---|---|---|---|
| 1. Susipažinimas | [welcome-01.html](welcome-01.html) | [welcome-01-local.html](welcome-01-local.html) | [JPG](welcome-01-desktop.jpg) |
| 2. Kategorijos | [welcome-02.html](welcome-02.html) | [welcome-02-local.html](welcome-02-local.html) | [JPG](welcome-02-desktop.jpg) |
| 3. Pagalba ir priminimas | [welcome-03.html](welcome-03.html) | [welcome-03-local.html](welcome-03-local.html) | [JPG](welcome-03-desktop.jpg) |

### Kaip atidaryti

GitHub HTML puslapyje rodo kodą. Pasirinkite Code → Download ZIP, išarchyvuokite visą aplanką ir naršyklėje atidarykite `welcome-01-local.html`, `welcome-02-local.html` arba `welcome-03-local.html`. Šios versijos naudoja greta esančius asset failus ir nepriklauso nuo šio kompiuterio kelių.

`welcome-01.html`, `welcome-02.html`, `welcome-03.html` vaizdų, fonų (įskaitant Outlook VML) ir šriftų adresai yra vieši HTTPS adresai į šios repozitorijos raw failus. Atsisiuntus vieną tokį HTML, assetai kraunami internetu. Nekeičiant jų nuorodų, asset failų nepervadinti ir nešalinti.

Kartu įkelti visi naudojami vaizdai ir šriftai, TXT tekstai bei desktop/mobile JPG. Istorinės vaizdų iteracijos ir privatus kliento klausimynas neįtraukti.

### Prieš siuntimą

ESP sistemoje pakeiskite `[[unsubscribe_link]]` savo atsisakymo žyma, patikrinkite ZEMA5 veikimą ir atlikite bandomąjį siuntimą. Automatizacija šiuo įkėlimu neaktyvuojama. Patikimam ilgalaikiam siuntimui vaizdus galima perkelti į ESP media biblioteką arba savo CDN; dabar nuorodos naudoja GitHub raw.

## Atkūrimo automatizacijos ×8

Pastatyta 2026-09-16 / 2026-09-17 pagal `AUTOMATION-FLOW-PLANAS.md`. Trys srautai iš plano: apleistas krepšelis ×3, apleistas atsiskaitymas ×3, produkto peržiūra ×2. Dizainas tęsia welcome laiškus. Pilnas diegimo aprašas - [AUTOMATIZACIJU-HANDOFF.md](AUTOMATIZACIJU-HANDOFF.md).

| Laiškas | Delay | Tema | HTML | Tekstas |
|---|---|---|---|---|
| C1 krepšelis | +2 val. | Jūsų pasirinkimai liko krepšelyje | [cart-01.html](cart-01.html) | [txt](cart-01.txt) |
| C2 krepšelis | +26 val. | Gera kaina svarbu. Tinkamas pasirinkimas - taip pat. | [cart-02.html](cart-02.html) | [txt](cart-02.txt) |
| C3 krepšelis | +74 val. | Grįžkime prie Jūsų krepšelio | [cart-03.html](cart-03.html) | [txt](cart-03.txt) |
| CO1 atsiskaitymas | +1 val. | Iki pirkinio liko vienas žingsnis | [checkout-01.html](checkout-01.html) | [txt](checkout-01.txt) |
| CO2 atsiskaitymas | +25 val. | Užstrigo pirkimas? Padėsime. | [checkout-02.html](checkout-02.html) | [txt](checkout-02.txt) |
| CO3 atsiskaitymas | +73 val. | Dar vienas priminimas apie Jūsų pasirinkimą | [checkout-03.html](checkout-03.html) | [txt](checkout-03.txt) |
| P1 peržiūra | +2 val. | Verta pasižiūrėti dar kartą | [product-01.html](product-01.html) | [txt](product-01.txt) |
| P2 peržiūra | +26 val. | Padėsime išsirinkti tai, kas pravers | [product-02.html](product-02.html) | [txt](product-02.txt) |

Šie HTML yra peržiūros versijos: prekių vietoje jose yra brūkšniuotas rėmelis su žyma `OMNISEND · ...`. Gyvame laiške ten įdedamas natyvus Omnisend dinaminis prekių blokas, kuris parodo tikrą gavėjo krepšelį, užsakymą ar peržiūrėtą prekę. Į Omnisend keliami `omnisend/` aplanko `-top` ir `-bottom` fragmentai, tarp jų - natyvus blokas.

Šiuose laiškuose logotipo ir footerio nėra, nes Omnisend juos prideda pats. Naujų nuolaidų nėra; C3 ir CO3 turi sąlyginį ZEMA5 priminimą, rodomą tik tiems, kam kodas dar galioja. Automatizacijos Omnisende dar nesukurtos ir neaktyvuotos.


## Win-back laiškai ×2

Atnaujinta 2026-09-17. Abiejuose laiškuose po 6 kategorijas ir 12 produktų. Produktų fonai skaidrūs, pašalinti mažmenininko vandens ženklai; džiovyklė sumažinta, vienkartinių stalo užvalkalų nuotraukoje matomas visas stalas.

| Laiškas | HTML su HTTPS assetais | Vietinis HTML | Desktop JPG |
|---|---|---|---|
| 1. Sugrįžimas | [winback-01.html](winback-01.html) | [winback-01-local.html](winback-01-local.html) | [JPG](winback-01-desktop.jpg) |
| 2. Priedai ir priežiūra | [winback-02.html](winback-02.html) | [winback-02-local.html](winback-02-local.html) | [JPG](winback-02-desktop.jpg) |

Pagrindinės HTML versijos naudoja viešus šios repozitorijos HTTPS vaizdų ir šriftų adresus. Atsisiuntus visą repozitoriją, `-local.html` versijas galima atidaryti su greta esančiais `wb-asset-*` failais. Įtraukti TXT ir desktop/mobile JPG.

Win-back laiškuose naujas nuolaidos kodas netaikomas. Prieš siuntimą `[[unsubscribe_link]]` pakeiskite savo ESP atsisakymo žyma. Įkėlimas neaktyvuoja automatizacijos.
