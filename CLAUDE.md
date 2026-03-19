# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the website source repository for the IC2S2 (International Conference on Computational Social Science) conference. It's a Jekyll-based static site generator that uses YAML data files to manage conference information, allowing for easy updates without extensive HTML editing.

## Development Commands

### Local Development
```bash
# Start development server (auto-rebuilds on file changes, serves at http://localhost:4000)
bundle exec jekyll serve

# Build static site to _site/ directory
bundle exec jekyll build
```

**Important**: The `serve` command does NOT react to changes in `_config.yml`. You must restart the server (Ctrl+C and rerun) after editing `_config.yml`.

### Dependencies
```bash
# Install Jekyll and required gems
bundle install
```

## Architecture

### Data-Driven Configuration

The site separates content (YAML data files) from presentation (HTML templates), making it easy to update conference information without touching HTML. All configuration lives in YAML files under `_data/`:

- **`_config.yml`**: Primary Jekyll configuration with global variables (domain, emails, location) and page exclusion control
- **`_data/dates.yml`**: Important conference dates with `done: true/false` flags for strikethrough styling
- **`_data/navigation.yml`**: Site navigation structure (groups, pages, external links)
- **`_data/people.yml`**: Conference chairs organized by groups (General, Program, Tutorial, Poster, etc.)
- **`_data/keynotes.yml`**: Keynote speaker information
- **`_data/tutorials.yml`**: Tutorial details and schedule, plus `past_tutorials` archive
- **`_data/sponsors.yml`**: Sponsor information grouped by tier
- **`_data/registration.yml`**: Registration fees and `registration_open: true/false` toggle
- **`_data/submission.yml`**: Controls submission link visibility via `call4abstracts_open` and `call4tutorials_open` flags
- **`_data/accommodation.yml`**: Hotel and lodging information
- **`_data/travel_grants.yml`**: Travel grant details
- **`_data/social_media.yml`**: Social media links

### Directory Structure

- **`_layouts/`**: Contains `default.html` - the main page scaffolding
- **`_includes/`**: Reusable HTML partials (`header.html`, `footer.html`, `nav.html`, `dates.html`, `sponsors.html`, `head.html`)
- **`_site/`**: Generated static site output (upload this to web server)
- **Root HTML files**: Each file (e.g., `venue.html`, `register.html`) becomes a page at `/venue/`, `/register/`, etc.
- **`assets/`**: JavaScript and CSS files
- **`images/`**: All image assets (organize by subdirectories like `people/`, `venue/`)
- **`files/`**: Non-HTML files (PDFs, ZIPs, CSV schedules)

### Page Control System

Pages can be selectively hidden using the `exclude:` section in `_config.yml`. Uncomment page names (remove `#`) to hide them from the public site during development.

### YAML Data Access

Access data in templates using Liquid syntax:
- Global config: `{{ site.emails.general }}`
- Data files: `{{ site.data.dates }}`, `{{ site.data.people }}`

### Conferia.js Integration (Optional)

IC2S2 2025 introduced Conferia.js for interactive conference schedules:
1. Export schedule as CSV to `files/ic2s2_YYYY_schedule.csv`
2. Update Conferia version in `_includes/head.html` (check latest at https://github.com/nathanlesage/conferia/tags)
3. Configure in `schedule.html` (if it exists) with timezone and CSV path

### Deployment

**GitHub Actions**: Workflow at `.github/workflows/jekyll.yml` auto-builds and deploys on push. Requires repository secrets:
- `SSH_PRIVATE_KEY`: Private SSH key for deployment
- `REMOTE_HOST`: Web server domain (e.g., `www.example.com`)
- `REMOTE_USER`: SSH username
- `TARGET`: Absolute path on server (e.g., `/var/www/html/ic2s2`)

**Alternative**: Use Netlify for simple deployment (connect GitHub repo), but watch bandwidth limits during conference.

## Development Workflows

### Starting a New Conference Edition

1. Update `_config.yml`: domain, title, location, emails, conference year references
2. Uncomment all pages in `exclude:` section to hide them initially
3. Comment out all items in `_data/navigation.yml` to hide navigation links
4. Reset `_data/dates.yml` with new conference dates
5. Clear old images from `images/` (keep IC2S2 logos)
6. Clear old sponsors from `_data/sponsors.yml`
7. Move current `items` in `_data/tutorials.yml` to `past_tutorials` section

### Customizing Header Images/Videos

Edit `_includes/header.html`:
- For images: Set `background: url()` to image path
- For videos: Update `<source>` tags with MP4/WebM files, set `cover` attribute to screenshot

### Gradually Publishing Pages

1. Edit page content in root HTML files
2. Uncomment page in `_config.yml` `exclude:` section to include it
3. Uncomment corresponding navigation entry in `_data/navigation.yml`

### Updating Important Dates

Manually set `done: true` in `_data/dates.yml` as deadlines pass to apply strikethrough styling.

### YAML Formatting Rules

- Use spaces, NOT tabs for indentation
- Maintain consistent indentation levels
- Validate with https://www.yamllint.com/ if unsure

## Important Notes

- All root HTML files MUST have frontmatter (even if empty) between `---` markers, or Jekyll treats them as plain text files
- `_config.yml` and page exclusions require server restart to take effect
- The `MANUAL.md` file contains comprehensive setup instructions for conference organizers - reference it for detailed workflows
- When adding new pages: create HTML file, add frontmatter with title, add to `navigation.yml`, add to `exclude:` in `_config.yml` with `#`
