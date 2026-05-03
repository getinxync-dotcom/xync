# Deployment Guide

This guide covers deploying the Xync website to various hosting platforms.

## Prerequisites

- A GitHub account (for most platforms)
- Your domain name (optional, but recommended)
- Access to DNS settings (if using custom domain)

## Platform-Specific Guides

### 1. Netlify (Recommended for Simplicity)

**Method A: Drag & Drop**
1. Go to [netlify.com](https://netlify.com)
2. Sign up/login
3. Drag the `xync-website` folder onto the Netlify dashboard
4. Your site is live! You'll get a URL like `random-name-123.netlify.app`

**Method B: Git-based Deployment**
1. Push this repository to GitHub
2. Go to Netlify and click "New site from Git"
3. Connect your GitHub repository
4. Build settings:
   - Build command: (leave empty)
   - Publish directory: `/`
5. Click "Deploy site"

**Custom Domain:**
1. Go to Site settings > Domain management
2. Add custom domain
3. Update your DNS with the provided records

---

### 2. Vercel

**Method A: CLI**
```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
cd xync-website
vercel --prod
```

**Method B: Web Interface**
1. Go to [vercel.com](https://vercel.com)
2. Import your GitHub repository
3. Click "Deploy"

**Configuration:**
- Framework Preset: Other
- Build Command: (leave empty)
- Output Directory: ./

---

### 3. GitHub Pages

1. Push this repository to GitHub
2. Go to repository Settings > Pages
3. Source: Deploy from a branch
4. Branch: `main` (or `master`)
5. Folder: `/ (root)`
6. Click Save

**Custom Domain:**
1. Add a `CNAME` file with your domain name
2. Configure DNS:
   - Add CNAME record pointing to `yourusername.github.io`
3. Enable HTTPS in GitHub Pages settings

---

### 4. Cloudflare Pages

1. Go to [pages.cloudflare.com](https://pages.cloudflare.com)
2. Create a new project
3. Connect your GitHub repository
4. Build settings:
   - Build command: (leave empty)
   - Build output directory: `/`
5. Click "Save and Deploy"

**Benefits:**
- Global CDN
- Free SSL
- DDoS protection
- Excellent performance

---

### 5. Hostinger (Traditional Hosting)

1. Login to your Hostinger control panel
2. Go to File Manager
3. Navigate to `public_html` (or your domain's folder)
4. Upload `index.html`
5. Visit your domain

**FTP Upload:**
```bash
# Using FileZilla or any FTP client
Host: ftp.yourdomain.com
Username: [your FTP username]
Password: [your FTP password]
Port: 21

# Upload index.html to public_html/
```

---

### 6. AWS S3 + CloudFront

**S3 Setup:**
```bash
# Create S3 bucket
aws s3 mb s3://xync-website

# Upload files
aws s3 sync . s3://xync-website --exclude ".git/*"

# Configure for static hosting
aws s3 website s3://xync-website --index-document index.html
```

**CloudFront Setup:**
1. Create CloudFront distribution
2. Set S3 bucket as origin
3. Configure SSL certificate (AWS Certificate Manager)
4. Point your domain to CloudFront distribution

---

### 7. Firebase Hosting

```bash
# Install Firebase CLI
npm install -g firebase-tools

# Login
firebase login

# Initialize
firebase init hosting
# Choose: existing project or create new
# Public directory: .
# Single-page app: Yes
# GitHub automatic builds: (optional) Yes

# Deploy
firebase deploy --only hosting
```

---

### 8. DigitalOcean App Platform

1. Go to [cloud.digitalocean.com](https://cloud.digitalocean.com)
2. Click "Create" > "Apps"
3. Connect your GitHub repository
4. Choose "Static Site"
5. Output directory: `/`
6. Click "Next" and deploy

**Pricing:** Free tier available

---

## Post-Deployment Checklist

### 1. Test All Pages
- [ ] Home page loads
- [ ] About page works
- [ ] Services page displays correctly
- [ ] Ops Clarity Index assessment functions
- [ ] Contact page is accessible
- [ ] Mobile responsive on all pages

### 2. Configure Integrations

**Google Sheets (for assessment data):**
```javascript
// In index.html, update line ~2725
const SHEETS_URL = 'https://script.google.com/macros/s/YOUR_ID/exec';
```

**Booking Calendar:**
```javascript
// In index.html, update line ~2722
const BOOKING_URL = 'https://calendly.com/your-link';
```

**Contact Form:**
```javascript
// In index.html, update line ~2723
const TYPEFORM_URL = 'https://form.typeform.com/to/your-form';
```

### 3. Add Analytics

**Google Analytics:**
Add before `</body>` tag:
```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

**Alternative: Simple Analytics, Plausible, Fathom**
- More privacy-friendly
- Lightweight
- No cookie consent needed (GDPR-friendly)

### 4. SEO Setup

**Add to `<head>`:**
```html
<!-- Open Graph for social sharing -->
<meta property="og:title" content="Xync — Ops & Growth Systems">
<meta property="og:description" content="Stop operational leakage, remove founder dependency">
<meta property="og:image" content="https://yourdomain.com/og-image.jpg">
<meta property="og:url" content="https://yourdomain.com">

<!-- Twitter Card -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="Xync — Ops & Growth Systems">
<meta name="twitter:description" content="Stop operational leakage, remove founder dependency">
<meta name="twitter:image" content="https://yourdomain.com/twitter-card.jpg">
```

**Create sitemap.xml:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://yourdomain.com/</loc>
    <lastmod>2026-05-04</lastmod>
    <priority>1.0</priority>
  </url>
</urlset>
```

**Submit to:**
- Google Search Console
- Bing Webmaster Tools

### 5. Performance Optimization

**Already optimized:**
- ✅ Single HTML file (minimal HTTP requests)
- ✅ No JavaScript dependencies
- ✅ CSS variables for theming
- ✅ Mobile-first responsive design

**Optional enhancements:**
- Add preconnect for Google Fonts
- Enable HTTP/2 or HTTP/3 (server-level)
- Enable gzip/brotli compression (platform handles this)

### 6. SSL Certificate

Most platforms (Netlify, Vercel, Cloudflare Pages) auto-provision SSL certificates.

**For custom setups:**
- Use [Let's Encrypt](https://letsencrypt.org/) (free)
- Configure auto-renewal

### 7. CDN Configuration

If using traditional hosting:
- Consider Cloudflare (free tier)
- Adds global CDN, SSL, DDoS protection

---

## Domain Configuration

### DNS Records

**For Netlify/Vercel:**
```
Type: CNAME
Name: www
Value: [platform-provided-value].netlify.app

Type: A (for apex domain)
Name: @
Value: [platform-provided-IP]
```

**For Cloudflare Pages:**
```
Type: CNAME
Name: www
Value: your-site.pages.dev

Cloudflare handles apex automatically
```

---

## Troubleshooting

**Issue: Pages not switching**
- Check browser console for JavaScript errors
- Ensure `show()` function is working
- Test with `?page=about` in URL

**Issue: Assessment not submitting**
- Verify SHEETS_URL is configured
- Check CORS settings on Google Apps Script
- Look for 403/404 errors in Network tab

**Issue: Slow load times**
- Enable CDN
- Check if fonts are loading efficiently
- Consider font-display: swap in CSS

**Issue: Mobile menu not working**
- Test on actual mobile device (not just resize)
- Check touch event handlers
- Verify hamburger icon displays

---

## Monitoring & Maintenance

### Uptime Monitoring
- [UptimeRobot](https://uptimerobot.com/) - Free
- [Pingdom](https://www.pingdom.com/)
- [StatusCake](https://www.statuscake.com/)

### Performance Monitoring
- Google PageSpeed Insights
- GTmetrix
- WebPageTest

### Analytics
- Weekly review of traffic sources
- Monthly assessment conversion rate tracking
- User behavior flow analysis

---

## Support

For deployment issues:
- Platform-specific support docs
- Community forums (Stack Overflow, Reddit)
- Platform-specific Slack/Discord channels

For website-specific questions:
- Open an issue in the GitHub repository
- Contact: [your support email]
