# O3P Google Drive

Google Drive integration addon for Odoo 20.

The first version adds a **Google Drive** section to the Odoo Settings app. System
administrators can store the OAuth `type`, `client_id`, `client_secret`, and
`refresh_token` together as JSON.

## Explorer item actions

New items are starting points by default. Items discovered automatically while
scanning or navigating the tree are explicitly created with Starting Point off.
Existing items keep their current flag, and API callers can explicitly supply
`is_starting_point=False`.

Item names can be edited in the item form or through the Odoo API. Changing an
existing item's name renames the corresponding Google Drive file and stores the
metadata returned by Google. Metadata scans update local names without sending
renames back to Google. If the ID and name change together, the new ID takes
precedence: its metadata is fetched and its full tree is scanned if it is a folder.

Creating an item with an explicit Google Drive ID preserves the supplied name
until a metadata scan. When only a name is supplied, it is copied into the ID and
metadata retrieval is attempted immediately. An invalid or inaccessible ID causes
the save to fail. Google must allow the configured account to rename files for
name edits to succeed.

Item indexes are installed or updated when the addon is upgraded. Parent IDs use
a JSON containment index for folder browsing and reconciliation; names and custom
metadata use Odoo trigram indexes for text searches (requires PostgreSQL
`pg_trgm` support). Starting points have a partial index. Custom metadata is also
available in the Items search bar. These indexes speed up local database queries;
automatic refreshes of stale items still require Google API requests.

Other addons can add MIME-aware entries to the explorer context menu by creating
an `o3p.google.drive.item.action` linked to an `ir.actions.server` whose model is
`o3p.google.drive.item`. The `mime_type` accepts exact values and wildcards such
as `image/*` or `*/*`; `icon` is a Material Symbols icon name.

Action registrations are loaded with the current user's access rights. Extension
addons should grant read access to their intended groups, optionally with an
access domain that limits which registration records those groups can see. The
server action's own group and record access checks are also enforced when it is
run.
