# Schoolagenda — Digitale vervanging voor Microsoft Publisher

Twee identieke repos voor LON en KA, gehost op GitHub Pages.

## Bestanden
- `index.html` — de agenda-app (alles-in-één)
- `setup.html` — eenmalige SharePoint-setup (als beheerder uitvoeren)
- `ARCENA.ttf` — lettertype voor titels (kopieer vanuit uploads)

## Stap 1 — Azure App Registratie (per tenant)

1. Ga naar [portal.azure.com](https://portal.azure.com) → **Azure Active Directory** → **App-registraties** → **Nieuwe registratie**
2. Naam: `Schoolagenda`
3. Ondersteunde accounttypen: **Alleen accounts in deze organisatiemap**
4. Redirect URI: `Single-page application (SPA)` → `https://lon-ka.github.io/LON-Agenda/` (of KA-equivalent)
5. Na aanmaken: noteer **Application (client) ID** en **Directory (tenant) ID**
6. Ga naar **API-machtigingen** → **Machtiging toevoegen** → **Microsoft Graph** → **Gedelegeerde machtigingen**
   - `Sites.ReadWrite.All`
7. Klik **Beheerderstoestemming verlenen**

## Stap 2 — SharePoint-lijsten aanmaken

1. Open `setup.html` in je browser
2. Vul Client ID, Tenant ID en Site URL in
3. Klik **Lijsten aanmaken** en log in als beheerder
4. Controleer met **Bestaande lijsten controleren**

**SharePoint Site URL's:**
- LON: `https://londerwijs.sharepoint.com/sites/JOUW_SITE`
- KA: `https://gebaskabe.sharepoint.com/sites/JOUW_SITE`

## Stap 3 — index.html configureren

Pas het `CONFIG`-object bovenaan `index.html` aan:

```javascript
const CONFIG = {
  clientId:   '08dcc1b5-...',   // uit stap 1
  tenantId:   '655d15e1-...',   // uit stap 1
  siteUrl:    'https://londerwijs.sharepoint.com/sites/BaOLON-Agenda',
  listPrefix: 'Agenda',
  appTitle:   'Schoolagenda — Londerwijs',
  schoolNaam: 'Scholengroep Londerzeel',
};
```

## Stap 4 — GitHub Pages deployen

```bash
# LON
git clone https://github.com/lon-ka/LON-Agenda
cp index.html setup.html ARCENA.ttf LON-Agenda/
cd LON-Agenda && git add . && git commit -m "init" && git push

# KA (aparte CONFIG in index.html)
git clone https://github.com/lon-ka/KA-Agenda
# pas CONFIG aan voor KA tenant
cp index.html setup.html ARCENA.ttf KA-Agenda/
cd KA-Agenda && git add . && git commit -m "init" && git push
```

Activeer GitHub Pages in de repo-instellingen (branch: main, folder: /).

## Config voor LON
```javascript
clientId: '08dcc1b5-6041-4e67-ba6f-14650d4e6e62',
tenantId: '655d15e1-e5fc-4868-8462-8463dc0ff014',
```

## Config voor KA
```javascript
clientId: '4e41368c-ff81-4ffb-b1f8-e9a1e46704c5',
tenantId: '165074eb-b8b7-49e8-a9b4-54cac7939ed2',
```

## Printweergave

De app bevat geoptimaliseerde printcss voor A4-portret:
- ARCENA-lettertype voor de DK-titel
- Blauwe dagkoppen (kleurafdruk)
- Handtekening ouder-vak vergroot
- Geen navigatie/knoppen in afdruk

Gebruik `Ctrl+P` / de knop **Afdrukken** in de app.

## Datastructuur (DagData JSON)

```json
{
  "_spId": "sharepoint-item-id",
  "days": {
    "0": { "taken": {"0":"spelling oef. 3", "c0": true}, "hw": "blz 45 lezen", "hwCheck": false, "zwem": false, "brief": true, "rk": false, "tegen": {"0": false} },
    "1": { ... },
    "2": { ... },
    "3": { ... },
    "4": { ... }
  }
}
```
