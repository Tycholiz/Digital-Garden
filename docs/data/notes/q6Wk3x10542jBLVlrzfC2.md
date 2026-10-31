
Consider that we can style text when it is in the HTML, such as bolding it, italicizing it, etc. But how does this work with dynamic text? This is not something we can build into our front-end code, because it changes depending on the data
- ex. Blog posts have their own italicizing, bolding, paragraph breaks etc. built in. That "formatting" is coupled with the text itself. Therefore, it must be stored somehow. Sanity accomplishes this by storing content as Rich Text

When you query your Sanity project’s API your Rich Text content is returned as Portable Text
- Other Headless CMSs may store the data in html or markdown.
