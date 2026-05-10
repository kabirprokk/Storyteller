# StoriesPub - API & Function Reference

## Authentication Functions

### `signup(email, password, displayName)`
Register a new user with email and password
```typescript
import { signup } from '@/lib/auth-utils';

await signup('user@example.com', 'password123', 'John Doe');
```

### `login(email, password)`
Log in existing user
```typescript
import { login } from '@/lib/auth-utils';

await login('user@example.com', 'password123');
```

### `loginWithGoogle()`
Sign in with Google account
```typescript
import { loginWithGoogle } from '@/lib/auth-utils';

await loginWithGoogle();
```

### `logout()`
Log out current user
```typescript
import { logout } from '@/lib/auth-utils';

await logout();
```

### `getCurrentUser()`
Get current Firebase user
```typescript
import { getCurrentUser } from '@/lib/auth-utils';

const user = await getCurrentUser();
```

### `onAuthChange(callback)`
Subscribe to auth state changes
```typescript
import { onAuthChange } from '@/lib/auth-utils';

const unsubscribe = onAuthChange((user) => {
  if (user) console.log('User logged in:', user.email);
});

// Cleanup
unsubscribe();
```

## User Functions

### `getUserById(userId)`
Get user profile by ID
```typescript
import { getUserById } from '@/lib/firestore-utils';

const user = await getUserById('user-id-123');
```

### `createUser(userId, userData)`
Create new user in Firestore
```typescript
import { createUser } from '@/lib/firestore-utils';

await createUser('user-id-123', {
  email: 'user@example.com',
  displayName: 'John Doe',
  photoURL: 'https://...'
});
```

### `updateUser(userId, data)`
Update user profile
```typescript
import { updateUser } from '@/lib/firestore-utils';

await updateUser('user-id-123', {
  bio: 'I am a writer',
  photoURL: 'https://...'
});
```

## Post Functions

### `getPosts(constraints?, pageLimit)`
Get posts with optional filters
```typescript
import { getPosts } from '@/lib/firestore-utils';
import { where, orderBy } from 'firebase/firestore';

// Get all published posts
const posts = await getPosts([
  where('isPublished', '==', true),
  orderBy('createdAt', 'desc')
], 10);
```

### `getPostById(postId)`
Get single post by ID
```typescript
import { getPostById } from '@/lib/firestore-utils';

const post = await getPostById('post-123');
```

### `createPost(userId, postData)`
Create new post
```typescript
import { createPost } from '@/lib/firestore-utils';

const postId = await createPost('user-123', {
  title: 'My Story',
  content: 'Once upon a time...',
  excerpt: 'A great story',
  type: 'story',
  tags: ['adventure', 'fiction']
});
```

### `updatePost(postId, data)`
Update existing post
```typescript
import { updatePost } from '@/lib/firestore-utils';

await updatePost('post-123', {
  title: 'Updated Title',
  isPublished: true
});
```

### `deletePost(postId)`
Delete a post
```typescript
import { deletePost } from '@/lib/firestore-utils';

await deletePost('post-123');
```

### `searchPosts(queryText, maxResults)`
Search posts by title or tags
```typescript
import { searchPosts } from '@/lib/firestore-utils';

const results = await searchPosts('adventure', 20);
```

## Comment Functions

### `getPostComments(postId)`
Get all comments for a post
```typescript
import { getPostComments } from '@/lib/firestore-utils';

const comments = await getPostComments('post-123');
```

### `addComment(postId, userId, content)`
Add comment to post
```typescript
import { addComment } from '@/lib/firestore-utils';

const commentId = await addComment('post-123', 'user-123', 'Great post!');
```

## Like Functions

### `toggleLike(postId, userId)`
Like or unlike a post
```typescript
import { toggleLike } from '@/lib/firestore-utils';

await toggleLike('post-123', 'user-123');
```

### `isPostLikedByUser(postId, userId)`
Check if user liked post
```typescript
import { isPostLikedByUser } from '@/lib/firestore-utils';

const liked = await isPostLikedByUser('post-123', 'user-123');
```

## Follow Functions

### `toggleFollow(followerId, followingId)`
Follow or unfollow user
```typescript
import { toggleFollow } from '@/lib/firestore-utils';

await toggleFollow('user-123', 'author-456');
```

## Donation Functions

### `createDonation(donationData)`
Create donation record
```typescript
import { createDonation } from '@/lib/firestore-utils';

const donationId = await createDonation({
  donorId: 'user-123',
  creatorId: 'author-456',
  amount: 1000,
  currency: 'USD',
  stripePaymentId: 'pi_...',
  message: 'Love your work!'
});
```

### `getUserDonations(userId)`
Get donations received by user
```typescript
import { getUserDonations } from '@/lib/firestore-utils';

const donations = await getUserDonations('author-456');
```

## Custom Hooks

### `useAuth()`
Get authentication context
```typescript
import { useAuth } from '@/lib/auth-context';

const { user, firebaseUser, loading, error } = useAuth();
```

### `usePosts(filters?)`
Fetch posts
```typescript
import { usePosts } from '@/hooks/usePost';

const { posts, loading, error, refetch } = usePosts();
```

### `usePost(postId)`
Fetch single post
```typescript
import { usePost } from '@/hooks/usePost';

const { post, loading, error } = usePost('post-123');
```

### `usePostComments(postId)`
Fetch post comments
```typescript
import { usePostComments } from '@/hooks/usePost';

const { comments, loading, error, refetch } = usePostComments('post-123');
```

### `useLike(postId, userId)`
Manage post like
```typescript
import { useLike } from '@/hooks/usePost';

const { liked, loading, toggleLike } = useLike('post-123', 'user-123');
```

### `useForm(initialValues, onSubmit)`
Form state management
```typescript
import { useForm } from '@/hooks/useForm';

const { values, errors, isSubmitting, handleChange, handleSubmit } = useForm(
  { email: '', password: '' },
  async (values) => {
    await login(values.email, values.password);
  }
);
```

### `useFetch(url, options?, dependencies)`
Fetch data from API
```typescript
import { useFetch } from '@/hooks/useFetch';

const { data, error, loading, refetch } = useFetch(
  '/api/posts',
  { method: 'GET' }
);
```

## Utility Functions

### `formatDate(date)`
Format date to readable string
```typescript
import { formatDate } from '@/lib/utils';

formatDate(new Date()); // "May 10, 2024"
```

### `formatTimeAgo(date)`
Format date as time ago
```typescript
import { formatTimeAgo } from '@/lib/utils';

formatTimeAgo(new Date(Date.now() - 3600000)); // "1 hour ago"
```

### `truncateText(text, length)`
Truncate text with ellipsis
```typescript
import { truncateText } from '@/lib/utils';

truncateText('Long story...', 10); // "Long sto..."
```

### `formatNumber(num)`
Format number to K/M notation
```typescript
import { formatNumber } from '@/lib/utils';

formatNumber(1500);     // "1.5K"
formatNumber(1500000);  // "1.5M"
```

### `validateEmail(email)`
Validate email format
```typescript
import { validateEmail } from '@/lib/utils';

validateEmail('user@example.com'); // true
```

### `getInitials(name)`
Get name initials
```typescript
import { getInitials } from '@/lib/utils';

getInitials('John Doe'); // "JD"
```

### `generateSlug(text)`
Generate URL-friendly slug
```typescript
import { generateSlug } from '@/lib/utils';

generateSlug('My Amazing Story'); // "my-amazing-story"
```

## Component Props

### Button
```typescript
interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'ghost' | 'danger';
  size?: 'sm' | 'md' | 'lg';
  isLoading?: boolean;
  onClick?: () => void;
  disabled?: boolean;
  children: React.ReactNode;
}

<Button variant="primary" size="lg" isLoading={false}>
  Click me
</Button>
```

### Card
```typescript
interface CardProps {
  hover?: boolean;
  shadow?: 'sm' | 'md' | 'lg' | 'none';
  children: React.ReactNode;
}

<Card hover shadow="md">
  Content here
</Card>
```

### Input
```typescript
interface InputProps {
  label?: string;
  error?: string;
  helpText?: string;
  icon?: React.ReactNode;
  placeholder?: string;
  type?: string;
}

<Input
  label="Email"
  type="email"
  placeholder="you@example.com"
  error={errors.email}
/>
```

### PostCard
```typescript
interface PostCardProps {
  post: Post;
  onLike?: () => void;
  liked?: boolean;
  onBookmark?: () => void;
  bookmarked?: boolean;
}

<PostCard post={post} onLike={handleLike} liked={isLiked} />
```

## TypeScript Types

```typescript
// User
interface User {
  id: string;
  email: string;
  displayName: string;
  photoURL?: string;
  bio?: string;
  role: 'user' | 'creator' | 'admin';
  createdAt: Date;
  updatedAt: Date;
  followersCount: number;
  followingCount: number;
  totalLikes: number;
}

// Post
interface Post {
  id: string;
  authorId: string;
  author: Partial<User>;
  title: string;
  content: string;
  excerpt: string;
  type: 'story' | 'poem' | 'quote' | 'thought';
  category: string;
  imageUrl?: string;
  coverImage?: string;
  tags: string[];
  likes: number;
  comments: number;
  shares: number;
  views: number;
  createdAt: Date;
  updatedAt: Date;
  isPublished: boolean;
  isDraft?: boolean;
}

// Comment
interface Comment {
  id: string;
  postId: string;
  authorId: string;
  author: Partial<User>;
  content: string;
  likes: number;
  replies: Comment[];
  createdAt: Date;
  updatedAt: Date;
}

// Donation
interface Donation {
  id: string;
  donorId: string;
  creatorId: string;
  amount: number;
  currency: string;
  stripePaymentId: string;
  message?: string;
  createdAt: Date;
  status: 'pending' | 'completed' | 'failed';
}
```

## Environment Variables

```env
# Firebase
NEXT_PUBLIC_FIREBASE_API_KEY=your_api_key
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your_domain
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your_project_id
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=your_bucket
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
NEXT_PUBLIC_FIREBASE_APP_ID=your_app_id

# Stripe (Optional)
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_SECRET_KEY=sk_test_...
```

## Common Patterns

### Protected Component
```typescript
import { useAuth } from '@/lib/auth-context';
import { useRouter } from 'next/navigation';

export default function ProtectedPage() {
  const { user, loading } = useAuth();
  const router = useRouter();

  if (loading) return <div>Loading...</div>;
  if (!user) {
    router.push('/login');
    return null;
  }

  return <div>Protected content</div>;
}
```

### Form with Validation
```typescript
import { useForm } from '@/hooks/useForm';
import { Input } from '@/components/ui/Form';

export default function LoginForm() {
  const { values, errors, isSubmitting, handleChange, handleSubmit } = useForm(
    { email: '', password: '' },
    async (formData) => {
      await login(formData.email, formData.password);
    }
  );

  return (
    <form onSubmit={handleSubmit}>
      <Input
        name="email"
        value={values.email}
        onChange={handleChange}
        error={errors.email}
      />
      <Input
        type="password"
        name="password"
        value={values.password}
        onChange={handleChange}
        error={errors.password}
      />
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Loading...' : 'Login'}
      </button>
    </form>
  );
}
```

### Fetch with Error Handling
```typescript
import { usePosts } from '@/hooks/usePost';

export default function PostsList() {
  const { posts, loading, error, refetch } = usePosts();

  if (loading) return <div>Loading posts...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <div>
      {posts.map(post => (
        <PostCard key={post.id} post={post} />
      ))}
      <button onClick={refetch}>Refresh</button>
    </div>
  );
}
```

---

This is your complete API reference. All functions are typed with TypeScript for optimal development experience.
