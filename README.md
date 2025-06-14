# QuickPlate

# QuickPlate - Food Ordering System

## Group Information

**Group Name**: G2
**Section**: 4

**Group Members** :
- MUHAMMAD SYAHMI FIRDAUS BIN IZHAM - 2212287
- MUHAMAD AQIL HAZMI BIN ZAHIDI - 2311655
- SITI SARAH BINTI ROSMAN - 2021128 
- MUHAMMAD HAIKAL BIN SHAMSURI - 2215913


## Project Overview

Introduction :
QuickPlate is a web-based food ordering system developed using Laravel framework. The application connects customers with local restaurants, allowing users to browse menus, place orders, and track deliveries in real-time. Restaurant owners can manage their menus, process orders, and update order status through an intuitive dashboard.

## Project Objectives

- Primary Goal: Create a functional food ordering platform that connects customers and restaurants
- Technical Goal: Implement Laravel MVC architecture with full CRUD operations
- User Experience Goal: Provide an intuitive, responsive interface for both customers and restaurant owners
- Business Goal: Enable efficient order management and tracking system

## Target Users

- Customers: Individuals looking to order food online
- Restaurant Owners: Business owners who want to manage their restaurant digitally
- Administrators: System managers who oversee the platform


## Features and Functionalities

** Customer Features**

- User Registration & Login: Secure account creation and authentication
- Restaurant Browsing: View available restaurants and their menus
- Menu Exploration: Browse food items with descriptions, prices, and images
- Shopping Cart: Add/remove items, modify quantities, view total cost
- Order Placement: Secure checkout process with order confirmation
- Order Tracking: Real-time status updates on order progress
- Order History: View previous orders and reorder functionality
- Profile Management: Update personal information and delivery addresses

**Admin Features**

- Restaurant Dashboard: Overview of orders, sales, and performance
- Menu Management: Add, edit, delete menu items with images
- Order Management: View incoming orders and update order status
- Profile Management: Update restaurant information and operating hours
- Sales Analytics: Basic reporting on sales and popular items

## Technical Implementation

** Technology Stack**

- Backend Framework: Laravel 10.x
- Frontend: Blade Templates with Bootstrap 5
- Database: MySQL 8.0
- Authentication: Laravel Breeze
- Image Storage: Laravel File Storage
- Development Environment: XAMPP

** Database Design**

Database Schema Overview
Our database consists of [X] main tables designed to handle users, restaurants, menus, orders, and related data:
Core Tables:

- users - Customer and restaurant owner accounts
- restaurants - Restaurant information and details
- menu_items - Food items with pricing and descriptions
- orders - Customer order records
- order_items - Individual items within each order
- categories - Food categories for menu organization

### Entity Relationship Diagram (ERD)

https://docs.google.com/document/d/1gQeg-at7jM69PBJz9OZSUhfYTOzc6jTW5IDaH_PtuKg/edit?usp=sharing

Key Relationships:

- Users can have multiple Orders (One-to-Many)
- Restaurants can have multiple Menu Items (One-to-Many)
- Orders can have multiple Order Items (One-to-Many)
- Menu Items belong to Categories (Many-to-One)

** Laravel Components Implementation**

- Routes (Web.php)
  
php
`// Authentication Routes`
Auth::routes();

`// Public Routes`
Route::get('/', [HomeController::class, 'index'])->name('home');
Route::get('/restaurants', [RestaurantController::class, 'index'])->name('restaurants.index');
Route::get('/restaurants/{restaurant}', [RestaurantController::class, 'show'])->name('restaurants.show');

`// Customer Protected Routes`
Route::middleware(['auth', 'customer'])->group(function () {
    Route::get('/dashboard', [CustomerController::class, 'dashboard'])->name('customer.dashboard');
    Route::resource('orders', OrderController::class);
    Route::post('/cart/add', [CartController::class, 'add'])->name('cart.add');
});

`// Restaurant Owner Protected Routes`
Route::middleware(['auth', 'restaurant'])->group(function () {
    Route::get('/restaurant/dashboard', [RestaurantOwnerController::class, 'dashboard'])->name('restaurant.dashboard');
    Route::resource('menu-items', MenuItemController::class);
});

- Controllers
  
  *Main Controllers Implemented are below :*

  1. HomeController: Handles homepage display and restaurant listings
  2. RestaurantController: Manages restaurant information and menu display
  3. OrderController: Processes order creation, tracking, and management
  4. MenuItemController: Handles CRUD operations for menu items
  5. CartController: Manages shopping cart functionality
  6. CustomerController: Customer dashboard and profile management
  7. RestaurantOwnerController: Restaurant owner dashboard and analytics

- Models and Relationships
  
php// User Model
class User extends Authenticatable {
    public function orders() {
        return $this->hasMany(Order::class);
    }
    
    public function restaurant() {
        return $this->hasOne(Restaurant::class);
    }
}

// Restaurant Model  
class Restaurant extends Model {
    public function user() {
        return $this->belongsTo(User::class);
    }
    
    public function menuItems() {
        return $this->hasMany(MenuItem::class);
    }
}

// Order Model
class Order extends Model {
    public function user() {
        return $this->belongsTo(User::class);
    }
    
    public function orderItems() {
        return $this->hasMany(OrderItem::class);
    }
}

- Views and User Interface

  *Blade Templates Structure:*
  - layouts/app.blade.php - Main application layout
  - home.blade.php - Homepage with restaurant listings
  - restaurants/index.blade.php - Restaurant browsing page
  - restaurants/show.blade.php - Individual restaurant menu
  - orders/create.blade.php - Order placement form
  - dashboard/customer.blade.php - Customer dashboard
  - dashboard/restaurant.blade.php - Restaurant owner dashboard

   *Design Features:*
   - Responsive Design: Bootstrap 5 for mobile-first approach
   - Color Scheme: Modern orange and white theme representing food industry
   - Navigation: Intuitive menu structure with user role-based options
   - Interactive Elements: Dynamic cart updates, real-time order tracking


## User Authentication System

### ** Authentication Features**
- **Registration System**: Email validation, password confirmation, role selection
- **Login System**: Secure authentication with "Remember Me" option
- **Password Reset**: Email-based password recovery system
- **Role-Based Access**: Different dashboards for customers, restaurant owners, and admins
- **Profile Management**: Users can update their information and preferences

### **Security Measures**
- Password encryption using Laravel's built-in hashing
- CSRF protection on all forms
- Input validation and sanitization
- Middleware protection for authenticated routes


## Installation and Setup Instructions
### Prerequisites :
- PHP >= 8.1
- Composer
- Node.js and NPM
- MySQL 8.0
- XAMPP 

### Step-by-Step Installation

1. Clone the Repository

bash
git clone https://github.com/[your-username]/QuickPlate.git
cd QuickPlate

2. Install Dependencies

bashcomposer install
npm install

3. Environment Configuration

bashcp .env.example .env
php artisan key:generate

4. Database Setup

bash# Configure database in .env file
php artisan migrate
php artisan db:seed

5. Start Development Server

bashphp artisan serve
npm run dev

## Testing and Quality Assurance

###  Functionality Testing

 - User registration and login system
 - Restaurant browsing and menu display
 - Shopping cart add/remove functionality
 - Order placement and confirmation
 - Order status tracking
 - Restaurant owner menu management
 - Admin user management
 - Responsive design across devices

### Browser Compatibility

-  Google Chrome (Latest)
-  Mozilla Firefox (Latest)
-  Safari (Latest)
-  Microsoft Edge (Latest)

### Performance Testing

- Page load times under 3 seconds
- Database queries optimized
- Image compression implemented
- Responsive design tested on multiple screen sizes


## Challenges Faced and Solutions
### Challenge 1: Complex Order Management
- Problem: Managing relationships between orders, order items, and menu items
- Solution: Implemented proper Eloquent relationships and created pivot tables for many-to-many relationships
### Challenge 2: Real-time Order Tracking
- Problem: Updating order status in real-time without page refresh
- Solution: Used AJAX calls to update order status dynamically
### Challenge 3: Role-based Authentication
- Problem: Different user types requiring different access levels
- Solution: Implemented middleware to check user roles and redirect appropriately

## Future Enhancements
### Phase 2 Features (Potential Improvements)
- **Real-time Notifications**: Push notifications for order updates
- **Payment Integration**: Stripe or PayPal payment processing
- **GPS Tracking** : Real-time delivery tracking with maps
- **Rating System** : Customer reviews and restaurant ratings
- **Advanced Analytics** : Detailed sales reports and customer insights
- **Mobile App** : Native mobile application for iOS and Android

### Scalability Considerations

- Database optimization for larger datasets
- Caching implementation for improved performance
- API development for mobile app integration
- Load balancing for high traffic scenarios


## Learning Outcomes
### Technical Skills Gained

- Laravel Framework: Understanding of MVC architecture and Eloquent ORM
- Database Design: Creating efficient database schemas and relationships
- Authentication: Implementing secure user authentication systems
- Frontend Development: Building responsive interfaces with Bootstrap
- Version Control: Using Git and GitHub for project management

### Soft Skills Developed

- **Team Collaboration** : Working effectively in a group environment
- **Project Management** : Planning and executing a complex web application
- **Problem Solving** : Debugging and resolving technical challenges
- **Documentation** : Creating comprehensive project documentation


## References

1. Laravel Documentation. (2024). Laravel 10.x Documentation. Retrieved from https://laravel.com/docs/10.x
2. Bootstrap Documentation. (2024). Bootstrap 5.3 Documentation. Retrieved from https://getbootstrap.com/docs/5.3/
3. MySQL Documentation. (2024). MySQL 8.0 Reference Manual. Retrieved from https://dev.mysql.com/doc/refman/8.0/en/
4. MDN Web Docs. (2024). Web Development Resources. Retrieved from https://developer.mozilla.org/
5. Stack Overflow. (2024). Programming Q&A Platform. Retrieved from https://stackoverflow.com/


## Conclusion
QuickPlate successfully demonstrates the implementation of a comprehensive food ordering system using Laravel framework. The project showcases proficiency in web development fundamentals including MVC architecture, database design, user authentication, and responsive web design.

### Key Achievements

- Successfully implemented all required Laravel components (Routes, Controllers, Views, Models)
- Created a functional food ordering system with user role management
- Developed a responsive, user-friendly interface
- Demonstrated understanding of database relationships and CRUD operations
- Applied security best practices for user authentication

### Project Impact
This project provides practical experience in building real-world web applications and demonstrates the ability to work collaboratively in a team environment. The skills gained through this project are directly applicable to professional web development scenarios.

- Project Completion Date: 11/6/2025
- Course: INFO 3305 Web Application Development


