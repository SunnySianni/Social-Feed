# Social Feed Project

A React-based social media feed application with features like creating posts, liking, and commenting. Built with React and styled using Tailwind CSS.

## Features

- Create new posts
- Like posts
- Add comments to posts
- Responsive design
- Real-time updates

## Project Setup & Structure

```
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
```

## Prerequisites

- Node.js (v16 or higher)
- npm (v8 or higher)
- [React DevTools](https://react.dev/learn/react-developer-tools)

## Installation

1. Create a new project using Vite:
   ```bash
   npm create vite@latest social-feed -- --template react
   cd social-feed
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Install Tailwind CSS and its peer dependencies:
   ```bash
   npm install -D tailwindcss postcss autoprefixer
   ```

4. Generate Tailwind CSS and PostCSS configuration files:
   ```bash
   npx tailwindcss init -p
   ```
   **Note:** If you encounter errors running `npx tailwindcss init -p`, refer to the troubleshooting section.

5. Add Tailwind directives to `src/index.css`:
   ```css
   @tailwind base;
   @tailwind components;
   @tailwind utilities;
   ```

## Configuration Files

### `tailwind.config.js`
```javascript
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
```

### `postcss.config.js`
```javascript
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

## Component Files

### `src/data/sampleData.js`
```javascript
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
  }
];
```

## Running the Application

To start the development server:
```bash
npm run dev
```

Visit `http://localhost:5173` in your browser to see the application.

## Building for Production

To create a production build:
```bash
npm run build
```

The built files will be in the `dist` directory.

## Features Implementation Details

### Creating Posts
- Users can create new posts using the CreatePost component
- Posts are added to the top of the feed
- Each post includes the author's name, avatar, timestamp, and content

### Liking Posts
- Users can like posts by clicking the like button
- The like count updates immediately

### Comments
- Users can add comments to any post
- Comments display the author's name and comment text
- Comments are added in chronological order
- Each comment has a unique ID and timestamp

## Styling

The application uses Tailwind CSS for styling. Key style features include:

- Responsive design that works on mobile and desktop
- Card-based layout for posts
- Shadow effects for depth
- Rounded corners for modern look
- Hover effects on interactive elements
- Consistent spacing and typography

## Future Improvements

Possible enhancements for the project:

1. User authentication
2. Image upload support
3. Rich text editing for posts
4. Comment threading
5. Share functionality
6. Post deletion and editing
7. User profiles
8. Real-time updates using WebSocket

## Troubleshooting

If you encounter issues during setup, manually create the following files:

### `tailwind.config.js`
```javascript
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
```

### `postcss.config.js`
```javascript
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

