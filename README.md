# IC2S2 Conference Website — Guide

## How It Works

This repo hosts all IC2S2 conference websites in one place. Each year gets its own **branch**, and Netlify automatically deploys each branch to a subdomain:

| Branch | URL |
|--------|-----|
| `main` | [ic2s2.net](https://ic2s2.net) |
| `2026` | [2026.ic2s2.net](https://2026.ic2s2.net) |
| `2025` | [2025.ic2s2.net](https://2025.ic2s2.net) |
| `2024` | [2024.ic2s2.net](https://2024.ic2s2.net) |
| ... | ... |

**Push to a branch → site goes live automatically.** That's it.

---

## For New Conference Organizers (Step-by-Step)

Using 2026 as an example.

### Step 1: Set up your year branch with the previous year's site

Clone this repo and check out your year's branch (placeholder branches have been pre-created through 2028):

```sh
git clone https://github.com/iscss/ic2s2.git
cd ic2s2
git checkout 2026
```

Then replace its contents with the previous year's site as a starting point:

```sh
git rm -rf .
git checkout origin/2025 -- .
git add -A
git commit -m "Initialize 2026 site from 2025"
git push origin 2026
```

If your year's branch doesn't exist yet, create it from the previous year:

```sh
git checkout 2025
git checkout -b 2027
git push -u origin 2027
```

### Step 2: Build your site

Update the content for your year — dates, venue, sponsors, speakers, etc. You can push your branch at any time to preview it live:

```sh
git add -A
git commit -m "Update site for 2026"
git push -u origin 2026
```

As soon as you push, the site goes live at **2026.ic2s2.net**. Subsequent pushes to the `2026` branch will automatically update the live site.

### Step 3: Keep updating

As the conference takes shape, keep pushing changes to your year branch:

```sh
git checkout 2026
# make changes
git add -A
git commit -m "Add keynote speakers"
git push origin 2026
```

That's the whole deployment workflow. Edit, commit, push — the site updates automatically.

The rest of this document covers how the site template itself works so you know what to edit.

---

## The Site Template

> The site template and the documentation below were originally created by
> Hendrik Erz for IC2S2 2025 in Norrköping. Hendrik decoupled the layout from
> the data so that adding and modifying conference information is much quicker,
> and the site can be passed from year to year more easily. The original
> unmodified manual is also available at [MANUAL.md](./MANUAL.md). Thank you,
> Hendrik, for the excellent work!

> [!NOTE]
> This website is intended as a living document and to be improved over time! If
> you find some data or pages that you can make easier to edit for further
> years, please do so and adapt this guide as you see fit.

### Using Jekyll

This site runs on [Jekyll](https://jekyllrb.com/), a static site generator. Jekyll takes the contents of the branch and compiles them into a static HTML folder called `_site`. This is what gets deployed.

#### Installing Jekyll

To preview changes locally, install Jekyll on your computer. Head over to the [Jekyll docs](https://jekyllrb.com/docs/) for setup instructions.

#### Previewing Locally

```bash
bundle exec jekyll serve
```

This starts a development server at http://localhost:4000. It watches for changes and rebuilds automatically.

> [!NOTE]
> The `serve` command does not react to changes in `_config.yml`. When you
> change that file, stop the server with `Ctrl+C` and restart it.

To just build without serving:

```bash
bundle exec jekyll build
```

This builds the site into `_site` and quits.

### Folder Structure

#### `_data`

All the data files live here, in YAML format. You will spend most of your time editing these. If YAML is unfamiliar, [here is a quick introduction](https://learnxinyminutes.com/docs/yaml/). Usually you can just take what's in the existing files and swap the values.

> [!WARNING]
> YAML is sensitive to indentation. Use spaces, not tabs. If something breaks,
> try a [YAML checker](https://www.yamllint.com/).

Key data files:

| File | What it controls |
|------|-----------------|
| `navigation.yml` | Main navigation menu items |
| `dates.yml` | Important dates shown on the landing page |
| `people.yml` | Conference chairs and organizers |
| `sponsors.yml` | Sponsor logos and links (grouped by tier) |
| `registration.yml` | Registration fees, currency, and registration link |
| `submission.yml` | Submission links (e.g., OpenReview) and open/closed toggle |
| `tutorials.yml` | Tutorial listings with instructors and abstracts |

#### `_includes`

HTML partials included in various places. You should rarely need to edit these. They use standard HTML with [Liquid](https://jekyllrb.com/docs/liquid/) template syntax. The naming is intended to be self-explanatory, and there are comments inside the files.

> [!TIP]
> If you use [Conferia.js](https://github.com/nathanlesage/conferia) for the
> schedule (see below), you will need to update the version number in
> `head.html`.

#### `_layouts`

Contains the major layouts. There is only one: `default.html`, the default scaffolding for every page.

#### `_site`

After building, this folder contains the complete static website. You don't edit this directly — it's generated by Jekyll.

#### `assets`

JavaScript and CSS files for the website.

#### `files`

Non-HTML files (ZIP templates, CSVs, schedule files, etc.).

#### `images`

All images used on the website — speaker photos, venue photos, sponsor logos, etc.

> [!NOTE]
> Stick to a reasonable folder structure here (e.g., `people/` for headshots,
> `venue/` for venue photos, `sponsors/` for logos). Otherwise this folder
> becomes a mess.

#### HTML Pages (main folder)

Each HTML file in the main folder corresponds to a subpage. For example, `venue.html` becomes `YEAR.ic2s2.net/venue`. These files must start with a frontmatter block:

```html
---
title: This will become the page's title
---
<div>
  <p>Your HTML content here.</p>
</div>
```

The two lines of `---` are required. Jekyll ignores files without them.

#### `_config.yml`

The primary configuration file. The most important section is the global variables (conference dates, email addresses, etc.), accessible in any HTML file using `{{ site.property_name }}`. For example, `{{ site.emails.general }}`.

The `exclude` section lists files Jekyll should ignore. Use this to hide pages that aren't ready yet by uncommenting their lines.

#### Other files

* `Gemfile` / `Gemfile.lock`: Jekyll plugin configuration. You can usually ignore these.
* `MANUAL.md`: Hendrik's original website manual.
* `README.md`: General readme.

---

### Adapting the Site for Your Year

#### First Things First

1. **Adapt `_config.yml`** — update dates, emails, venue info. Use placeholders for anything you don't know yet.
2. **Hide unready pages** — uncomment lines in the `exclude` section of `_config.yml` to prevent Jekyll from including pages that still have last year's content. The landing page (`index.html`) is never excluded.
3. **Comment out navigation entries** in `_data/navigation.yml`. This hides nav links so you can gradually "unlock" pages as they're ready.
4. **Update or comment out dates** in `_data/dates.yml`.
5. **Archive last year's tutorials** — in `_data/tutorials.yml`, move `items` to `past_tutorials` and start fresh.
6. **Update `about.html`** — add a link to last year's conference and add your edition on top of the list.
7. **Clear old images** from the `images` folder (keep the IC2S2 logos).
8. **Clear old sponsors** from `_data/sponsors.yml`.

At this point you have a clean starting point and can begin adapting.

#### Choose a Header Image

Each conference traditionally chooses a custom header image or video. Edit `_includes/header.html` — it allows separate images for the landing page and subpages.

**For an image:**
* Comment out the `<video>` element
* Update the `background: url()` to point to your image

**For a video:**
* Encode in both `mp4` and `webm` ([Handbrake](https://handbrake.fr/) works well)
* Take a screenshot of the first frame as a fallback
* Place files in `images/` with clear names (e.g., `ic2s2_2027_teaser`)
* Update the `<source>` elements and `cover` attribute in the `<video>` element
* Set `background: url()` to the screenshot as a fallback

#### Adapt the Landing Page

Open `index.html` and update:
* The iteration number (e.g., `12<sup>th</sup>`)
* The location and conference dates
* Venue photo
* "About the Conference" section

Run `bundle exec jekyll serve` and check http://localhost:4000 to verify. At this point you have the bare minimum for a live site.

#### Important Dates

Edit `_data/dates.yml`. When a deadline passes, set `done: true` to apply strikethrough styling.

#### Conference Chairs

Edit `_data/people.yml`. Groups include General Chairs, Program Chairs, Tutorial Chairs, Poster Chairs, Website & Social Media Chairs, and Volunteers. Groups with no `people` entries are automatically hidden. Each person has: name, affiliation, field, image, and URL.

#### Registration Fees

Edit `_data/registration.yml` — set currency, fees, and the link to your registration system. Set `registration_open` to `false` initially, then `true` when registration opens.

#### Submission Links

Edit `_data/submission.yml` — add links (usually OpenReview) and toggle submission open/closed.

#### Keynote Speakers

Added similarly to chairs, with additional fields for bio and talk details.

#### Tutorials

Edit `_data/tutorials.yml`. Each tutorial needs:
* `title`: The tutorial's title
* `tutors`: A list of instructors (each with name, image, affiliation, url)
* `time`: When the tutorial runs
* `room`: Which room
* `website`: Link to tutorial materials
* `abstract`: Description of the tutorial

#### Sponsors

Edit `_data/sponsors.yml`. Grouped by tier (Academic, Gold, Bronze, etc.). Each sponsor has: name, image, URL. They appear on both the landing page and sponsors subpage.

#### Adding New Pages

1. Create a new HTML file in the main folder (e.g., `my-new-page.html`)
2. Add a frontmatter at the top:
   ```yaml
   ---
   title: My New Page
   layout: default
   ---
   ```
3. Add HTML content below the frontmatter. Tip: copy an existing page and adapt it.
4. Add a navigation entry in `_data/navigation.yml`
5. Add the filename to the `exclude` section in `_config.yml`, commented out with `#` (so it's included now, but future organizers can easily hide it)

#### Creating a Schedule with Conferia.js

IC2S2 2025 introduced [Conferia.js](https://github.com/nathanlesage/conferia), a framework by Hendrik Erz for interactive conference agendas. To use it:

1. Create your agenda and export a CSV file (see the [Conferia manual](https://github.com/nathanlesage/conferia))
2. Save it as `files/ic2s2_YEAR_schedule.csv`
3. In `_includes/head.html`, update the Conferia version to the [latest release](https://github.com/nathanlesage/conferia/tags) (e.g., change `conferia@0.18.0` to `conferia@1.2.0`)
4. In `schedule.html`, update the `src` path to your CSV file and set the `timeZone` for your conference location

When the schedule changes, export a new CSV and replace the file.

> [!TIP]
> If you find bugs or have ideas for Conferia.js, Hendrik welcomes
> [feedback](https://github.com/nathanlesage/conferia/issues)!

---

## Quick Reference

### Initializing a pre-created year branch from the previous year

```sh
git checkout NEW_YEAR
git rm -rf .
git checkout origin/PREVIOUS_YEAR -- .
git add -A
git commit -m "Initialize NEW_YEAR site from PREVIOUS_YEAR"
git push origin NEW_YEAR
```

### Creating a year branch that doesn't exist yet

```sh
git checkout PREVIOUS_YEAR
git checkout -b NEW_YEAR
git push -u origin NEW_YEAR
```

### Checking what's deployed

```sh
# See all year branches
git branch -r | grep origin

# See what a branch contains
git log origin/2026 --oneline -5
```

---

## Netlify

There is a Netlify account associated with this repo, maintained by ISCSS. To log in, go to [app.netlify.com](https://app.netlify.com/teams/iscss/sites) and select "Login with Github" using the credentials for the `iscss` GitHub account.

Netlify's **branch deploys** feature is what makes this all work — every branch automatically gets deployed to `BRANCHNAME.ic2s2.net`.

---

## FAQ

**Why not just set up DNS forwarding for each year's subdomain?**
This was tried, and several past websites would time out with DNS forwarding across multiple providers. More people are familiar with git than DNS configuration, and having everything in one repo is simpler and more reliable.

**Why a branch per year?**
Netlify branch deploys mean each branch automatically gets its own subdomain. Pushing to the `2026` branch deploys to `2026.ic2s2.net`. No DNS changes needed for new years — just create a branch.

**What about older sites that are just redirects?**
We've been replacing redirects with actual archived site content so everything is preserved. If a past conference site goes down, the content is still available here.

**Can I preview before going live?**
You can serve locally with Jekyll (`bundle exec jekyll serve`) or push to the branch — the site deploys within minutes and you can check it at `YEAR.ic2s2.net`.

**Where can I learn more about the site template?**
See [MANUAL.md](./MANUAL.md) for the original detailed manual written by Hendrik Erz for IC2S2 2025.
