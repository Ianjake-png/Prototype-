<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SavoryBites - Online Food Ordering System</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                    colors: {
                        primary: {
                            50: '#fff1f2',
                            100: '#ffe4e6',
                            500: '#f43f5e',
                            600: '#e11d48',
                            700: '#be123c',
                        }
                    }
                }
            }
        }
    </script>
</head>
<body class="bg-gray-50 text-gray-800 font-sans min-h-screen flex flex-col">

    <!-- Toast Notification Container -->
    <div id="toast-container" class="fixed bottom-5 right-5 z-50 flex flex-col space-y-2"></div>

    <!-- HEADER & NAVIGATION -->
    <header class="bg-white shadow-sm sticky top-0 z-40 border-b border-gray-100">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-20 flex items-center justify-between">
            <!-- Branding -->
            <div class="flex items-center space-x-3 cursor-pointer" onclick="switchTab('menu')">
                <div class="w-12 h-12 bg-primary-500 rounded-2xl flex items-center justify-center text-white shadow-lg shadow-primary-500/30">
                    <i class="fa-solid fa-utensils text-2xl"></i>
                </div>
                <div>
                    <h1 class="text-xl font-bold tracking-tight text-gray-900">SavoryBites</h1>
                    <p class="text-xs text-gray-500 font-medium">Online Food Ordering System</p>
                </div>
            </div>

            <!-- Navigation Tabs -->
            <nav class="hidden md:flex items-center space-x-1 bg-gray-100/80 p-1.5 rounded-xl">
                <button onclick="switchTab('menu')" id="nav-menu" class="px-5 py-2.5 rounded-lg text-sm font-semibold transition-all duration-200 bg-white text-primary-600 shadow-sm">
                    <i class="fa-solid fa-burger mr-2"></i>Customer Menu
                </button>
                <button onclick="switchTab('cart')" id="nav-cart" class="px-5 py-2.5 rounded-lg text-sm font-semibold transition-all duration-200 text-gray-600 hover:text-gray-900 relative">
                    <i class="fa-solid fa-bag-shopping mr-2"></i>Shopping Cart
                    <span id="cart-badge" class="absolute -top-1 -right-1 bg-primary-500 text-white text-xs w-5 h-5 rounded-full flex items-center justify-center font-bold hidden shadow-md">0</span>
                </button>
                <button onclick="switchTab('admin')" id="nav-admin" class="px-5 py-2.5 rounded-lg text-sm font-semibold transition-all duration-200 text-gray-600 hover:text-gray-900">
                    <i class="fa-solid fa-user-shield mr-2"></i>Admin Panel
                </button>
            </nav>

            <!-- Mobile Cart & Menu Buttons -->
            <div class="flex md:hidden items-center space-x-3">
                <button onclick="switchTab('cart')" class="p-2.5 bg-gray-100 rounded-xl relative text-gray-700">
                    <i class="fa-solid fa-bag-shopping text-lg"></i>
                    <span id="cart-badge-mobile" class="absolute -top-1 -right-1 bg-primary-500 text-white text-xs w-5 h-5 rounded-full flex items-center justify-center font-bold hidden">0</span>
                </button>
                <button onclick="toggleMobileMenu()" class="p-2.5 bg-gray-100 rounded-xl text-gray-700">
                    <i class="fa-solid fa-bars text-lg"></i>
                </button>
            </div>
        </div>

        <!-- Mobile Dropdown Menu -->
        <div id="mobile-menu" class="hidden md:hidden border-t border-gray-100 bg-white px-4 pt-2 pb-4 space-y-2">
            <button onclick="switchTab('menu'); toggleMobileMenu();" class="w-full text-left px-4 py-2 rounded-lg text-sm font-medium text-gray-700 hover:bg-gray-50 flex items-center">
                <i class="fa-solid fa-burger mr-3 text-primary-500 w-5"></i>Customer Menu
            </button>
            <button onclick="switchTab('cart'); toggleMobileMenu();" class="w-full text-left px-4 py-2 rounded-lg text-sm font-medium text-gray-700 hover:bg-gray-50 flex items-center">
                <i class="fa-solid fa-bag-shopping mr-3 text-primary-500 w-5"></i>Shopping Cart
            </button>
            <button onclick="switchTab('admin'); toggleMobileMenu();" class="w-full text-left px-4 py-2 rounded-lg text-sm font-medium text-gray-700 hover:bg-gray-50 flex items-center">
                <i class="fa-solid fa-user-shield mr-3 text-primary-500 w-5"></i>Admin Panel
            </button>
        </div>
    </header>

    <!-- MAIN CONTAINER -->
    <main class="flex-grow max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-8">

        <!-- ================= VIEW 1: CUSTOMER MENU ================= -->
        <section id="view-menu" class="space-y-8">
            <!-- Hero Banner -->
            <div class="relative bg-gradient-to-r from-gray-900 to-gray-800 rounded-3xl overflow-hidden shadow-xl p-8 sm:p-12 text-white flex flex-col md:flex-row items-center justify-between">
                <div class="z-10 max-w-xl space-y-4">
                    <span class="bg-primary-500/20 text-primary-400 text-xs font-bold uppercase tracking-wider px-3 py-1.5 rounded-full border border-primary-500/30">Fast & Convenient Delivery</span>
                    <h2 class="text-3xl sm:text-5xl font-extrabold tracking-tight">Delicious Food Delivered To Your Door</h2>
                    <p class="text-gray-300 text-sm sm:text-base leading-relaxed">Explore our chef-crafted menu, choose your favorite meals, and place your order instantly without leaving your seat.</p>
                    <div class="pt-2 flex flex-wrap gap-4">
                        <a href="#menu-catalog" class="bg-primary-500 hover:bg-primary-600 text-white font-semibold px-6 py-3 rounded-xl shadow-lg shadow-primary-500/30 transition-all duration-200 flex items-center">
                            <i class="fa-solid fa-utensils mr-2"></i> Browse Menu
                        </a>
                    </div>
                </div>
                <div class="mt-8 md:mt-0 z-10 w-full md:w-1/2 flex justify-center">
                    <img src="https://images.unsplash.com/photo-1504674900247-0877df9cc836?auto=format&fit=crop&w=600&q=80" alt="Delicious Food Spread" class="rounded-2xl shadow-2xl object-cover h-64 sm:h-80 w-full max-w-md border-4 border-white/10">
                </div>
                <div class="absolute inset-0 opacity-10 bg-[radial-gradient(#fff_1px,transparent_1px)] [background-size:16px_16px]"></div>
            </div>

            <!-- Search & Filters -->
            <div id="menu-catalog" class="flex flex-col md:flex-row gap-4 items-center justify-between bg-white p-4 rounded-2xl shadow-sm border border-gray-100">
                <!-- Category Filters -->
                <div class="flex items-center space-x-2 overflow-x-auto w-full md:w-auto pb-2 md:pb-0 scrollbar-none">
                    <button onclick="filterCategory('All')" id="cat-All" class="category-btn px-4 py-2 rounded-xl text-sm font-semibold bg-primary-500 text-white shadow-sm transition-all whitespace-nowrap">All Items</button>
                    <button onclick="filterCategory('Burgers')" id="cat-Burgers" class="category-btn px-4 py-2 rounded-xl text-sm font-semibold bg-gray-100 text-gray-600 hover:bg-gray-200 transition-all whitespace-nowrap">Burgers</button>
                    <button onclick="filterCategory('Pizzas')" id="cat-Pizzas" class="category-btn px-4 py-2 rounded-xl text-sm font-semibold bg-gray-100 text-gray-600 hover:bg-gray-200 transition-all whitespace-nowrap">Pizzas</button>
                    <button onclick="filterCategory('Drinks')" id="cat-Drinks" class="category-btn px-4 py-2 rounded-xl text-sm font-semibold bg-gray-100 text-gray-600 hover:bg-gray-200 transition-all whitespace-nowrap">Drinks</button>
                    <button onclick="filterCategory('Desserts')" id="cat-Desserts" class="category-btn px-4 py-2 rounded-xl text-sm font-semibold bg-gray-100 text-gray-600 hover:bg-gray-200 transition-all whitespace-nowrap">Desserts</button>
                </div>

                <!-- Search Input -->
                <div class="relative w-full md:w-72">
                    <span class="absolute inset-y-0 left-0 pl-3.5 flex items-center pointer-events-none text-gray-400">
                        <i class="fa-solid fa-search"></i>
                    </span>
                    <input type="text" id="search-input" oninput="handleSearch()" placeholder="Search food items..." class="w-full pl-10 pr-4 py-2.5 bg-gray-50 border border-gray-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-primary-500 focus:bg-white transition-all">
                </div>
            </div>

            <!-- Food Items Grid -->
            <div id="food-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
                <!-- Dynamically populated -->
            </div>
        </section>


        <!-- ================= VIEW 2: SHOPPING CART & CHECKOUT ================= -->
        <section id="view-cart" class="hidden space-y-8">
            <div class="flex items-center justify-between border-b border-gray-200 pb-4">
                <div>
                    <h2 class="text-2xl font-bold tracking-tight text-gray-900">Your Shopping Cart</h2>
                    <p class="text-sm text-gray-500">Review your selected food items before proceeding to checkout.</p>
                </div>
                <button onclick="switchTab('menu')" class="text-sm font-semibold text-primary-600 hover:text-primary-700 flex items-center">
                    <i class="fa-solid fa-arrow-left mr-2"></i>Continue Shopping
                </button>
            </div>

            <div id="cart-content-wrapper" class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                <!-- Cart Items List -->
                <div class="lg:col-span-2 space-y-4" id="cart-items-container">
                    <!-- Dynamically populated -->
                </div>

                <!-- Order Summary & Checkout Form -->
                <div class="space-y-6">
                    <div class="bg-white p-6 rounded-2xl shadow-sm border border-gray-100 space-y-4">
                        <h3 class="text-lg font-bold text-gray-900 border-b border-gray-100 pb-3">Order Summary</h3>
                        <div class="space-y-2 text-sm">
                            <div class="flex justify-between text-gray-600">
                                <span>Subtotal</span>
                                <span id="cart-subtotal" class="font-semibold text-gray-900">$0.00</span>
                            </div>
                            <div class="flex justify-between text-gray-600">
                                <span>Delivery Fee</span>
                                <span class="font-semibold text-gray-900">$3.50</span>
                            </div>
                            <div class="flex justify-between text-gray-600">
                                <span>Estimated Tax (8%)</span>
                                <span id="cart-tax" class="font-semibold text-gray-900">$0.00</span>
                            </div>
                            <div class="border-t border-gray-100 pt-3 flex justify-between text-base font-bold text-gray-900">
                                <span>Total Amount</span>
                                <span id="cart-total" class="text-primary-600 text-xl">$0.00</span>
                            </div>
                        </div>
                    </div>

                    <!-- Customer Information Form -->
                    <div class="bg-white p-6 rounded-2xl shadow-sm border border-gray-100 space-y-4">
                        <h3 class="text-lg font-bold text-gray-900 border-b border-gray-100 pb-3">Customer Information</h3>
                        <form id="checkout-form" onsubmit="submitOrder(event)" class="space-y-4">
                            <div>
                                <label class="block text-xs font-semibold uppercase tracking-wider text-gray-600 mb-1">Full Name *</label>
                                <input type="text" id="cust-name" required placeholder="e.g. John Doe" class="w-full px-4 py-2.5 bg-gray-50 border border-gray-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-primary-500">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold uppercase tracking-wider text-gray-600 mb-1">Contact Number *</label>
                                <input type="tel" id="cust-contact" required placeholder="e.g. +1 (555) 019-2834" class="w-full px-4 py-2.5 bg-gray-50 border border-gray-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-primary-500">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold uppercase tracking-wider text-gray-600 mb-1">Delivery Address *</label>
                                <textarea id="cust-address" required rows="2" placeholder="Street address, apartment, suite, etc." class="w-full px-4 py-2.5 bg-gray-50 border border-gray-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-primary-500"></textarea>
                            </div>
                            <div>
                                <label class="block text-xs font-semibold uppercase tracking-wider text-gray-600 mb-1">Order Notes (Optional)</label>
                                <input type="text" id="cust-notes" placeholder="e.g. Extra napkins, ring doorbell" class="w-full px-4 py-2.5 bg-gray-50 border border-gray-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-primary-500">
                            </div>
                            <button type="submit" id="checkout-btn" class="w-full bg-primary-500 hover:bg-primary-600 text-white font-semibold py-3.5 rounded-xl shadow-lg shadow-primary-500/30 transition-all duration-200 flex items-center justify-center">
                                <i class="fa-solid fa-circle-check mr-2"></i> Place Order Now
                            </button>
                        </form>
                    </div>
                </div>
            </div>
        </section>


        <!-- ================= VIEW 3: ADMIN PANEL ================= -->
        <section id="view-admin" class="hidden space-y-8">
            <!-- Admin Authentication Wrapper -->
            <div id="admin-auth-container" class="max-w-md mx-auto bg-white p-8 rounded-3xl shadow-sm border border-gray-100 my-12">
                <div class="text-center space-y-2 mb-6">
                    <div class="w-16 h-16 bg-primary-50 text-primary-500 rounded-2xl mx-auto flex items-center justify-center text-2xl shadow-inner">
                        <i class="fa-solid fa-lock"></i>
                    </div>
                    <h3 class="text-2xl font-bold text-gray-900">Admin Portal Login</h3>
                    <p class="text-sm text-gray-500">Enter your credentials to access management tools.</p>
                </div>
                <form onsubmit="handleAdminLogin(event)" class="space-y-4">
                    <div>
                        <label class="block text-xs font-semibold uppercase tracking-wider text-gray-600 mb-1">Username</label>
                        <input type="text" id="admin-user" required value="admin" class="w-full px-4 py-3 bg-gray-50 border border-gray-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-primary-500">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold uppercase tracking-wider text-gray-600 mb-1">Password</label>
                        <input type="password" id="admin-pass" required value="admin" class="w-full px-4 py-3 bg-gray-50 border border-gray-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-primary-500">
                    </div>
                    <button type="submit" class="w-full bg-primary-500 hover:bg-primary-600 text-white font-semibold py-3.5 rounded-xl shadow-lg shadow-primary-500/30 transition-all duration-200">
                        Login to Dashboard
                    </button>
                    <p class="text-xs text-center text-gray-400 mt-2">Default: Username: <b>admin</b> | Password: <b>admin</b></p>
                </form>
            </div>

            <!-- Admin Dashboard (Protected) -->
            <div id="admin-dashboard" class="hidden space-y-6">
                <!-- Admin Header & Logout -->
                <div class="flex flex-col sm:flex-row items-start sm:items-center justify-between bg-white p-6 rounded-2xl shadow-sm border border-gray-100 gap-4">
                    <div class="flex items-center space-x-3">
                        <div class="w-12 h-12 bg-primary-500 text-white rounded-xl flex items-center justify-center text-xl font-bold shadow-md shadow-primary-500/20">
                            <i class="fa-solid fa-user-gear"></i>
                        </div>
                        <div>
                            <h2 class="text-xl font-bold text-gray-900">Administrator Dashboard</h2>
                            <p class="text-xs text-gray-500 font-medium">Manage Food Menu & Customer Orders</p>
                        </div>
                    </div>
                    <div class="flex items-center space-x-3">
                        <button onclick="openAddFoodModal()" class="bg-primary-500 hover:bg-primary-600 text-white text-sm font-semibold px-4 py-2.5 rounded-xl shadow-md transition-all flex items-center">
                            <i class="fa-solid fa-plus mr-2"></i> Add New Food Item
                        </button>
                        <button onclick="handleAdminLogout()" class="bg-gray-100 hover:bg-gray-200 text-gray-700 text-sm font-semibold px-4 py-2.5 rounded-xl transition-all">
                            <i class="fa-solid fa-right-from-bracket mr-2"></i> Logout
                        </button>
                    </div>
                </div>

                <!-- Admin Sub-tabs -->
                <div class="flex space-x-2 border-b border-gray-200 pb-2">
                    <button onclick="switchAdminTab('orders')" id="admin-tab-orders" class="px-5 py-2.5 rounded-xl text-sm font-bold bg-primary-500 text-white shadow-sm transition-all">
                        <i class="fa-solid fa-clipboard-list mr-2"></i>Customer Orders (<span id="admin-order-count">0</span>)
                    </button>
                    <button onclick="switchAdminTab('foods')" id="admin-tab-foods" class="px-5 py-2.5 rounded-xl text-sm font-bold bg-white text-gray-600 hover:bg-gray-100 transition-all border border-gray-200">
                        <i class="fa-solid fa-utensils mr-2"></i>Food Menu Management
                    </button>
                </div>

                <!-- Admin Orders View -->
                <div id="admin-orders-section" class="bg-white rounded-2xl shadow-sm border border-gray-100 overflow-hidden">
                    <div class="p-6 border-b border-gray-100 flex justify-between items-center">
                        <h3 clas