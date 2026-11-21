
Jekyll is a static site generator, a la [[Nextjs|nextjs]] or Gatsby, but built in the Ruby ecosystem.

We can add a `.nojekyll` file to the root of the `pages` branch of our repo as a way to inform Github Pages that our static site is not a Jekyll site.
- This should only be necessary if your site uses files or directories that start with underscores since Jekyll considers these to be special resources and does not copy them to the final site.