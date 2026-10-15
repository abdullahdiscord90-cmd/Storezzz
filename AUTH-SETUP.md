# DAXORA MC — activate authentication

Integration code is included. Authentication is NOT active until these dashboard steps are completed. No project, provider configuration or live login was created/tested on your behalf.

## 1. Create Supabase project
Create a project at https://supabase.com/dashboard . In project settings find the Project URL and publishable key (legacy anon key also works). Put these public values into `auth-config.js`.
Never use a service_role key, secret key, database password, Discord client secret or Google client secret in website files.

## 2. Host the files on HTTPS
Upload index.html, coins.html, ranks.html, reset-password.html and auth-config.js together to your static hosting. Set `redirectUrl` in auth-config.js to your real HTTPS homepage, for example https://your-domain.example/index.html (replace this example).
In Supabase Authentication > URL Configuration:
- Site URL: your actual HTTPS homepage.
- Allowed Redirect URLs: exact homepage and exact reset-password.html URL.
The temporary preview is not permanent hosting. File previews may block external scripts/network requests; test using the real hosted URL.

## 3. Email login
Enable the Email provider. Keep email confirmation enabled. Configure production SMTP, password policy and appropriate auth rate limits. Development email sending can have limits. Test sign-up with a real email you control, confirm email, sign in, sign out and password recovery. PKCE email flows should be completed in the same browser that initiated them.

## 4. Discord OAuth (removed from current login UI; not required)
Create an app in Discord Developer Portal (https://discord.com/developers/applications). Under OAuth2, add the callback URL shown in Supabase's Discord provider settings (normally https://YOUR-PROJECT.supabase.co/auth/v1/callback).
Copy Discord Client ID and Client Secret into Supabase Authentication > Providers > Discord. Enable the provider. Keep the client secret only in the provider dashboard, never auth-config.js.
Discord invite https://discord.gg/NqP9DcPP7 is already configured for purchases; an invite is not an OAuth application.

## 5. Google OAuth
In Google Cloud configure consent screen and OAuth web application. Add the callback URL displayed by Supabase. Put Client ID and Client Secret in the Supabase Google provider settings and enable it. During testing, add allowed test users or publish the consent configuration as appropriate.

## 6. Test before launch
- Missing configuration produces an honest setup message, never a fake successful login.
- Test invalid password, duplicate email, unconfirmed email and network failure.
- Test Google consent accept/cancel paths and successful return.
- Check session across all store pages and logout.
- Check password reset in the originating browser.
- Authentication does not grant Minecraft ranks or deliver coins. Purchases still direct users to Discord.
- Any future order/profile tables need Row Level Security and server-side authorization. Never rely on browser UI to protect purchases/admin operations.

The website uses the pinned Supabase JS CDN SDK. Network access is required. For production, consider bundling/self-hosting the SDK and adding a Content Security Policy after refactoring inline scripts. Session persistence uses browser storage; avoid untrusted scripts on the site. No plaintext passwords are saved by these website files.
