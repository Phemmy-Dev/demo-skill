# Apps with login or sign-up

Read this in full before recording any app that has a login, sign-up, or role-based screens. Proof you followed it: `work/demo-accounts.md` exists and the plan has a "Test accounts and data" section.

Most real apps hide their best screens behind a login. Get past it safely, with test accounts on a local copy, never with real ones:

1. **Run it locally against test data.** Use the project's own local or dev database (SQLite file, local Postgres/Mongo, Docker compose, emulators). If the backend is hosted (Supabase, Firebase, a live API) and there's no local or staging setup, stop and ask the user for a separate test project, a staging URL, or a test account they create. Never record against production or real member/customer data.
2. **Get test accounts, in this order:** demo users from the project's seed, fixture or example-config files; accounts created through the app's own sign-up page on the local copy with fictional details (e.g. "Grace Adeyemi", `grace.demo@example.test`); or accounts the user creates and types into chat for this. Generate the passwords yourself. Write every account to `work/demo-accounts.md` (gitignored with the rest of `demo-output*/`); never show passwords on screen or repeat them in chat.
3. **One account per role.** If the app has roles (admin and member, teacher and student, seller and buyer), make one account for each and log each into its own Playwright browser context. The strongest shots often show both at once: the member acts on one side, the admin's view updates on the other.
4. **Seed fictional data.** Empty dashboards look broken. Use the project's seed script if it has one; otherwise create realistic fictional records through the app's own UI or local API (members, events, attendance history, orders) so every screen looks lived-in. Never copy real records. List what you created in the plan.
5. **Log in once, record many times.** After the first login, save the session with `context.storageState({ path: 'work/auth-admin.json' })` and start every recording context from it, so each beat opens already signed in and no credentials appear in the footage.
6. **Show login only if it earns its place.** A 2-3s "sign up in seconds" shot can be a beat; otherwise skip straight to the product. If you show typing in a password field, it must show masked dots only.
7. **What you don't get past alone:**
   - Google, Apple, GitHub or other third-party sign-in: open a visible browser window and ask the user to sign in themselves to a test account, then save that session with `storageState`. Never type into a provider's sign-in page.
   - Email codes, OTP, magic links: use the project's dev bypass or local mail catcher (Mailpit, MailHog, Supabase Inbucket, the dev server log) if one exists; otherwise ask the user for the code.
   - CAPTCHA or bot checks: never solve them. Ask the user to turn them off for local dev or to get past it themselves.
   - Payments behind the login: only a provider's test mode with its published test cards, on the local copy.

When done, tell the user which test accounts and data you created and where, so they can delete them.
