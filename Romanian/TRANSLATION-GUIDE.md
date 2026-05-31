# Ghid de traducere (RO) — convenții pentru localizarea scripturilor

Acest document fixează regulile folosite la traducerea în română a scripturilor din `English/`.
Scopul: traduceri fidele, naturale și **consecvente** între toate episoadele, cu termenii
tehnici păstrați în engleză și explicați în română la prima apariție.

> Documentul este scris în engleză + română intenționat, ca să poată fi verificat ușor.
> The guide is intentionally bilingual so the conventions can be reviewed quickly.

---

## 1. Reguli generale

1. **Limba țintă:** română literară, naturală, cu diacritice corecte (ă, â, î, ș, ț). Ton calm,
   curios, ușor jucăuș — la fel ca originalul (vezi `00-SERIES-OVERVIEW.md`, secțiunea „Tone & voice").
2. **Se traduce** tot textul destinat oamenilor: narațiunea `**VO:**`, textul din
   `[ON SCREEN: ...]`, descrierile din `[GFX: ...]`, `[B-ROLL: ...]`, `[SFX: ...]`,
   subtitlurile descriptive din `[LOWER THIRD: ...]`, titlurile de secțiune, metadatele din antet.
3. **NU se traduc (rămân în engleză):**
   - **Cuvintele-cheie ale etichetelor de producție:** `**VO:**`, `[ON SCREEN: ...]`,
     `[GFX: ...]`, `[B-ROLL: ...]`, `[LOWER THIRD: ...]`, `[SFX: ...]`, `[CHECKLIST]`,
     `COLD OPEN`, `TITLE CARD`, `SEGMENT`, `RECAP + TEASE`, `SOURCES`.
     *Motiv:* sunt definite ca o convenție fixă în „production bible" și pot fi citite de un
     parser/pipeline (vezi `VIDEO-PRODUCTION-PLAN.md`). Conținutul **dinăuntrul** etichetei se traduce.
   - **Numele proprii:** persoane (Edward Bernays), instituții, branduri (Lucky Strike).
   - **Titlurile de carte/lucrări:** *Propaganda* (1928) — se păstrează, cu glosă în paranteză
     la nevoie: *Propaganda* („Propagandă").
   - **Citările din `SOURCES` și `[LOWER THIRD]`:** „Source: RAND Corp., 2016", URL-uri, date.
     Eticheta „Source:" poate rămâne sau deveni „Sursă:" — vezi mai jos, se folosește **„Sursă:"**.
   - **Termenii tehnici** (vezi secțiunea 3 și glosarul) — păstrați în engleză, explicați la prima apariție.

4. **Sloganuri/citate istorice în engleză** (ex. „It's toasted", „Reach for a Lucky instead of a
   sweet", „torches of freedom"): se **păstrează în engleză** (sunt artefacte care apar pe ecran
   în original) și se adaugă traducerea română în paranteze pătrate. Exemplu:
   > „It's toasted" *[„E prăjit"]*

5. **Numere, date, statistici:** se păstrează identic (14 miliarde, 1929, 31 martie). Se folosește
   formatul românesc pentru numere mari („14 miliarde de țigări").

---

## 2. Convenția pentru termeni tehnici (REGULA CHEIE)

La **prima apariție într-un episod**, termenul tehnic se păstrează în engleză (în *italice*),
urmat imediat de o scurtă glosă în română. Episoadele sunt gândite să fie urmărite independent,
deci glosa se repetă o dată în fiecare episod în care apare termenul.

**Tipar:** *termen englezesc* — în română, „echivalent" — explicație scurtă.

**Exemple:**
- „...operatori care folosesc *sockpuppets* — în română, „marionete": conturi false controlate
  de oameni reali pentru a ascunde cine vorbește de fapt."
- „Aici intervine *framing*-ul (încadrarea) — controlul a ceea ce *înseamnă* un fapt înainte
  să apuci să-l judeci."
- „Este un *pseudo-event* (pseudo-eveniment): un eveniment regizat special ca să fie relatat ca știre."

La aparițiile **următoare din același episod**, se folosește doar termenul englezesc (în italice),
fără a repeta explicația: „...încă un *pseudo-event*..."

Acolo unde limba română are un echivalent deja încetățenit și clar (ex. „propagandă",
„relații publice"), se poate folosi direct echivalentul românesc, menționând termenul englez în
paranteză la prima apariție: relații publice (*public relations*).

---

## 2b. Marcaje pentru TTS (pronunție engleză + accent)

Pentru ca motorul TTS să pronunțe corect termenii englezești și să sublinieze traducerile-cheie,
folosim două etichete inline, **doar în textul rostit (`**VO:**`)**. NU se pun în `[ON SCREEN]`,
`[GFX]`, `[LOWER THIRD]` etc. (acelea nu se rostesc).

- `<en>...</en>` — text de pronunțat în **engleză** (termeni tehnici, sloganuri istorice rostite).
- `<emf>...</emf>` — text de **accentuat** (de regulă echivalentul/traducerea în română, la prima
  definire a termenului).

Reguli:
- Fiecare apariție a unui termen tehnic englezesc în VO se înconjoară cu `<en>` (pentru pronunție corectă).
- La prima definire a termenului, glosa românească se accentuează **o singură dată** cu `<emf>`.
- Se pot îngloba: `<emf><en>sock puppet</en></emf>`.
- Când un cuvânt e marcat cu `<en>`/`<emf>`, nu mai e nevoie de *italice* în VO.
- Numele proprii (Bernays, Lucky Strike) NU se marchează implicit, ca să nu încărcăm textul.

Conversie la randare (un mic script transformă marcajele în funcție de motor):

| Marcaj | Motoare cu SSML (Azure / Google Cloud) | Motoare fără SSML (Piper / Kokoro) |
|---|---|---|
| `<en>X</en>` | `<lang xml:lang="en-US">X</lang>` | se elimină eticheta, se citește textul |
| `<emf>X</emf>` | `<emphasis level="moderate">X</emphasis>` | se elimină eticheta (opțional, micro-pauză) |

**Atenție la motorul TTS pentru română:** Kokoro **nu** suportă limba română. Pentru VO în română
folosiți Edge TTS (voci `ro-RO`, gratuit), **Azure Speech** (SSML complet — recomandat pentru
emfază + schimbare de limbă pe cuvânt; are nivel gratuit) sau ElevenLabs multilingv (calitate
mare, detectează singur cuvintele englezești). Pe Piper/Kokoro etichetele se ignoră elegant.

---

## 3. Glosar (termen EN → RO + explicație de referință)

Explicațiile de mai jos sunt versiuni de referință; pot fi adaptate ușor la contextul frazei.

| Termen (EN) | Echivalent RO | Explicație scurtă (RO) |
|---|---|---|
| Transfer | transfer | atașarea unui produs de o emoție/valoare pe care o ai deja (ex. libertatea) |
| Testimonial | mărturie / recomandare | o persoană (adesea o autoritate) garantează pentru un produs |
| Borrowed authority | autoritate împrumutată | folosirea prestigiului unui expert sau al unei instituții |
| Bandwagon | efectul de turmă | îndemnul „alătură-te, toți o fac deja" |
| Social proof | dovada socială | tendința de a crede corect ceea ce fac mulți alții |
| Pseudo-event | pseudo-eveniment | eveniment regizat special ca să fie relatat ca știre |
| Front group | grup-paravan | organizație care ascunde adevăratul sponsor în spatele unei fețe „neutre" |
| Third-party technique | tehnica terței părți | ascunderea vânzătorului în spatele unor voci „independente" |
| Glittering generalities | generalități strălucitoare | cuvinte-virtute vagi (libertate, schimbare) care sună bine, dar nu spun nimic concret |
| Framing | încadrare / formulare | controlul sensului unui fapt înainte să-l poți judeca |
| Illusory truth effect | efectul adevărului iluzoriu | repetarea face ca o afirmație să pară adevărată |
| Microtargeting | microtargetare | livrarea de mesaje diferite unor grupuri foarte mici, pe baza datelor |
| Psychographics | psihografie | profilare după personalitate, valori și frici, nu doar vârstă/sex |
| Bot | bot | cont automat, controlat de software |
| Sockpuppet | marionetă | identitate falsă operată de o persoană reală, ca să ascundă cine vorbește |
| Coordinated inauthentic behavior | comportament neautentic coordonat | multe conturi false care acționează împreună ca să simuleze o mișcare spontană |
| Astroturfing | astroturfing (falsă mișcare „de bază") | simularea unui sprijin popular spontan care e de fapt organizat și plătit |
| Scapegoating | țap ispășitor | mutarea vinei pe un grup |
| Fear appeal | apel la frică | folosirea fricii ca să te determine să acționezi |
| Threat inflation | inflația amenințării | exagerarea deliberată a unui pericol |
| Name-calling | etichetare (atac prin etichete) | lipirea unei etichete negative ca să descalifici fără argument |
| Demonization | demonizare | înfățișarea adversarului ca răul absolut |
| False dilemma | falsă dilemă | prezentarea a doar două opțiuni când există mai multe |
| Whataboutism | whataboutism („dar voi?") | deturnarea unei critici arătând cu degetul spre altceva |
| Dog whistle | mesaj codat („fluier pentru câini") | mesaj inocent pentru public, dar cu sens ascuns pentru un grup-țintă |
| Firehose of falsehood | „furtunul de minciuni" | inundarea cu multe falsuri rapide și contradictorii |
| Reid technique | tehnica Reid | metodă (controversată) de interogatoriu — nume propriu, se păstrează |
| DARVO | DARVO | „Deny, Attack, Reverse Victim and Offender" — Neagă, Atacă, Inversează rolurile victimă–agresor |
| Gaslighting | gaslighting | manipulare prin care victima ajunge să se îndoiască de propria percepție și memorie |
| Paltering | paltering | inducerea în eroare folosind afirmații tehnic adevărate |
| Straw man | om de paie | răstălmăcirea poziției adversarului ca să fie ușor de combătut |
| Slippery slope | panta alunecoasă | argumentul că un pas mic duce inevitabil la dezastru |
| Push poll | push poll (fals sondaj) | un „sondaj" care de fapt răspândește un mesaj, nu măsoară opinii |
| Loaded question | întrebare tendențioasă | întrebare care conține o presupunere ascunsă |
| Euphemism | eufemism | cuvânt blând pus în locul unuia incomod |
| Doublespeak | limbaj dublu / „limbaj de lemn" | limbaj voit ambiguu care ascunde adevărul |
| Dark patterns | dark patterns (tipare-capcană) | elemente de design create ca să te păcălească (ex. abonament greu de anulat) |
| Reciprocity | reciprocitate | nevoia de a întoarce un favor primit |
| Scarcity | raritate / penurie | „mai sunt doar 2 locuri" — presiunea lipsei |
| Commitment & consistency | angajament și consecvență | tendința de a rămâne fideli unei decizii deja luate |
| Liking | simpatie | spunem „da" mai ușor celor care ne plac |
| Anchoring | ancorare | primul număr văzut îți distorsionează judecata ulterioară |
| Decoy effect | efectul de momeală | o a treia opțiune pusă special ca să te împingă spre alta |
| Card stacking | stivuirea cărților | selecție părtinitoare a faptelor (doar ce te avantajează) |
| Plain folks | „omul simplu" | „sunt unul de-al vostru" — apropierea falsă de public |
| Manufactured doubt | îndoială fabricată | semănarea deliberată a incertitudinii asupra unui fapt dovedit |
| Deepfake | deepfake | material audio/video fals, generat de AI |
| Synthetic media | media sintetică | conținut generat sau modificat de AI |
| Intermittent reinforcement | întărire intermitentă | recompense imprevizibile care creează dependență |
| Negging | negging | „compliment"-insultă menit să-ți scadă încrederea |
| Triangulation | triangulare | introducerea unei a treia persoane ca să creeze gelozie/competiție |
| System 1 / System 2 | Sistemul 1 / Sistemul 2 | gândirea rapidă/automată vs. cea lentă/logică (Kahneman) |
| Cui bono | cui bono | (latină) „cui îi folosește?" — cine are de câștigat |
| Engineering of consent | ingineria consimțământului | expresia lui Bernays: modelarea deliberată a opiniei publice |
| Negative selection | selecție negativă | atragerea deliberată a celor mai vulnerabile ținte |
| Ad-tech pipeline | lanțul ad-tech | infrastructura tehnologică din spatele publicității online |
| Recommendation rabbit hole | spirala recomandărilor | algoritmul care te duce din aproape în aproape spre conținut tot mai extrem |
| Manufactured outrage | indignare fabricată | provocarea deliberată a furiei ca să generezi reacții și distribuiri |

> Glosarul va fi extins pe măsură ce apar termeni noi în episoadele traduse, păstrând aceleași
> echivalente peste tot.

---

## 4. Verificare (per fișier)

- [ ] Diacritice corecte peste tot.
- [ ] Toate etichetele de producție păstrate în engleză; conținutul lor tradus.
- [ ] Fiecare termen tehnic explicat la prima apariție, apoi folosit doar în engleză.
- [ ] Numele proprii, sloganurile istorice EN, datele, URL-urile și citările păstrate.
- [ ] Marcaje TTS `<en>`/`<emf>` puse doar în `**VO:**`, echilibrate (fiecare deschis e și închis).
- [ ] Sensul și nuanțele identice cu originalul (fără adăugiri/omisiuni).
- [ ] Lungimea narațiunii apropiată de original (țintă ~135 cuvinte/min — vezi „production bible").
