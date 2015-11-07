# Þróunarumhverfi og vinnuferli

Í þessu skjali verður safnað saman leiðbeiningum um uppsetningu á þróunarumhvefi og venjur teymisins varðandi atriði eins og utanumhald á sameiginlegum kóðabasa og vinnuflæði.

## Þróunarumhverfi

Premis hefur lagt okkkur til þróunarserver. Þetta er Debian 8 kerfi sem er sem stendur með frekar litlum hugbúnaði uppsettum, en þó nægilegum til að komast í gang. (Óskir um uppsetningu á frekari hugbúnaði er einfaldast að koma á framfæri gegnum Slack). Nýlegar útgáfur af bæði MongoDB (3.0.7) og Redis (3.0.5) eru uppsettar og keyrandi, og búið er að setja inn kerfislæga node-útgáfu, nánar tiltekið 4.2.2 ásamt `npm` 3.3.10.

Til að komast í gang er einfaldast að innskrá sig með ssh og búa til sértækt node-umhverfi með

```bash
nodeenv --node=system env-422-sys
. env-422-sys/bin/activate
```

(Hér má auðvitað kalla möppuna eitthvað annað en `env-422-sys`). Síðan má setja inn þau tól sem fólk vill, t.d.:

```bash
npm install -g yo
npm install -g gulp
npm install -g generator-keystone
npm install -g pencilblue-cli
```

og í framhaldi stofna grunnprójekt fyrir framtíðarvinnusvæði (með `pbctl install` eða `yo keystone`).
