# Tech Practice Platform

A simple test platform where tech professionals can connect for random short calls/texts to practice communication and interview-style conversations.

## Features

- **Authentication & Verification**: Email signup with LinkedIn/GitHub verification
- **User Profiles**: Complete profile setup with experience level and tech stack
- **Smart Matching**: Queue-based matching system with preference support
- **Real-time Chat**: Text conversations with 10-minute timer
- **Video Calls**: WebRTC-based video calling
- **Interview Prompts**: Random technical, behavioral, and system design prompts
- **Modern UI**: Responsive design with dark mode support

## Tech Stack

- **Frontend**: React 18 + Vite + TypeScript + Tailwind CSS
- **Backend**: Supabase (PostgreSQL + Auth + Realtime)
- **Video**: WebRTC (native browser APIs)
- **Routing**: React Router v6

## Prerequisites

- Node.js 18+ and npm
- A Supabase account and project
- (Optional) GitHub OAuth app for verification
- (Optional) LinkedIn OAuth app for verification

## Setup Instructions

### 1. Clone and Install

```bash
npm install
```

### 2. Set Up Supabase

1. Create a new project at [supabase.com](https://supabase.com)
2. Go to SQL Editor and run the migrations:
   - `supabase/migrations/001_initial_schema.sql`
   - `supabase/migrations/002_seed_prompts.sql`

3. Enable Realtime for the `messages` table:
   - Go to Database > Replication
   - Enable replication for `messages` table

4. Get your Supabase credentials:
   - Go to Settings > API
   - Copy your Project URL and anon/public key

### 3. Configure Environment Variables

Create a `.env` file in the root directory:

```env
VITE_SUPABASE_URL=your_supabase_project_url
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key
VITE_GITHUB_CLIENT_ID=your_github_oauth_client_id (optional)
VITE_LINKEDIN_CLIENT_ID=your_linkedin_oauth_client_id (optional)
```

### 4. Set Up OAuth (Optional)

#### GitHub OAuth
1. Go to GitHub Settings > Developer settings > OAuth Apps
2. Create a new OAuth App
3. Set Authorization callback URL to: `http://localhost:5173/auth/callback/github`
4. Copy the Client ID to your `.env` file

#### LinkedIn OAuth
1. Go to LinkedIn Developers > My Apps
2. Create a new app
3. Add redirect URL: `http://localhost:5173/auth/callback/linkedin`
4. Copy the Client ID to your `.env` file

### 5. Run the Development Server

```bash
npm run dev
```

The app will be available at `http://localhost:5173`

## Project Structure

```
src/
├── components/          # React components
│   ├── auth/          # Authentication components
│   ├── chat/          # Chat interface components
│   ├── interview/     # Interview prompt components
│   ├── layout/        # Layout components (Header, Sidebar)
│   ├── matching/      # Matching queue components
│   ├── profile/       # Profile management components
│   ├── ui/            # Reusable UI components
│   └── video/         # Video call components
├── contexts/          # React contexts (Auth, etc.)
├── hooks/             # Custom React hooks
├── lib/               # Utility functions and helpers
├── pages/             # Page components
└── router.tsx         # React Router configuration

supabase/
└── migrations/        # Database migration files
```

## Database Schema

The platform uses the following main tables:

- `profiles` - User profiles with verification status
- `conversations` - Conversation records
- `messages` - Chat messages
- `interview_prompts` - Interview questions/prompts
- `conversation_prompts` - Prompts assigned to conversations
- `user_preferences` - User matching preferences
- `feedback` - Post-conversation feedback

## Key Features Implementation

### Authentication
- Email/password authentication via Supabase Auth
- Protected routes with `ProtectedRoute` component
- Session management via `AuthContext`

### Matching System
- Users join a queue by setting availability to "online"
- Matching algorithm finds available partners
- Supports preference-based matching (experience level, topics)

### Real-time Chat
- Uses Supabase Realtime subscriptions
- Messages sync instantly across clients
- 10-minute conversation timer

### Video Calls
- WebRTC-based peer-to-peer video
- Mute/unmute and video toggle controls
- Local and remote video streams

### Interview Prompts
- Random prompt selection from database
- Categorized by type (technical, behavioral, system design)
- Difficulty levels (junior, mid, senior)

## Building for Production

```bash
npm run build
```

The built files will be in the `dist` directory, ready for deployment to Vercel, Netlify, or any static hosting service.

## Deployment

### Frontend (Vercel)

The repo includes a `vercel.json` that builds with Vite, serves the `dist` directory, and rewrites all routes to `index.html` so client-side routes (e.g. `/dashboard`) work on refresh and direct links.

1. Push your code to GitHub
2. Import the project in Vercel (the Vite framework preset is detected automatically)
3. In Project Settings > Environment Variables, add:
   - `VITE_SUPABASE_URL`
   - `VITE_SUPABASE_ANON_KEY`
   - `VITE_GITHUB_CLIENT_ID` (optional)
   - `VITE_LINKEDIN_CLIENT_ID` (optional)
4. Deploy (redeploy after changing environment variables, since `VITE_` variables are baked in at build time)
5. In Supabase, go to Authentication > URL Configuration and add your Vercel domain (e.g. `https://your-app.vercel.app`) to the Site URL and Redirect URLs

### Database (Supabase)

The database is already hosted on Supabase. Make sure to:
- Run migrations in production Supabase project
- Update environment variables with production URLs
- Configure OAuth redirect URLs for production domain

## Security Considerations

- Row Level Security (RLS) policies protect all database tables
- Users can only access their own data and conversations they're part of
- OAuth state validation for verification flows
- Input sanitization for user-generated content

## Future Enhancements

- Call recordings
- Advanced feedback system
- Matching history and analytics
- Mobile app
- Group conversations
- Scheduled conversations

## License

MIT
