# Claimsys — Firebase Claim Records

Live static frontend: `index.html` — host using GitHub Pages (or Firebase Hosting). Uses Firebase Auth, Firestore and client-side XLSX/PDF libraries.

## Included in initial build
- Firebase Email/Password and Google login; self-registration starts as Staff.
- Superadmin promotes registered users to Manager by UID.
- Manager creates multiple company workspaces.
- Each company has independent chart of accounts and claim records.
- Staff registers once; Manager grants the same UID access to multiple companies.
- Month-based Firestore claim partitions, annual aggregation, account filter.
- Fields: bill date, description, vendor, account, amount (stored as integer cents).
- Records view with individual review actions and bulk Mark Checked.
- Reports view without review controls; unchecked and correction-required included, void excluded.
- Excel XLSX export and landscape PDF (also printable).

## Mandatory Firebase setup
1. In Firebase Console for `claimsys-ada3b`, enable Authentication Email/Password and/or Google.
2. Add the deployed site hostname to **Authentication > Settings > Authorized domains**.
3. Create a Cloud Firestore database.
4. Publish `firestore.rules` from this repository as Firestore Rules, **before allowing real users**. Merely committing rules to GitHub does not deploy them.
5. Sign in once with the intended Superadmin account, creating `users/{uid}` with role `staff`.
6. Using trusted Firebase Console administrator access, set **only that user's** Firestore `users/{uid}.role` to `superadmin`. Do not offer public UI-based self-promotion.
7. Sign out and back in; open the Managers section to promote registered manager accounts by their Firebase UIDs.
8. Managers can then create companies, accounts, and assign registered Staff users using their UIDs.

## Recommended deployment
GitHub Pages: repository Settings > Pages > Deploy from branch > main / root. GitHub Pages is static hosting. Firebase login and database require internet access.

## Design/security notes
- The public Firebase web config is not a secret. Restrict access through Authentication and Firestore Security Rules; do not place service-account credentials in the frontend.
- New Staff cannot create companies or self-promote. Manager owns their created companies; Superadmin can see all.
- Claims are divided under `companies/{companyId}/months/YYYY-MM/claims/{claimId}`.
- Existing account code/name are copied into each claim to preserve historical reports.
- Void is retained in Records but excluded from Reports and active totals.
- No receipt uploads, payment tracking, invitations by email, automatic transfer of company ownership, or cloud functions in this initial version.
- The individual Edit flow currently does not preserve full before/after audit history. Before accounting or compliance production use, implement audited corrections, stronger record state transitions, and server-validated monetary summaries.
- Superadmin promotion and Firestore Rules publication require trusted Firebase Console setup outside GitHub.

## Operational warning
This is an initial working foundation, **not** a production-tested financial system. Test rules using Firebase Emulator or Rules Playground (cross-company reads, staff review escalation, unauthorised update, and monthly data loading) before real financial records are entered.
