# Flask þróunaruppsetning fyrir Akkeri

## Grunnleiðbeiningar

Kóðahirslan fyrir flask-appið okkar er ekki á Github heldur á þróunarvélinni. Slóðin á hana er `/export/repos/akkeri-flask.git`. Til að geta pushað þangað ætti að nægja að vera í `team`-grúppunni.

Þegar pushað er í kóðahirsluna ættu að berast skilaboð þar um inn á akkeri.slack.com (merkt `incoming-webhook`).

Hér eru skrefin til þess að komast í gang:

Fyrst þarf að búa til virtual environment fyrir Flask, klóna git-hirsluna, setja upp nauðsynlega python-módúla og búa til gagnagrunnstöflur í PostgreSQL-grunninum á þróunarvélinni:

```bash
mkvirtualenv flask-env
git clone /export/repos/akkeri-flask.git akkeri-flask
cd akkeri-flask
psql akkeri-$USER < sql/initial_schema/akkeri-schema.sql
pip install -r requirements.txt
```

Þá þarf að stilla umhverfisbreytur (m.a. fyrir gagnagrunnsaðgang) og endurvirkja `flask-env` virtualenv-umhverfið.

```bash
echo "export PROJECTDIR=~/akkeri-flask/" >> $VIRTUAL_ENV/bin/postactivate
echo "export APP_SETTINGS=config.DevelopmentConfig" >> $VIRTUAL_ENV/bin/postactivate
echo "export DATABASE_URL=postgresql://${USER}@/akkeri-$USER" >> $VIRTUAL_ENV/bin/postactivate
echo 'cd $PROJECTDIR' >> $VIRTUAL_ENV/bin/postactivate
deactivate
workon flask-env
```

Því næst þarf að stofna admin-notanda svo að hægt sé að komast inn á umsjónarsíður. Þetta er best að gera í ipython-skel. Gefið skipunina `ipython`, og skrifið svo eftirfarandi python-skipanir (veljið sjálf viðeigandi lykilorð):

```python
from models import db, User
user = User(username='admin', email='admin@akkeri.is', is_superuser=True, fullname='Admin user')
user.set_password('pleasepicksomething')
db.session.add(user)
db.session.commit()
```

Að lokum fer maður út úr ipython-skelinni og gefur skipunina til að ræsa þróunarserverinn (þetta er gott að gera í tmux svo að hann haldist gangandi eftir að maður skráir sig út):

```bash
python manage.py runserver --host=0.0.0.0 --port=xxxx
```

Athugið að velja viðeigandi portnúmer fyrir þessa skipun. Sjálfgefið portnúmer fyrir flask er 5000, en mælst er gegn því að velja það.

Framvegis má síðan gefa skipunina `workon flask-env` til þess að virkja þróunarumhverfið eftir innskráningu. Einnig má setja þá skipun í `~/.bashrc` sé þess óskað.
