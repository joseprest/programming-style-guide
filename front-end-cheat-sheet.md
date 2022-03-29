# 📄 RedLayers Front-End Cheat Sheet

## 🤙 Rules of thumb

1. Do not re-create the wheel; any complex solution is bad, so keep it simple.
2. Do not use hacks and tricks; if you feel like you should, you lack the knowledge to do things the right way.


## 🎨 Designing

### 🔸 General

1. Do not use any UI frameworks or toolkits except for [TailwindCSS 3](https://tailwindcss.com/), [TailwindUI](https://tailwindui.com/) and [RedLayers UI](https://uikit.redlayers.com/).
1. Create a pixel-perfect version of the Figma design.
*Note that many components and pages won't be on Figma, and we'll have to design them ourselves using the same layout, fonts and colours.*
2. The Figma design represents the Desktop version of the website; it's our job to ensure it looks great on small screens and [Tailwind's 5 screens](https://tailwindcss.com/docs/theme#screens).
3. Do not ignore the first or last few pixels of a screen when making the design responsive; we must cover every pixel from 0 to 1920.
4. Do not use fixed heights or widths unless absolutely necessary and instead rely on grid and flex layouts.
5. Do not use [leading](https://tailwindcss.com/docs/line-height) unless absolutely necessary.
6. Do not forget to use [NuxtLink](https://nuxtjs.org/docs/features/nuxt-components/#the-nuxtlink-component) instead of `a links`.
7.  Use font-awesome for icons (it'll be available in your project). Download icons from Figma if you fail to find them on font-awesome.
**Do not use any other icons from the web unless they're free and don't require attribution, such as icons from [uxwing](https://uxwing.com/)**
1.  Create components for modals, pop-ups or dialogs.
2.  Also create components if you need to use the same template or code in different places or reduce code on a massive page.


### 🔸 Images

1. Export HQ images(2x or 3x) from the Figma design before using them.
2. Images must have a `srcset` and a `sizes` attribute because we use [Cloudflare](https://developers.cloudflare.com/images/image-resizing/responsive-images/#the-sizes-attribute) CDN for responsive images.
For `srcset`, we support the following sizes: 320, 480, 640, 768, 1024, 1280, 1536 and 1920.
For `sizes`, use CSS3 media queries to provide the browser with enough information to start downloading the right image: https://imagekit.io/responsive-images/#chapter-5---srcset-with-sizes
1.  Images must have the `width` and `height` attributes set to prevent [Cumlative Layout Shifts](https://web.dev/cls/). You'll have to set them manually if it's a static image; otherwise, they'll be available in an API response.
2.  Images must have the `alt` attribute for better SEO. You'll have to set it manually if it's a static image; otherwise, it'll be available in an API response.
3.  Use `data-srcset` and the `v-lazy-load` directive on images to enable lazy-loading because we use the [nuxt-lazy-load](https://www.npmjs.com/package/nuxt-lazy-load) package.
4.  Do not use the `v-lazy-load` directive on images on top of the page because we want these to load instantly. 
5.  Use the [aspect ratio](https://tailwindcss.com/docs/plugins#aspect-ratio) Tailwind plugin when you have a set width and want to limit an image's height.


## 📦 Packages and Libraries

1. Do not use any new npm package unless absolutely necessary to prevent chaos and keep everyone on the same page.
2. Do not include any package globally; only include it in the page or component you need it so it wouldn't affect the loading speed of other pages.
3. If one of your packages does not support server-side rendering, create a [Nuxtjs plugin](https://nuxtjs.org/docs/directory-structure/plugins/) that will be automatically included on the client-side: https://nuxtjs.org/docs/directory-structure/plugins/#client-or-server-side-only
4. If a plugin doesn't work for packages such as [Hammer.js](https://hammerjs.github.io/), download `hammer.min.js`, use the [head](https://nuxtjs.org/docs/components-glossary/head/) property to include it and the [callback](https://vue-meta.nuxtjs.org/api/#callback) attribute to specify a function that is called once the script loads.


## 💻 API Calls, Server-side rendering and SEO

You'll be given access to a Swagger documentation for the API similar to this one: https://petstore.swagger.io/

1. Update the UI immediately when adding or updating data without waiting for the API call's response. We'll revert our UI changes if we get a bad resposne (status code != 200). This creates a better user experience. 
2. Use [the Nuxtjs fetch hook](https://nuxtjs.org/docs/components-glossary/fetch/) to fetch data from an API call instead of using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) in `created` or `mounted` because it executes on the server-side, and we'll benefit from server-side rendering.
3. The fetch hook also executes on the client-side when navigating between pages, so do not forget to wrap your client-side code with [process.client](https://nuxtjs.org/docs/concepts/server-side-rendering#window-or-document-undefined)
4. Only use the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) to fetch data from or send data to the API **after** the page loads; otherwise, use [the Nuxtjs fetch hook](https://nuxtjs.org/docs/components-glossary/fetch/).
5. Ensure that API calls don't execute twice *(once on the server-side and once on the client-side **OR** twice on the client-side)*
6. Update the page's SEO inside [the Nuxtjs fetch hook](https://nuxtjs.org/docs/components-glossary/fetch/) and include [the essential meta tags for social media](https://css-tricks.com/essential-meta-tags-social-media/) using the [head](https://nuxtjs.org/docs/components-glossary/head/) attribute.


## ⌨ Javascript and Nuxtjs

1. Do not use Javascript for interactive animations unless necessary because Tailwind's [peer](https://tailwindcss.com/docs/hover-focus-and-other-states#styling-based-on-sibling-state), [group](https://tailwindcss.com/docs/hover-focus-and-other-states#styling-based-on-parent-state), [checked](https://tailwindcss.com/docs/hover-focus-and-other-states#checked) and [hover](https://tailwindcss.com/docs/hover-focus-and-other-states#hover) suffice.
2. `default.vue` is the default layout of every page, so it should only contain components and code that every page needs. This includes using [the fetch hook](https://nuxtjs.org/docs/components-glossary/fetch/) to get the login status of a user, for example, since we need it on every page.
3. We can set the default SEO tags in `nuxt.config.js`, but if we first need to execute some javascript code we must use the [head](https://nuxtjs.org/docs/components-glossary/head/) property in `default.vue`
4. Create another layout, such as `admin.vue` for an admin panel, instead of mixing the code with `default.vue`.
5. Use [Nuxtjs Anonymous Middlewares](https://nuxtjs.org/docs/directory-structure/middleware/#anonymous-middleware) to redirect users because they execute on the server-side as well.
```js
export default {
    name: 'GalleriesPage',
    middleware({ redirect, params, app }) {
        // the fastest way to redirect /gallery to /gallery/all on the server side based on https://nuxtjs.org/docs/concepts/nuxt-lifecycle
        if (params.type === undefined) { // type parameter is missing
            console.log('redirecting on the server side')

            redirect(app.localePath('/gallery/all'))
        }
    }
}
```


## 🙊 Multilingual Support

We use [nuxt/i18n](https://i18n.nuxtjs.org/) for multi-lingual support in some projects.

1. Do not forget to use [localpath](https://i18n.nuxtjs.org/basic-usage/#nuxt-link) in all NuxtLinks.
2. Check [$i18n.locale](https://i18n.nuxtjs.org/api#i18n) to print the text in the correct language.
```js
<h2>
    {{
        $i18n.locale === 'en'
            ? group.name
            : $i18n.locale in
                  group.alt_names &&
              group.alt_names[
                  $i18n.locale
              ] !== null
            ? group.alt_names[
                  $i18n.locale
              ]
            : group.name
    }}
</h2>
```