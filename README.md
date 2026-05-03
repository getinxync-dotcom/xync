# Xync Website

A single-page website for Xync — Ops & Growth Systems for Founder-Led Businesses.

## Overview

Xync helps founder-led service businesses stop operational leakage, remove founder dependency, and build systems that scale. Based in India, serving India & UAE.

## Features

- **Multi-page SPA**: Home, About, Services, Ops Clarity Index, Contact
- **Ops Clarity Index**: Interactive assessment tool with 12 questions
- **Responsive Design**: Mobile-first approach with clean, professional aesthetics
- **Zero Dependencies**: Pure HTML, CSS, and vanilla JavaScript
- **Google Sheets Integration**: Assessment submissions (configurable)

## Quick Start

### Local Development

Simply open `index.html` in your browser:

```bash
# Using Python 3
python3 -m http.server 8000

# Using Node.js
npx http-server

# Then visit http://localhost:8000
```

### Deployment

This website can be deployed to any static hosting service:

#### Hostinger
1. Upload `index.html` to your public_html directory
2. Access via your domain

#### Netlify
```bash
# Drag and drop the folder, or use Netlify CLI
netlify deploy --prod
```

#### Vercel
```bash
vercel --prod
```

#### GitHub Pages
1. Push this repository to GitHub
2. Enable GitHub Pages in repository settings
3. Select main branch as source

#### Cloudflare Pages
1. Connect your GitHub repository
2. Build command: (leave empty)
3. Build output directory: `/`

## Configuration

### Google Sheets Integration

To enable assessment data collection:

1. Create a Google Sheets spreadsheet
2. Create a Google Apps Script web app that accepts POST requests
3. Replace the `SHEETS_URL` variable in `index.html` (line ~2725):

```javascript
const SHEETS_URL = 'YOUR_GOOGLE_APPS_SCRIPT_URL_HERE';
```

Sample Google Apps Script:
```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = JSON.parse(e.postData.contents);
  
  sheet.appendRow([
    new Date(),
    data.timestamp,
    data.name,
    data.email,
    data.company,
    // Add other fields as needed
    data.totalScore,
    data.stage,
    data.intentScore,
    data.intentTier
  ]);
  
  return ContentService.createTextOutput(JSON.stringify({success: true}));
}
```

### Contact Form & Calendar Booking

Update these URLs in `index.html`:

```javascript
const WEBSITE_URL = 'https://www.xync.in';  // Line ~2720
const BOOKING_URL = 'https://calendly.com/your-link';  // Line ~2722
const TYPEFORM_URL = 'https://form.typeform.com/to/your-form';  // Line ~2723
```

## Project Structure

```
xync-website/
├── index.html          # Main website file (all-in-one)
├── README.md           # This file
├── .gitignore         # Git ignore rules
└── LICENSE            # MIT License
```

## Technology Stack

- **HTML5**: Semantic markup
- **CSS3**: Custom properties (CSS variables), Flexbox, Grid
- **JavaScript (ES6+)**: Vanilla JS, no frameworks
- **Fonts**: Google Fonts (Syne, DM Sans)

## Browser Support

- Chrome/Edge (latest 2 versions)
- Firefox (latest 2 versions)
- Safari (latest 2 versions)
- Mobile browsers (iOS Safari, Chrome Mobile)

## Performance

- **Zero external dependencies** (except Google Fonts)
- **Single HTTP request** (after font loading)
- **Lightweight**: ~100KB total file size
- **Fast load times**: < 1 second on 3G

## Customization

### Colors

Edit CSS variables in the `:root` section (lines 22-42):

```css
:root {
  --ink: #0D0D0D;      /* Primary text */
  --paper: #F7F4EF;    /* Background */
  --gold: #C8963E;     /* Accent color */
  --teal: #0D6B5E;     /* CTA color */
  /* ... other colors */
}
```

### Content

All content is inline in `index.html`. Search for the relevant section IDs:
- `#home-page` - Home page content
- `#about-page` - About page content
- `#services-page` - Services page content
- `#oci-page` - Ops Clarity Index assessment
- `#contact-page` - Contact page content

## SEO

Meta tags are included in the `<head>` section. Update as needed:
- Title: Line 6
- Description: Line 7
- Add Open Graph tags for social sharing
- Add structured data (JSON-LD) for better search visibility

## Analytics

Add your analytics tracking code before the closing `</body>` tag:

```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

## License

MIT License - see LICENSE file for details

## Support

For questions or support, contact: [Add your contact email]

## Credits

Website built for Xync - Ops & Growth Systems for Founder-Led Businesses
