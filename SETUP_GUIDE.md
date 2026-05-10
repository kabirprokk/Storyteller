# StoriesPub - Complete Setup & Deployment Guide

## Project Overview

StoriesPub is a modern content-sharing platform built with Next.js, Firebase, and TailwindCSS. Users can create accounts, post stories/poems/quotes, explore content, like/comment, follow authors, and support creators through donations.

## Tech Stack

- **Frontend**: Next.js 15, React 19, TypeScript
- **Styling**: TailwindCSS 4, Framer Motion
- **Backend**: Firebase (Auth, Firestore, Storage)
- **Payments**: Stripe
- **UI Components**: Radix UI

## Project Structure

```
src/
├── app/                    # Next.js app directory
│   ├── (auth)/            # Auth pages
│   │   ├── login/page.tsx
│   │   ├── signup/page.tsx
│   │   └── forgot-password/page.tsx
│   ├── explore/page.tsx    # Explore feed
│   ├── create/page.tsx     # Create post
│   ├── post/[id]/page.tsx  # View post
│   ├── profile/page.tsx    # User profile
│   ├── donate/page.tsx     # Donation system
│   ├── admin/page.tsx      # Admin dashboard
│   ├── layout.tsx          # Root layout with AuthProvider
│   └── page.tsx            # Landing page
├── components/
│   ├── ui/                 # Reusable UI components
│   │   ├── Button.tsx
│   │   ├── Card.tsx
│   │   ├── Form.tsx
│   │   └── Modal.tsx
│   ├── PostCard.tsx        # Post display component
│   ├── Navigation.tsx      # Main navigation
│   └── ...
├── hooks/                  # Custom React hooks
│   ├── useForm.ts
│   ├── useFetch.ts
│   ├── usePost.ts
│   └── useAuth.ts
├── lib/
│   ├── firebase.ts         # Firebase config
│   ├── auth-utils.ts       # Auth functions
│   ├── firestore-utils.ts  # Firestore CRUD
│   ├── auth-context.tsx    # Auth context provider
│   └── utils.ts            # Utility functions
├── types/
│   └── index.ts            # TypeScript interfaces
└── styles/
    └── globals.css         # Global styles
```

## Setup Instructions

### 1. Prerequisites

- Node.js 18+
- Bun package manager (or npm/yarn)
- Firebase project
- Stripe account (for donations)

### 2. Environment Setup

Create a `.env.local` file in the root directory:

```env
# Firebase Configuration
NEXT_PUBLIC_FIREBASE_API_KEY=your_api_key
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your_project_id
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=your_project.appspot.com
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
NEXT_PUBLIC_FIREBASE_APP_ID=your_app_id

# Stripe Configuration (Optional - for donations)
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_SECRET_KEY=sk_test_...

# Admin Email (for admin dashboard)
NEXT_ADMIN_EMAIL=admin@example.com
```

### 3. Firebase Setup

1. Create a Firebase project at https://console.firebase.google.com
2. Enable Authentication methods:
   - Email/Password
   - Google Sign-In
3. Create Firestore Database (Start in test mode)
4. Enable Storage for image uploads
5. Copy your credentials to `.env.local`

### 4. Firebase Firestore Collections & Schemas

Create these collections in Firestore:

**`users` collection**
```json
{
  "id": "string",
  "email": "string",
  "displayName": "string",
  "photoURL": "string (optional)",
  "bio": "string (optional)",
  "role": "user | creator | admin",
  "createdAt": "timestamp",
  "updatedAt": "timestamp",
  "followersCount": "number",
  "followingCount": "number",
  "totalLikes": "number"
}
```

**`posts` collection**
```json
{
  "id": "string",
  "authorId": "string",
  "title": "string",
  "content": "string",
  "excerpt": "string",
  "type": "story | poem | quote | thought",
  "category": "string",
  "imageUrl": "string (optional)",
  "coverImage": "string (optional)",
  "tags": "array of strings",
  "likes": "number",
  "comments": "number",
  "shares": "number",
  "views": "number",
  "createdAt": "timestamp",
  "updatedAt": "timestamp",
  "isPublished": "boolean",
  "isDraft": "boolean"
}
```

**`comments` collection**
```json
{
  "id": "string",
  "postId": "string",
  "authorId": "string",
  "content": "string",
  "likes": "number",
  "replies": "array",
  "createdAt": "timestamp",
  "updatedAt": "timestamp"
}
```

**`likes` collection**
```json
{
  "id": "string",
  "userId": "string",
  "postId": "string",
  "createdAt": "timestamp"
}
```

**`follows` collection**
```json
{
  "id": "string",
  "followerId": "string",
  "followingId": "string",
  "createdAt": "timestamp"
}
```

**`donations` collection**
```json
{
  "id": "string",
  "donorId": "string",
  "creatorId": "string",
  "amount": "number",
  "currency": "string",
  "stripePaymentId": "string",
  "message": "string (optional)",
  "createdAt": "timestamp",
  "status": "pending | completed | failed"
}
```

### 5. Local Development

```bash
# Install dependencies
bun install

# Run development server
bun run dev

# Type checking
bun run typecheck

# Build for production
bun run build

# Start production server
bun run start
```

The app will be available at `http://localhost:3000`

## Features Implemented

### ✅ Core Features
- User authentication (Email/Password + Google OAuth)
- Create, read, update, delete posts
- Multiple post types (stories, poems, quotes, thoughts)
- Post categorization and tagging
- Search functionality
- User profiles with bio and photo

### ✅ Engagement
- Like posts
- Comment on posts
- Follow/unfollow authors
- View count tracking
- Share posts

### 📋 Features to Implement

These require backend API endpoints or additional setup:

1. **Post Creation Page** (`/create`)
   - Rich text editor
   - Image upload to Firebase Storage
   - Draft auto-save
   - Category and tag selection

2. **Post Reader** (`/post/[id]`)
   - Beautiful distraction-free reader view
   - Comments section
   - Like/bookmark actions
   - Author info card with follow button

3. **User Profile** (`/profile`)
   - User stats (posts, followers, total views)
   - Edit bio and photo
   - List of user's posts
   - Follower/following list

4. **Donation System** (`/donate/[userId]`)
   - Stripe integration
   - Payment processing
   - Donation history

5. **Admin Dashboard** (`/admin`)
   - User management
   - Content moderation
   - Analytics
   - System settings

6. **Additional Features**
   - Notifications system
   - Dark mode toggle
   - Reading time calculation
   - Trending posts section
   - AI content suggestions
   - Plagiarism detection

## Deployment

### Deploy to Vercel (Recommended)

1. Push your code to GitHub
2. Go to https://vercel.com/import
3. Select your GitHub repository
4. Add environment variables from `.env.local`
5. Deploy

### Deploy to Firebase Hosting

```bash
# Install Firebase CLI
npm install -g firebase-tools

# Login to Firebase
firebase login

# Initialize Firebase hosting
firebase init hosting

# Build and deploy
bun run build
firebase deploy
```

### Deploy to Netlify

1. Connect your GitHub repository to Netlify
2. Set environment variables in Netlify UI
3. Netlify will auto-build and deploy on push

## Security Best Practices

1. **Firebase Security Rules**: Configure Firestore rules to protect data
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       // Users can only read/write their own data
       match /users/{userId} {
         allow read, write: if request.auth.uid == userId;
       }
       
       // Posts are publicly readable but only author can write
       match /posts/{postId} {
         allow read: if resource.data.isPublished == true;
         allow write: if request.auth.uid == resource.data.authorId;
         allow create: if request.auth.uid != null;
       }
       
       // Comments and likes follow similar patterns
       match /comments/{commentId} {
         allow read: if true;
         allow write: if request.auth.uid == request.resource.data.authorId;
       }
     }
   }
   ```

2. **API Keys**: Never expose secret keys in frontend code
3. **Rate Limiting**: Implement rate limiting for API calls
4. **Input Validation**: Validate all user inputs
5. **HTTPS**: Ensure all communication is encrypted

## Database Indexes

For optimal query performance, create these indexes in Firestore:

- `posts`: Collection Group Index on `type` + `createdAt`
- `posts`: Single Field Index on `authorId`
- `posts`: Single Field Index on `isPublished`
- `comments`: Single Field Index on `postId`
- `likes`: Collection Group Index on `userId` + `postId`
- `follows`: Single Field Index on `followerId`

## Environment Variables Reference

| Variable | Description | Example |
|----------|-------------|---------|
| `NEXT_PUBLIC_FIREBASE_API_KEY` | Firebase API key | `AIzaSyD...` |
| `NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN` | Firebase auth domain | `myapp.firebaseapp.com` |
| `NEXT_PUBLIC_FIREBASE_PROJECT_ID` | Firebase project ID | `myapp-12345` |
| `NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET` | Firebase storage | `myapp-12345.appspot.com` |
| `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY` | Stripe public key | `pk_test_...` |
| `STRIPE_SECRET_KEY` | Stripe secret key | `sk_test_...` |

## Performance Optimization Tips

1. **Image Optimization**: Use Next.js Image component
2. **Code Splitting**: Use dynamic imports for large components
3. **Caching**: Configure ISR (Incremental Static Regeneration)
4. **Database**: Create proper indexes in Firestore
5. **Lazy Loading**: Implement pagination/infinite scroll

## Troubleshooting

### Firebase Connection Issues
- Check `.env.local` has correct credentials
- Verify Firebase project allows your domain
- Check browser console for CORS errors

### Authentication Not Working
- Verify Google OAuth redirect URIs in Firebase Console
- Check email/password provider is enabled
- Clear browser cache and cookies

### Build Errors
- Run `bun run typecheck` to check TypeScript errors
- Clear `.next` folder: `rm -rf .next && bun run build`
- Check Node.js version: `node --version`

## Next Steps

1. Complete the remaining pages (create, profile, donation, admin)
2. Add Stripe integration for payments
3. Implement real-time notifications
4. Set up email notifications
5. Create mobile app (React Native)
6. Add analytics and monitoring
7. Implement advanced search with Algolia
8. Add AI features for content suggestions

## Contributing

1. Create a feature branch
2. Make your changes
3. Run `bun run typecheck` and `bun run build`
4. Submit a pull request

## License

MIT License - see LICENSE file for details

## Support

For issues and questions:
- GitHub Issues: [project-repo/issues](https://github.com/yourusername/stories-pub/issues)
- Email: support@storiespub.com
- Documentation: [docs.storiespub.com](https://docs.storiespub.com)
