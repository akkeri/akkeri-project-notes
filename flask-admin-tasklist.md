# Verkefni í sambandi við flask-admin

Hér er (ófullkominn) listi yfir nokkur atriði sem betur mættu fara varðandi virknina á Flask-admin kerfinu. Listanum er skipt upp eftir þeim módelum sem aðgengileg eru gegnum stjórnborðið. Auk þess eru almenn atriði nefnd sérstaklega síðast í þessu skjali.

## Attachment

- [x] Vantar upload-virkni.
- [x] Innskráður notandi ætti að vera sjálfvalinn við Owner þegar færsla er stofnuð og aðeins superuser ætti að geta sérvalið eða breytt gildinu.
- [x] Posts ætti ekki að vera í forminu eða þyrfti að breyta í inline (og væri réttara að kalla "link to post" eða álíka). Hugsanlega mætti sleppa því, þar sem eðlilegra er að hugsa um tenginguna frá bloggfærslunni fremur en viðhenginu.
- [x] Attachment Path og Bytes ættu að uppfærast sjálfkrafa samhliða upload.
- [x] Added, changed og attachment-date ættu að uppfærast sjálfkrafa og vera falin eða readonly
- [x] Active ætti að vera virkt í upphafi.

Við frestum eftirfarandi þar til síðar - það er óþarfa lúxus að tengja preview-mynd við viðhengi, þannig að þessi fítus dettur út í bili:

- [ ] Vantar preview í lista (mynd, ef hún fylgir með, annars content-type tákn, t.d. PDF-lógó).
- [ ] Image ætti að vera neðar í forminu, með preview og upload-möguleika.

## Featured

- [ ] Búa þarf til sérstaka lista-/yfirlitssíðu svo að auðvelt sé að sjá hvaða efni er virkt/sérvalið (og hvar) miðað við gildandi forsendur.
- [ ] Í forminu þarf að fela reitina User og Added At.
- [ ] Takmarka þarf Post-valið við birtar færslur eingöngu.
- [ ] Featured From og Featured To þurfa að vera skyldureitir (módelbreyting?).

## Image

- [x] List view: Taka út Caption, setja inn preview.
- [x] Stofna: Width, Height, Bytes ættu að vera faldir reitir sem fyllast út sjálfkrafa m.v. myndina sem uploadað er.
- [x] Posts ætti ekki að vera í forminu eða þyrfti að breyta í inline (og væri réttara að kalla "link to post" eða álíka). Hugsanlega mætti sleppa því, þar sem eðlilegra er að hugsa um tenginguna frá bloggfærslunni fremur en myndinni.
- [x] Added og Changed ættu að uppfærast sjálfkrafa og vera faldir eða readonly.
- [x] Við upload verður til smámynd sem bara er notuð í admin-kerfinu. Betra væri að búa til dýnamíska smámynd með almennri thumbnailing-lausn (Það gerir ekkert til að hafa þessa auka smámynd í viðtökumöppunni - en við notum samt okkar sérsniðnu smámyndalausn, þar sem hún er mun sveigjanlegri).

## Language

Þessu módeli má sleppa þar sem engar líkur eru á að það þurfi að bæta við eða breyta tungumálum.

## Post

Þetta er augljóslega síðan sem mest þarf að vinna með og fínpússa. Hana þarf að gera eins notendavæna en um leið öfluga og hægt er. Nokkur helstu atriði:

- [x] Author, Last Changed By, Created, Changed séu reitir sem séu faldir eða read-only, og a.m.k. ekki hægt að breyta fyrir almennan notanda.
- [x] Body verði með TinyMCE-widget
- [ ] Summary verði fyllt út sjálfkrafa út frá Body, en möguleiki á að skrifa sjálfur.
- [x] Slug verði fyllt út sjálfkrafa út frá Title (og e.t.v. Published + Post Type), ~~en möguleiki á að velja/breyta sjálfur~~.
- [ ] Images og Attachments verði inlines eða bætt við gegnum lightbox-widget, og eins auðvelt að vinna með og unnt er (t.d. með tilliti til röðunar eða þess að skrifa myndatexta). — *komið að hluta*
- [ ] Author Visible og Author Line þarf að útskýra á síðunni. Author Line sjáist ekki nema þegar búið er að haka í Author Visible.
- [ ] Það vantar Preview-hnapp.
- [x] Eins og með önnur módel hafi aðrir en superusers bara aðgang að því að breyta eigin efni.

## PostType

Í lagi. Sjá til þess að aðeins superusers hafi aðgang að þessu.

## Role

Í lagi. Sjá til þess að aðeins superusers hafi aðgang.

## Tag

- [x] Created sé Read-only og sjálfkrafa uppfært.
- [x] Einfaldað form sé sýnt öðrum en superusers (ekki með "For ..."-reitunum, ekki með "Is Important", og ekki með möguleika á að tagga efni sem er búið til af öðrum en innskráðum notanda).

## User

- [x] Falið fyrir öðrum en superusers.
- [ ] Bæta þarf við möguleika á að breyta lykilorði notenda (e.t.v. sérstakt form eins og í django-admin).
- [ ] Aðrir en superusers geti þó breytt eigin upplýsingum og sér í lagi breytt lykilorði.

## Almennt

Það gæti komið betur út að setja módelin inn í vinstri valmynd í stað þess að hafa þau efst - þau eru fullmörg til að komast þægilega fyrir.

Á forsíðuna (Home) gæti komið einhvers konar dashboard með bæði upplýsingum og tenglum á algengar aðgerðir.

Er ruglandi að setja viðbótarupplýsingar í tengitöflurnar milli Post og Image/Attachment? Það myndi einfalda málið gagnvart flask-admin að láta tengitöfluna vera hreina tilvísunartöflu, en hins vegar minnkar það möguleikann á því að endurnýta myndir (þar sem endurnýting myndi jafnframt nauðsynlega þýða endurnýtingu viðkomandi myndatexta o.s.frv., ólíkt því sem gert er ráð fyrir í núverandi gagnaskema).
