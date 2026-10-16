<!-- LOVABLE:BEGIN -->
> [!IMPORTANT]
> This project is connected to [Lovable](https://lovable.dev). Avoid rewriting
> published git history — force pushing, or rebasing/amending/squashing commits
> that are already pushed — as it rewrites history on Lovable's side and the
> user will likely lose their project history.
>
> Commits you push to the connected branch sync back to Lovable and show up in
> the editor, so keep the branch in a working state.
<!-- LOVABLE:END -->

## Architecture rules
- Roles live in `public.user_roles` (admin/tutor/student) checked via `has_role()`; signup trigger assigns student/tutor from metadata and makes the first-ever user admin. Why: privileges never come from editable profile data.
- Data access goes through the browser client with RLS (no service role); booking status transitions are enforced by a DB trigger. Why: UI checks alone are not security.
- Private pages live under `src/routes/_authenticated/` and render inside `AppShell`; role-only pages wrap content in `RoleGate`. Why: one shell, per-role sidebar.
- Only `tutor_profiles.is_approved = true` tutors are public. Why: admin moderation before listing.
