nextjs常见问题记录

1. nextjs export 静态页面时避免出现404

   修改next.config.js。添加trailingSlash: true

2. you can specify a name to use for a custom build directory to use instead of `.next`. Open `next.config.js` and add the `distDir` 

   config: `distDir: 'build'`.

3. Since Next.js supports this static export, it can be deployed and hosted on any web server that can serve HTML/CSS/JS static assets.

   config: `output: 'export'`.

4. since config `output: 'export'`, this is because Next.js optimizes images on-demand, as users request them (not at build time). need to nextjs 

   config: 

   ```
   module.exports = {
     images: {
       unoptimized: true,
     },
   }
   ```

5. To protect your application from malicious users, configuration is required in order to use external images.

      ```
      module.exports = {
        images: {
          remotePatterns: [
            {
              protocol: 'https',
              hostname: 'example.com',
              port: '',
              pathname: '/account123/**',
            },
          ],
        },
      }
      ```

6. **Tailwindcss** : The `important` option lets you control whether or not Tailwind's utilities should be marked with `!important`. This can be really useful when using Tailwind with existing CSS that has high specificity selectors.

   To generate utilities as `!important`, set the `important` key in your configuration options to `true`:

   ```
   /** @type {import('tailwindcss').Config} */
   module.exports = {
     important: true,
   }
   ```

7. 