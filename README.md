# WHISPER — Anonymous Messaging (MVP)

<p align="center">
  <img src="https://i.imgur.com/woPujRs.jpeg" alt="WHISPER" width="480"/>
</p>

**WHISPER** is a lightweight anonymous messaging prototype — fans drop short, candid "whispers" (text + emojis). Images are "coming soon". Built with plain HTML/CSS/JS and Supabase for storage & DB.

---

## Quick Links
- **Owner / GitHub:** https://github.com/Voltzdistro  
- **Contact (WhatsApp):** https://wa.me/2347081427486  
- **Email:** voltzdistro@gmail.com

---

## Tagline
> Send whispers. Stay anonymous. Share honestly.

---

## Features (MVP)
- Anonymous message submission (text + emojis)  
- Optional image support (bucket storage + image URLs) — handled server-side via Supabase Storage  
- Admin dashboard (private) to view and manage incoming whispers  
- Minimal, mobile-first UI

---

## Repo Contents
- /index.html ← Frontend entry (Landing + form)
- /styles.css* ← CSS (styles)
- /script.js ← Frontend logic (form, preview, supabase calls)
- /README.md

---

---

## Supabase Setup (high level)
1. Create a Supabase project.  
2. Create a **table** `messages` (schema example below).  
3. Create a **Storage bucket** (e.g. `uploads`) — public for MVP.  
4. Get your **Project URL** and **anon public key** from Project Settings → API.

**Example SQL for `messages` table**
```sql
create table if not exists public.messages (
  id uuid default gen_random_uuid() primary key,
  text text check (char_length(text) <= 500),
  image_urls text[] default '{}',
  created_at timestamp with time zone default now()
);

alter table public.messages enable row level security;

create policy "Allow inserts for anyone" 
 on public.messages
 for insert
 to public
 with check (true);

create policy "Allow select for authenticated users only"
 on public.messages
 for select
 to authenticated
 using (true);

create policy "Allow delete for authenticated users only"
 on public.messages
 for delete
 to authenticated
 using (true);
