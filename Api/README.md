# Supabase and API routes

## Api routes?

The API routes in Nextjs are useful when you want to perform client-side fetching of data in a client-component, but you don't want the api credentials to be exposed in the request headers.

We are currently using Supabase which uses PostgreSQL, and has a security feature called RLS, which stands for Row Level Security. When implemented properly, you can expose the public key in the request header, and we could in theory do client-side fetching and sending requests directly to Supabase, but we prefer to be extra certain and do the actual API call to Supabase from the server, meaning through the api routes.

## Api routes structure and location

You find the api routes in `/src/app/api`.

```bash
api/
    |
    ---> /auth
    |
    ---> /public
```

### Auth and public routes

Supabase separates the database into different schemas, and the two most important ones are the `auth` and `public` ones. We've mirrored this distiction within the api routes, like you can see above.

The public routes handles requests going to the public schema. An example is a user creating, updating or deleting a template.

The auth routes handles requests going to the `auth` schema, but also Supabase's authtenticaiton functionality. An example of handling the auth schema is a user updating their password or email. Managing authentication requests like signing in or signing out is also done through the `auth` api routes.

## ApiService class

```ts
class ApiService {
  private static instance: ApiService;
  private baseURL: string;

  private constructor(baseURL: string) {
    this.baseURL = baseURL;
  }

  public static getInstance(): ApiService {
    if (!ApiService.instance) {
      ApiService.instance = new ApiService('/api');
    }
    return ApiService.instance;
  }

  private async request(method: 'get' | 'post' | 'put' | 'delete', path: string, data?: any) {
    const url = `${this.baseURL}${path}`;
    return axios({ method, url, data });
  }

  public auth = {
    signup: (data: {firstName: string, lastName: string, password: string, email: string}) => this.request('post', '/auth/signup', data),
    signin: (data: {password: string, email: string}) => this.request('post', '/auth/signin', data),
    signout: () => this.request('post', '/auth/signout'),
    confirm: (data: any) => this.request('post', '/auth/confirm', data),
    user: {
      delete: (id: string) => this.request('delete', `/auth/users/${id}`),
      create: (data: SignUpData) => this.request('post', `/auth/signup`, data)
    }
  };
  public public = {
    user: {
      update: (id: string, data: {firstName?: string, lastName?: string}) => this.request('put', `/public/users/${id}`, data),
    },
    templates: {
      get: (id?: string) => this.request('get', `/public/templates/${id}`),
      update: (id: string, data: TemplateUpdateFields) => this.request('put', `/public/templates/${id}`, data),
      delete: (id: string) => this.request('delete', `/public/templates/${id}`),
      create: (data: {category_id: string, is_collection: boolean, order?: number}) => this.request('post', `/public/templates/`, data),
        }
    }
}

export default ApiService;
```

### Using the ApiService class

```ts
import ApiService from "@/app/api/_ApiService";
const apiService = ApiService.getInstance()
const bodyData = {
            password: password,
            email: email
        }
const response = await apiService.auth.signin(bodyData)
```
