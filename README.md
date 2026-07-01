# HTSI ECOSOC Youth Innovation Map

A real-time participatory mapping initiative at the *2026 ECOSOC Youth Forum* to collect and map global youth programs, regions, and constraints. The aim was to turn our conversations at ECOSOC into a real-time map of global
youth innovation systems, so we can better understand where support is needed and how to act on it.

<div style="padding:50% 0 0 0;position:relative;"><iframe src="https://player.vimeo.com/video/1205991019?badge=0&amp;autopause=0&amp;player_id=0&amp;app_id=58479" frameborder="0" allow="autoplay; fullscreen; picture-in-picture; clipboard-write; encrypted-media; web-share" title="HTSI ECOSOC Map - Demo"></iframe></div>

## Table of Contents

- [Why We Built This](#why-we-built-this)
- [How It Works](#how-it-works)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Environment Variables](#environment-variables)
  - [Running Locally](#running-locally)
- [Database Schema](#database-schema)
- [Deployment](#deployment)
- [License](#license)


## Why We Built This

There are countless youth programs, entrepreneurship initiatives, and development efforts across the world, however, there has been **no simple, real-time way to understand**:

- **Who** is actually operating on the ground
- **What types** of programs exist
- **Where** the system is breaking down (funding, execution, engagement, etc.)

And we realized: at the 2026 ECOSOC Youth Forum, hundreds of youth leaders, policymakers, funders, and practitioners from around the world come together to discuss youth development. But without all that information,

- A youth program operator in Nigeria has no way to know a funder in London is looking for exactly their kind of initiative.
- A government official in Indonesia can't easily see what types of programs are succeeding in neighboring countries.
- Bottlenecks like funding gaps or execution capacity issues remain invisible at a global scale.

And so, this came to fruition.

## How It Works

Forum participants submit a single entry describing:

| Field | Description |
|---|---|
| **Country / Region** | Where they operate |
| **Role Type** | What type of work they do |
| **Focus Area** | Their primary domain
| **Primary Constraint** | Their biggest bottleneck |
| **Organization Name** | Optional but encouraged |
| **Description** | Brief overview of their initiative |
| **Estimated Youth Reach** | Rough number of youth they reach |
| **Contact** | LinkedIn or email (optional) |

Each input is added to a live global dataset. As the forum progresses, a live global picture emerges.

The HTSI map also visualizes connection arcs between entries, showing, for example, which funders could help address specific funding constraints, or which training organizations could support skills gaps.


## Getting Started

### Prerequisites

- Node.js 20+
- npm or pnpm
- A Supabase project (free tier works)

### Installation

```bash
# Clone the repository
git clone https://github.com/ethan-yip/htsi_ecosoc.git
cd htsi_ecosoc

# Install dependencies
npm install
```

### Environment Variables

Create a `.env` file in the project root:

```env
VITE_SUPABASE_URL=https://your-project.supabase.co
VITE_SUPABASE_ANON_KEY=your-anon-key
```

### Running Locally

```bash
npm run dev
```

The app will be available at `http://localhost:5173`.

### Building for Production

```bash
npm run build
npm run preview
```


## Database Schema

The application uses a single `entries` table in Supabase (PostgreSQL):

```sql
create table if not exists public.entries (
  id uuid primary key default gen_random_uuid(),
  country text not null,
  state text,
  role_type text not null check (role_type in (
    'youth-program-operator', 'ngo-nonprofit', 'school-university',
    'startup-builder', 'government-policy', 'funder-donor', 'other'
  )),
  focus_area text not null check (focus_area in (
    'entrepreneurship', 'education', 'employment',
    'peacebuilding-civic-engagement', 'technology-innovation', 'other'
  )),
  primary_constraint text not null check (primary_constraint in (
    'funding', 'execution-capacity', 'engagement',
    'institutional-support', 'training-skills', 'other'
  )),
  organization_name text not null default '',
  organization_description text not null default '',
  estimated_reach bigint not null default 0,
  contact text,
  created_at timestamptz not null default now()
);
```

Row Level Security (RLS) is enabled with policies allowing anonymous inserts and reads.

## Deployment

The app is configured for deployment on **Vercel**:

```json
{
  "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }]
}
```

The `vercel.json` ensures client-side routing works correctly by redirecting all paths to `index.html`.

## License
AGPL 3.0

Built by **Rehan Ali** and **Ethan Yip** (Founder, HTSI) for the **2026 ECOSOC Youth Forum**.
