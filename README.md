<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Authen Mart - Authentic Products by Artisans</title>
    
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Google Fonts (Nunito Sans for body, Merriweather for headings) -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Merriweather:wght@400;700&family=Nunito+Sans:wght@400;600;700&display=swap" rel="stylesheet">
    
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide-react@latest/dist/lucide-react.js"></script>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>

    <style>
        body {
            font-family: 'Nunito Sans', sans-serif;
            background-color: #f8f7f5; /* Soft beige background */
        }
        
        h1, h2, h3, h4, h5, h6, .logo {
            font-family: 'Merriweather', serif;
        }

        /* Custom accent colors for soft theme */
        .accent-bg { background-color: #c8a46e; } /* Muted gold/amber */
        .accent-bg-hover:hover { background-color: #b08f5c; }
        .accent-text { color: #c8a46e; }
        .accent-border { border-color: #c8a46e; }
        .accent-ring:focus { ring-color: #c8a46e; }

        /* Custom scrollbar for product carousel */
        .no-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .no-scrollbar {
            -ms-overflow-style: none;  /* IE and Edge */
            scrollbar-width: none;  /* Firefox */
        }

        /* Modal styles */
        .modal {
            display: none; /* Hidden by default */
            transition: opacity 0.3s ease;
        }
        .modal-overlay {
            position: fixed;
            inset: 0;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 50;
        }
        .modal-content {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: white;
            padding: 2rem;
            border-radius: 0.75rem;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
            z-index: 60;
            width: 90%;
            max-width: 500px;
        }
        
        /* Toast notification */
        #toast {
            position: fixed;
            bottom: 1.5rem;
            right: 1.5rem;
            transform: translateX(200%);
            transition: transform 0.5s ease-in-out;
            z-index: 100;
        }
        #toast.show {
            transform: translateX(0);
        }
    </style>
</head>
<body class="bg-gray-50">

    <!-- Header -->
    <header class="bg-white shadow-sm sticky top-0 z-40">
        <div class="container mx-auto px-4 sm:px-6 lg:px-8 py-3">
            <div class="flex items-center justify-between gap-4">
                
                <!-- Logo -->
                <a href="#" class="logo text-3xl font-bold text-amber-800">
                    Authen<span class="text-amber-600">Mart</span>
                </a>

                <!-- Search Bar -->
                <div class="hidden md:flex flex-grow max-w-xl">
                    <input type="text" placeholder="Search for authentic crafts, textiles, and more..." class="flex-1 w-full p-3 border border-gray-300 rounded-l-md focus:outline-none focus:ring-2 focus:ring-amber-600 focus:border-transparent">
                    <button class="px-5 py-3 bg-amber-600 text-white rounded-r-md hover:bg-amber-700 transition-colors">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
                    </button>
                </div>

                <!-- Header Nav -->
                <nav class="flex items-center gap-4 sm:gap-6">
                    <button onclick="showModal('login-modal')" class="hidden sm:flex items-center gap-2 text-gray-700 hover:text-amber-700 transition-colors">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"></path></svg>
                        <span class="font-semibold">Login</span>
                    </button>
                    <a href="#" class="flex items-center gap-2 text-gray-700 hover:text-amber-700 transition-colors">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"></path></svg>
                        <span class="hidden sm:inline font-semibold">Cart</span>
                        <span id="cart-count" class="bg-amber-500 text-white text-xs font-bold rounded-full h-5 w-5 flex items-center justify-center">0</span>
                    </a>
                    <!-- The "OLX" Style Button -->
                    <button onclick="showModal('sell-modal')" class="bg-amber-600 text-white font-bold py-2 px-4 rounded-lg shadow-md hover:bg-amber-700 transition-colors transform hover:scale-105">
                        Start Selling
                    </button>
                </nav>
            </div>
             <!-- Search Bar (Mobile) -->
             <div class="md:hidden mt-3">
                <div class="flex">
                    <input type="text" placeholder="Search..." class="flex-1 w-full p-2 border border-gray-300 rounded-l-md focus:outline-none focus:ring-2 focus:ring-amber-600">
                    <button class="px-4 py-2 bg-amber-600 text-white rounded-r-md hover:bg-amber-700">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
                    </button>
                </div>
            </div>
        </div>
    </header>

    <!-- Main Content -->
    <main class="container mx-auto px-4 sm:px-6 lg:px-8 py-6">

        <!-- Hero Banner -->
        <section class="mb-8">
            <div class="bg-amber-100 rounded-2xl p-8 md:p-12 text-amber-900 overflow-hidden shadow-lg flex flex-col md:flex-row items-center">
                <div class="md:w-1/2">
                    <h1 class="text-3xl md:text-5xl font-bold mb-4">From the Artisan's Heart,</h1>
                    <h2 class="text-3xl md:text-5xl font-bold mb-6">Directly to Your Home.</h2>
                    <p class="text-lg md:text-xl mb-8 text-amber-800">Discover 100% authentic, handcrafted goods sold directly by the creators.</p>
                    <a href="#product-grid" class="bg-amber-600 text-white font-bold py-3 px-8 rounded-lg shadow-md hover:bg-amber-700 transition-colors text-lg">
                        Shop Now
                    </a>
                </div>
                <div class="md:w-1/2 mt-8 md:mt-0">
                    <img src="https://placehold.co/600x400/fdf6e7/c8a46e?text=Authentic+Handicrafts&font=merriweather" alt="Authentic Handicrafts" class="rounded-xl shadow-md">
                </div>
            </div>
        </section>

        <!-- Category Section -->
        <section class="mb-10">
            <h2 class="text-2xl font-bold mb-5 text-gray-800">Shop by Category</h2>
            <div class="grid grid-cols-3 sm:grid-cols-4 md:grid-cols-8 gap-4">
                <!-- Category Item -->
                <a href="#" class="text-center group">
                    <div class="w-24 h-24 mx-auto bg-white rounded-full shadow-md p-5 overflow-hidden transform group-hover:scale-105 group-hover:shadow-lg transition-all duration-300">
                        <img src="https://placehold.co/100/fdf6e7/b08f5c?text=Textiles" alt="Textiles" class="w-full h-full object-contain">
                    </div>
                    <span class="block mt-2 font-semibold text-gray-700 group-hover:text-amber-700">Textiles</span>
                </a>
                <!-- Category Item -->
                <a href="#" class="text-center group">
                    <div class="w-24 h-24 mx-auto bg-white rounded-full shadow-md p-5 overflow-hidden transform group-hover:scale-105 group-hover:shadow-lg transition-all duration-300">
                        <img src="https://placehold.co/100/fdf6e7/b08f5c?text=Pottery" alt="Pottery" class="w-full h-full object-contain">
                    </div>
                    <span class="block mt-2 font-semibold text-gray-700 group-hover:text-amber-700">Pottery</span>
                </a>
                <!-- Category Item -->
                <a href="#" class="text-center group">
                    <div class="w-24 h-24 mx-auto bg-white rounded-full shadow-md p-5 overflow-hidden transform group-hover:scale-105 group-hover:shadow-lg transition-all duration-300">
                        <img src="https://placehold.co/100/fdf6e7/b08f5c?text=Jewelry" alt="Jewelry" class="w-full h-full object-contain">
                    </div>
                    <span class="block mt-2 font-semibold text-gray-700 group-hover:text-amber-700">Jewelry</span>
                </a>
                <!-- Category Item -->
                <a href="#" class="text-center group">
                    <div class="w-24 h-24 mx-auto bg-white rounded-full shadow-md p-5 overflow-hidden transform group-hover:scale-105 group-hover:shadow-lg transition-all duration-300">
                        <img src="https://placehold.co/100/fdf6e7/b08f5c?text=Paintings" alt="Paintings" class="w-full h-full object-contain">
                    </div>
                    <span class="block mt-2 font-semibold text-gray-700 group-hover:text-amber-700">Paintings</span>
                </a>
                <!-- Category Item -->
                <a href="#" class="text-center group">
                    <div class="w-24 h-24 mx-auto bg-white rounded-full shadow-md p-5 overflow-hidden transform group-hover:scale-105 group-hover:shadow-lg transition-all duration-300">
                        <img src="https://placehold.co/100/fdf6e7/b08f5c?text=Woodwork" alt="Woodwork" class="w-full h-full object-contain">
                    </div>
                    <span class="block mt-2 font-semibold text-gray-700 group-hover:text-amber-700">Woodwork</span>
                </a>
                <!-- Category Item -->
                <a href="#" class="text-center group">
                    <div class="w-24 h-24 mx-auto bg-white rounded-full shadow-md p-5 overflow-hidden transform group-hover:scale-105 group-hover:shadow-lg transition-all duration-300">
                        <img src="https://placehold.co/100/fdf6e7/b08f5c?text=Decor" alt="Decor" class="w-full h-full object-contain">
                    </div>
                    <span class="block mt-2 font-semibold text-gray-700 group-hover:text-amber-700">Home Decor</span>
                </a>
                <!-- Category Item -->
                <a href="#" class="text-center group">
                    <div class="w-24 h-24 mx-auto bg-white rounded-full shadow-md p-5 overflow-hidden transform group-hover:scale-105 group-hover:shadow-lg transition-all duration-300">
                        <img src="https://placehold.co/100/fdf6e7/b08f5c?text=Food" alt="Food" class="w-full h-full object-contain">
                    </div>
                    <span class="block mt-2 font-semibold text-gray-700 group-hover:text-amber-700">Artisan Food</span>
                </a>
                <!-- Category Item -->
                <a href="#" class="text-center group">
                    <div class="w-24 h-24 mx-auto bg-white rounded-full shadow-md p-5 overflow-hidden transform group-hover:scale-105 group-hover:shadow-lg transition-all duration-300">
                        <img src="https://placehold.co/100/fdf6e7/b08f5c?text=More" alt="More" class="w-full h-full object-contain">
                    </div>
                    <span class="block mt-2 font-semibold text-gray-700 group-hover:text-amber-700">View All</span>
                </a>
            </div>
        </section>

        <!-- Product Carousel - Today's Deals -->
        <section class="mb-10">
            <h2 class="text-2xl font-bold mb-5 text-gray-800">Today's Deals</h2>
            <div class="flex overflow-x-auto gap-4 sm:gap-6 pb-4 no-scrollbar">
                <!-- Product Card (Repeated) -->
                <div class="flex-shrink-0 w-64 bg-white rounded-lg shadow-md overflow-hidden group transition-all duration-300">
                    <img src="https://placehold.co/300x200/c8a46e/333?text=Terracotta+Set" alt="Product" class="w-full h-44 object-cover">
                    <div class="p-4">
                        <h3 class="font-semibold text-lg text-gray-800 truncate">Terracotta Pot Set</h3>
                        <p class="text-sm text-green-600 font-medium mb-2">Deal of the Day</p>
                        <div class="flex items-center justify-between">
                            <span class="text-xl font-bold text-gray-900">₹899</span>
                            <span class="text-md text-gray-500 line-through">₹1,299</span>
                        </div>
                    </div>
                </div>
                <!-- Card 2 -->
                <div class="flex-shrink-0 w-64 bg-white rounded-lg shadow-md overflow-hidden group transition-all duration-300">
                    <img src="https://placehold.co/300x200/c8a46e/333?text=Handwoven+Shawl" alt="Product" class="w-full h-44 object-cover">
                    <div class="p-4">
                        <h3 class="font-semibold text-lg text-gray-800 truncate">Handwoven Shawl</h3>
                        <p class="text-sm text-green-600 font-medium mb-2">30% Off</p>
                        <div class="flex items-center justify-between">
                            <span class="text-xl font-bold text-gray-900">₹1,499</span>
                            <span class="text-md text-gray-500 line-through">₹2,199</span>
                        </div>
                    </div>
                </div>
                <!-- Card 3 -->
                <div class="flex-shrink-0 w-64 bg-white rounded-lg shadow-md overflow-hidden group transition-all duration-300">
                    <img src="https://placehold.co/300x200/c8a46e/333?text=Brass+Idol" alt="Product" class="w-full h-44 object-cover">
                    <div class="p-4">
                        <h3 class="font-semibold text-lg text-gray-800 truncate">Brass Nandi Idol</h3>
                        <p class="text-sm text-green-600 font-medium mb-2">Special Offer</p>
                        <div class="flex items-center justify-between">
                            <span class="text-xl font-bold text-gray-900">₹2,499</span>
                            <span class="text-md text-gray-500 line-through">₹2,999</span>
                        </div>
                    </div>
                </div>
                <!-- Card 4 -->
                <div class="flex-shrink-0 w-64 bg-white rounded-lg shadow-md overflow-hidden group transition-all duration-300">
                    <img src="https://placehold.co/300x200/c8a46e/333?text=Blue+Pottery" alt="Product" class="w-full h-44 object-cover">
                    <div class="p-4">
                        <h3 class="font-semibold text-lg text-gray-800 truncate">Jaipuri Blue Pottery</h3>
                        <p class="text-sm text-green-600 font-medium mb-2">Limited Time</p>
                        <div class="flex items-center justify-between">
                            <span class="text-xl font-bold text-gray-900">₹799</span>
                            <span class="text-md text-gray-500 line-through">₹999</span>
                        </div>
                    </div>
                </div>
                <!-- Card 5 -->
                <div class="flex-shrink-0 w-64 bg-white rounded-lg shadow-md overflow-hidden group transition-all duration-300">
                    <img src="https://placehold.co/300x200/c8a46e/333?text=Warli+Painting" alt="Product" class="w-full h-44 object-cover">
                    <div class="p-4">
                        <h3 class="font-semibold text-lg text-gray-800 truncate">Warli Art Painting</h3>
                        <p class="text-sm text-green-600 font-medium mb-2">Artisan Special</p>
                        <div class="flex items-center justify-between">
                            <span class="text-xl font-bold text-gray-900">₹3,199</span>
                            <span class="text-md text-gray-500 line-through">₹3,499</span>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- "Calling all Artisans" CTA (OLX Feature) -->
        <section class="mb-10">
            <div class="bg-white rounded-2xl shadow-lg overflow-hidden flex flex-col md:flex-row items-center">
                <div class="md:w-1/2 p-8 md:p-12">
                    <h2 class="text-3xl font-bold text-gray-800 mb-4">Are You an Artisan?</h2>
                    <p class="text-lg text-gray-600 mb-6">Join our community and sell your authentic, handcrafted creations directly to customers across the nation. We provide the tools, you provide the art.</p>
                    <button onclick="showModal('sell-modal')" class="bg-amber-600 text-white font-bold py-3 px-8 rounded-lg shadow-md hover:bg-amber-700 transition-colors text-lg">
                        Start Selling Today
                    </button>
                </div>
                <div class="md:w-1/2">
                    <img src="https://placehold.co/600x400/f3eade/8c6d3a?text=Join+Our+Community&font=merriweather" alt="Artisan working" class="w-full h-full object-cover">
                </div>
            </div>
        </section>

        <!-- Product Grid - Featured Products -->
        <section id="product-grid-section">
            <h2 class="text-2xl font-bold mb-5 text-gray-800">New Arrivals</h2>
            <div id="product-grid" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-4 sm:gap-6">
                <!-- Product cards will be injected here by JavaScript -->
            </div>
        </section>

    </main>

    <!-- Footer -->
    <footer class="bg-stone-100 text-stone-700 mt-12 border-t border-stone-200">
        <div class="container mx-auto px-4 sm:px-6 lg:px-8 py-10">
            <!-- Coordinator Section (from first code) -->
            <div class="mb-8 p-6 bg-white rounded-lg shadow-sm">
                <h2 class="text-2xl font-bold text-center text-amber-800 mb-6">Our Team</h2>
                <div class="flex flex-wrap justify-around gap-6 text-center">
                    <div>
                        <h4 class="text-lg font-semibold text-gray-800">I. Sai Preetam</h4>
                        <p class="text-sm text-gray-600">Chief Technology Officer (CTO)</p>
                        <p class="text-sm text-gray-500">mail:ispreetam@gmail.com</p>
                    </div>
                    <div>
                        <h4 class="text-lg font-semibold text-gray-800">G. S. S. Abhi Ram</h4>
                        <p class="text-sm text-gray-600">Chief Executive Officer (CEO)</p>
                        <p class="text-sm text-gray-500">mail:gssabhiram@gmail.com</p>
                    </div>
                    <div>
                        <h4 class="text-lg font-semibold text-gray-800">S. Pardhiv Aacharya</h4>
                        <p class="text-sm text-gray-600">Chief Financial Officer (CFO)</p>
                        <p class="text-sm text-gray-500">mail:spaacharya@gmail.com</p>
                    </div>
                </div>
            </div>

            <!-- Standard Footer Links -->
            <div class="grid grid-cols-2 md:grid-cols-4 gap-8">
                <div>
                    <h5 class="font-bold text-stone-800 mb-3">HELP</h5>
                    <ul class="space-y-2">
                        <li><a href="#" class="hover:text-amber-700">Payments</a></li>
                        <li><a href="#" class="hover:text-amber-700">Shipping</a></li>
                        <li><a href="#" class="hover:text-amber-700">Returns</a></li>
                        <li><a href="#" class="hover:text-amber-700">FAQ</a></li>
                    </ul>
                </div>
                <div>
                    <h5 class="font-bold text-stone-800 mb-3">AUTHEN MART</h5>
                    <ul class="space-y-2">
                        <li><a href="#" class="hover:text-amber-700">About Us</a></li>
                        <li><a href="#" class="hover:text-amber-700">Careers</a></li>
                        <li><a href="#" class="hover:text-amber-700">Press</a></li>
                        <li><a href="#" class="hover:text-amber-700">Sell on Authen Mart</a></li>
                    </ul>
                </div>
                <div>
                    <h5 class="font-bold text-stone-800 mb-3">POLICY</h5>
                    <ul class="space-y-2">
                        <li><a href="#" class="hover:text-amber-700">Return Policy</a></li>
                        <li><a href="#" class="hover:text-amber-700">Terms of Use</a></li>
                        <li><a href="#" class="hover:text-amber-700">Privacy</a></li>
                    </ul>
                </div>
                <div>
                    <h5 class="font-bold text-stone-800 mb-3">SOCIAL</h5>
                    <ul class="space-y-2">
                        <li><a href="#" class="hover:text-amber-700">Facebook</a></li>
                        <li><a href="#" class="hover:text-amber-700">Instagram</a></li>
                        <li><a href="#" class="hover:text-amber-700">Twitter</a></li>
                    </ul>
                </div>
            </div>
        </div>
        <div class="bg-stone-200 py-4">
            <p class="text-center text-sm text-stone-600">&copy; 2025 Authen Mart. All rights reserved.</p>
        </div>
    </footer>

    <!-- MODALS -->

    <!-- Login Modal -->
    <div id="login-modal" class="modal">
        <div class="modal-overlay" onclick="hideModal('login-modal')"></div>
        <div class="modal-content">
            <button onclick="hideModal('login-modal')" class="absolute top-4 right-4 text-gray-400 hover:text-gray-600 text-3xl">&times;</button>
            <h2 class="text-2xl font-bold text-center text-amber-800 mb-6">Welcome Back</h2>
            <form>
                <div class="mb-4">
                    <label for="email" class="block text-sm font-medium text-gray-700 mb-2">Email Address</label>
                    <input type="email" id="email" class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-amber-600" placeholder="you@example.com">
                </div>
                <div class="mb-6">
                    <label for="password" class="block text-sm font-medium text-gray-700 mb-2">Password</label>
                    <input type="password" id="password" class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-amber-600" placeholder="••••••••">
                </div>
                <button type="submit" class="w-full bg-amber-600 text-white font-bold py-3 px-4 rounded-lg shadow-md hover:bg-amber-700 transition-colors">Login</button>
            </form>
            <p class="text-center text-sm text-gray-600 mt-4">New to Authen Mart? <a href="#" class="text-amber-700 font-semibold hover:underline">Create an account</a></p>
        </div>
    </div>

    <!-- Start Selling Modal (The "OLX" feature) -->
    <div id="sell-modal" class="modal">
        <div class="modal-overlay" onclick="hideModal('sell-modal')"></div>
        <div class="modal-content">
            <button onclick="hideModal('sell-modal')" class="absolute top-4 right-4 text-gray-400 hover:text-gray-600 text-3xl">&times;</button>
            <h2 class="text-2xl font-bold text-center text-amber-800 mb-6">Become an Authen Mart Artisan</h2>
            <p class="text-center text-gray-600 mb-6">Start selling your unique creations in 3 simple steps!</p>
            <div class="space-y-4">
                <div class="flex items-center gap-4 p-4 bg-gray-50 rounded-lg">
                    <span class="flex-shrink-0 bg-amber-600 text-white font-bold rounded-full h-10 w-10 flex items-center justify-center text-xl">1</span>
                    <div>
                        <h3 class="font-semibold text-lg text-gray-800">Create Your Artisan Profile</h3>
                        <p class="text-sm text-gray-600">Tell us your story and show your craft.</p>
                    </div>
                </div>
                <div class="flex items-center gap-4 p-4 bg-gray-50 rounded-lg">
                    <span class="flex-shrink-0 bg-amber-600 text-white font-bold rounded-full h-10 w-10 flex items-center justify-center text-xl">2</span>
                    <div>
                        <h3 class="font-semibold text-lg text-gray-800">Upload Your Products</h3>
                        <p class="text-sm text-gray-600">Take photos, set your price, and list your items. (Just like OLX!)</p>
                    </div>
                </div>
                <div class="flex items-center gap-4 p-4 bg-gray-50 rounded-lg">
                    <span class="flex-shrink-0 bg-amber-600 text-white font-bold rounded-full h-10 w-10 flex items-center justify-center text-xl">3</span>
                    <div>
                        <h3 class="font-semibold text-lg text-gray-800">Start Earning</h3>
                        <p class="text-sm text-gray-600">Ship directly to customers and get paid.</p>
                    </div>
                </div>
            </div>
            <button class="w-full bg-amber-600 text-white font-bold py-3 px-4 rounded-lg shadow-md hover:bg-amber-700 transition-colors mt-8">Get Started</button>
        </div>
    </div>

    <!-- Toast Notification -->
    <div id="toast" class="bg-green-600 text-white py-3 px-6 rounded-lg shadow-lg font-semibold">
        Item added to cart!
    </div>


    <script>
        document.addEventListener('DOMContentLoaded', () => {
            
            // Mock product data (Authentic Products)
            const products = [
                { id: 1, name: "Hand-Painted Kalamkari Saree", price: "4,299", rating: 5, img: "https://placehold.co/400x400/8d6e63/ffffff?text=Kalamkari+Saree" },
                { id: 2, name: "Terracotta Clay Pot Set (3)", price: "1,199", rating: 4, img: "https://placehold.co/400x400/a1887f/ffffff?text=Terracotta+Pots" },
                { id: 3, name: "Hand-Carved Brass Diya", price: "799", rating: 5, img: "https://placehold.co/400x400/ffc107/333?text=Brass+Diya" },
                { id: 4, name: "Jaipuri Blue Pottery Vase", price: "1,499", rating: 4, img: "https://placehold.co/400x400/4fc3f7/ffffff?text=Blue+Pottery" },
                { id: 5, name: "Warli Painting on Canvas", price: "2,999", rating: 5, img: "https://placehold.co/400x400/795548/ffffff?text=Warli+Painting" },
                { id: 6, name: "Pure Sandalwood Incense", price: "499", rating: 5, img: "https://placehold.co/400x400/f5e5c0/8c6d3a?text=Incense" },
                { id: 7, name: "Hand-Carved Wooden Elephant", price: "2,199", rating: 4, img: "https://placehold.co/400x400/a1887f/ffffff?text=Wooden+Elephant" },
                { id: 8, name: "Organic Wild Forest Honey", price: "649", rating: 5, img: "https://placehold.co/400x400/ffab00/ffffff?text=Honey" },
                { id: 9, name: "Pattachitra Coasters (Set of 4)", price: "899", rating: 4, img: "https://placehold.co/400x400/e57373/ffffff?text=Pattachitra" },
                { id: 10, name: "Dhokra Art Tribal Musician", price: "3,499", rating: 5, img: "https://placehold.co/400x400/bcaaa4/3e2723?text=Dhokra+Art" },
            ];

            const productGrid = document.getElementById('product-grid');
            const cartCountEl = document.getElementById('cart-count');
            const toastEl = document.getElementById('toast');
            let cartCount = 0;
            let toastTimeout;

            // Function to generate star ratings
            function getStarRating(rating) {
                let stars = '';
                // Solid Star
                const starSVG = `<svg class="w-4 h-4 text-yellow-400" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.54-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.462a1 1 0 00.95-.69l1.07-3.292z"></path></svg>`;
                // Empty Star
                const emptyStarSVG = `<svg class="w-4 h-4 text-gray-300" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.54-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.462a1 1 0 00.95-.69l1.07-3.292z"></path></svg>`;
                
                for(let i = 0; i < 5; i++) {
                    stars += (i < rating) ? starSVG : emptyStarSVG;
                }
                return `<div class="flex">${stars}</div>`;
            }

            // Populate product grid
            products.forEach(product => {
                const card = document.createElement('div');
                card.className = 'bg-white rounded-lg shadow-md overflow-hidden flex flex-col group transform transition-all duration-300 hover:shadow-xl hover:scale-105';
                card.innerHTML = `
                    <div class="h-56 w-full overflow-hidden">
                        <img src="${product.img}" alt="${product.name}" class="w-full h-full object-cover">
                    </div>
                    <div class="p-4 flex flex-col flex-grow">
                        <h3 class="font-semibold text-lg text-gray-800 truncate" title="${product.name}">${product.name}</h3>
                        <div class="flex items-center my-2">
                            ${getStarRating(product.rating)}
                        </div>
                        <p class_="text-xl font-bold text-gray-900 mb-3">₹${product.price}</p>
                        <button class="add-to-cart-btn mt-auto w-full bg-amber-100 text-amber-800 border-2 border-amber-600 font-bold py-2 px-4 rounded-lg hover:bg-amber-600 hover:text-white transition-colors">
                            Add to Cart
                        </button>
                    </div>
                `;
                productGrid.appendChild(card);
            });

            // Add event listeners to all "Add to Cart" buttons
            document.querySelectorAll('.add-to-cart-btn').forEach(button => {
                button.addEventListener('click', () => {
                    // Update cart count
                    cartCount++;
                    cartCountEl.textContent = cartCount;
                    
                    // Show toast message
                    if (toastTimeout) clearTimeout(toastTimeout);
                    toastEl.classList.add('show');

                    // Hide toast message after 3 seconds
                    toastTimeout = setTimeout(() => {
                        toastEl.classList.remove('show');
                    }, 3000);
                });
            });
        });

        // Modal Functions
        function showModal(modalId) {
            const modal = document.getElementById(modalId);
            if (modal) {
                modal.style.display = 'block';
                // Trigger reflow to animate
                setTimeout(() => {
                    modal.style.opacity = '1';
                    modal.querySelector('.modal-content').style.transform = 'translate(-50%, -50%) scale(1)';
                }, 10);
            }
        }

        function hideModal(modalId) {
            const modal = document.getElementById(modalId);
            if (modal) {
                modal.style.opacity = '0';
                modal.querySelector('.modal-content').style.transform = 'translate(-50%, -45%) scale(0.95)';
                setTimeout(() => {
                    modal.style.display = 'none';
                }, 300); // Wait for transition to finish
            }
        }
    </script>
</body>
</html>
