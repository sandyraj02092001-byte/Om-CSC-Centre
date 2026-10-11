# Om CSC Centre — website starter

A bilingual (English/Hindi), mobile-responsive website for Om CSC Centre, Darbhanga, Bihar.

## Included

- Responsive public website
- English/Hindi toggle
- Service cards and search
- Basic enquiry form
- Supabase database integration for saved enquiries
- Supabase Auth email/password admin login
- Admin dashboard for viewing requests and updating request status
- Admin notice publishing and deletion
- SQL schema and row-level security (RLS) policies
- Official portal links and privacy reminders

## Important limitations

- This is a starter project, not a government-authorized portal.
- Service prices, official status, centre phone, exact address, working hours and map link must be verified and configured by the centre owner.
- The public form collects only basic contact/request information. Do not request Aadhaar numbers, OTPs, passwords, bank details or document scans in this form.
- Do not add a Supabase `service_role` or secret key to `app.js`. The browser must use only the publishable/anon key with RLS policies enabled.
- Supabase free-plan limits and terms can change. Check the provider's current pricing.
- Before real use, test the policies, enable MFA on your accounts, and review the privacy/legal requirements applicable to your operation.

## A. Configure Supabase (free tier may be sufficient for a small project)

1. Go to https://supabase.com/ and create a project.
2. Open **SQL Editor** in the project dashboard.
3. Open `supabase-setup.sql` from this folder, copy all of it into the SQL Editor and run it.
4. Go to **Authentication → Users** and create an account for the centre owner/admin. Disable public signups if not needed.
5. Copy the new user's UUID.
6. Back in SQL Editor, run this command after replacing the placeholder with the exact UUID:

   ```sql
   insert into public.profiles (id, role)
   values ('PASTE-AUTH-USER-UUID-HERE', 'admin');
   ```

7. Open **Project Settings / Connect** and copy the project URL and publishable key (older projects may label it `anon` key).
8. Edit `app.js` and replace:
   - `PASTE_YOUR_SUPABASE_PROJECT_URL_HERE`
   - `PASTE_YOUR_SUPABASE_PUBLISHABLE_KEY_HERE`
   - Set `CENTRE_PHONE` to your own verified phone number, country code first, digits only, e.g. `919876543210`.
9. Do not place any secret/service-role key in browser files.

## B. Test on your computer

You can open `index.html` directly for a quick design preview, but some browsers restrict scripts or API calls for local files. Prefer a local server:

- Install Node.js from https://nodejs.org/
- Open a terminal in this folder.
- Run `npx serve .` and open the local URL it prints.

Or use VS Code and a local preview extension. You can also upload the files to a Git repository and test through its hosted preview.

## C. Publish for free

### Option 1: GitHub Pages

1. Create a GitHub repository, e.g. `om-csc-centre`.
2. Upload `index.html`, `styles.css`, `app.js`, and `assets/` to the repository root. Keep `README.md` and SQL file too if you want project documentation.
3. Open repository **Settings → Pages**.
4. Choose deployment from the `main` branch and `/ (root)`, then save.
5. Wait for the build to finish and open the URL shown by GitHub Pages.

### Option 2: Cloudflare Pages

1. Push the project to a GitHub repository.
2. Sign in at https://pages.cloudflare.com/ and create a Pages project connected to the repository.
3. For this plain static site, use no build command and set the output directory to the repository root (or `.` if requested).
4. Deploy and open the generated `pages.dev` URL.

## D. Test the backend

1. Open the public website and submit a test request with dummy information.
2. In Supabase Table Editor, confirm it appears in `enquiries`.
3. Open the site's Admin section and sign in with the admin account.
4. Confirm that the request appears and its status can be changed.
5. Publish a test notice and confirm it appears on the public page.
6. Sign out and verify the admin panel no longer shows private data.
7. Test from a separate browser/device as a non-admin visitor.

## E. Before public launch

- Replace demo contact details and verify the exact address and hours.
- Verify every service, official link, fee and notice.
- Add a clear privacy policy and data-retention procedure.
- Do not upload private customer documents to public storage.
- Consider anti-spam/rate limiting for the public form. Public insert access can be abused; monitor it and add a server-side CAPTCHA/rate limit before advertising widely.
- Review RLS policies and run Supabase's security checks. Keep MFA enabled for admin accounts.
- Back up important records and delete old enquiries according to your retention policy.
- Do not claim government affiliation unless formally authorized.

## Files

- `index.html` — public pages and admin interface
- `styles.css` — responsive design
- `app.js` — language toggle, forms, Supabase connection and admin dashboard
- `supabase-setup.sql` — tables, seed services, grants and RLS policies
