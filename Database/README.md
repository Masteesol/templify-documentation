# Database and types

**[< Go home](/)**

## Commands and links

### Create local database and use it to run tests

For setting up local database, view the complete guide [here](./local-db-steps/).

#### How to test

User must first run supabase login to first generate their user token.
Creating a new env file called setEnvVars.ts under tests is also necessary. This file must include:

```bash
process.env.Testing_Password = "******"
process.env.Testing_Email = "******@gmail.com"
process.env.SUPABASE_URL = "http://127.0.0.1:54321" -> database url for the newly created database
process.env.SUPABASE_ANON_KEY='' -> here you must check what key your database generates for you. This happens when running the script for the first time.
```

After the token is generated, simply run:
`npm run-script tests`

This will create a local instance of our supabase database, create the correct schema through a migration, fetch data from prod and use it to populate it.

Then it uses `jest` to run some example tests against this database, outputting the results.

After running tests the script then cleans the database containers (psql, etc) and removes the directory it created on step 1.

### TypeScript database Schema

Run this command to authenticate the Supabase CLI:

```bash
npx supabase login
```

If you don't have the the Supabase CLI installed, I think you'll get prompted for it.

Run the command below to get the database schema as a TypeScript file:

```bash
npx supabase gen types typescript --project-id jvbphwcsxjqbzowqlqfq | Set-Content -Encoding utf8  .\src\utils\supabase\database.types.ts
```

This file will end up at the project root.

The `database.types.ts` referenced by Supabase is located in the current path: `/src/utils/supabase/database.types.ts`.

### Links

* [Schema visualizer](https://supabase.com/dashboard/project/jvbphwcsxjqbzowqlqfq/database/schemas)
* [Functions / Stored Procedures](https://supabase.com/dashboard/project/jvbphwcsxjqbzowqlqfq/database/functions)
* [Table Editor](https://supabase.com/dashboard/project/jvbphwcsxjqbzowqlqfq/editor)

## Table descriptions

See either the schema visializer or the `database.types.ts` for the latest structure in detail. This section covers the database objects at a high level.

### categories

Serves as more like a container for the each templates and is the top level object of the text templates.

### templates

The templates are the main object of the application and contains the actual text content, along with rows for tracking the state of each text template.

These rows are for example tracking character and word limit set by the user, and if the limiter is on or not, and if this template is favourited, etc.

### template_collections

The template table has a special row called `is_collection` which is used to conditionally join the templates table with the `template_collections` table.

The `template_collections` table holds data for another component type: the template collection. The table is simple and joins all rows in the table with the
`template` table based on its foreign key relationship.

The JSON output will look like this:

```ts
{
    char_limit: null,
    copy_count: 30,
    favourited: true,
    is_collection: true,
    limiter_active: false,
    order: 0,
    template_id: "d889b9c7-59e2-434b-97ee-ec19b082cebd",
    text: "Text Placeholder",
    title: "Chat Basics",
    word_limit: null,
    template_collections: [
        {
            id: 'd0b88334-c142-4bb5-82ec-5700b37d9f5c', 
            text: 'Please give me a minute so I can read through your query.', 
            order: 0, created_at: '2023-07-28T10:41:03.822438+00:00', 
            template_id: 'd889b9c7-59e2-434b-97ee-ec19b082cebd'
        },
        {
            id: '586737c2-36ee-4ec4-bf81-d4f03047adb2', 
            text: 'Is there anything else I can help you with today', 
            order: 1, 
            created_at: '2023-07-28T11:22:08.368939+00:00', 
            template_id: 'd889b9c7-59e2-434b-97ee-ec19b082cebd'
        },
        {
            id: '1a25ae45-dcfc-4690-add5-a995bc6810bc', 
            text: 'Welcome to IKEA customer support. How may I help you today?', 
            order: 2, created_at: '2023-07-28T10:40:46.486223+00:00', 
            template_id: 'd889b9c7-59e2-434b-97ee-ec19b082cebd'
        },
        {
            id: '55cfd3b6-70ca-43b2-86e6-1e73295e2dda', 
            text: 'Could you please provide me with your order number?', 
            order: 3, 
            created_at: '2023-07-28T11:20:55.252728+00:00', 
            template_id: 'd889b9c7-59e2-434b-97ee-ec19b082cebd'
        }
    ]
}
```

### auth user table

The auth `users` table is a table created by Supabase when the user signs up and contain rows related to authentication.

### public users

The public `users` table has rows related to the user which do not exist in the auth table, like `first_name` and `last_name`, `subscription_tier` amongst others.

### subscription_tier

Contains the different subscription tier along with their ID's. This table exists to have a centralised place for all the tiers and their plan information.

## Custom types

In order to efficiently we need to manipulate the data, and the processed data we also need to correctly type. Below you find the current types used in the application.

```ts
import { Database } from "@/utils/supabase/database.types";

export type TemplateCollection = Omit<Database["public"]["Tables"]["template_collections"]["Row"], "created_at">
  
export type TemplateBase = Database["public"]["Tables"]["templates"]["Row"]

export type TemplateWithCategoryName = {
  category_name: string;
  template_collections: TemplateCollection[] | null;
} & TemplateBase;

type Categories = Omit<Database["public"]["Tables"]["categories"]["Row"], "user_id">

export type CategoryTemplateContainer = {
    templates: TemplateWithCategoryName[];
  } & Categories;
  
  export type CategoryForDashboard = Omit<Categories, "order">
```
