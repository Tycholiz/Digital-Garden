
## Website vs Webapp
Web sites are documents; they are content‐centric. Sites are geared towards content consumption.
Web apps are tools; they are behaviour‐centric. Apps are geared towards content creation and manipulation.

It can be argued that there is a false dichotomy between a web app and website, and that it is really more of a continuum
- a site, built out of static documents, connected via links, is on the left end and a pure behaviour driven, contentless application like an online photo editor is on the right.

If you would position your project on the left side of this spectrum, an integration on webserver level is a good fit. With this model a server collects and concatenates HTML strings from all components that make up the page requested by the user. Updates are done by reloading the page from the server or replacing parts of it via ajax.

When your user interface has to provide instant feedback, even on unreliable connections, a pure server rendered site is not sufficient anymore. To implement techniques like Optimistic UI or Skeleton Screens you need to be able to also update your UI on the device itself.

Of course, some are firmly in one camp or the other:
- ex.  a simple online brochure for a university programme that contains nothing but text, images, and hyperlinks is clearly a document. It is clearly a site. An HTML‐based Photoshop clone, on the other hand, is clearly an app — it has no inherent content of its own. Instead, it is used to create and manipulate content.
