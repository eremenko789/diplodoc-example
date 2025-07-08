# PDF Generation and Publication Setup

This document explains how to set up and use the PDF generation workflow for your Diplodoc documentation.

## Overview

The PDF generation workflow automatically converts your Diplodoc documentation into PDF format using `wkhtmltopdf` after building HTML with the `diplodoc-platform/docs-build-action`. The workflow provides multiple publication options:

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
- Converts HTML to PDF using `wkhtmltopdf` with professional formatting
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

You can customize the PDF generation by modifying the `wkhtmltopdf` command options in the workflow's Python script:

```python
cmd = [
    'wkhtmltopdf',
    '--page-size', 'A4',
    '--margin-top', '20mm',
    '--margin-right', '15mm',
    '--margin-bottom', '20mm',
    '--margin-left', '15mm',
    '--print-media-type',
    '--enable-local-file-access',
    '--header-center', 'Documentation',
    '--header-font-size', '10',
    '--footer-center', 'Page [page] of [toPage]',
    '--footer-font-size', '10',
    html_file,
    pdf_path
]
```

### Available Options

- `--page-size`: Paper size (A4, A3, Letter, Legal, etc.)
- `--margin-*`: Page margins (top, right, bottom, left)
- `--header-*`: Header configuration and content
- `--footer-*`: Footer configuration and content
- `--print-media-type`: Use print CSS media type
- `--enable-local-file-access`: Allow access to local files
- `--orientation`: Portrait or Landscape
- `--zoom`: Scale factor for the content

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

You can customize the PDF appearance by modifying the wkhtmltopdf command options in the workflow:

```python
# Custom header and footer examples
cmd = [
    'wkhtmltopdf',
    '--page-size', 'A4',
    '--header-left', 'My Documentation',
    '--header-right', 'Version 1.0',
    '--header-font-size', '12',
    '--footer-left', 'Generated on [date]',
    '--footer-right', 'Page [page] of [toPage]',
    '--footer-font-size', '10',
    html_file,
    pdf_path
]
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

You can extend the workflow to generate different PDF formats by modifying the Python script:

```python
# Generate multiple formats
formats = [
    {'size': 'A4', 'dir': 'a4'},
    {'size': 'Letter', 'dir': 'letter'},
    {'size': 'A3', 'dir': 'a3'}
]

for fmt in formats:
    output_subdir = os.path.join(output_dir, fmt['dir'])
    os.makedirs(output_subdir, exist_ok=True)
    
    cmd = [
        'wkhtmltopdf',
        '--page-size', fmt['size'],
        '--margin-top', '20mm',
        # ... other options
        html_file,
        os.path.join(output_subdir, pdf_name)
    ]
    subprocess.run(cmd, check=True)
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
- **PDF Generation**: Check the [wkhtmltopdf documentation](https://wkhtmltopdf.org/usage/wkhtmltopdf.txt) or [GitHub issues](https://github.com/wkhtmltopdf/wkhtmltopdf/issues)
- **Documentation Building**: Check the [diplodoc-platform/docs-build-action](https://github.com/diplodoc-platform/docs-build-action) repository
- **This Workflow**: Open an issue in this repository

## License

This workflow configuration is provided under the same license as your documentation project.