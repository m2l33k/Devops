# Power Gym BookStore

A full-featured React bookstore application with user and admin interfaces.

## Features

### User Interface (Public Area)
- **Home Page**: Welcome page with navigation to books
- **Book List**: Browse all available books with search functionality
- **Book Details**: View detailed information about each book
- **Favorites**: Save and view your favorite books (stored in localStorage)
- **Search**: Search books by title

### Admin Interface (Private Area)
- **Admin Login**: Secure login page (static credentials)
- **Dashboard**: View all books in a table format
- **Add Book**: Create new books
- **Edit Book**: Update existing book information
- **Delete Book**: Remove books from the store

## Tech Stack

- **React 19** - UI library
- **Vite** - Build tool and dev server
- **React Router** - Client-side routing
- **Context API** - State management
- **LocalStorage** - Data persistence

## Project Structure

```
src/
├── components/          # Reusable components
│   ├── admin/          # Admin-specific components
│   │   ├── AdminSidebar.jsx
│   │   ├── AdminBookTable.jsx
│   │   └── AdminBookForm.jsx
│   ├── BookCard.jsx
│   ├── BookList.jsx
│   ├── SearchBar.jsx
│   ├── FavoriteButton.jsx
│   ├── Navbar.jsx
│   └── ProtectedRoute.jsx
├── pages/              # Page components
│   ├── admin/          # Admin pages
│   │   ├── AdminLogin.jsx
│   │   ├── AdminDashboard.jsx
│   │   ├── AddBook.jsx
│   │   └── EditBook.jsx
│   ├── Home.jsx
│   ├── BookList.jsx
│   ├── BookDetail.jsx
│   └── Favorites.jsx
├── context/            # Context providers
│   ├── BooksContext.jsx
│   └── AuthContext.jsx
├── assets/             # Static assets
├── App.jsx             # Main app component with routing
└── main.jsx            # Entry point
```

## Installation

1. Install dependencies:
```bash
npm install
```

2. Start the development server:
```bash
npm run dev
```

3. Open your browser and navigate to `http://localhost:5173`

## Admin Credentials

- **Username**: `admin`
- **Password**: `admin123`

## Data Storage

All data is stored in the browser's localStorage:
- Books are stored under the key `powerGymBooks`
- Favorites are stored under the key `powerGymFavorites`
- Admin authentication is stored under the key `powerGymAdminAuth`

## Available Scripts

- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build
- `npm run lint` - Run ESLint

## Routes

### Public Routes
- `/` - Home page
- `/books` - Book list page
- `/book/:id` - Book detail page
- `/favorites` - Favorites page

### Admin Routes (Protected)
- `/admin/login` - Admin login page
- `/admin/dashboard` - Admin dashboard
- `/admin/add-book` - Add new book
- `/admin/edit-book/:id` - Edit existing book

## Features Implementation

### Search Functionality
- Real-time search by book title
- Case-insensitive matching
- Results update as you type

### Favorites System
- Click the heart icon on any book card or detail page
- Favorites persist across page refreshes
- View all favorites on the Favorites page

### Admin Features
- Protected routes require authentication
- Full CRUD operations for books
- Form validation for book data
- Image preview in admin forms

## Styling

The application uses custom CSS with:
- Responsive design for mobile and desktop
- Modern UI with clean aesthetics
- Smooth transitions and hover effects
- Consistent color scheme

## Browser Support

Works on all modern browsers that support:
- ES6+ JavaScript
- LocalStorage API
- CSS Grid and Flexbox

## CI/CD Pipeline (GitHub Actions)

The project includes a comprehensive CI/CD pipeline using GitHub Actions to ensure code quality, build stability, and automated deployment verification.

### Workflows

#### 1. Build & Smoke (Pull Request)
- **File:** `.github/workflows/pr.yml`
- **Trigger:** Open/Update Pull Request to `dev` branch.
- **Actions:**
  - Builds a Docker image tagged with the commit SHA.
  - Deploys a temporary container on port **3002**.
  - Runs a **Smoke Test** (`ci/smoke_test.sh`) to verify the app responds with HTTP 200.
  - Archives smoke test results.
  - Cleans up resources.

#### 2. Full Build (Dev)
- **File:** `.github/workflows/dev.yml`
- **Trigger:** Push to `dev` branch.
- **Features:**
  - **Parallel Testing:** Validates the build on **Node.js 18** and **Node.js 20** simultaneously (Matrix Strategy).
  - **Deployment:** Builds the final Docker image and deploys it on port **3001**.
  - **Verification:** Runs smoke tests and archives the results.

#### 3. Versioned Build (Release)
- **File:** `.github/workflows/tag.yml`
- **Trigger:** Pushing a tag matching `v*.*.*` (e.g., `v1.0.0`).
- **Actions:**
  - Builds a Docker image tagged with the version number (e.g., `power_gym:v1.0.0`).
  - Deploys to port **3003** for final verification.
  - **Artifacts:** Exports the Docker image as a `.tar` file and archives it along with test reports for release.

### Local Testing Scripts
- **Smoke Test:** `ci/smoke_test.sh <url>` - Checks if the application is reachable and returns HTTP 200.

## Jenkins CI/CD

The project also supports Jenkins pipelines located in the `jenkins/` directory.

### Pipelines

#### 1. PR Pipeline
- **File:** `jenkins/Jenkinsfile.pr`
- **Purpose:** Fast validation for Pull Requests.
- **Port:** 3002

#### 2. Dev Pipeline
- **File:** `jenkins/Jenkinsfile.dev`
- **Purpose:** Full build and deployment for the development branch with parallel Node.js version testing.
- **Port:** 3001

#### 3. Tag Pipeline
- **File:** `jenkins/Jenkinsfile.tag`
- **Purpose:** Production release builds triggered by tags (e.g., `v1.0.0`). Archives Docker images.
- **Port:** 3003
