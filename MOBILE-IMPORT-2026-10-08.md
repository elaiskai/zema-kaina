# Mobilus išdėstymas, 2026 m. spalio 8 d.

Welcome ×3 ir Winback ×2: importui naudoti pagrindinius HTML. Local failai skirti vietinei peržiūrai.
Cart ×3, Checkout ×3 ir Product ×2: Omnisend HTML blokuose pakeisti atitinkamus omnisend/*-top.html ir *-bottom.html fragmentus. Tarp jų palikti esamą natyvų Omnisend dinaminį prekių bloką. Pilni HTML turi pavyzdines prekių vietas ir skirti peržiūrai, jų nedubliuoti su natyviu bloku.

Telefonuose kategorijos ir produktai turi plačius blokus. Kompiuteryje W1 kategorijos lieka po tris eilėje. Privalumų ikonos ir tekstai laikomi vienoje lentelės eilutėje. Panaikinta priklausomybė nuo CSS fiksuotiems pločiams, teksto aukščiams ir baziniams tarpams. Winback 1 naudoja patvirtintą lifestyle hero.

QA: 104 peržiūros, 600, 320, 390 ir 430 px su CSS ir pašalinus head stilius bei šriftų nuorodas. Atlikta papildoma importo fragmentų patikra. QA-Mobile-2026-10-08.json susieta su aktualiais HTML hash.

Tikras Omnisend / Gmail siuntimo rezultatas dar nepatvirtintas. Pirmiausia atnaujinti W1, siųsti testą ir telefone patikrinti kategorijas, privalumų sekciją bei footerį. Tada patikrinti W2, W3, Winback ir atkūrimo srautus.

Dinaminės prekės nuotraukos problema atskira: įvykio vaizdo URL turi veikti. HTML išdėstymo pakeitimai neatkuria neveikiančių senos parduotuvės nuotraukų adresų.
