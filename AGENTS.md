# Codex instructions for Инженер 24

Product: Russian-language facility maintenance web application for multiple buildings. Build ticketing MVP first.

## Technology
PHP 8.3, Laravel 12, PostgreSQL 16+, Livewire, Blade and Tailwind. Start from the official Laravel skeleton, commit actual runnable source files (not just instructions). Use Docker Compose for local PostgreSQL if possible. Do not replace the chosen stack without a documented reason.

## Implementation milestones
1. Bootstrap Laravel, .env.example, secure defaults, README commands and an automated smoke test.
2. Users, 5 role names, building_user scoping, buildings and locations migrations, factories, Policies.
3. Tickets with transaction-safe reference numbers, category/system, priorities, statuses, due times, reporters, assignees; index and details screens, create, edit and assignment.
4. Comments, status history, work logs, materials, private photos with authorization for download.
5. Responsive Russian dashboard and filters; write tests demonstrating users cannot read or edit tickets outside assigned buildings.

## Architecture and conventions
- Controllers/actions thin, business behavior in services; validation in Form Requests.
- Server-side authorization mandatory for every operation, including attachments and statistics.
- Record ticket transitions and important changes in append-only logs.
- Preserve timestamps in UTC, display to users in Asia/Vladivostok.
- Do not commit passwords, tokens, private building details or real staff data.
- Do not deploy or merge to main without owner review.

## Definition of done
Run composer install, migrations, tests, front-end build where tool environment supports it. State which checks were actually executed and which couldn't run. Never fabricate successful tests. Demo data must be entirely fictitious.
