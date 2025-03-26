---
title: Python Web Training
---

**Week 1: Backend Foundation & Core Frontend Structure**

- **Day 1-2: Setup & Backend Core (FastAPI, SQLAlchemy, PostgreSQL)**
    
    - **Environment:** Set up Python virtual environment. Install FastAPI, Uvicorn, SQLAlchemy, Psycopg2 (or asyncpg for async), Pydantic, python-jose[cryptography] & passlib[bcrypt] (for auth).
        
    - **Project Structure:** Create a root folder, backend subfolder. Initialize FastAPI app (main.py).
        
    - **Database:** Install PostgreSQL. Create a database for the project.
        
    - **SQLAlchemy Models (backend/models.py):** Define basic SQLAlchemy models for User, Product, Order, OrderItem. Keep relationships simple (e.g., User -> Orders, Order -> OrderItems, Product -> OrderItems).
        
    - **Database Connection (backend/database.py):** Set up SQLAlchemy engine, session maker. Add basic dependency for getting a DB session in FastAPI routes.
        
    - **Pydantic Schemas (backend/schemas.py):** Create Pydantic models for request/response validation (e.g., ProductCreate, Product, UserCreate, User, Token).
        
    - **(Optional but Recommended):** Set up Alembic for database migrations early. Create initial migration.
        
- **Day 3-4: Core Backend API Endpoints (Products & Basic Auth)**
    
    - **Product CRUD (backend/routers/products.py):** Implement basic API endpoints:
        
        - GET /products: List all products.
            
        - GET /products/{product_id}: Get a single product detail.
            
        - POST /products: Create a new product (for admin/setup initially).
            
    - **User Auth (backend/routers/auth.py, backend/security.py):**
        
        - Implement user registration (POST /users or /register). Hash passwords!
            
        - Implement login (POST /token) using OAuth2PasswordBearer and JWT. Return an access token.
            
        - Create a dependency to get the current logged-in user from the token.
            
    - **Testing:** Use curl or Postman/Insomnia to test your endpoints manually. Make sure product fetching and user registration/login work.
        
- **Day 5-7: Frontend Setup & Basic Product Display (React)**
    
    - **Project Setup:** In the root folder, create a frontend subfolder. Use create-react-app or vite (recommended for speed) to initialize the React project.
        
    - **Core Libraries:** Install axios (for API calls) and react-router-dom (for routing).
        
    - **Basic Structure:** Set up basic file structure (components, pages, services).
        
    - **Routing:** Set up basic routes (e.g., /, /products/:id, /login, /register).
        
    - **API Service (frontend/src/services/api.js):** Create functions to call your backend API endpoints (e.g., fetchProducts, fetchProductById).
        
    - **Product List Page:** Create a page component that fetches products from your FastAPI backend using useEffect and axios, then displays them simply (list or basic grid).
        
    - **Product Detail Page:** Create a page that takes a product ID from the URL params, fetches the specific product details, and displays them.
        
    - **Basic Layout/Nav:** Create a simple header/navbar component with links.
        

**Week 2: Core Features, Integration & Polish**

- **Day 8-9: Frontend Auth & Cart Logic**
    
    - **Auth Forms:** Create Login and Registration page components with forms.
        
    - **Auth State:** Implement basic auth state management (React Context API is simplest for speed, or Zustand/Redux Toolkit if you're faster with those). Store token (e.g., in localStorage) and user info. Protect routes that require login.
        
    - **Backend Cart (Choose one approach for speed):**
        
        - Simple (DB based): Create CartItem model/table linked to User. Add API endpoints (POST /cart, GET /cart, DELETE /cart/{item_id}).
            
        - Simpler (In-memory/Session - less ideal but faster): Handle cart purely on the frontend state initially, maybe storing in localStorage. Less scalable, data lost on clear. (Let's assume DB based for slightly better practice).
            
    - **Frontend Cart:**
        
        - Add "Add to Cart" buttons on product list/detail pages. These should call your backend cart endpoint.
            
        - Create a Cart component/page to display items fetched from the backend GET /cart endpoint.
            
        - Implement functionality to adjust quantity or remove items (calling backend DELETE/PUT endpoints).
            
- **Day 10-11: Checkout Process & Orders**
    
    - **Backend Order Endpoint:**
        
        - Create an endpoint like POST /orders. This endpoint should:
            
            - Require authentication.
                
            - Get the user's cart items.
                
            - Create an Order record.
                
            - Create corresponding OrderItem records from the cart items.
                
            - Clear the user's cart.
                
            - Return the created order details (or just a success message).
                
    - **Frontend Checkout:**
        
        - Create a simple Checkout button/page.
            
        - When clicked, call the POST /orders backend endpoint.
            
        - On success, redirect to an Order Confirmation page or Order History page. Show a success message.
            
    - **Backend Order History:** Create GET /orders (for logged-in user) and GET /orders/{order_id} endpoints.
        
    - **Frontend Order History:** Create a page to display the user's past orders fetched from the backend.
        
- **Day 12-13: Basic Styling & Refinement**
    
    - **Styling:** Apply basic CSS or use a simple component library (like Bootstrap, Material UI basic components, or Tailwind CSS if you know it well already). Don't aim for pixel-perfect design. Focus on usability: make buttons clickable, forms understandable, layout okay on common screen sizes.
        
    - **Error Handling:** Add basic error handling. Display meaningful error messages to the user on the frontend (e.g., "Login failed", "Could not add item"). Ensure backend returns appropriate HTTP status codes.
        
    - **Seed Data:** Create a simple Python script (or use Alembic data migrations) to populate your database with some sample products.
        
- **Day 14: Buffer, Manual Testing & (Optional) Basic Deployment**
    
    - **Testing:** Manually click through all the core user flows: Register -> Login -> View Products -> Add to Cart -> View Cart -> Checkout -> View Order History -> Logout. Fix bugs!
        
    - **Debugging:** Use browser dev tools and FastAPI logs extensively.
        
    - **Buffer:** Use this day for anything that took longer than expected (likely!).
        
    - **(Stretch Goal):** If things went amazingly well, research basic deployment options (e.g., Dockerizing both apps, deploying to a service like Heroku (paid), Railway, Render, or a simple VPS). Don't expect to fully finish deployment.
        

---

**Key Strategies for Speed:**

1. **MVP Focus:** Cut anything non-essential (search, filtering, reviews, admin panel, payment, complex styling, comprehensive tests).
    
2. **Keep it Simple:** Simple data models, simple API logic, simple state management (Context API), simple UI.
    
3. **Leverage Framework Features:** Use Pydantic for validation, FastAPI's dependency injection, React Router for routing.
    
4. **Don't Reinvent:** Use libraries like axios, passlib, python-jose.
    
5. **Test Manually Early & Often:** Use Postman/Insomnia for the backend, click through the frontend.
    
6. **Timebox:** If a feature is taking too long, simplify it or skip it.