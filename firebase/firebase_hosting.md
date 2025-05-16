# Firebase Hosting

Firebase Hosting is production-grade web content hosting for developers, providing fast and secure hosting for web applications. With a single command, you can quickly deploy web apps to a global CDN (content delivery network).

## Overview

Firebase Hosting is optimized for static and single-page web apps, but can also be paired with Cloud Functions or Cloud Run to build and host dynamic content and microservices.

## Key Capabilities

### Secure Content Delivery
- Zero-configuration SSL is built into Firebase Hosting
- All content is always delivered securely
- SSL certificates are automatically provisioned, even for custom domains

### High-Performance CDN
- Files are cached on SSDs at CDN edges around the world
- Content is served with gzip or Brotli compression (automatically selected)
- Optimized global content delivery regardless of user location

### Development and Testing Workflow
- View and test changes on a locally hosted URL
- Emulate backend resources using Firebase Local Emulator Suite
- Share changes with teammates using temporary preview URLs
- GitHub integration for easy iteration during development

### Simple Deployment Process
- Deploy with one command using the Firebase CLI
- Integrate deployment into your build process
- One-click rollbacks if you need to undo a deploy

## How It Works

Firebase Hosting provides the infrastructure, features, and tooling needed for deploying and managing websites and apps:

1. **Deployment**: Using the Firebase CLI, you deploy files from local directories on your computer to Firebase Hosting servers

2. **Dynamic Content**: Beyond static files, you can use Cloud Functions or Cloud Run to serve dynamic content and host microservices

3. **Content Delivery**: All content is served over SSL from the closest edge server on the global CDN

4. **Testing and Preview**: You can test locally with the Firebase Local Emulator Suite and share changes with preview URLs

5. **Configuration Options**: Lightweight configuration options allow you to:
   - Rewrite URLs for client-side routing
   - Set up custom headers
   - Serve localized content

## Domain Options

Firebase offers several domain and subdomain options:

- **Default Subdomains**: Every Firebase project has free subdomains on `web.app` and `firebaseapp.com`
- **Multiple Sites**: Create multiple sites sharing the same Firebase project resources
- **Custom Domains**: Connect your own domain name to a Firebase-hosted site

## Implementation Path

1. **Install the Firebase CLI**
   - The CLI makes it easy to set up projects, run a local server, and deploy content

2. **Set Up a Project Directory**
   - Add static assets to a local directory
   - Run `firebase init` to connect the directory to a Firebase project
   - Configure Cloud Functions or Cloud Run for dynamic content (optional)

3. **Test and Preview Changes**
   - Run `firebase emulators:start` to test at a locally hosted URL
   - Use `firebase hosting:channel:deploy` to share changes at a temporary preview URL
   - Set up GitHub integration for collaborative preview workflows

4. **Deploy Your Site**
   - Run `firebase deploy` to upload your content to Firebase servers
   - Use one-click rollback if needed

5. **Link to a Firebase Web App (Optional)**
   - Connect your site to a Firebase Web App
   - Use Google Analytics to collect usage data
   - Implement Firebase Performance Monitoring

## Use Cases

Firebase Hosting is ideal for:

- Static websites and landing pages
- Progressive Web Apps (PWAs)
- Single-page applications (SPAs)
- Web applications with serverless backends
- Preview and staging environments for web projects

## Best Practices

- Use the emulator for local development
- Implement CI/CD with the GitHub integration
- Set up caching and headers for optimal performance
- Use URL rewriting for SPAs with client-side routing
- Leverage preview channels for testing before deployment

## Next Steps

- Get started with Firebase Hosting
- Set up a local development workflow
- Integrate with CI/CD pipelines
- Build and host microservices using Cloud Functions or Cloud Run