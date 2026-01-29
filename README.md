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
│   │   ├── index.html
│   │   └── images/
│   │       └── hero.jpg      # (optional) cover photo
│   ├── property-two/
│   │   └── index.html
│   └── CNAME                 # (optional) custom domain
├── TEMPLATE.html             # Starter template to copy
├── examples/
│   └── campfire-cottage/     # Working example
└── README.md
```

Each property gets its own folder. The `index.html` file means guests can visit `/property-one/` without the `.html` extension.

---

## Hero Image Setup (Optional but Recommended)

Adding a cover photo to your guidebook creates a more premium look and enables rich link previews in iMessage, Slack, and social media.

### What You Get

| Without Hero Image | With Hero Image |
|-------------------|-----------------|
| Simple dark gradient header | Beautiful cover photo with overlay |
| No image in link previews | Property photo shows in iMessage/Slack |
| Works out of the box | Requires image setup |

### Step 1: Prepare Your Image

Use your best property photo (exterior or signature shot). Compress it for fast loading:

**On macOS (using built-in `sips`):**
```bash
# Resize to max 1920px width, compress to ~150-200KB
sips -Z 1920 --setProperty formatOptions 50 source.jpg --out hero.jpg
```

**Or use any image editor** - aim for:
- Width: 1200-1920px
- File size: 150-200KB
- Format: JPEG

### Step 2: Add Image to Your Property Folder

```bash
mkdir -p docs/your-property/images
mv hero.jpg docs/your-property/images/hero.jpg
```

### Step 3: Extract a Matching Background Color

When viewed on desktop, the hero image is constrained to 600px width with rounded corners. The space on either side needs a background color that complements your image.

**Using ImageMagick** (install with `brew install imagemagick`):
```bash
magick docs/your-property/images/hero.jpg -resize 1x1\! -format "#%[hex:u.p{0,0}]" info:
```

This outputs a hex color like `#A0815B` based on the average color of your image.

**Without ImageMagick:**
- Open your image in any image editor
- Use an eyedropper tool on a dominant color area
- Pick something from the middle tones (not too light, not too dark)

### Step 4: Update the HTML

In your `index.html`, make these changes:

**1. Add the og:image meta tag** (in `<head>`, after other og: tags):
```html
<meta property="og:image" content="https://your-domain.com/your-property/images/hero.jpg">
```

**2. Enable the hero image CSS** (find the HERO SECTION comment in `<style>`):

Replace the simple gradient:
```css
.hero {
  background: linear-gradient(135deg, #1a1a1a 0%, #2d2d2d 100%);
  /* ... */
}
```

With the hero image version:
```css
.hero {
  background:
    linear-gradient(to bottom, rgba(0,0,0,0.4), rgba(0,0,0,0.6)),
    url('images/hero.jpg') center/cover no-repeat;
  color: white;
  padding: 60px 20px;
  text-align: center;
  min-height: 200px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
```

**3. Enable the desktop wrapper styles** (uncomment this block):
```css
@media (min-width: 768px) {
  .hero-wrapper {
    background-color: #A0815B;  /* YOUR EXTRACTED COLOR HERE */
  }

  .hero {
    max-width: 600px;
    margin: 0 auto;
    border-radius: 12px;
  }
}
```

### Quick Reference (Full Workflow)

```bash
# Variables - update these
PROPERTY="my-property"
SOURCE_IMAGE="~/Downloads/listing-photo.jpg"
DOMAIN="guide.mydomain.com"

# Create directory and compress image
mkdir -p "docs/$PROPERTY/images"
sips -Z 1920 --setProperty formatOptions 50 "$SOURCE_IMAGE" --out "docs/$PROPERTY/images/hero.jpg"

# Check file size (should be ~150-200KB)
ls -lh "docs/$PROPERTY/images/hero.jpg"

# Extract dominant color (requires ImageMagick)
magick "docs/$PROPERTY/images/hero.jpg" -resize 1x1\! -format "#%[hex:u.p{0,0}]" info:

# Now update your HTML with the extracted color
```

### For AI Assistants

If you're an LLM helping someone set up a guidebook with a hero image:

1. **Image compression**: Use `sips` on macOS or guide user to compress to ~150-200KB
2. **Color extraction**: Use the ImageMagick command above, or ask user to pick a mid-tone color from their image using an eyedropper
3. **CSS changes needed**:
   - Replace the `.hero` background gradient with the image version (includes overlay gradient + `url('images/hero.jpg')`)
   - Add `min-height: 200px`, `display: flex`, `flex-direction: column`, `justify-content: center`, `align-items: center` to `.hero`
   - Uncomment the `@media (min-width: 768px)` block for `.hero-wrapper` and nested `.hero`
   - Set `.hero-wrapper` background-color to the extracted hex color
4. **Meta tag**: Add `<meta property="og:image" content="...">` with full URL to hero.jpg

---

## Link Previews (og:image)

When someone shares your guidebook link in iMessage, Slack, or social media, the `og:image` meta tag controls what image appears in the preview.

**Without og:image:** Generic link with no image
**With og:image:** Rich preview showing your property photo

### Setup

Add this meta tag in `<head>` (after the other og: tags):

```html
<meta property="og:image" content="https://your-domain.com/your-property/images/hero.jpg">
```

Requirements:
- Use the **full URL** (not a relative path)
- Image should be at least **1200x630px** for best results
- The same hero.jpg works great for this

### Testing Link Previews

After deploying, test your link preview:
- **iMessage**: Send the link to yourself
- **Slack**: Paste the link in a channel
- **Facebook**: Use [Facebook Sharing Debugger](https://developers.facebook.com/tools/debug/)
- **Twitter**: Use [Twitter Card Validator](https://cards-dev.twitter.com/validator)

Note: Platforms cache previews. If you update your image, you may need to use the debugger tools to refresh the cache.

---

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

See `examples/campfire-cottage/` for a complete working example with hero image setup.

## Tips

- **Keep it scannable** - Guests skim on phones. Use headers, bullets, and short paragraphs.
- **Front-load the essentials** - Check-in, Wi-Fi, parking at the top.
- **Include anchor links** - Makes it easy to point guests to specific info.
- **Test on mobile** - Most guests will view on their phones.
- **Update regularly** - Outdated info causes more questions than no info.
- **Add a hero image** - Takes 10 minutes and makes a big visual difference.

## License

Do whatever you want with this. It's just HTML.
