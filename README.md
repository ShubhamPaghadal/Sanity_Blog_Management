# Sanity Blog Management Demo

A simple blog management system built with **Sanity CMS** and **Next.js**. This project demonstrates how to create, manage, query, and display blog content using Sanity.

## 🚀 Tech Stack

* Next.js
* React
* TypeScript
* Sanity CMS
* GROQ
* `next-sanity`

## 📁 Project Structure

```text
project/
│
├── sanity/
│   ├── lib/
│   │   ├── client.ts
│   │   └── queries.ts
│   │
│   └── schemaTypes/
│       ├── postType.ts
│       ├── authorType.ts
│       └── index.ts
│
├── app/
│   └── blog/
│       └── page.tsx
│
├── .env.local
├── package.json
└── README.md
```

## 📝 Features

* Create blog posts using Sanity Studio
* Edit and publish blog posts
* Manage authors
* Add blog titles, slugs, excerpts, images, and content
* Query blog posts using GROQ
* Display Sanity content in Next.js
* Connect authors with blog posts using references
* Sort posts by published date

## ⚙️ Sanity Setup

### 1. Install dependencies

Install the Sanity Next.js integration:

```bash
npm install next-sanity
```

If you are creating the Sanity Studio separately, follow the Sanity project setup instructions.

### 2. Environment Variables

Create a `.env.local` file in the Next.js project root:

```env
NEXT_PUBLIC_SANITY_PROJECT_ID=your_project_id
NEXT_PUBLIC_SANITY_DATASET=production
```

Replace:

```text
your_project_id
```

with your actual Sanity Project ID.

The dataset is usually:

```text
production
```

## 🔌 Sanity Client

Create:

```text
sanity/lib/client.ts
```

Example:

```ts
import { createClient } from "next-sanity";

export const client = createClient({
  projectId: process.env.NEXT_PUBLIC_SANITY_PROJECT_ID!,
  dataset: process.env.NEXT_PUBLIC_SANITY_DATASET!,
  apiVersion: "2026-08-10",
  useCdn: true,
});
```

## 🔎 GROQ Query

Create:

```text
sanity/lib/queries.ts
```

Example:

```ts
import { defineQuery } from "next-sanity";

export const POSTS_QUERY = defineQuery(`
  *[_type == "post"] | order(publishedAt desc) {
    _id,
    title,
    slug,
    excerpt,
    mainImage,
    publishedAt,
    body,
    author->{
      name
    }
  }
`);
```

This query retrieves published blog posts from Sanity.

## 🖥️ Display Posts in Next.js

Example:

```tsx
import { client } from "@/sanity/lib/client";
import { POSTS_QUERY } from "@/sanity/lib/queries";

export default async function BlogPage() {
  const posts = await client.fetch(POSTS_QUERY);

  return (
    <main>
      <h1>My Blog</h1>

      {posts.map((post) => (
        <article key={post._id}>
          <h2>{post.title}</h2>

          <p>{post.excerpt}</p>

          <p>Slug: {post.slug?.current}</p>

          <p>Author: {post.author?.name}</p>
        </article>
      ))}
    </main>
  );
}
```

## ✍️ Blog Post Schema

A blog post contains fields such as:

```text
Blog Post
│
├── Title
├── Slug
├── Excerpt
├── Main Image
├── Published Date
├── Author
└── Body
```

Example:

```text
Title:
Getting Started With Sanity CMS

Slug:
getting-started-with-sanity-cms

Excerpt:
A beginner-friendly guide to building a modern blog
using Sanity CMS and Next.js.
```

## 👤 Author

Authors can be stored as separate Sanity documents.

Example:

```text
Author
│
├── Name
├── Profile Image
└── Bio
```

A blog post can reference an author instead of storing the author's information directly.

Example:

```text
Blog Post
    │
    └── Author
          │
          ▼
       Author Document
```

## 🔄 How It Works

The complete data flow is:

```text
Sanity Studio
      │
      │ Create / Edit / Publish
      ▼
Sanity Content Lake
      │
      │ GROQ Query
      ▼
Sanity Client
      │
      ▼
Next.js
      │
      ▼
Blog UI
```

## ▶️ Run the Project

Start the Next.js application:

```bash
npm run dev
```

Then open:

```text
http://localhost:3000
```

For the blog page:

```text
http://localhost:3000/blog
```

If you have a separate Sanity Studio, start it with:

```bash
npm run dev
```

and open the Studio URL shown in your terminal.

## 🧪 Testing the Sanity Connection

You can test your GROQ query directly from **Sanity Studio → Vision**:

```groq
*[_type == "post"]{
  _id,
  title,
  slug,
  excerpt,
  publishedAt
}
```

If your published posts appear in the result, your Sanity content is working correctly.

## 📌 Important Sanity Concepts

### Schema

Defines the structure of your content.

### Document

An actual piece of content created from a schema.

### Dataset

A collection of documents inside your Sanity project.

### Sanity Studio

The interface used to create and manage content.

### GROQ

Sanity's query language used to retrieve content.

### Reference

Connects one document to another, such as a blog post to an author.

### Portable Text

Sanity's structured rich-text format used for blog content.

## 🎯 Future Improvements

This demo can be extended with:

* Dynamic blog routes using `/blog/[slug]`
* Blog post detail pages
* Sanity image optimization
* Portable Text rendering
* Categories
* Search and filtering
* Pagination
* Draft previews
* Live content updates
* SEO metadata
* Related posts
* Deployment

## 📚 Learning Goal

The main goal of this project is to understand the complete Sanity + Next.js workflow:

```text
Create Schema
      ↓
Create Content
      ↓
Publish Content
      ↓
Write GROQ Query
      ↓
Fetch Content
      ↓
Display Content in Next.js
```

This project is intended as a learning/demo project for understanding how a headless CMS can be integrated with a modern Next.js application.
