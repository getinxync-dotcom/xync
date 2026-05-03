# Quick Start Guide

Get your Xync website up and running in 5 minutes.

## ⚡ Super Quick Deploy

### Option 1: Netlify Drop (Easiest - 2 minutes)

1. Go to [app.netlify.com/drop](https://app.netlify.com/drop)
2. Drag the entire `xync-website` folder onto the page
3. Done! Your site is live

**Result:** You'll get a URL like `random-name-123.netlify.app`

### Option 2: Vercel (3 minutes)

```bash
npx vercel
```

Follow the prompts. Done!

### Option 3: GitHub Pages (5 minutes)

```bash
# In the xync-website directory
git remote add origin https://github.com/yourusername/xync-website.git
git push -u origin main
```

Then:
1. Go to your GitHub repo settings
2. Click "Pages" in the sidebar
3. Select "main" branch as source
4. Save

Your site will be at `yourusername.github.io/xync-website`

---

## 🧪 Local Testing

Want to preview locally first?

```bash
# Python 3 (built into macOS/Linux)
python3 -m http.server 8000

# Or with Node.js
npx http-server

# Or with PHP
php -S localhost:8000
```

Visit `http://localhost:8000` in your browser.

---

## ⚙️ Essential Configuration

After deploying, update these URLs in `index.html`:

### 1. Google Sheets Integration (Line ~2725)

Replace:
```javascript
const SHEETS_URL = 'YOUR_GOOGLE_APPS_SCRIPT_URL_HERE';
```

With your Google Apps Script web app URL.

**Why?** This captures assessment submissions.

**How to set up:**
1. Create a Google Sheet
2. Extensions > Apps Script
3. Paste this code:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Responses");
  var data = JSON.parse(e.postData.contents);
  
  sheet.appendRow([
    new Date(),
    data.name,
    data.email,
    data.company,
    data.totalScore,
    data.stage,
    data.intentScore,
    data.intentTier
  ]);
  
  return ContentService.createTextOutput(JSON.stringify({success: true}))
    .setMimeType(ContentService.MimeType.JSON);
}
```

4. Deploy > New deployment > Web app
5. Execute as: Me
6. Who has access: Anyone
7. Copy the web app URL

### 2. Booking Calendar (Line ~2722)

Replace:
```javascript
const BOOKING_URL = 'https://calendly.com/your-link';
```

With your Calendly/Cal.com/SavvyCal link.

### 3. Contact Form (Line ~2723)

Replace:
```javascript
const TYPEFORM_URL = 'https://form.typeform.com/to/your-form';
```

With your Typeform URL.

---

## 📊 Add Analytics (Optional)

Before `</body>` tag, add:

```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

Replace `G-XXXXXXXXXX` with your GA4 measurement ID.

---

## 🎨 Customization Basics

### Change Colors

Edit in `index.html` (lines 22-42):

```css
:root {
  --ink: #0D0D0D;      /* Text color */
  --paper: #F7F4EF;    /* Background */
  --gold: #C8963E;     /* Accent */
  --teal: #0D6B5E;     /* CTA buttons */
}
```

### Update Company Info

Search for these strings in `index.html` and replace:
- `Xync` - Your company name
- `www.xync.in` - Your website URL
- `ops@xync.in` - Your email

### Modify Content

Each page has a clear section ID:
- Home: Search for `id="home-page"`
- About: Search for `id="about-page"`
- Services: Search for `id="services-page"`
- Assessment: Search for `id="oci-page"`
- Contact: Search for `id="contact-page"`

---

## ✅ Post-Deploy Checklist

After your first deploy:

- [ ] Test all pages (Home, About, Services, Assessment, Contact)
- [ ] Test mobile menu on phone
- [ ] Complete the assessment to verify it works
- [ ] Update Google Sheets URL
- [ ] Update booking calendar URL
- [ ] Update contact form URL
- [ ] Add Google Analytics (optional)
- [ ] Test on mobile device
- [ ] Add custom domain (optional)
- [ ] Enable HTTPS (most platforms do this automatically)

---

## 🆘 Troubleshooting

**Q: Pages aren't switching when I click navigation**
- Check browser console for errors
- Make sure JavaScript isn't blocked
- Try in incognito/private mode

**Q: Assessment doesn't submit**
- Verify SHEETS_URL is configured
- Check Google Apps Script deployment settings
- Ensure "Who has access" is set to "Anyone"

**Q: Mobile menu doesn't open**
- Test on actual mobile device (not just desktop resize)
- Check if JavaScript errors are present
- Clear browser cache

**Q: Deployment failed**
- Make sure you're deploying the whole folder
- Check that index.html is in the root directory
- Try a different platform

---

## 📚 Next Steps

1. **Read the full [README.md](README.md)** for detailed information
2. **Check [DEPLOYMENT.md](DEPLOYMENT.md)** for platform-specific guides
3. **Review [CONTRIBUTING.md](CONTRIBUTING.md)** if you plan to modify code

---

## 🚀 You're All Set!

Your Xync website is now live. Time to start capturing leads and qualifying prospects!

**Need help?** Open an issue on GitHub or refer to the documentation.
