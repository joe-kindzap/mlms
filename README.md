# Marker.io LMS Widget Generator

> **Proof of Concept** — This tool is in early development and intended for testing and evaluation purposes only. Not recommended for production use yet.

A web-based code generator that creates Marker.io feedback widget integration code for Learning Management Systems. The generator produces ready-to-use JavaScript snippets that LMS administrators can install without any coding knowledge.

## Live Demo

**https://joe-kindzap.github.io/mlms/**

## The Problem

Integrating Marker.io into an LMS typically requires:
- Custom development work for each LMS platform
- Understanding of LMS-specific APIs and DOM structures
- Manual configuration of user context extraction
- Security considerations around targeting and privacy

This generator solves these problems by providing a visual interface that outputs production-ready code tailored to each LMS platform.

## Supported LMS Platforms

| Platform | Version | Status |
|----------|---------|--------|
| **Moodle** | 3.9+ | Fully supported |
| **Canvas LMS** | All versions | Fully supported |
| Blackboard | — | Planned |
| Brightspace (D2L) | — | Planned |
| Schoology | — | Planned |

## Key Features

### Automatic User Context Extraction

The generated code automatically captures:

- **User identity** — Name and email (when available)
- **User role** — Student, Teacher, Administrator, Guest
- **Course context** — Course name and ID
- **Page context** — Activity type, section, page layout
- **LMS metadata** — Version, theme, language

This context is attached to every feedback submission, giving support teams full visibility into where issues occur.

### Security-First Targeting

Two targeting modes are available:

| Mode | Description | Use Case |
|------|-------------|----------|
| **Restrictive** (default) | Widget only appears on explicitly allowed pages/roles | Production deployments, privacy-conscious institutions |
| **Permissive** | Widget appears everywhere except excluded pages/roles | Broad rollouts, testing phases |

Restrictive mode follows the principle of least privilege — the widget is hidden by default and only shown where explicitly configured.

### Granular Controls

**Page targeting:**
- Course pages
- Activity pages (quizzes, assignments, forums)
- Dashboard/Home page
- Admin/Settings pages

**Role targeting:**
- Students
- Teachers/Instructors
- Administrators
- Guests

**Course filtering:**
- Show on all courses
- Limit to specific courses (for pilots)

### Privacy Options

- **Auto-fill toggle** — Enable or disable automatic population of reporter name/email
- Institutions with strict privacy policies can disable auto-fill to require explicit user consent

### Debug Mode

Enable console logging to troubleshoot targeting rules:

```
[Marker.io] Role: Teacher
[Marker.io] Page: course
[Marker.io] Visible: all checks passed
```

## How It Works

### For Moodle

The generated code uses Moodle's built-in DOM structure:

1. **Role detection** via body classes (`role-admin`, `role-teacher`, `role-student`)
2. **Page type detection** via path classes (`path-course`, `path-mod-quiz`, `pagelayout-admin`)
3. **User info extraction** from `.usertext` and `mailto:` links
4. **Course context** from breadcrumb navigation
5. **Moodle config** from the global `M.cfg` object (version, theme, language)

Installation location: **Site administration → Appearance → Additional HTML → Before BODY is closed**

### For Canvas

The generated code uses Canvas's `ENV` JavaScript object:

1. **Role detection** from `ENV.current_user_roles` and `ENV.current_user_is_admin`
2. **User info** from `ENV.current_user`
3. **Course context** from `ENV.COURSE`
4. **Page type detection** from URL path patterns

Installation location: **Admin → Settings → Themes → JavaScript/CSS**

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Generator (Static HTML)                   │
│                                                             │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐ │
│  │ LMS Select  │  │ Project ID  │  │ Targeting Options   │ │
│  └─────────────┘  └─────────────┘  └─────────────────────┘ │
│                           │                                 │
│                           ▼                                 │
│              ┌─────────────────────────┐                   │
│              │   Code Generation       │                   │
│              │   (Client-side JS)      │                   │
│              └─────────────────────────┘                   │
│                           │                                 │
│                           ▼                                 │
│              ┌─────────────────────────┐                   │
│              │   Output: Ready-to-use  │                   │
│              │   LMS-specific code     │                   │
│              └─────────────────────────┘                   │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼ Copy/Download
┌─────────────────────────────────────────────────────────────┐
│                         LMS                                  │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  Generated Code                                      │   │
│  │  ├── Targeting logic (shouldShow function)          │   │
│  │  ├── User context extraction                        │   │
│  │  ├── markerConfig setup                             │   │
│  │  └── Marker.io shim.js loader                       │   │
│  └─────────────────────────────────────────────────────┘   │
│                           │                                 │
│                           ▼                                 │
│              ┌─────────────────────────┐                   │
│              │   Marker.io Widget      │                   │
│              │   (loaded from CDN)     │                   │
│              └─────────────────────────┘                   │
└─────────────────────────────────────────────────────────────┘
```

## Security Considerations

### What the generated code does

1. Checks targeting rules (role, page type, course)
2. Extracts user context from the LMS DOM/JavaScript
3. Configures `window.markerConfig` with project ID and context
4. Loads the official Marker.io widget from `edge.marker.io`

### What it does NOT do

- Send data to any server other than Marker.io
- Store any data locally
- Modify LMS functionality
- Access APIs or make authenticated requests

### Recommendations

1. **Configure domain allowlist** in Marker.io dashboard to restrict which domains can use your project ID
2. **Use restrictive mode** to limit widget visibility
3. **Disable auto-fill** if institution privacy policies require explicit consent
4. **Test with debug mode** before deploying to production

## Local Development

No build process required. Open `index.html` directly in a browser:

```bash
# Clone the repository
git clone https://github.com/joe-kindzap/mlms.git
cd mlms

# Open in browser
open index.html
# or
xdg-open index.html
# or simply drag index.html to your browser
```

## Testing the Generated Code

### Moodle Sandbox

1. Visit https://sandbox.moodledemo.net
2. Login as admin (admin / sandbox)
3. Go to Site administration → Appearance → Additional HTML
4. Paste generated code in "Before BODY is closed"
5. Save and navigate to any course page
6. Verify the red feedback button appears

### Canvas Free Account

1. Create a free Canvas account at https://canvas.instructure.com
2. Go to Account → Settings → Themes
3. Add custom JavaScript
4. Test on course pages

## Roadmap

- [ ] Blackboard Learn support
- [ ] Brightspace (D2L) support
- [ ] Schoology support
- [ ] Custom CSS injection for widget styling
- [ ] Multiple project ID support (different widgets for different contexts)
- [ ] Import/export configuration profiles

## Contributing

This is a proof of concept. Feedback and suggestions welcome via GitHub issues.

## License

MIT

---

**Note**: This is a community-developed tool, not an official Marker.io product.
