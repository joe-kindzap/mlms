# Marker.io LMS Widget Generator

A simple web tool to generate Marker.io feedback widget code for Learning Management Systems.

## Supported LMS Platforms

- **Moodle** (3.9+)
- **Canvas LMS**

## How to Use

1. Visit the generator page
2. Select your LMS (Moodle or Canvas)
3. Enter your Marker.io Project ID
4. Configure targeting options (optional)
5. Copy the generated code
6. Follow the installation instructions for your LMS

## Features

- Automatic user detection (name, email, role)
- Course context capture
- Role-based targeting (show/hide for specific roles)
- Page-type targeting (hide on login, admin pages)
- Course exclusions
- Debug mode for testing

## Hosting on GitHub Pages

To host this generator:

1. Create a new GitHub repository
2. Copy `index.html` to the repository root
3. Go to Settings > Pages
4. Set Source to "Deploy from a branch"
5. Select the `main` branch and `/ (root)` folder
6. Save - your site will be live at `https://username.github.io/repo-name/`

## Local Testing

Open `index.html` directly in a browser - no server required.

## Security Notes

- Generated code runs entirely client-side
- No data is sent to any server
- Project IDs are validated for format only
- All code is human-readable (not minified)

## License

MIT
