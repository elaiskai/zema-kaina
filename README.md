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
