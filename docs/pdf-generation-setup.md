# PDF Generation and Publication Setup

This document explains how to set up and use the PDF generation workflow for your Diplodoc documentation.

## Overview

The PDF generation workflow automatically converts your Diplodoc documentation into PDF format using the `diplodoc-platform/pdf-generator` action. The workflow provides multiple publication options:

- **Artifacts**: PDFs are uploaded as GitHub workflow artifacts
- **GitHub Pages**: PDFs are published to a dedicated `/pdf` subdirectory
- **Release Assets**: PDFs are automatically attached to GitHub releases

## Workflow Triggers

The PDF generation workflow is triggered by:

1. **Push to main branch**: When documentation changes are pushed to the `main` branch
2. **Pull requests**: When PRs contain changes to the `docs/**` directory
3. **Manual trigger**: Using the "Run workflow" button in GitHub Actions
4. **Release events**: When a new release is published

## Features

### 1. Automated PDF Generation

The workflow:
- Builds HTML documentation using `diplodoc-platform/docs-build-action`
- Converts HTML to PDF using `diplodoc-platform/pdf-generator`
- Applies professional formatting with headers, footers, and page numbering
- Supports multi-language documentation (English and Russian)

### 2. Multiple Publication Methods

**Workflow Artifacts**
- All generated PDFs are uploaded as workflow artifacts
- Artifacts are retained for 30 days
- Available for download from the Actions tab

**GitHub Pages**
- PDFs are automatically published to GitHub Pages
- Accessible at: `https://[username].github.io/[repository-name]/pdf/`
- Includes a user-friendly index page for easy navigation

**Release Assets**
- PDFs are automatically attached to GitHub releases
- Perfect for version-specific documentation distribution

### 3. Professional PDF Formatting

The generated PDFs include:
- A4 format with proper margins
- Header with document title
- Footer with page numbers
- Professional styling that matches your documentation theme
- Print-friendly background and colors

## Configuration

### PDF Generation Options

You can customize the PDF generation by modifying the `config` section in the workflow:

```yaml
config: |
  {
    "format": "A4",
    "margin": {
      "top": "20mm",
      "right": "15mm", 
      "bottom": "20mm",
      "left": "15mm"
    },
    "displayHeaderFooter": true,
    "headerTemplate": "<div style='font-size: 10px; margin: 0 auto;'>Documentation</div>",
    "footerTemplate": "<div style='font-size: 10px; margin: 0 auto;'><span class='pageNumber'></span> / <span class='totalPages'></span></div>",
    "printBackground": true
  }
```

### Available Options

- `format`: Paper size (A4, A3, Letter, Legal, etc.)
- `margin`: Page margins (top, right, bottom, left)
- `displayHeaderFooter`: Show/hide headers and footers
- `headerTemplate`: Custom header HTML template
- `footerTemplate`: Custom footer HTML template
- `printBackground`: Include background colors and images
- `landscape`: Portrait or landscape orientation
- `scale`: Scale factor for the content

## GitHub Pages Setup

To enable GitHub Pages for PDF publication:

1. Go to your repository settings
2. Navigate to "Pages" in the left sidebar
3. Under "Source", select "Deploy from a branch"
4. Choose "gh-pages" as the branch
5. Select "/ (root)" as the folder
6. Click "Save"

Your PDFs will be available at: `https://[username].github.io/[repository-name]/pdf/`

## Usage Examples

### Manual Workflow Trigger

1. Go to the "Actions" tab in your repository
2. Click on "Generate and Publish PDF"
3. Click "Run workflow"
4. Select the branch and click "Run workflow"

### Accessing Generated PDFs

**From Workflow Artifacts:**
1. Go to the "Actions" tab
2. Click on a completed workflow run
3. Scroll down to "Artifacts"
4. Download the PDF artifact

**From GitHub Pages:**
- Visit: `https://[username].github.io/[repository-name]/pdf/`
- Click on any PDF to view or download

**From Releases:**
1. Go to the "Releases" section
2. Click on a release
3. Download PDFs from the "Assets" section

## Multi-Language Support

The workflow automatically detects and generates PDFs for all languages configured in your `.yfm` file:

```yaml
langs: ['ru', 'en']
```

Each language will generate a separate PDF:
- `documentation-en.pdf`
- `documentation-ru.pdf`

## Troubleshooting

### Common Issues

**PDF Generation Fails**
- Check that your documentation builds successfully first
- Verify that all markdown files are properly formatted
- Ensure images and assets are accessible

**No PDFs Generated**
- Verify that the `docs/**` path trigger is correctly set
- Check that your documentation structure matches the expected format
- Review the workflow logs for specific error messages

**GitHub Pages Not Working**
- Ensure GitHub Pages is enabled in repository settings
- Check that the `gh-pages` branch is created and has content
- Verify repository permissions allow GitHub Pages

### Debugging Steps

1. **Check Workflow Logs**
   - Go to Actions tab → Select failed workflow → Review logs

2. **Verify Documentation Structure**
   - Ensure `docs/` directory exists
   - Check that `.yfm` configuration is correct
   - Verify all referenced files exist

3. **Test Local Build**
   - Run `npm start` locally to test documentation build
   - Fix any build errors before running the workflow

## Advanced Configuration

### Custom PDF Styling

You can customize the PDF appearance by modifying the header and footer templates:

```yaml
headerTemplate: |
  <div style='font-size: 12px; width: 100%; text-align: center; color: #666;'>
    <span style='float: left;'>My Documentation</span>
    <span style='float: right;'>Version 1.0</span>
  </div>
```

### Conditional Triggers

To run PDF generation only for specific branches or paths:

```yaml
on:
  push:
    branches:
      - main
      - release/*
    paths:
      - 'docs/**'
      - 'content/**'
```

### Multiple Output Formats

You can extend the workflow to generate different PDF formats:

```yaml
- name: Generate PDF - A4 Format
  uses: diplodoc-platform/pdf-generator@v1
  with:
    input-dir: "./build"
    output-dir: "./pdf-output/a4"
    config: '{"format": "A4"}'

- name: Generate PDF - Letter Format  
  uses: diplodoc-platform/pdf-generator@v1
  with:
    input-dir: "./build"
    output-dir: "./pdf-output/letter"
    config: '{"format": "Letter"}'
```

## Best Practices

1. **Keep Documentation Updated**: Regularly update your documentation to ensure PDFs reflect the latest information

2. **Test Locally**: Always test your documentation build locally before pushing changes

3. **Use Meaningful Commit Messages**: This helps identify which changes triggered PDF regeneration

4. **Monitor Workflow Status**: Check the Actions tab to ensure workflows complete successfully

5. **Optimize Images**: Use optimized images to reduce PDF file size

6. **Version Control**: Tag releases to create version-specific PDF documentation

## Support

For issues with:
- **PDF Generation**: Check the [diplodoc-platform/pdf-generator](https://github.com/diplodoc-platform/pdf-generator) repository
- **Documentation Building**: Check the [diplodoc-platform/docs-build-action](https://github.com/diplodoc-platform/docs-build-action) repository
- **This Workflow**: Open an issue in this repository

## License

This workflow configuration is provided under the same license as your documentation project.