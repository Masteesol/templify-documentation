# Database

## Commands and links

### TypeScript database Schema

Run this command to authenticate the Supabase CLI:

```bash
npx supabase login
```

If you don't have the the Supabase CLI installed, I think you'll get prompted for it.

Run the command below to get the database schema as a TypeScript file:

```bash
npx supabase gen types typescript --project-id jvbphwcsxjqbzowqlqfq > database.types.ts
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
