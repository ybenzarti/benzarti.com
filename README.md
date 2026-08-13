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

1. **Add back Google Scholar and NBER links, with real URLs.** The originals were constructed
   from standard patterns, never verified, and were **removed on 11 August** rather than left
   broken. Scholar IDs are random strings, so the profile URL has to be copied from the live
   page. The Ministry's reply link was also a placeholder and is gone. **There are currently no
   knowingly-broken links on the site.**
2. **A public URL for the Danish Ministry's reply**, if one exists, to sit beside the memo in the
   Denmark entry.
3. **Getting this to outrank the old Google Site.** On-page work is done (meta descriptions,
   Open Graph, sitemap, robots). What remains needs logins:
   - Strip the old Google Site to one line pointing here. **Do not delete it** — Google Sites
     cannot issue a redirect, and deleting throws away the ranking and breaks every CV link.
   - Update inbound links, which matter far more than anything on the page: NBER profile, the
     UCSB department page, Google Scholar's homepage field, RePEc/IDEAS, coauthors' pages, and
     the CV PDF on Dropbox (Google indexes it, and it still prints the old address).
   - Google Search Console: verify the domain (DNS TXT is easiest, you control DNS at Square),
     submit `https://www.benzarti.com/sitemap.xml`, request indexing.
4. **Read the Greece entry in `policy.qmd` before circulating the address.** It cites IME GSEVEE
   Brief 37, which argues *against* the asymmetry for recent episodes. Included deliberately —
   it forecloses any charge of cherry-picking, and it is the only trace of the work in Greece —
   but it is a judgment call and striking it is one line.
5. *(closed 11 Aug)* The coverage archive asserted there was no named Japanese coverage. The
   Nikkei interview of **7 July 2026** disproves it. Corrected in four files there, and a new
   convention added: sweep every file type, not just `.md`/`.txt`/`.json` — the interview sat in
   that folder as a `.pdf` and was missed by two sweeps because of the extension filter.

## ⚠ Do not trust an automated link check on this page

Three hosts return errors to `curl`/scripts but are fine in a browser. All three were flagged as
broken on 11–12 August and all three were false positives:

| Host | What a script sees | Reality |
|---|---|---|
| `cas.go.jp` (Japanese Cabinet Secretariat minutes) | **404** | 200 with a browser user-agent; 338 KB PDF, both quoted passages verified inside it |
| `bloomberg.com` (Giugliano column) | **403** | Fine — confirmed good by Youssef, 12 Aug |
| `ftc.gov` (H&R Block case page) | **403** | Fine — confirmed good by Youssef, 12 Aug |

Before removing any link on a 403/404, retry with a browser user-agent:

    curl -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 \
      (KHTML, like Gecko) Chrome/126.0 Safari/537.36" -sSI -L <url>

**One link on the page rests on Youssef's own knowledge, not on retrieval:** the Bloomberg column
(Ferdinando Giugliano, "A Bad New Tax Idea Is Doing the Rounds", 26 June 2020) is paywalled and
could not be read, so that it cites the work is his confirmation, not something verified here.

## Layout notes — non-obvious, easy to break

- **Do not add `max-width` to `main`.** Quarto lays the page out as a CSS grid whose content
  track is `minmax(500px, 850px - 3em)`. An earlier `max-width: 46rem` squeezed that track and
  pulled the side column left; the negative margin added to compensate then pushed the column
  out of its track and it was visibly clipped. Both are gone. Let the grid do the work.
- **`main` uses a clearfix, not `overflow: hidden`.** `overflow: hidden` contains the float but
  also clips anything extending past the box, which silently swallowed a layout change.
- **The home page hides Quarto's title block** (`#title-block-header { display: none }`, scoped
  in `index.qmd`'s own header, not the shared stylesheet). Quarto injects that block above the
  body, so a floated column could never start level with it. The name is an ordinary `h1` placed
  *after* the `.sidecol` div, which is what lets the two sit side by side. The `<title>` tag and
  Open Graph title still come from the front matter and are unaffected. **`policy.html` keeps its
  normal title block** — verify that if you touch this.
- **The side column follows eml.berkeley.edu/~saez**, mirrored to the right: a 200px column with
  the photo on top and affiliations and contacts stacked beneath. Under 700px it stops floating
  and centres.
- **Body text is justified with `hyphens: auto`**; the side column is deliberately excluded,
  being too narrow to justify well. Hyphenation depends on `lang="en"` on the `<html>` element.
- **Quarto wraps each section's content in `<section>`**, so `main > p` matches nothing. Use
  `main p`.

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
  28 May and 11 June 2018 covering the IPP note, which is the **France paper**. Microeconomic Insights
  stayed with the JPE paper. **Bloomberg was later identified** (Ferdinando Giugliano, Bloomberg
  Opinion, 26 June 2020) and moved into the Europe 2020 entry, four days before the Il Sole
  piece and in the same debate.

## Source material

Website copy and the reasoning behind it: `../VAT Reform Paper/website_policy_and_press.md`
(now partly superseded by this repo — the site is the canonical copy).
Press and citation inventories: `../VAT asymmetry news coverage/`.
The Danish memo and its fact check: `../VAT Reform Paper/danish_reform_2026/`.
