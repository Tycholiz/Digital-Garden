
JAM: JavaScript, APIs, and markup
- JavaScript: any dynamic programming during the request/response cycle is handled by JS, running entirely on the client. This could be any frontend framework or library, or even vanilla JavaScript.
- APIs: all server-side processes or database actions are abstracted into reusable APIs, accessed over HTTPS with JavaScript. These can be custom-built or leverage third-party services.
- Markup: templated markup should be prebuilt at deploy time, usually using a site generator for content sites, or a build tool for web apps.

![](/assets/images/2021-03-20-18-45-27.png)

## Examples
### Site Generators
Next.js, Gatsby, Jekyll

### Headless CMS
Netflify

## Architecture
Purpose is to make the web faster, more secure, and easier to scale
- core principles are pre-rendering, and decoupling
    - pre-render - to generate the markup which represents a view (front-end) in advance of when it is required. This happens during a build rather than on-demand so that web servers do not need to perform this activity for each request recieved.
- entire front end is prebuilt into highly optimized static pages and assets during a build process. This process of pre-rendering results in sites which can be served directly from a CDN. Since the CDN only has to serve already-rendered markup, it can be delivered very quickly and securely
    - On this foundation, Jamstack sites can use JavaScript and APIs to talk to backend services, allowing experiences to be enhanced and personalized.
- The Jamstack philosophy is to be modular and have a strong capacity to be able to hook into various 3rd party services/APIs
- Jamstack distinguishes between front-end and back-end and allows for efficient front-end development that is independent of any back-end APIs.

## Benefits
### Security
- Jamstack naturally results in less moving parts, meaning there are naturally less surfaces to have to protect against.
- Since CDNs only serve pre-generated files, meaning we can use read-only hosting, further reducing the damage that a third party can do.

### Scaling
since everything is pre-generated, the CDN can cache the whole site.

### Performance
Jamstack sites remove the need to generate page views on a server at request time by instead generating pages ahead of time during a build.

### Maintenance
Jamstack apps are easier to maintain, as they only need to be served directly from a simple host (or CDN)
- The work was done during the build, so now the generated site is stable and can be hosted without servers which might require patching, updating and maintain.

## Approaches
#### Thinking from CDN perspective
Because Jamstack projects don’t rely on server-side code, they can be distributed instead of living on a single server. Serving directly from a CDN unlocks speeds and performance that can’t be beat. The more of your app you can push to the edge, the better the user experience.

#### Automating builds
Because Jamstack markup is prebuilt, content changes won’t go live until you run another build. Automating this process will save you lots of frustration. You can do this yourself with webhooks, or use a publishing platform that includes the service automatically.

#### Atomic Deploys
As Jamstack projects grow really large, new changes might require re-deploying hundreds of files. Uploading these one at a time can cause inconsistent state while the process completes. You can avoid this with a system that lets you do “atomic deploys,” where no changes go live until all changed files have been uploaded.

#### Instant Cache Invalidation
When the build-to-deploy cycle becomes a regular occurrence, you need to know that when a deploy goes live, it really goes live. Eliminate any doubt by making sure your CDN can handle instant cache purges.

#### Everything Lives in Git
With a Jamstack project, anyone should be able to do a git clone, install any needed dependencies with a standard procedure (like npm install), and be ready to run the full project locally. No databases to clone, no complex installs. This reduces contributor friction, and also simplifies staging and testing workflows.
