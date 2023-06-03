
> To deploy your app, you may need to install an [adapter](https://kit.svelte.dev/docs/adapters) for your target environment.

# To deploy in vercel
```npm i -D @sveltejs/adapter-vercel```

## in svelte.config.js
```
import adapter from '@sveltejs/adapter-vercel';
import { vitePreprocess } from '@sveltejs/kit/vite';
/** @type {import('@sveltejs/kit').Config} */
const config = {
  kit: {
    adapter: adapter(
      {runtime: "nodejs18.x"}
    )
  },
  preprocess: vitePreprocess()
};
export default config;
```


# add to +page.svelte or +page.js

```
export const config = {
    runtime: 'edge'
};
```
