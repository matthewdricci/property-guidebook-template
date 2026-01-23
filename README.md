# Property Guidebook Template

Free, beautiful guest guidebooks hosted on GitHub Pages. No monthly fees, no dependencies, just clean HTML.

## What This Is

A simple system for creating digital guidebooks for your vacation rental properties:

- **Free hosting** via GitHub Pages
- **Single HTML file** per property - no build tools, no frameworks
- **Mobile-first design** - looks great on phones
- **Custom domain support** - use your own subdomain
- **LLM-friendly** - AI assistants can read and understand the content

## Quick Start

### 1. Fork This Repository

Click the "Fork" button in the top-right corner to create your own copy.

### 2. Copy the Template

```bash
cp TEMPLATE.html docs/your-property-name/index.html
```

Or create the folder manually in GitHub and copy the template contents.

### 3. Edit Your Guidebook

Open the file and update:
- Property name in `<title>` and `<h1>`
- OG meta tags for link previews
- All the property-specific content in each card

### 4. Enable GitHub Pages

1. Go to your repo's **Settings** → **Pages**
2. Under "Source", select **Deploy from a branch**
3. Choose **main** branch and **/ (root)** or **/docs** folder
4. Click Save

Your guidebook will be live at: `https://yourusername.github.io/your-repo-name/your-property-name/`

## Custom Domain Setup

Want to use something like `guide.yourbrand.com`?

### 1. Add CNAME File

Create a file called `CNAME` in your docs folder (or repo root if that's your source) containing just your domain:

```
guide.yourbrand.com
```

### 2. Configure DNS

Add a CNAME record at your domain registrar:
- **Type:** CNAME
- **Name:** guide (or whatever subdomain you want)
- **Value:** yourusername.github.io

### 3. Enable HTTPS

Go to **Settings** → **Pages** and check "Enforce HTTPS" after DNS propagates (may take up to 24 hours).

## File Structure

```
your-repo/
├── docs/
│   ├── property-one/
│   │   └── index.html
│   ├── property-two/
│   │   └── index.html
│   └── CNAME           # (optional) custom domain
├── TEMPLATE.html       # Starter template to copy
└── README.md
```

Each property gets its own folder. The `index.html` file means guests can visit `/property-one/` without the `.html` extension.

## Template Breakdown

The template uses semantic HTML sections called "cards" for each topic. Here's what's included:

### Sections (with anchor IDs)

| Section | Anchor | Purpose |
|---------|--------|---------|
| Quick Links | (none) | Navigation to other sections |
| Check-In | `#checkin` | Times, door codes, entry instructions |
| Parking | `#parking` | Where to park, restrictions |
| The Space | `#space` | Rooms, beds, amenities |
| Climate | `#climate` | Heating, cooling, thermostat |
| Wi-Fi | `#wifi` | Network info, remote work setup |
| Kitchen | `#kitchen` | Appliances, supplies |
| Outdoor | `#firepit` | Fire pit, grill, yard |
| House Rules | `#rules` | Smoking, pets, quiet hours |
| Local Area | `#area` | Restaurants, activities, hiking |
| Need Help | (none) | Contact info, emergency |

### Using Anchors

Link directly to sections: `https://guide.yourdomain.com/your-property/#checkin`

Useful for:
- Guest messages ("Here's info on the fire pit: [link]#firepit")
- Automated pre-arrival messages
- Quick reference when guests ask questions

## Customization

### Colors

The template uses a dark theme with gold accents. To customize, find these in the `<style>` section:

```css
/* Dark background */
background: linear-gradient(135deg, #1a1a1a 0%, #2d2d2d 100%);

/* Gold accent */
color: #f5c518;
text-decoration-color: #f5c518;
background: #fff8e1;
```

### Fonts

Uses system fonts by default for fast loading:

```css
font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
```

### Adding Sections

Copy an existing card and modify:

```html
<div class="card" id="your-anchor">
  <h2><span class="emoji">🎯</span> Section Title</h2>
  <p>Your content here.</p>
</div>
```

### Highlight Boxes

Three styles available:

```html
<!-- Default (gold) - tips and highlights -->
<div class="highlight-box">
  <strong>Pro tip:</strong> Your helpful tip here.
</div>

<!-- Warning (amber) - important notices -->
<div class="highlight-box warning">
  <strong>Note:</strong> Something guests should be aware of.
</div>

<!-- Info (blue) - explanatory context -->
<div class="highlight-box info">
  <strong>Why:</strong> Explanation of a policy or procedure.
</div>
```

### Numbered Steps

For instructions:

```html
<ol class="steps">
  <li>First step description</li>
  <li>Second step description</li>
  <li>Third step description</li>
</ol>
```

### Info Tables

For key-value pairs:

```html
<table class="info-table">
  <tr>
    <td>Check-in</td>
    <td>4:00 PM</td>
  </tr>
  <tr>
    <td>Check-out</td>
    <td>11:00 AM</td>
  </tr>
</table>
```

## The Bigger Picture

This template is the guest-facing output of a broader property management system:

```
_property.md     →  Master file with ALL property data (internal)
       ↓
_guidebook.md    →  Public-safe subset (no sensitive info)
       ↓
index.html       →  Guest-facing web page (this template)
```

If you want to maintain property info in markdown and generate HTML, you can:

1. Keep a `_property.md` with complete info (vendor contacts, backup codes, owner details)
2. Keep a `_guidebook.md` with just guest-facing info
3. Manually update the HTML when the guidebook changes, or build tooling to generate it

The HTML is designed to be readable by AI assistants, so you can use tools like Claude to help draft content or answer guest questions based on the guidebook.

## Examples

See `examples/` for a sanitized sample guidebook you can reference.

## Tips

- **Keep it scannable** - Guests skim on phones. Use headers, bullets, and short paragraphs.
- **Front-load the essentials** - Check-in, Wi-Fi, parking at the top.
- **Include anchor links** - Makes it easy to point guests to specific info.
- **Test on mobile** - Most guests will view on their phones.
- **Update regularly** - Outdated info causes more questions than no info.

## License

Do whatever you want with this. It's just HTML.
