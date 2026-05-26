# Supervisory Authorities Directory

Use this file to: identify the correct lead supervisory authority, find contact details for breach notifications and DPO registration, and route complaints correctly.

---

## Table of Contents
1. [EU Member State Authorities](#eu-member-state-authorities)
2. [UK](#uk)
3. [EEA Non-EU](#eea-non-eu)
4. [Adequacy countries (selected)](#adequacy-countries-selected)
5. [Lead authority routing logic](#lead-authority-routing-logic)
6. [Breach notification contacts](#breach-notification-contacts)

---

## EU Member State Authorities

| Country | Authority | Abbreviation | Website | Breach notification portal | DPO registration required? |
|---------|-----------|-------------|---------|--------------------------|---------------------------|
| Austria | Datenschutzbehörde | DSB | dsb.gv.at | Online portal on website | No |
| Belgium | Autorité de Protection des Données / Gegevensbeschermingsautoriteit | APD/GBA | autoriteprotectiondonnees.be | Online portal | No |
| Bulgaria | Commission for Personal Data Protection | CPDP | cpdp.bg | Email: kzld@cpdp.bg | No |
| Croatia | Croatian Personal Data Protection Agency | AZOP | azop.hr | Online portal | No |
| Cyprus | Commissioner for Personal Data Protection | CPDD | dataprotection.gov.cy | Online form | No |
| Czech Republic | Office for Personal Data Protection | ÚOOÚ | uoou.cz | Online portal | No |
| Denmark | Datatilsynet | Datatilsynet | datatilsynet.dk | Online portal | No |
| Estonia | Data Protection Inspectorate | AKI | aki.ee | Online portal | No |
| Finland | Office of the Data Protection Ombudsman | TSV | tietosuoja.fi | Online form | No |
| France | Commission Nationale de l'Informatique et des Libertés | CNIL | cnil.fr | Online portal (notifications.cnil.fr) | No |
| Germany | Federal: Die Bundesbeauftragte für den Datenschutz und die Informationsfreiheit | BfDI | bfdi.bund.de | Email/portal — varies by state DPA | No (federal level); some states differ |
| Greece | Hellenic Data Protection Authority | HDPA | dpa.gr | Online form | No |
| Hungary | National Authority for Data Protection and Freedom of Information | NAIH | naih.hu | Online portal | No |
| Ireland | Data Protection Commission | DPC | dataprotection.ie | Online portal (dataprotection.ie/en/report-a-breach) | No |
| Italy | Garante per la protezione dei dati personali | Garante | gpdp.it | Online portal | No |
| Latvia | Data State Inspectorate | DSI | dvi.gov.lv | Online portal | No |
| Lithuania | State Data Protection Inspectorate | SDPI | vdai.lrv.lt | Online portal | No |
| Luxembourg | Commission Nationale pour la Protection des Données | CNPD | cnpd.public.lu | Online portal | No |
| Malta | Office of the Information and Data Protection Commissioner | IDPC | idpc.org.mt | Online form | No |
| Netherlands | Autoriteit Persoonsgegevens | AP | autoriteitpersoonsgegevens.nl | Online portal (meldportaal.autoriteitpersoonsgegevens.nl) | No |
| Poland | Urząd Ochrony Danych Osobowych | UODO | uodo.gov.pl | Online portal | Yes — register DPO with UODO |
| Portugal | Comissão Nacional de Protecção de Dados | CNPD | cnpd.pt | Online portal | No |
| Romania | National Supervisory Authority for Personal Data Processing | ANSPDCP | dataprotection.ro | Online form | No |
| Slovakia | Office for Personal Data Protection | UOOU | dataprotection.gov.sk | Online form | No |
| Slovenia | Information Commissioner | IC | ip-rs.si | Online portal | No |
| Spain | Agencia Española de Protección de Datos | AEPD | aepd.es | Online portal (sedeagpd.gob.es) | No |
| Sweden | Integritetsskyddsmyndigheten | IMY | imy.se | Online portal | No |

---

## UK

| Authority | Website | Breach portal | ICO registration |
|-----------|---------|--------------|-----------------|
| Information Commissioner's Office (ICO) | ico.org.uk | ico.org.uk/for-organisations/report-a-breach | Required for most controllers — ico.org.uk/registration |

**ICO registration fee**: £40–£2,900/year depending on size. Most SMEs = Tier 1 (£40/year). Required unless exempt (e.g. not-for-profit, only processing for personal family purposes).

---

## EEA Non-EU

| Country | Authority | Website |
|---------|-----------|---------|
| Iceland | Persónuvernd | personuvernd.is |
| Liechtenstein | Data Protection Office | datenschutzstelle.li |
| Norway | Datatilsynet | datatilsynet.no |

---

## Adequacy Countries (Selected)

These countries have EU adequacy decisions — data can flow to them without SCCs:

| Country | Status | Notes |
|---------|--------|-------|
| UK | Adequate | Adequacy under review — verify current status |
| Switzerland | Adequate | |
| Japan | Adequate | |
| South Korea | Adequate | |
| New Zealand | Adequate | |
| Canada | Partially adequate | Commercial orgs under PIPEDA only |
| Israel | Adequate | |
| USA | Partially adequate | Data Privacy Framework participants only — check DPF list at dataprivacyframework.gov |
| Argentina | Adequate | |
| Uruguay | Adequate | |

**US transfers — DPF verification**: Check if US processor is DPF-certified at: https://www.dataprivacyframework.gov/s/participant-search

---

## Lead Authority Routing Logic

```
Is company established in EU/EEA?
├── NO → No lead authority; each member state DPA has jurisdiction
│         → Must designate EU representative (Art. 27) if processing EU data at scale
│         → Gap signal: No EU representative found for non-EU company processing EU data
│
└── YES → Where is main EU establishment?
          (Main establishment = central admin OR where processing decisions are made)
          ├── Single EU country → That country's DPA is the lead authority
          └── Multiple EU countries → Lead is where central admin is, OR
                                       where the controller/processor with decision-making
                                       authority over cross-border processing is located
```

**Common scenarios**:

| Scenario | Lead authority |
|----------|---------------|
| US company, EU ops run from Irish office | DPC (Ireland) |
| US company, no EU office, EU users | No LSA — each national DPA can act; need Art. 27 rep |
| German company, German users only | BfDI or relevant Landesbehörde (state DPA where company is registered) |
| French company, sells across EU | CNIL |
| Company registered in Luxembourg for tax, decisions made in Netherlands | AP (Netherlands) — substance over form |
| UK company post-Brexit, EU users | No EU LSA; need EU Art. 27 representative. ICO is lead for UK operations |

---

## Breach Notification Contacts

**72-hour clock**: Starts when the controller becomes aware of a breach likely to result in risk to individuals. Report to the lead supervisory authority.

**What to include in notification**:
- Nature of the breach (approximate number of data subjects and records)
- Contact details of DPO or contact point
- Likely consequences
- Measures taken or proposed

**If full information not yet available**: Report what you know within 72 hours and provide further information in phases.

**Threshold**: Notify if breach "is likely to result in a risk to the rights and freedoms of natural persons." Low-risk breaches (e.g. encrypted device lost) may not require notification — document the decision either way.

**Notify individuals** (Art. 34): Required additionally if breach is likely to result in *high* risk to individuals. No 72-hour deadline but "without undue delay."

**Breach log**: Even non-notifiable breaches must be documented internally (Art. 33(5)). Use the breach log template in `breach-response-pack.md`.

---

## DPO Registration Requirements by Country

| Country | Registration required | Where |
|---------|----------------------|-------|
| Poland | Yes | UODO online portal |
| Germany (some Länder) | Varies | Check relevant Landesdatenschutzbehörde |
| All others | No mandatory registration | But DPO contact must be published and shared with supervisory authority on request |

**Best practice**: Even where not mandatory, notify your lead supervisory authority of your DPO's contact details. Most authorities have a voluntary notification portal.
