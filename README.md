#Social Feed Project
A React-based social media feed ReactJS application with features like creating posts, liking, and commenting. Built with React and styled using Tailwind CSS.

Features
Create new posts
Like posts
Add comments to posts
Responsive design
Real-time updates
Project Setup & Structure
social-feed/
├── src/
│   ├── components/
│   │   ├── CreatePost.jsx
│   │   ├── Post.jsx
│   │   └── PostList.jsx
│   ├── data/
│   │   └── sampleData.js
│   ├── App.jsx
│   ├── main.jsx
│   └── index.css
├── index.html
├── package.json
├── tailwind.config.js
└── postcss.config.js
Prerequisites
Node.js (v16 or higher)
npm (v8 or higher)
React DevTool
Installation
Create a new project using Vite:
npm create vite@latest social-feed -- --template react
cd social-feed
Install dependencies:
npm install
Install Tailwind CSS and its peer dependencies:
npm install -D @tailwind/postcss tailwindcss postcss autoprefixer
Generate Tailwind CSS and PostCSS configuration files:
npx tailwindcss init -p 
Note :: if you hit an error runing npx tailwindcss init -p see troubleshooting

Add Tailwind directives to src/index.css:
@import "tailwindcss"
Configuration Files
tailwind.config.js
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
postcss.config.js
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
Component Files
src/data/sampleData.js
export const initialPosts = [
  {
    id: 1,
    author: {
      name: "John Doe",
      avatar: "/api/placeholder/50/50"
    },
    content: "Just had an amazing weekend! 🎉",
    likes: 5,
    comments: [
      {
        id: 1,
        author: "Jane Smith",
        text: "Looks awesome!",
        timestamp: "2024-01-27T10:00:00Z"
      }
    ],
    timestamp: "2024-01-27T09:00:00Z"
  },
  {
    id: 2,
    author: {
      name: "Jane Smith",
      avatar: "/api/placeholder/50/50"
    },
    content: "Working on a new project with React and Tailwind CSS 💻",
    likes: 3,
    comments: [],
    timestamp: "2024-01-27T08:30:00Z"
  }
];
src/components/CreatePost.jsx
import { useState } from 'react';

function CreatePost({ onCreatePost }) {
  const [content, setContent] = useState('');
  
  const handleSubmit = (e) => {
    e.preventDefault();
    if (content.trim()) {
      onCreatePost(content);
      setContent('');
    }
  };

  return (
    <div className="bg-white rounded-lg shadow p-4 mb-6">
      <form onSubmit={handleSubmit}>
        <textarea
          value={content}
          onChange={(e) => setContent(e.target.value)}
          placeholder="What's on your mind?"
          className="w-full px-3 py-2 border rounded-lg mb-4"
          rows="3"
        />
        <button 
          type="submit"
          className="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700"
        >
          Post
        </button>
      </form>
    </div>
  );
}

export default CreatePost;
src/components/PostList.jsx
import { useState } from 'react';
import Post from './Post';

function PostList({ posts, onLike, onComment }) {
  return (
    <div className="space-y-4">
      {posts.map(post => (
        <Post 
          key={post.id}
          post={post}
          onLike={onLike}
          onComment={onComment}
        />
      ))}
    </div>
  );
}

export default PostList;
src/components/Post.jsx
import { useState } from 'react';

function Post({ post, onLike, onComment }) {
  const [commentText, setCommentText] = useState('');
  const { author, content, likes, comments, timestamp } = post;
  
  const handleSubmitComment = (e) => {
    e.preventDefault();
    if (commentText.trim()) {
      onComment(post.id, commentText);
      setCommentText('');
    }
  };

  return (
    <article className="bg-white rounded-lg shadow p-4">
      <header className="flex items-center mb-4">
        <img 
          src={author.avatar} 
          alt={author.name} 
          className="w-12 h-12 rounded-full mr-4"
        />
        <div>
          <h3 className="font-semibold">{author.name}</h3>
          <time className="text-gray-500 text-sm">
            {new Date(timestamp).toLocaleDateString()}
          </time>
        </div>
      </header>
      
      <div className="mb-4">
        <p className="text-gray-800">{content}</p>
      </div>
      
      <div className="flex items-center gap-4 mb-4">
        <button 
          onClick={() => onLike(post.id)}
          className="flex items-center gap-2 text-blue-600 hover:text-blue-800"
        >
          <span>👍</span> {likes} Likes
        </button>
        <span className="text-gray-500">
          {comments.length} Comments
        </span>
      </div>

      <div className="border-t pt-4">
        {comments.map(comment => (
          <div key={comment.id} className="mb-2 text-sm">
            <span className="font-semibold">{comment.author}: </span>
            <span>{comment.text}</span>
          </div>
        ))}
        
        <form onSubmit={handleSubmitComment} className="mt-4 flex gap-2">
          <input
            type="text"
            value={commentText}
            onChange={(e) => setCommentText(e.target.value)}
            placeholder="Write a comment..."
            className="flex-1 px-3 py-2 border rounded-lg"
          />
          <button 
            type="submit"
            className="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700"
          >
            Comment
          </button>
        </form>
      </div>
    </article>
  );
}

export default Post;
src/App.jsx
import { useState } from 'react';
import PostList from './components/PostList';
import CreatePost from './components/CreatePost';
import { initialPosts } from './data/sampleData';

function App() {
  const [posts, setPosts] = useState(initialPosts);
  
  const handleCreatePost = (newPostContent) => {
    const newPost = {
      id: Date.now(),
      author: {
        name: "Current User",
        avatar: "/api/placeholder/50/50"
      },
      content: newPostContent,
      likes: 0,
      comments: [],
      timestamp: new Date().toISOString()
    };
    
    setPosts(prevPosts => [newPost, ...prevPosts]);
  };
  
  const handleLike = (postId) => {
    setPosts(prevPosts => 
      prevPosts.map(post =>
        post.id === postId 
          ? { ...post, likes: post.likes + 1 }
          : post
      )
    );
  };
  
  const handleComment = (postId, commentText) => {
    const newComment = {
      id: Date.now(),
      author: "Current User",
      text: commentText,
      timestamp: new Date().toISOString()
    };
    
    setPosts(prevPosts =>
      prevPosts.map(post =>
        post.id === postId
          ? { ...post, comments: [...post.comments, newComment] }
          : post
      )
    );
  };

  return (
    <div className="container mx-auto max-w-2xl p-4">
      <h1 className="text-3xl font-bold mb-6">Social Feed</h1>
      <CreatePost onCreatePost={handleCreatePost} />
      <PostList 
        posts={posts}
        onLike={handleLike}
        onComment={handleComment}
      />
    </div>
  );
}

export default App;
Running the Application
To start the development server:

npm run dev
Visit http://localhost:5173 in your browser to see the application.

Building for Production
To create a production build:

npm run build
The built files will be in the dist directory.

Features Implementation Details
Creating Posts
Users can create new posts using the CreatePost component
Posts are added to the top of the feed
Each post includes the author's name, avatar, timestamp, and content
Liking Posts
Users can like posts by clicking the like button
The like count updates immediately
Multiple likes from the same user are allowed (for simplicity)
Comments
Users can add comments to any post
Comments display the author's name and comment text
Comments are added in chronological order
Each comment has a unique ID and timestamp
Styling
The application uses Tailwind CSS for styling. Key style features include:

Responsive design that works on mobile and desktop
Card-based layout for posts
Shadow effects for depth
Rounded corners for modern look
Hover effects on interactive elements
Consistent spacing and typography
Future Improvements
Possible enhancements for the project:

User authentication
Image upload support
Rich text editing for posts
Comment threading
Share functionality
Post deletion and editing
User profiles
Real-time updates using WebSocket
Troubleshooting
Create the below file manually if you ran into an issue at step 4

// tailwind.config.js 
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
// postcss.config.js
export default {
  plugins: {
        "@tailwindcss/postcss" : {},
 }
}
