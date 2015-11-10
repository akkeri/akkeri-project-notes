# Keystone þróunaruppsetning fyrir Akkeri

Þar sem um viðkvæmar upplýsingar gæti verið að ræða, er kóðahirslan að Keystone-vefnum okkar ekki á Github, heldur í miðlægri hirslu á þróunarvélinni. Sem stendur er slóðin á hirsluna `/export/repos/akkeri-keystone.git`. Til að geta pushað þangað ætti að nægja að vera í `team`-grúppunni.

Upphaflegt innihald hófst einfaldlega sem grunnuppsetning af Keystone, sem búin var til með `yo keystone`. Kostir sem valdir voru í því ferli:

* Nafn: Akkeri Keystone.
* Templates: Handlebars.
* CSS preprocessor: LESS.
* Uppsettar einingar: Blog og Contact form – en ekki Gallery.
* Notendamódel: Users.
* Gulp eða Grunt: hvorugt valið sérstaklega.
* Kveikja á póstsendingum: já, og gefinn upp Mandrill-lykill.

## Að hefjast handa

Til að stofna vinnusvæði gerið þið eftirfarandi:

1. `. env-422-sys/bin/activate` (eða hvað sem þið nú kölluðuð nodeenv-möppuna).
2. `npm install -g nodemon`
3. Klónið þessa kóðahirslu: `git clone /export/repos/akkeri-keystone.git`.
4. `cd akkeri-keystone` (eða inn í möppuna með nýja vinnutrénu ef þið ákváðuð að kalla hana eitthvað annað).
5. `npm install`
6. Búa til skrána `.env` – sjá hér að neðan.
7. `nodemon keystone` (helst í tmux sessjón) til að ræsa þróunarserver. Athugið að `node keystone` gerir ekki live reload ef eitthvað breytist, þess vegna er `nodemon` þægilegra.

## Skráin `.env`

Þessi skrá inniheldur stillibreytur fyrir Keystone. Til að við séum ekki að traðka hvert öðru um tær þarf hér a.m.k. tvær sérsniðnar stillingar: `PORT` og `MONGODB_URL`.

Ég legg til að við komum okkur saman um að nota eftirfarandi stillingar fyrir þessar tvær breytur:

Notandi  | PORT | MONGODB_URL
-------- | ---- | -------------------------------------------
anna     | 3100 | mongodb://localhost/akkeri-keystone-anna
bk       | 3200 | mongodb://localhost/akkeri-keystone-bk
daggala  | 3300 | mongodb://localhost/akkeri-keystone-daggala
elisabet | 3400 | mongodb://localhost/akkeri-keystone-elisabet
gezim    | 3500 | mongodb://localhost/akkeri-keystone-gezim
gunnar   | 3600 | mongodb://localhost/akkeri-keystone-gunnar
gussi    | 3700 | mongodb://localhost/akkeri-keystone-gussi
natan    | 3800 | mongodb://localhost/akkeri-keystone-natan
tobbi    | 3900 | mongodb://localhost/akkeri-keystone-tobbi

Almennt væri þá t.d. allt bilið 3100–3199 frátekið fyrir Önnu (ef hún skyldi vilja keyra fleiri en eina uppsetningu í einu eða einhverjar aðrar þjónustur), og auðvitað hliðstætt fyrir aðra notendur.

Auk þess þarf að stilla breytuna `COOKIE_SECRET`, sem á að vera 64 handahófskennd tákn að lengd. Við munum ekki nota Cloudinary, svo að það er ekki þörf á `CLOUDINARY_URL`, né `MANDRILL_API_KEY` til póstsendinga (þess í stað er sent gegnum SMTP-þjón sem er keyrandi á vélinni sjálfri).

Hér er dæmi um `.env`-skrá fyrir notandann bk:

```bash
PORT=3200
MONGODB_URL=mongodb://localhost/akkeri-keystone-bk
COOKIE_SECRET=1I))HVy!,O2k%-psS/v%VGrE+o+-.@bz9v>2PQt)]N}t/)^o|F>xyOENl+&dASc|
IMAGE_UPLOAD_DIR=public/uploads/images
```

Síðasta breytan, `IMAGE_UPLOAD_DIR`, er fyrir okkar kerfi fremur en fyrir Keystone og er semsagt einungis notuð í kóða sem við höfum skrifað. Sjálfgefna gildið fyrir breytuna er það sem sést hér fyrir ofan.

Gera má ráð fyrir að fleiri svona stillibreytur sem við höfum skilgreint bætist í hópinn á síðari stigum.

Heildarlisti yfir stillingar sem Keystone sjálft gerir ráð fyrir að hægt sé að setja í `.env` virðist vera sem hér segir:

* `ALLOWED_IP_RANGES`
* `AZURE_STORAGE_ACCESS_KEY`
* `AZURE_STORAGE_ACCOUNT`
* `BLUEBIRD_DEBUG`
* `CHARTBEAT_DOMAIN`
* `CHARTBEAT_PROPERTY`
* `CLOUDINARY_URL`
* `COOKIE_SECRET`
* `EMBEDLY_APIKEY`
* `EMBEDLY_API_KEY`
* `GA_DOMAIN`
* `GA_PROPERTY`
* `GOOGLE_BROWSER_KEY`
* `GOOGLE_SERVER_KEY`
* `HOST`
* `IP`
* `KEYSTONE_DEV`
* `LISTEN`
* `MANDRILL_APIKEY`
* `MANDRILL_API_KEY`
* `MANDRILL_USERNAME`
* `MONGODB_URL`
* `MONGOLAB_URI`
* `MONGOLAB_URL`
* `MONGOOSE_DISABLE_STABILITY_WARNING`
* `MONGO_URI`
* `MONGO_URL`
* `MQUERY_URI`
* `NODE_DEBUG`
* `NODE_ENV`
* `OPENSHIFT_MONGODB_DB_URL`
* `OPENSHIFT_NODEJS_IP`
* `OPENSHIFT_NODEJS_PORT`
* `PORT`
* `S3_BUCKET`
* `S3_KEY`
* `S3_REGION`
* `S3_SECRET`
* `SSL`
* `SSL_CERT`
* `SSL_HOST`
* `SSL_IP`
* `SSL_KEY`
* `SSL_PORT`

Áhugasömum er bent á að skoða keystone-kóðahirsluna til að komast að því nákvæmlega hvaða áhrif hver stilling fyrir sig hefur á virknina.

## Athugasemd um útgáfu 0.4

Útgáfa 0.4 af Keystone er væntanleg innan tíðar – vonandi í desemberbyrjun 2015. Hún felur í sér miklar breytingar á stjórnborðinu, sem verður byggt á React og mun auðveldara að útvíkka og breyta en núverandi lausn. Ef maður vill er hægt að prófa alpha-release af þessari útgáfu með því að breyta línunni

```
    "keystone": "^0.3.14",
```

undir `dependencies` í `package.json` yfir í

```
    "keystone": "https://github.com/keystonejs/keystone.git",
```

keyra því næst `npm install` upp á nýtt og bæta inn þeim pökkum sem kvartað verður yfir þegar maður reynir að ræsa þróunarserverinn að nýju.

## Thumbnailer: `imaginary`

Nú er kominn stuðningur við að gera smámyndir með því að kalla á viðeigandi slóð sem er byggð á slóðinni á sjálfa myndina. Dæmi: Ef maður er með myndina `$DEVROOT/uploads/images/2015/11/10/ecf1d87e55a4ab1e7fd76fffd8e1dfd9.jpg`, þá má t.d. fá fram 200x200 „kroppaðan“ ferning úr henni með því að biðja um `$DEVROOT/tn/crop/2015/11/10/ecf1d87e55a4ab1e7fd76fffd8e1dfd9.jpg?h&height=200&width=200` (þar sem `$DEVROOT` er vélarheiti og port viðkomandi þróunarservers).

Það er jafnframt kominn template helper til að hjálpa til að búa til slíkar slóðir, t.d.

    {{{thumbnailUrl image action=crop width=250 height='auto'}}}

Auk `crop` er hægt að tilgreina `resize` og  `thumbnail` sem aðgerðir.

Þessi virkni byggist á [imaginary](https://github.com/h2non/imaginary) eftir Tomás Aparicio. Ef þið viljið setja upp þróunarumhverfi á ykkar eigin vél þurfið þið því að setja upp þennan hugbúnað.

Á þróunarvélinni keyrir hann með eftirfarandi stillingum:

    imaginary -a 127.0.0.1 -p 8088 -http-cache-ttl 3600 -enable-url-source

Kveikt er á `-enable-url-source` til að þetta geti gagnast öllum með þróunarsvæði á vélinni, en mun hraðvirkara væri að binda virknina við tiltekna slóð í skráarkerfinu í staðinn, með sviss á borð við `-mount /home/akkeri/akkeri-keystone/public/uploads/images`. Þá þarf hins vegar að breyta `routes/thumbnailer.js` til samræmis. Þetta verður hugsanlega hægt að stilla í gegnum `.env` síðar meir.
