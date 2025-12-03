# Gosch Resume API

A headless CMS backend built with **Strapi 5**, providing multilingual resume data through a RESTful API. Powers the [gosch-resume-client](https://github.com/ohgosch/gosch-resume-client) frontend application.

## Overview

This is a Strapi-based API that manages and delivers structured resume data including professional experiences, skills, education, and cover letters. Built with internationalization support for English (en-US) and Portuguese (pt-BR).

## Key Features

- **Headless CMS**: Complete content management through Strapi admin panel
- **Multi-language Support**: Built-in i18n for English and Portuguese
- **RESTful API**: Well-structured endpoints for resume data
- **Rich Text Support**: CKEditor integration for formatted content
- **Media Management**: Cloudinary integration for image hosting
- **API Documentation**: Auto-generated documentation with Swagger/OpenAPI
- **Database Support**: SQLite for development, PostgreSQL for production
- **Authentication**: JWT-based API token authentication

## Tech Stack

| Component        | Technology                         |
| ---------------- | ---------------------------------- |
| Framework        | Strapi 5.30.0                      |
| Database         | SQLite 3 (dev) / PostgreSQL (prod) |
| Rich Text Editor | CKEditor                           |
| File Upload      | Cloudinary                         |
| Documentation    | Strapi Documentation Plugin        |
| Node Version     | ≥18.0.0 ≤20.x.x                    |

## Prerequisites

- Node.js ≥18.0.0 ≤20.x.x
- npm ≥6.0.0
- PostgreSQL (for production) or SQLite (for development)

## Installation

```bash
# Clone repository
git clone <repository-url>
cd gosch-resume-api

# Install dependencies
npm install

# Create environment file
cp .env.example .env

# Configure environment variables (see Environment Variables section)
```

## Development

```bash
# Start development server with auto-reload
npm run develop
```

Access the admin panel at [http://localhost:1337/admin](http://localhost:1337/admin).

On first run, create an admin user to access the CMS.

## Production

```bash
# Build the admin panel
npm run build

# Start production server
npm run start
```

## Project Structure

```
├── config/             # Configuration files
│   ├── admin.js        # Admin panel settings
│   ├── api.js          # API configuration
│   ├── database.js     # Database connection
│   ├── middlewares.js  # Middleware configuration
│   ├── plugins.js      # Plugin settings
│   └── server.js       # Server configuration
├── src/
│   ├── admin/          # Admin panel customizations
│   ├── api/            # API endpoints and content types
│   │   ├── company-cover-letter/
│   │   ├── course/
│   │   ├── experience/
│   │   ├── skill/
│   │   ├── skill-category/
│   │   ├── skills-section/
│   │   └── skeleton/
│   └── extensions/     # Plugin extensions
│       └── documentation/
└── public/             # Static files
```

## Content Types

### Skeleton

User profile and personal information

- Name, title, contact details
- Social media links
- Location and availability

### Experience

Professional work experience

- Company, position, period
- Description and responsibilities
- Associated skills and technologies

### Course

Education and certifications

- Institution, course name
- Period and description
- Credentials and links

### Skill

Technical and soft skills

- Skill name and category
- Proficiency level

### Skill Category

Skill groupings

- Category name (e.g., Frontend, Backend, DevOps)

### Skills Section

Organized skill display sections

### Company Cover Letter

Customized cover letters

- Company-specific content
- Rich text formatted

## API Endpoints

Base URL: `http://localhost:1337/api`

### Available Endpoints

| Endpoint                 | Method | Description              |
| ------------------------ | ------ | ------------------------ |
| `/skeletons`             | GET    | User profile data        |
| `/experiences`           | GET    | Work experiences         |
| `/courses`               | GET    | Education and courses    |
| `/skills`                | GET    | All skills               |
| `/skill-categories`      | GET    | Skill categories         |
| `/skills-sections`       | GET    | Organized skill sections |
| `/company-cover-letters` | GET    | Cover letters            |

### Query Parameters

Strapi supports powerful query parameters:

```bash
# Populate relations
GET /api/experiences?populate=*

# Filter by locale
GET /api/experiences?locale=pt-BR

# Pagination
GET /api/experiences?pagination[page]=1&pagination[pageSize]=10

# Sorting
GET /api/experiences?sort=startDate:desc

# Fields selection
GET /api/experiences?fields[0]=company&fields[1]=position
```

## Authentication

API requests require a bearer token:

```bash
curl -H "Authorization: Bearer YOUR_API_TOKEN" \
  http://localhost:1337/api/experiences
```

Generate API tokens in: Admin Panel → Settings → API Tokens → Create new API Token

## Environment Variables

Create a `.env` file with the following variables:

```env
# Server
HOST=0.0.0.0
PORT=1337

# Secrets (generate with: openssl rand -base64 32)
APP_KEYS=app-key-1,app-key-2
API_TOKEN_SALT=your-api-token-salt
ADMIN_JWT_SECRET=your-admin-jwt-secret
TRANSFER_TOKEN_SALT=your-transfer-token-salt
JWT_SECRET=your-jwt-secret

# Database (Development - SQLite)
DATABASE_CLIENT=sqlite
DATABASE_FILENAME=.tmp/data.db

# Database (Production - PostgreSQL)
DATABASE_CLIENT=postgres
DATABASE_HOST=your-db-host
DATABASE_PORT=5432
DATABASE_NAME=your-db-name
DATABASE_USERNAME=your-db-user
DATABASE_PASSWORD=your-db-password
DATABASE_SSL=false

# Cloudinary (Optional - for media upload)
CLOUDINARY_NAME=your-cloudinary-name
CLOUDINARY_KEY=your-cloudinary-key
CLOUDINARY_SECRET=your-cloudinary-secret

# Admin Panel
ADMIN_URL=/admin

# Environment
NODE_ENV=development
```

### Required Variables

| Variable              | Description                       | Required |
| --------------------- | --------------------------------- | -------- |
| `APP_KEYS`            | Encryption keys (comma-separated) | Yes      |
| `API_TOKEN_SALT`      | Salt for API tokens               | Yes      |
| `ADMIN_JWT_SECRET`    | JWT secret for admin              | Yes      |
| `TRANSFER_TOKEN_SALT` | Salt for transfer tokens          | Yes      |
| `JWT_SECRET`          | JWT secret for users              | Yes      |

## Internationalization

The API supports multiple locales through Strapi's i18n plugin:

- **en-US** (English - United States)
- **pt-BR** (Portuguese - Brazil)

To create content in multiple languages:

1. Go to Content Manager
2. Select a content type
3. Create entry in default locale
4. Click "Locales" to add translations

## API Documentation

Auto-generated API documentation is available through the Documentation plugin:

Access at: [http://localhost:1337/documentation](http://localhost:1337/documentation)

The documentation includes:

- All available endpoints
- Request/response schemas
- Authentication requirements
- Query parameter options

## Plugins

### Installed Plugins

- **CKEditor**: Rich text editor with advanced formatting
- **Documentation**: Auto-generated API documentation (Swagger/OpenAPI)
- **Users & Permissions**: Authentication and authorization
- **Internationalization (i18n)**: Multi-language content support
- **Upload**: Media management with Cloudinary provider

## Database

### Development

Uses **SQLite** for easy setup. Database file stored at `.tmp/data.db`.

### Production

Recommended to use **PostgreSQL** for better performance and scalability.

Migration between databases:

```bash
# Export data
npm run strapi export

# Import data
npm run strapi import
```

## Deployment

Strapi can be deployed to various platforms:

- **Strapi Cloud**: Official hosting platform
- **Vercel**: Frontend + backend
- **Heroku**: Container or Node.js buildpack
- **DigitalOcean**: App Platform or Droplet
- **AWS**: ECS, Elastic Beanstalk, or EC2
- **Railway**: Simple deployment

Deployment checklist:

1. Set `NODE_ENV=production`
2. Configure production database (PostgreSQL)
3. Set secure secrets and keys
4. Configure Cloudinary for media
5. Enable SSL/HTTPS
6. Run `npm run build` before starting

## Frontend Client

This API powers the Next.js frontend application:
**[gosch-resume-client](https://github.com/ohgosch/gosch-resume-client)**

The client consumes this API to render a multilingual resume website with server-side rendering and SEO optimization.

## Learn More

- [Strapi Documentation](https://docs.strapi.io) - Official Strapi documentation
- [Strapi CLI](https://docs.strapi.io/dev-docs/cli) - Command line interface guide
- [Strapi Tutorials](https://strapi.io/tutorials) - Community tutorials
- [Strapi Blog](https://strapi.io/blog) - Latest updates and articles
- [Strapi Discord](https://discord.strapi.io) - Community support

## License

MIT License - see [LICENSE](LICENSE) file for details.

---

**Live API**: Powers [resume.gosch.io](https://resume.gosch.io)
