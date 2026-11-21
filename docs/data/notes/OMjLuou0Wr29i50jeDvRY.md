
```ts
import imageUrlBuilder from '@sanity/image-url'

function urlFor (source: SanityImageSource) {
  return imageUrlBuilder(client).image(source)
}

return (
  {authorImage && (
    <div>
      <img
        src={urlFor(authorImage)
          .width(50)
          .url()}
      />
    </div>
  )}
)
```
