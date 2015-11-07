# Samanburður á Keystone og PencilBlue

## Grundvöllur samanburðar

Hér verða bornir saman almennir fítusar kerfanna tveggja, notkunarmöguleikar þeirra og sveigjanleiki – en einnig tekið tillit til þess að ætlunin er að byggja upp vef með ákveðin einkenni, sem lýsa má á eftirfarandi hátt:

- Um er að ræða efnismikinn vef með fjölbreyttu efni með góðri tengingu við samfélagsmiðla, en að megni til statískan – lítið um eiginlega gagnvirkni.
- Margir einstaklingar geti lagt til efni. Til þess að það sé hægt þarf að vera hægt að skrá sig sem notanda. Efnið sem maður sendir inn er þó ekki sýnilegt öðrum nema maður hafi nægilegan aðgang (sem umsjónarmenn þurfa að gefa).
- Helst þarf að vera hægt að stofna sig sem notanda (og innskrá sig framvegis) með innskráningu á helstu samfélagsmiðla (facebook, twitter, google). En einnig þarf að vera hægt að skrá sig án þess að nota slíka innskráningu hjá þriðja aðila.
- Lítið teymi ritstjóra stjórni meginhluta vefsins. Ekki þarf kóðabreytingar eða forritunarkunnáttu til að velja inn efni á forsíðu. Það sé gert í sér tóli (sem við þurfum að smíða frá grunni), en sjálf uppröðun efnisins sé fyrirfram ákveðin út frá sniðmáti, þótt e.t.v. verði hægt að velja úr fáeinum mismunandi möguleikum hvað það varðar.
- Aðrar síður (og e.t.v. hluti forsíðu) verða til dýnamískt úr því efni sem er nýjast hverju sinni og í samræmi við flokka/tags.
- Á að vera hægt að senda inn efni gegnum vefinn án þess að vera skráður? Það er kostur ef svo er, en annars hægt að benda á tölvupóst í eitthvert tiltekið netfang, þar sem ritstjóri sæi svo um að vinna úr efninu og koma því fyrir, þannig að það er ekki krafa.

(Eru fleiri skilyrði sem þarf að bæta á þennan lista? Breytið honum endilega eða setjið inn athugasemdir ef svo er.)

## Grunnkerfi

Líklegt er að við séum ekki með sömu kerfisuppsetningu. Því er rétt að við lýsum henni hér, ef vera kynni að það hefði áhrif á vandræði sem við lendum í við uppsetningu/stillingar á kerfunum tveimur.

### Baldur

Ég nota Debian 8 (jessie) og einangra Node-umhverfið með [nodeenv](https://github.com/ekalinin/nodeenv). Er auðvitað búinn að setja upp MongoDB á vélinni og ræsa það fyrst. Ég set svo upp nýtt umhverfi fyrir hverja tilraunauppsetningu sem ég geri, t.d. svona (fyrir keystonejs):

```bash
nodeenv --prebuilt --node=4.2.2 keystone-env-422
. keystone-env-422/bin/activate
npm install -g npm # annars kvartar Yeoman yfir gamalli npm-útg.
npm install -g yo
npm install -g generator-keystone
yo keystone # o.s.frv.
```

Eitt sem ég rakst á er að það þarf að auka skráafjölda sem einn prósess getur fylgst með gegnum `inotify`-kerfiskallið. Ef það er of lágt lýsir það sér með því að þróunarserver gefst upp með dularfullri villu um plássleysi.  Þetta er gert svona (sem root):

```bash
echo "fs.inotify.max_user_watches=524288" >> /etc/sysctl.conf
sysctl -p
```

## Keystone

### Uppsetningarvandamál

- Tekst ekki að ljúka uppsetningu með Node 5.0.0. Nota 4.2.2.
- Vandamál með að velja SASS sem CSS-bakenda, sem virðist vera vegna þess að `node-sass` er sett upp á tveimur mismunandi stöðum í prójekt-trénu á meðan hið undirliggjandi `libscss` er bara á einum. Margir hafa lent í þessu - þær ábendingar um að laga þetta sem ég fann á netinu virkuðu ekki hjá mér. Ég gat fengið þetta til að virka með því að afrita node-sass módúlinn af einum stað á annan inni í prójekt-trénu, en treysti ekki þeirri lausn til frambúðar. LESS virkar samt, þannig að ef við veljum Keystone verður það væntanlega notað.
- Betra er að velja Gulp en Grunt fyrir utanumhald (ef Grunt er valið leiðir það til þess að setja þarf upp marga módúla aftur áður en hægt er að kalla á `node keystone`).
- Almennt virðist generatorinn (þ.e. `yo keystone`) mjög brothættur. Ekki sérlega traustvekjandi.

Ég valdi annars nunjucks sem template-bakenda (sjálfgefna gildið er jade, sem ég er ekki hrifinn af).

### „Out of the box“-upplifun

- Bjánalegt að setja lykilorðið sem maður valdi í `yo keystone`-ferlinu í plaintext á forsíðuna þegar maður ræsir upp þróunarserverinn.
- Ókostur að nota þjónustu þriðju aðila fyrir myndameðhöndlun (cloudinary) og póstsendingar (mandrill) og bjóða ekki upp á að nota lókallausnir í staðinn
- Einingarnar sem er hægt að velja í generator-ferlinu (blogg, „hafa samband“-form, myndaalbúm) eru ekkert sérstakar, en samt gott að hafa þær sem grunn til að vinna út frá.
- Uppbygging virðist vera venjuleg MVC-aðgreining: `models`, `templates`, `routes`. Stillingar í grunnmöppu og í `updates`, statískar skrár í `public`.
- Byggir á express.
- Enginn augljós staður til að setja prófanir og enginn testrunner.
- Notendamódelið er of einfalt: bara tvær gerðir notenda. Þarf að bæta við einhvers konar permissions-kerfi.
- Kostur að vera með tilbúna `Procfile` og `.gitignore` inni  í prójekt-möppunni.

### Skjölun

- Skjölunin mætti vera töluvert meiri. Hún dugir til uppflettingar á stillingum og varðandi módelin, en veitir ekki neinar almennilegar leiðbeiningar hvernig á að extenda kerfið eða smíða eitthvað nýtt á grundvelli þess.
- Varðandi extension-leiðbeiningar verður að skoða dæmi um prójekt: [Demo App](http://github.com/JedWatson/keystone-demo) og kóðann að [SydJS](http://github.com/JedWatson/sydjs-site).

### Heildarumsögn

Keystone hefur mjög ákveðna galla en gæti samt alveg gengið sem grunnur til að vinna út frá.

## PencilBlue

### Uppsetning

Uppsetning PencilBlue gengur mun auðveldar og fljótar fyrir sig en uppsetning Keystone. Mér tókst ekki að framkalla nein vandamál, að öðru leyti en því að viðvaranir fylgja innsetningu sumra bower-módúlanna ef maður velur að taka þá með þegar `pbctrl install [dir]` er keyrt. Það virðist þó ekki hafa áhrif á virkni uppsetningarinnar sem slíkrar.

### „Out of the box“-upplifun

Þótt PencilBlue sé léttara í keyrslu en Keystone virðist það hafa mun fleiri fítusa, og módelin sem grunnuppsetningin miðast við mun nær því sem við þurfum.

- Notar einhvers konar uppskiptingu eininga sem minnir á MVC, en fylgir ekki sama mynstri og express í þeim efnum. Controller-hlutinn er í `controllers/` á meðan módel virðast vera í mismunandi undirmöppum `include/`. Sniðmát (view-hlutinn) eru í `templates/`-undirmöppum í tilteknum `plugins`.
- Template-málið er eitthvað heimasmíðað, líklega vegna sérþarfa höfunda m.t.t. erfða í sambandi við plugins. Virkar hvorki aðlaðandi né öflugt við fyrstu sýn.
- Starter-prójektið sem er búið til með `pbctrl install` er klónað úr [github-prójektinu](https://github.com/pencilblue/pencilblue.git) sem hefur bæði kosti og galla.
- Plugins-hugmyndin er áhugaverð sem extension-leið, en tiltölulega lítið til af fyrirfram tilbúnum plugins enn sem komið er: eitthvað um [20 talsins](https://pencilblue.org/plugins). Þeir [óska eftir fleirum](https://github.com/pencilblue/pencilblue/wiki/Requested-Plugins), og ef PencilBlue verður ofaná og við búum til eitthvað nægilega almennt af því tagi, er sjálfsagt að leggja það til prójektsins.
- Ólíkt Keystone virðist ekki vera nein asset pipeline innbyggð í kerfið, og enginn sérstakur stuðningur við less/sass. Sjálfsagt er samt auðvelt að bæta því við.

### Skjölun

Skjölunin (sem er mestmegnis á [github-wiki prójektsins](https://github.com/pencilblue/pencilblue/wiki) er mun betri fyrir einhvern sem er að kynna sér kerfið en skjölun Keystone og sýnir/vísar í praktísk dæmi um hvernig hægt er að extenda það.

Á síðari stigum gæti það þó orðið ókostur að tæknilegri skjölun (um einingar, API o.s.frv.) er svolítið ábótavant.

### Heildarumsögn

Þrátt fyrir frekar óaðlaðandi sniðmát og skort á less/sass-stuðningi stendur PencilBlue Keystone framar að flestu leyti varðandi þá fítusa sem skipta máli fyrir okkur.

## Niðurstaða

PencilBlue er mun nær því að uppfylla þarfir okkar til að byrja með. Á móti kemur að kerfið hefur minna community en Keystone og byggir ekki á Express (sem er mest notaða vefkerfið fyrir node). Þess vegna er líklegra að Keystone sé traustari grunnur fyrir útvíkkanir og viðbætur.

Á fundi starfshópsins 7. nóvember 2015 var af þessum ástæðum í raun ákveðið að nota Keystone.
