# Setting up a local Supabase Instance

* Create folder structure

```bash
mkdir local-db
cd local-db
git init
```

* Login to supabase

`npx supabase login`

* Initialize Supabase and start the instance

```bash
npx supabase init - initalizes config for new supabase project 
npx supabase start
```

Note: Docker must be running

* Link local db with remote supabase

npx supabase link --project-ref jvbphwcsxjqbzowqlqfq

* Get migration file to align with prod

`npx supabase db pull`

* To apply the new migration to your local database
`npx supabase migration up`

* Get prod data into test DB
`npx supabase db dump --data-only -f supabase/seed.sql`

* Get data from seed to db
Note: only necessary first time? so maybe check if file exists in folder first

`psql -h localhost -U postgres -p 54322 -f supabase/seed.sql`

* To reset your local database completely

`npx supabase db reset`

* Alternative - get preexisting sql migration from sql file (bad because needs to be manually updated)

`supabase migration new create_all_tables`

* get sql structure from db file

`supabase db reset`

(apply migration)
