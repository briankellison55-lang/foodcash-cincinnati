# FoodCash Cincinnati

A zero-cost starter project for a restaurant referral and commission-tracking business.

## What this starter does

- Public restaurant directory
- Referral links using `?ref=RESTAURANT_CODE`
- Records referral clicks in Supabase
- Admin sign-in
- Admin-only restaurant and order management
- Commission calculations based on eligible subtotal and agreed percentage
- Pending / approved / payable / paid order statuses
- Payout request recordkeeping

## Important limitations

This is a starter application, not a live payment processor or a verified restaurant affiliate network.
A referral click does not prove that an order happened. Restaurants must provide order reports or an approved integration, and an administrator must verify orders before commissions are approved.
Cash App payouts are **not automated**. Record a payout only after FoodCash has received the restaurant's commission and the payment has actually been sent and confirmed.
Do not collect customers' food-order payments in this app.

## Free setup

1. Create a free GitHub account at https://github.com/signup.
2. Create a free Supabase project at https://supabase.com/dashboard.
3. In Supabase, open **SQL Editor**, create a new query, paste `supabase/schema.sql`, and run it.
4. In Supabase, open **Project Settings → API**. Copy the Project URL and the `anon` / publishable key.
5. Open `public/config.js` and replace the two placeholder values. The anon key is designed for browser use; never put a service-role key here.
6. In Supabase **Authentication → URL Configuration**, set your Site URL to your Cloudflare Pages URL after deployment. Add the preview and production URLs to Redirect URLs if needed.
7. Create a GitHub repository named `foodcash-cincinnati`.
8. Upload the contents of this folder to the repository. Ensure `index.html` is at the repository root.
9. In Cloudflare, create a Pages project and connect the GitHub repository. Use the root directory `/` and no build command. Set the output directory to `public` only if you move `index.html` into `public`; for this project, use root output `.`. Alternatively, upload the root files using Cloudflare Pages direct upload.
10. Open the deployed site and sign in using a Supabase Auth user you create.
11. In Supabase SQL Editor, promote your account to administrator by replacing the email below and running:
    `update public.profiles set role = 'admin' where id = (select id from auth.users where email = 'YOUR-EMAIL');`
12. Sign out and sign back in. The admin tools should now appear.

## Add your first restaurant

Sign in as the admin, open **Admin**, and add the restaurant's name, city, ordering URL, referral code, and agreed commission percentage. Only add a restaurant as an active partner after a written agreement is signed.

## Order verification workflow

1. A customer visits `https://YOUR-SITE/?ref=CODE`.
2. The app records a referral click and sends the customer to the restaurant's ordering page.
3. The restaurant supplies an order report or confirms the order using an agreed process.
4. An admin records the order reference and eligible subtotal.
5. The app calculates the commission using the restaurant's agreed percentage.
6. Keep it pending until the restaurant verifies it. Approve it only after verification.
7. Mark it payable only after FoodCash receives the corresponding funds from the restaurant. An administrator can do this in Supabase SQL Editor:
   `update public.orders set status = 'payable' where id = 'ORDER-UUID' and status = 'approved';`
8. Record a payout only after you actually send and confirm the payment. The app requires the exact commission amount and marks that order paid after saving the payout record.

## Security notes

- Row Level Security is enabled in the SQL schema.
- Admin privileges are assigned manually by the project owner.
- Never expose a Supabase service-role key in browser code, GitHub, or a public repository.
- Use strong passwords and enable MFA on GitHub and Supabase.
- This starter is not independently audited. Have a qualified developer review it before handling real customer data.
- Add a privacy notice and terms before public customer launch.
