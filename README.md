# maytarr.github.io

Personal portfolio, live at https://maytarr.github.io. GitHub Pages builds it with Jekyll every time you push.

## Add a project write-up

1. Copy `_templates/project.md` to `_projects/my-project.md`.
2. Fill in the front matter (the part between the `---` lines) and write the article in Markdown below it.
3. Put any images in `assets/images/my-project/`.
4. Publish:

   ```
   git add .
   git commit -m "Add my-project write-up"
   git push
   ```

The page appears at `https://maytarr.github.io/projects/my-project/` a minute or two later. It's added to the home page, the tags page and the RSS feed automatically.

Set `published: false` in the front matter to keep a half-finished write-up off the site.

## Edit other content

| What | File |
|---|---|
| Name, tagline, contact details (email, LinkedIn, location, CV) | `_config.yml` |
| About paragraph | `_includes/about.md` |
| Skills list | `_data/skills.yml` |
| Colours and styling | `assets/css/style.css` |
| Page structure | `_layouts/`, `_includes/`, `index.html` |

If a push breaks the build, GitHub shows the error under the repo's **Actions** tab and the live site keeps the last working version.
