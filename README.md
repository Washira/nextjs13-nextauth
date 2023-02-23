This is a [Next.js](https://nextjs.org/) project bootstrapped with [`create-next-app`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app).

## Getting Started

First, run the development server:

```bash
npm run dev
# or
yarn dev
# or
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `app/page.tsx`. The page auto-updates as you edit the file.

[API routes](https://nextjs.org/docs/api-routes/introduction) can be accessed on [http://localhost:3000/api/hello](http://localhost:3000/api/hello). This endpoint can be edited in `pages/api/hello.ts`.

The `pages/api` directory is mapped to `/api/*`. Files in this directory are treated as [API routes](https://nextjs.org/docs/api-routes/introduction) instead of React pages.

This project uses [`next/font`](https://nextjs.org/docs/basic-features/font-optimization) to automatically optimize and load Inter, a custom Google Font.

## Learn More

To learn more about Next.js, take a look at the following resources:

- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API.
- [Learn Next.js](https://nextjs.org/learn) - an interactive Next.js tutorial.

You can check out [the Next.js GitHub repository](https://github.com/vercel/next.js/) - your feedback and contributions are welcome!

## Deploy on Vercel

The easiest way to deploy your Next.js app is to use the [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js.

Check out our [Next.js deployment documentation](https://nextjs.org/docs/deployment) for more details.

## Step of development

### Set Up a Project

First, go to Next.js and command to install

```
npx create-next-app@latest --experimental-app
```

You can see [here](https://beta.nextjs.org/docs/installation) for more details.

After install Next.js, install Tailwind CSS

```
npm install -D tailwindcss postcss autoprefixer
```

```
npx tailwindcss init -p
```

then go to `tailwind.config.js` to add content inside this for adding the paths to all of your template files.

```
content: [
    "./app/**/*.{js,ts,jsx,tsx}",
    "./pages/**/*.{js,ts,jsx,tsx}",
    "./components/**/*.{js,ts,jsx,tsx}",
 
    // Or if using `src` directory:
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
```

Next, go to `globals.css` and add these `@tailwind` directives on top for each Tailwind’s layers.

```
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Run your build process with `npm run dev`

Check out our [Tailwind CSS documentation for Next.js](https://tailwindcss.com/docs/guides/nextjs) for more details.

### Set Up Prisma

Go to [Railway.app](https://railway.app) for create new project.

In this case we will create the project as `PostgreSQL` database.

Run command to install libraries as setting up Prisma

```
npm install prisma typescript ts-node @types/node --save-dev
```

then install Prisma CLI in the project

```
npx prisma init
```

We will see the `Next Steps:` for set up database and connect Prisma, please follow step by step.

```
Next steps:
1. Set the DATABASE_URL in the .env file to point to your existing database. If your database has no tables yet, read https://pris.ly/d/getting-started
2. Set the provider of the datasource block in schema.prisma to match your database: postgresql, mysql, sqlite, sqlserver, mongodb or cockroachdb.
3. Run prisma db pull to turn your database schema into a Prisma schema.
4. Run prisma generate to generate the Prisma Client. You can then start querying your database.
```

See the [Prisma document](https://www.prisma.io/docs/getting-started/quickstart) for more set up details.

After this step, we received new one file `.env` and one folder `prisma` obtain a file `schema.prisma`.

Go to Prisma project created, click tab `Connect`, copy `Postgres Connection URL` to replace in .env file (`DATABASE_URL`).

Next, see the [document](https://www.prisma.io/docs/getting-started/setup-prisma/start-from-scratch/relational-databases/using-prisma-migrate-typescript-postgres) for detail of schema.

Copy the schema to bottom line of `prisma/schema.prisma`.

```
model Post {
  id        Int      @id @default(autoincrement())
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  title     String   @db.VarChar(255)
  content   String?
  published Boolean  @default(false)
  author    User     @relation(fields: [authorId], references: [id])
  authorId  Int
}
```

and remove author for now, we will add it after this.

Then go to install Prisma CLI

```
npm install @prisma/client
```

And create new file as `prisma.client.js` and implement Prisma CLI just like this.

```
import { PrismaClient } from '@prisma/client'

const prisma = globalThis.prisma || new PrismaClient()

if (process.env.NODE_ENV === 'production') globalThis.prisma = client

export default client
```

Run command for create migrate database

```
npx prisma migrate dev
```

See the question after run command and type any project name.

```
✔ Enter a name for the new migration: <YOUR_PROJECT_NAME>
```

After process we can see the message look like

```
Your database is now in sync with your schema.

✔ Generated Prisma Client (4.10.1 | library) to ./node_modules/@prisma/client in 919ms
```

So, go back to the Railway project, we will see the tables as `_prisma_migrations` and `Post`.


