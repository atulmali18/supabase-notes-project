
# 📝 Supabase Notes Service

A minimal backend for a personal notes service using Supabase Edge Functions.

---

## 📐 Schema Design

```sql
create table notes (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null references auth.users(id),
  title text not null,
  content text,
  created_at timestamp with time zone default now()
);
Why this schema?

id as a UUID ensures uniqueness and supports distributed systems.

user_id links the note to the logged-in user securely via Supabase Auth.

title is required so every note has a meaningful name.

content is optional for flexibility.

created_at allows sorting and tracking note creation time.

🛠️ Setup & Deployment Steps
Create a Supabase project at https://supabase.com

Add the notes table

Go to SQLor → Paste and run the SQL from schema.sql

Create Edge Functions

Go to Edge Functions → New Function → Viaor

Create post_notes and get_notes functions with .ts files provided

Click Deploy function

Set secrets/environment variables (optional for local dev)

SUPABASE_URL – your project’s base URL

SUPABASE_ANON_KEY – found in Project Settings → API

🔌 Edge Functions
functions/post_notes.ts
ts

// Why: POST is used for creating new data. Body is used to receive title/content.
functions/get_notes.ts
ts

// Why: GET is used to retrieve user data. Auth user ID is used to filter notes.
🧪 Demo Commands
➕ Create a Note – POST /notes
bash

curl -X POST https://pcgmtbqchcrcnbwgtiqt.supabase.co/post_notes \
  -H "Authorization: Bearer <your-access-token>" \
  -H "Content-Type: application/json" \
  -d '{"title": "First Note", "content": "This is a test note."}'
✅ Expected Response

json

{
  "data": {
    "id": "uuid",
    "user_id": "user-uuid",
    "title": "First Note",
    "content": "This is a test note.",
    "created_at": "2025-04-30T12:00:00.000Z"
  },
  "error": null
}
📄 List Notes – GET /notes
bash

curl -X GET https://pcgmtbqchcrcnbwgtiqt.supabase.co/get_notes \
  -H "Authorization: Bearer <your-access-token>"
✅ Expected Response

json

{
  "data": [
    {
      "id": "uuid",
      "user_id": "user-uuid",
      "title": "First Note",
      "content": "This is a test note.",
      "created_at": "2025-04-30T12:00:00.000Z"
    }
  ],
  "error": null
}
📦 Project Structure
pgsql

supabase-notes/
├── schema.sql
├── functions/
│   ├── post_notes.ts
│   └── get_notes.ts
└── README.md