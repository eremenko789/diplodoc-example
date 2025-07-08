# PDF Generation - Quick Start Guide

## 🚀 Getting Started

The PDF generation workflow is now set up and ready to use! Here's how to get started:

### 1. Enable GitHub Pages (One-time setup)

1. Go to your repository **Settings** → **Pages**
2. Set **Source** to "Deploy from a branch"
3. Choose **gh-pages** branch
4. Click **Save**

### 2. Trigger PDF Generation

The workflow runs automatically when you:
- Push changes to the `main` branch that affect `docs/**`
- Create a pull request with documentation changes
- Create a new release

**Manual trigger:**
1. Go to **Actions** tab
2. Click **"Generate and Publish PDF"**
3. Click **"Run workflow"**

### 3. Access Your PDFs

**📄 Download from Artifacts:**
- Go to **Actions** tab → Click on a workflow run → Download artifacts

**🌐 View on GitHub Pages:**
- Visit: `https://[your-username].github.io/[repository-name]/pdf/`

**📦 From Releases:**
- Go to **Releases** → PDFs are automatically attached as assets

## 🎯 What You Get

✅ **Professional PDFs** with headers, footers, and page numbers  
✅ **Multi-language support** (English & Russian)  
✅ **Automatic publication** to GitHub Pages  
✅ **Release integration** - PDFs attached to releases  
✅ **30-day artifact retention** for downloads  

## 📋 File Structure

Your documentation should be organized like this:

```
docs/
├── .yfm                 # Language configuration
├── en/                  # English documentation
│   ├── index.yaml
│   ├── toc.yaml
│   └── *.md files
└── ru/                  # Russian documentation
    ├── index.yaml
    ├── toc.yaml
    └── *.md files
```

## 🔧 Customization

To customize PDF formatting, edit `.github/workflows/pdf-generation.yml`:

```yaml
config: |
  {
    "format": "A4",           # Paper size
    "margin": {
      "top": "20mm",
      "right": "15mm",
      "bottom": "20mm",
      "left": "15mm"
    },
    "displayHeaderFooter": true,
    "headerTemplate": "<div style='font-size: 10px; margin: 0 auto;'>Your Custom Header</div>",
    "footerTemplate": "<div style='font-size: 10px; margin: 0 auto;'><span class='pageNumber'></span> / <span class='totalPages'></span></div>"
  }
```

## 🆘 Quick Troubleshooting

**PDF generation fails?**
- Check that your docs build locally with `npm start`
- Verify all markdown files are properly formatted
- Review workflow logs in Actions tab

**No PDFs generated?**
- Ensure changes are in `docs/**` directory
- Check that `.yfm` configuration is correct
- Verify workflow triggered (check Actions tab)

**GitHub Pages not working?**
- Confirm GitHub Pages is enabled in Settings
- Check that `gh-pages` branch exists
- Wait a few minutes for deployment

## 📚 Full Documentation

For detailed configuration options and advanced usage, see [PDF Generation Setup](pdf-generation-setup.md).

## 🎉 That's It!

Your PDF generation workflow is ready to use. Every time you update your documentation, beautiful PDFs will be automatically generated and published!

---

**Need help?** Check the [full documentation](pdf-generation-setup.md) or open an issue in this repository.