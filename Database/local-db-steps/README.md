# Setting up a local Supabase Instance

* Create folder structure

```bash
mkdir local-db
cd local-db
git init
```

* Login to supabase

```bash
npx supabase login
```

* Initialize Supabase and start the instance

```bash
npx supabase init - initalizes config for new supabase project 
npx supabase start
```

Note: Docker must be running

* Link local db with remote supabase

```bash
npx supabase link --project-ref jvbphwcsxjqbzowqlqfq
```

* Get migration file to align with prod

````bash
npx supabase db pull
```

* To apply the new migration to your local database

````bash
npx supabase migration up
```

* Get prod data into test DB
```bash 
npx supabase db dump --data-only -f supabase/seed.sql
```

* Get data from seed to db
Note: only necessary first time? so maybe check if file exists in folder first

```bash
psql -h localhost -U postgres -p 54322 -f supabase/seed.sql
```

* To reset your local database completely

```bash 
npx supabase db reset
```

* Alternative - get preexisting sql migration from sql file (bad because needs to be manually updated)

```bash
supabase migration new create_all_tables
```

* get sql structure from db file

```bash
supabase db reset
```

(apply migration)
