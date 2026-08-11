# www.benzarti.com

Personal site, built with [Quarto](https://quarto.org), hosted free on GitHub Pages.
Live at **https://www.benzarti.com** · repo `ybenzarti/benzarti.com`.

## How to change something

Edit a `.qmd` file, commit, push. That is the whole workflow:

    git add -A && git commit -m "..." && git push

A GitHub Actions workflow (`.github/workflows/publish.yml`) renders the site and deploys it.
Takes about 90 seconds. **You never run `quarto render` yourself** — only if you want to preview
locally first:

    quarto preview          # live reload at localhost
    quarto render           # writes _site/, which is gitignored

## Files

| File | What it is |
|---|---|
| `index.qmd` | Home. Bio and contact, then working papers and publications. |
| `policy.qmd` | Policy & Press, organised by policy debate. |
| `_quarto.yml` | Navigation, theme, site URL, Open Graph. |
| `styles.css` | Small stylesheet. |
| `memo.pdf` | The Danish memo, served at /memo.pdf. |
| `CNAME` | `www.benzarti.com`. Do not delete — it is what binds the domain. |

## The domain

Registered at **Square/Weebly** (bought years ago, was parked there). Only the DNS was
repointed — the registration stayed put. Do not cancel the Square domain subscription; that
is what makes the site work.

DNS records currently set at Square:

- Four A records on `@` → `185.199.108.153`, `.109.153`, `.110.153`, `.111.153`
- CNAME on `www` → `ybenzarti.github.io`

Certificate is issued and HTTPS is enforced. **If HTTPS ever breaks with ECONNRESET or a 409**,
the fix is to clear and re-set the custom domain, which forces GitHub to re-verify DNS and
re-issue:

    gh api -X PUT repos/ybenzarti/benzarti.com/pages -f 'cname='
    gh api -X PUT repos/ybenzarti/benzarti.com/pages -f 'cname=www.benzarti.com'

That is exactly what was needed on first setup — GitHub had registered the domain but never
started provisioning.

## Still to do

1. **Two links on the site are knowingly wrong.** In `index.qmd`, the Google Scholar and NBER
   URLs were constructed from standard patterns and never verified — the Scholar one is almost
   certainly wrong, since Scholar IDs are random strings. In `policy.qmd`, the Ministry's reply
   link is a placeholder `#`. Fix or delete all three.
2. **Getting this to outrank the old Google Site.** On-page work is done (meta descriptions,
   Open Graph, sitemap, robots). What remains needs logins:
   - Strip the old Google Site to one line pointing here. **Do not delete it** — Google Sites
     cannot issue a redirect, and deleting throws away the ranking and breaks every CV link.
   - Update inbound links, which matter far more than anything on the page: NBER profile, the
     UCSB department page, Google Scholar's homepage field, RePEc/IDEAS, coauthors' pages, and
     the CV PDF on Dropbox (Google indexes it, and it still prints the old address).
   - Google Search Console: verify the domain (DNS TXT is easiest, you control DNS at Square),
     submit `https://www.benzarti.com/sitemap.xml`, request indexing.
3. **Read the Greece entry in `policy.qmd` before circulating the address.** It cites IME GSEVEE
   Brief 37, which argues *against* the asymmetry for recent episodes. Included deliberately —
   it forecloses any charge of cherry-picking, and it is the only trace of the work in Greece —
   but it is a judgment call and striking it is one line.
4. **The coverage archive has a gap.** `Projects/VAT asymmetry news coverage/` asserts there is
   no named Japanese coverage, and its 5 August sweep says nothing has named Benzarti since
   June 2026. Both are wrong: the Nikkei interview of **7 July 2026** names him six times with
   three direct quotes, and was found only because it sat in that folder as an untitled PDF.
   The archive's own summaries need correcting.

## Editorial decisions worth not undoing

- **Press is organised by policy debate, not by outlet or by paper.** The coverage tracks policy
  arguments, not publication dates. The French case is the proof: the IPP note appeared 28 May
  2018, the press ran it for four days, and on 7 June the Finance Minister said he did not rule
  out re-examining the reduced rates — at which point Les Echos and L'Express reframed the story
  around Bercy's target list.
- **Uncited uses have no heading of their own.** Japan's Cabinet Secretariat and Austria's
  Budgetdienst sit inside their event entries, with the *finding* as the subject of the sentence
  and no claim made about naming. A heading meaning "used without credit" reads as grievance
  however it is worded.
- **Working-paper-series citations were removed.** IMF, World Bank, OECD and Bank of Greece
  working papers are ordinary academic research that happens to carry an institution's name;
  listing them as institutional use overstated it. What remains under "In government documents"
  is genuinely governmental: the Economic Report of the President 2018 and the Austrian energy
  ministry factsheet.
- **Three press items are excluded on purpose:** the Irish Times piece (credits the Irish Fiscal
  Council explicitly), the German trade-press item (states the asymmetry as settled fact, no
  longer uniquely traceable), and POV International in Denmark (same problem).
- **The 2018 French coverage was re-filed.** Les Echos, L'Express, Capital, LCI, Ouest-France and
  BFM were listed under *What Goes Up May Not Come Down* on the old site, but all six ran between
  28 May and 11 June 2018 covering the IPP note, which is the **France paper**. Bloomberg and
  Microeconomic Insights stayed with the JPE paper. Bloomberg is still undated and unplaced.

## Source material

Website copy and the reasoning behind it: `../VAT Reform Paper/website_policy_and_press.md`
(now partly superseded by this repo — the site is the canonical copy).
Press and citation inventories: `../VAT asymmetry news coverage/`.
The Danish memo and its fact check: `../VAT Reform Paper/danish_reform_2026/`.
