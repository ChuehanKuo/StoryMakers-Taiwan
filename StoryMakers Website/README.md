# StoryMakers Taiwan Website

A modern, responsive website for StoryMakers Taiwan (故事造城), featuring community storytelling, cultural preservation, and urban regeneration projects.

## Features

- **Static Site**: HTML/CSS/JavaScript (no frameworks)
- **Supabase Backend**: Community submissions and moderation
- **Story System**: Community-contributed stories with approval workflow
- **Responsive Design**: Mobile-first, works on all devices
- **Bilingual**: English and Traditional Chinese support

## Setup Instructions

### 1. Prerequisites

- A Supabase account (sign up at [supabase.com](https://supabase.com))
- A web server (or use Python's built-in server for development)

### 2. Supabase Setup

#### Step 1: Create a Supabase Project

1. Go to [supabase.com](https://supabase.com) and create a new project
2. Wait for the project to be fully provisioned (takes a few minutes)

#### Step 2: Run Database Schema

1. In your Supabase dashboard, go to **SQL Editor**
2. Open the file `supabase/schema.sql` from this project
3. Copy and paste the entire SQL script into the SQL Editor
4. Click **Run** to execute the schema
5. Verify that all tables were created successfully

#### Step 3: Create Storage Bucket

1. Go to **Storage** in your Supabase dashboard
2. Click **New bucket**
3. Name: `post-images`
4. Set to **Private** (not public)
5. Click **Create bucket**

#### Step 4: Configure Storage Policies

The storage policies are included in `supabase/schema.sql`. If they weren't applied, you can add them manually:

1. Go to **Storage** → **Policies** → **post-images**
2. Add the following policies (or run the storage policy SQL from schema.sql):
   - **Public can upload images**: Allow INSERT for `anon` role
   - **Public can view images**: Allow SELECT for `anon` role
   - **Users can delete their own uploads**: Allow DELETE for `anon` role

#### Step 5: Create Admin User

1. Go to **Authentication** → **Users** in Supabase dashboard
2. Click **Add user** → **Create new user**
3. Enter an email and password for your admin account
4. Note: You'll need to set the user's role to 'admin' in the database

To set admin role:
1. Go to **SQL Editor**
2. Run this query (replace `admin@example.com` with your admin email):

```sql
UPDATE public.profiles
SET role = 'admin'
WHERE email = 'admin@example.com';
```

If the profile doesn't exist yet, create it:

```sql
INSERT INTO public.profiles (id, email, role)
SELECT id, email, 'admin'
FROM auth.users
WHERE email = 'admin@example.com';
```

### 3. Configure the Website

#### Step 1: Update Config File

1. Open `config.js`
2. Replace the placeholder values with your Supabase credentials:
   - `YOUR_SUPABASE_URL`: Found in **Settings** → **API** → **Project URL**
   - `YOUR_SUPABASE_ANON_KEY`: Found in **Settings** → **API** → **Project API keys** → **anon public**

```javascript
const CONFIG = {
    supabase: {
        url: 'https://your-project.supabase.co',
        anonKey: 'your-anon-key-here'
    }
};
```

### 4. Run the Website

#### Option 1: Python HTTP Server (Development)

```bash
cd "/Users/chuehankuo/Desktop/StoryMakers Website"
python3 -m http.server 8000
```

Then open `http://localhost:8000` in your browser.

#### Option 2: Any Web Server

Upload all files to your web server and access via your domain.

## File Structure

```
StoryMakers Website/
├── index.html              # Home page
├── about.html              # About us page
├── projects.html           # Projects listing
├── shilin.html             # Shilin Story Chronicle project page
├── stories.html            # Stories listing (fetches from Supabase)
├── story.html              # Individual story page
├── submit.html             # Story submission form
├── admin.html              # Admin moderation panel
├── contact.html            # Contact page
├── config.js               # Supabase configuration (UPDATE THIS!)
├── data/
│   └── posts.json          # Legacy JSON file (optional, for fallback)
├── scripts/
│   ├── main.js             # Main site JavaScript
│   ├── supabaseClient.js   # Supabase client initialization
│   ├── stories.js          # Stories listing logic
│   ├── story.js            # Story detail logic
│   ├── submit.js           # Story submission handler
│   └── admin.js            # Admin panel logic
├── styles/
│   └── main.css            # Main stylesheet
├── assets/
│   └── images/             # Image assets
└── supabase/
    └── schema.sql          # Database schema and RLS policies
```

## Features Overview

### Public Features

- **Browse Stories**: View approved community stories
- **Submit Stories**: Submit stories with photos (status: pending)
- **View Story Details**: Read full story content with images

### Admin Features

- **Moderation Panel**: Review pending submissions
- **Approve/Reject**: Approve or reject story submissions
- **View All Posts**: See all posts regardless of status

## Security

- **Row Level Security (RLS)**: Enabled on all tables
- **Public Access**: Can only create posts with `pending` status
- **Admin Access**: Only users with `role = 'admin'` can approve/reject
- **Storage**: Private bucket with signed URLs for images

## Troubleshooting

### Stories Not Loading

1. Check browser console for errors
2. Verify `config.js` has correct Supabase credentials
3. Ensure Supabase project is active
4. Check that RLS policies are correctly set

### Image Upload Failing

1. Verify storage bucket `post-images` exists
2. Check storage policies allow public uploads
3. Verify file size is under 5MB
4. Check file format is supported (JPG, PNG, WebP, GIF)

### Admin Login Not Working

1. Verify admin user exists in Supabase Auth
2. Check that user's profile has `role = 'admin'`
3. Ensure RLS policies allow admin access
4. Check browser console for authentication errors

### Submission Not Appearing

1. Check that post was created with `status = 'pending'`
2. Verify RLS policies allow public to insert pending posts
3. Check browser console for errors
4. Verify Supabase client is initialized correctly

## Development Notes

- All JavaScript is vanilla (no frameworks)
- Supabase JS library loaded via CDN
- No build process required
- Works with any static file server

## Support

For issues or questions:
- Check Supabase documentation: [supabase.com/docs](https://supabase.com/docs)
- Review browser console for error messages
- Verify all configuration steps were completed

## License

© 2025 StoryMakers Taiwan (故事造城). All rights reserved.

