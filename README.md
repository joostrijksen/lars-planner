# Lars Leerplanner

Slimme leerplanner met AI-planning voor Lars.

## Setup

### 1. Supabase tabellen aanmaken

Ga naar je Supabase project → SQL Editor → plak dit en run:

```sql
-- Vakken
create table lp_vakken (
  id uuid default gen_random_uuid() primary key,
  naam text not null,
  emoji text default '📘',
  kleur text default '#7c6ff7',
  created_at timestamptz default now()
);

-- Toetsen
create table lp_toetsen (
  id uuid default gen_random_uuid() primary key,
  vak_id uuid references lp_vakken(id) on delete cascade,
  naam text not null,
  datum date not null,
  created_at timestamptz default now()
);

-- Sessies
create table lp_sessies (
  id uuid default gen_random_uuid() primary key,
  toets_id uuid references lp_toetsen(id) on delete cascade,
  datum date not null,
  inhoud text not null,
  tijd_min integer default 25,
  done boolean default false,
  created_at timestamptz default now()
);

-- RLS uitschakelen (single user app)
alter table lp_vakken enable row level security;
alter table lp_toetsen enable row level security;
alter table lp_sessies enable row level security;

create policy "Allow all" on lp_vakken for all using (true) with check (true);
create policy "Allow all" on lp_toetsen for all using (true) with check (true);
create policy "Allow all" on lp_sessies for all using (true) with check (true);
```

### 2. GitHub Pages

1. Maak nieuwe repo: `lars-planner`
2. Upload alle bestanden
3. Settings → Pages → Source: main branch / root
4. URL wordt: `https://joostrijksen.github.io/lars-planner`

### 3. iPhone installeren

1. Open Safari op iPhone
2. Ga naar de GitHub Pages URL
3. Tik op Delen → "Zet op beginscherm"
4. Klaar — werkt als app!

## Bestanden

- `index.html` — de hele app
- `manifest.json` — PWA configuratie
- `sw.js` — service worker (offline support)
- `icon-192.png` + `icon-512.png` — app iconen
