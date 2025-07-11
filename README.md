<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Siddhi Vinayak Jewellers</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Firebase SDK -->
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-database-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-storage-compat.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;600;700&family=Inter:wght@300;400;500;600&family=Poppins:wght@300;400;500;600;700&family=Montserrat:wght@300;400;500;600;700&family=Roboto:wght@300;400;500;700&display=swap');
        
        .fade-in {
            animation: fadeIn 0.8s ease-in-out;
        }
        
        .fade-out {
            animation: fadeOut 0.5s ease-in-out;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        @keyframes fadeOut {
            from { opacity: 1; transform: translateY(0); }
            to { opacity: 0; transform: translateY(-20px); }
        }
        
        .rotate-logo {
            animation: rotateLogo 3s ease-in-out infinite;
        }
        
        @keyframes rotateLogo {
            0%, 100% { transform: rotate(0deg) scale(1); }
            50% { transform: rotate(360deg) scale(1.1); }
        }
        
        .gradient-bg {
            background: linear-gradient(135deg, #f59e0b 0%, #d97706 50%, #92400e 100%);
        }
        
        .glass-effect {
            backdrop-filter: blur(10px);
            background: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .product-card {
            transition: all 0.3s ease;
        }
        
        .product-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
        }
        
        .zoom-container {
            position: relative;
            overflow: hidden;
        }
        
        .zoom-image {
            transition: transform 0.3s ease;
            cursor: zoom-in;
        }
        
        .zoom-image:hover {
            transform: scale(1.2);
        }
        
        .category-card {
            transition: all 0.3s ease;
            cursor: pointer;
        }
        
        .category-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 15px 30px rgba(0,0,0,0.1);
        }
        
        .page-transition {
            transition: all 0.5s ease-in-out;
        }
    </style>
</head>
<body class="font-inter bg-gray-50 min-h-screen overflow-x-hidden">
    <!-- Loading Screen -->
    <div id="loadingScreen" class="fixed inset-0 gradient-bg flex items-center justify-center z-50">
        <div class="text-center">
            <div class="flex items-center justify-center space-x-8 mb-6">
                <div class="rotate-logo">
                    <div class="w-32 h-32 bg-white rounded-full flex items-center justify-center shadow-2xl border-4 border-amber-600">
                        <span class="text-4xl font-bold text-amber-800" style="font-family: 'Montserrat', sans-serif;">SVJ</span>
                    </div>
                </div>
                <div class="text-left">
                    <h1 class="text-4xl font-bold text-white mb-2" style="font-family: 'Poppins', sans-serif;">Siddhi Vinayak Jewellers</h1>
                    <p class="text-amber-100 text-lg" style="font-family: 'Roboto', sans-serif;">Excellence in Gold Since 2007</p>
                </div>
            </div>
        </div>
    </div>

    <!-- Auth Screen -->
    <div id="authScreen" class="hidden fixed inset-0 bg-gradient-to-br from-amber-50 to-white flex items-center justify-center p-4">
        <div class="max-w-md w-full">
            <!-- Store Info -->
            <div class="text-center mb-6 fade-in">
                <div class="w-16 h-16 bg-gradient-to-br from-amber-500 to-amber-700 rounded-full flex items-center justify-center shadow-2xl border-2 border-white mx-auto mb-3">
                    <span class="text-lg font-bold text-white" style="font-family: 'Montserrat', sans-serif;">SVJ</span>
                </div>
                <h2 class="text-2xl font-bold text-amber-900 mb-3" style="font-family: 'Poppins', sans-serif;">Siddhi Vinayak Jewellers</h2>
                
                <!-- About Us Section -->
                <div class="bg-white rounded-lg p-3 shadow-lg mb-4 text-center">
                    <h3 class="text-base font-semibold text-amber-700 mb-2" style="font-family: 'Montserrat', sans-serif;">About Us</h3>
                    <div class="text-xs text-gray-700 space-y-1" style="font-family: 'Roboto', sans-serif;">
                        <p>✨ <strong>Since 2007</strong> - 16+ years of excellence</p>
                        <p>👑 <strong>Custom Designs</strong> - Personalized jewelry</p>
                        <p>🔧 <strong>Expert Repairs</strong> - Professional restoration</p>
                        <p>🎉 <strong>5000+ Happy Customers</strong> - Trusted quality</p>
                        <p>🏆 <strong>Premium Gold</strong> - 24K, 22K, 20K, 18K options</p>
                    </div>
                </div>
            </div>

            <!-- Auth Tabs -->
            <div class="glass-effect rounded-2xl p-6 shadow-xl">
                <div class="flex mb-6">
                    <button id="loginTab" class="flex-1 py-2 px-4 text-center font-semibold text-amber-800 border-b-2 border-amber-500" style="font-family: 'Poppins', sans-serif;">Login</button>
                    <button id="signupTab" class="flex-1 py-2 px-4 text-center font-semibold text-amber-600 border-b-2 border-transparent" style="font-family: 'Poppins', sans-serif;">Sign Up</button>
                </div>

                <!-- Login Form -->
                <form id="loginForm" class="space-y-4">
                    <div>
                        <label class="block text-sm font-medium text-amber-800 mb-1" style="font-family: 'Roboto', sans-serif;">Email</label>
                        <input type="email" id="loginEmail" class="w-full px-4 py-2 border border-amber-200 rounded-lg focus:ring-2 focus:ring-amber-500 focus:border-transparent" required>
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-amber-800 mb-1" style="font-family: 'Roboto', sans-serif;">Password</label>
                        <input type="password" id="loginPassword" class="w-full px-4 py-2 border border-amber-200 rounded-lg focus:ring-2 focus:ring-amber-500 focus:border-transparent" required>
                    </div>
                    <button type="submit" class="w-full bg-gradient-to-r from-amber-600 to-amber-700 text-white py-2 px-4 rounded-lg font-semibold hover:from-amber-700 hover:to-amber-800 transition-all" style="font-family: 'Poppins', sans-serif;">
                        Login to Collection
                    </button>
                </form>

                <!-- Signup Form -->
                <form id="signupForm" class="space-y-4 hidden">
                    <div>
                        <label class="block text-sm font-medium text-amber-800 mb-1" style="font-family: 'Roboto', sans-serif;">Full Name</label>
                        <input type="text" id="signupName" class="w-full px-4 py-2 border border-amber-200 rounded-lg focus:ring-2 focus:ring-amber-500 focus:border-transparent" required>
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-amber-800 mb-1" style="font-family: 'Roboto', sans-serif;">Email</label>
                        <input type="email" id="signupEmail" class="w-full px-4 py-2 border border-amber-200 rounded-lg focus:ring-2 focus:ring-amber-500 focus:border-transparent" required>
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-amber-800 mb-1" style="font-family: 'Roboto', sans-serif;">Phone Number</label>
                        <input type="tel" id="signupPhone" class="w-full px-4 py-2 border border-amber-200 rounded-lg focus:ring-2 focus:ring-amber-500 focus:border-transparent" required>
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-amber-800 mb-1" style="font-family: 'Roboto', sans-serif;">Password</label>
                        <input type="password" id="signupPassword" class="w-full px-4 py-2 border border-amber-200 rounded-lg focus:ring-2 focus:ring-amber-500 focus:border-transparent" required>
                    </div>
                    <button type="submit" class="w-full bg-gradient-to-r from-amber-600 to-amber-700 text-white py-2 px-4 rounded-lg font-semibold hover:from-amber-700 hover:to-amber-800 transition-all" style="font-family: 'Poppins', sans-serif;">
                        Create Account
                    </button>
                </form>

                <!-- Contact Info -->
                <div class="mt-4 pt-4 border-t border-amber-200 text-center" style="font-family: 'Roboto', sans-serif;">
                    <h4 class="text-sm font-semibold text-amber-800 mb-2">Contact Us</h4>
                    <div class="space-y-2">
                        <a href="https://wa.me/919975624544" class="flex items-center justify-center space-x-1 bg-green-500 hover:bg-green-600 text-white py-1.5 px-3 rounded-md transition-colors text-xs">
                            <span>📞</span>
                            <span class="font-medium">WhatsApp: +91 9975624544</span>
                        </a>
                        <a href="https://maps.app.goo.gl/KaT1BHJBJTr6fzXE8" target="_blank" class="flex items-center justify-center space-x-1 bg-blue-500 hover:bg-blue-600 text-white py-1.5 px-3 rounded-md transition-colors text-xs">
                            <span>📍</span>
                            <span class="font-medium">Visit Our Store</span>
                        </a>
                        <a href="https://www.instagram.com/siddhi_vinayak_jewellers_106?igsh=MTByb29uN3Frb3FsOQ==" target="_blank" class="flex items-center justify-center space-x-1 bg-pink-500 hover:bg-pink-600 text-white py-1.5 px-3 rounded-md transition-colors text-xs">
                            <span>📸</span>
                            <span class="font-medium">Follow on Instagram</span>
                        </a>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Main App -->
    <div id="mainApp" class="hidden">
        <!-- Header -->
        <header class="bg-white shadow-lg sticky top-0 z-40">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex justify-between items-center py-4">
                    <div class="flex items-center space-x-3">
                        <div class="w-12 h-12 bg-amber-600 rounded-full flex items-center justify-center shadow-lg">
                            <span class="text-lg font-bold text-white" style="font-family: 'Montserrat', sans-serif;">SVJ</span>
                        </div>
                        <div>
                            <h1 class="text-xl font-bold text-amber-900" style="font-family: 'Poppins', sans-serif;">Siddhi Vinayak Jewellers</h1>
                            <p class="text-xs text-amber-700" style="font-family: 'Roboto', sans-serif;">Excellence Since 2007</p>
                        </div>
                    </div>
                    <div class="flex items-center space-x-4">
                        <div id="pageNavigation" class="hidden space-x-2">
                            <button id="homePage" class="px-4 py-2 bg-amber-600 text-white rounded-lg font-medium transition-colors">Home</button>
                            <button id="collectionPage" class="px-4 py-2 bg-gray-200 text-gray-700 rounded-lg font-medium hover:bg-gray-300 transition-colors">Collection</button>
                        </div>
                        <span id="userGreeting" class="text-amber-800 font-medium"></span>
                        <button id="logoutBtn" class="text-amber-600 hover:text-amber-800 transition-colors">Logout</button>
                    </div>
                </div>
            </div>
        </header>

        <!-- Owner Dashboard -->
        <div id="ownerDashboard" class="hidden">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
                <div class="mb-8">
                    <h2 class="text-3xl font-bold text-amber-900 mb-2" style="font-family: 'Poppins', sans-serif;">Owner Dashboard</h2>
                    <p class="text-amber-700" style="font-family: 'Roboto', sans-serif;">Manage your jewelry collection and customers</p>
                </div>

                <!-- Dashboard Tabs -->
                <div class="flex space-x-1 mb-8">
                    <button id="productsTab" class="px-6 py-3 bg-amber-600 text-white rounded-lg font-semibold" style="font-family: 'Poppins', sans-serif;">Products</button>
                    <button id="customersTab" class="px-6 py-3 bg-gray-200 text-gray-700 rounded-lg font-semibold hover:bg-gray-300" style="font-family: 'Poppins', sans-serif;">Customers</button>
                    <button id="addProductTab" class="px-6 py-3 bg-gray-200 text-gray-700 rounded-lg font-semibold hover:bg-gray-300" style="font-family: 'Poppins', sans-serif;">Add Product</button>
                </div>

                <!-- Products Management -->
                <div id="productsPanel" class="fade-in">
                    <div class="bg-white rounded-xl shadow-lg p-6 mb-6">
                        <h3 class="text-xl font-semibold text-amber-900 mb-4">Product Collection</h3>
                        <div id="ownerProductsList" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                            <div class="text-center py-12 text-gray-500 col-span-full">
                                <p>No products added yet. Click "Add Product" to get started.</p>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Customers Panel -->
                <div id="customersPanel" class="hidden">
                    <div class="bg-white rounded-xl shadow-lg p-6">
                        <h3 class="text-xl font-semibold text-amber-900 mb-4">Registered Customers</h3>
                        <div id="customersList" class="space-y-4">
                            <div class="text-center py-12 text-gray-500">
                                <p>No customers registered yet.</p>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Add Product Panel -->
                <div id="addProductPanel" class="hidden">
                    <div class="bg-white rounded-xl shadow-lg p-6">
                        <h3 class="text-xl font-semibold text-amber-900 mb-4">Add New Product</h3>
                        <form id="addProductForm" class="space-y-6">
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                                <div>
                                    <label class="block text-sm font-medium text-amber-800 mb-2">Product Name</label>
                                    <input type="text" id="productName" class="w-full px-4 py-2 border border-amber-200 rounded-lg focus:ring-2 focus:ring-amber-500" required>
                                </div>
                                <div>
                                    <label class="block text-sm font-medium text-amber-800 mb-2">Product Code</label>
                                    <input type="text" id="productCode" class="w-full px-4 py-2 border border-amber-200 rounded-lg focus:ring-2 focus:ring-amber-500" required>
                                </div>
                                <div>
                                    <label class="block text-sm font-medium text-amber-800 mb-2">Weight (grams)</label>
                                    <input type="number" id="productWeight" step="0.01" class="w-full px-4 py-2 border border-amber-200 rounded-lg focus:ring-2 focus:ring-amber-500" required>
                                </div>
                                <div>
                                    <label class="block text-sm font-medium text-amber-800 mb-2">Category</label>
                                    <select id="productCategory" class="w-full px-4 py-2 border border-amber-200 rounded-lg focus:ring-2 focus:ring-amber-500" required>
                                        <option value="">Select Category</option>
                                        <option value="Necklace">Necklace</option>
                                        <option value="Chains">Chains</option>
                                        <option value="Bangles">Bangles</option>
                                        <option value="Bracelet">Bracelet</option>
                                        <option value="Rings">Rings</option>
                                        <option value="Tops">Tops</option>
                                        <option value="Earrings">Earrings</option>
                                        <option value="Pendant">Pendant</option>
                                        <option value="Mangalsutra">Mangalsutra</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block text-sm font-medium text-amber-800 mb-2">Purity</label>
                                    <select id="productPurity" class="w-full px-4 py-2 border border-amber-200 rounded-lg focus:ring-2 focus:ring-amber-500" required>
                                        <option value="">Select Purity</option>
                                        <option value="24K">24K Gold</option>
                                        <option value="22K">22K Gold</option>
                                        <option value="20K">20K Gold</option>
                                        <option value="18K">18K Gold</option>
                                    </select>
                                </div>
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-amber-800 mb-2">Product Images</label>
                                <input type="file" id="productImages" multiple accept="image/*" class="w-full px-4 py-2 border border-amber-200 rounded-lg focus:ring-2 focus:ring-amber-500">
                                <p class="text-xs text-amber-600 mt-1">Select multiple images for your product</p>
                            </div>
                            <button type="submit" class="w-full bg-gradient-to-r from-amber-500 to-yellow-500 text-white py-3 px-6 rounded-lg font-semibold hover:from-amber-600 hover:to-yellow-600 transition-all">
                                Add Product to Collection
                            </button>
                        </form>
                    </div>
                </div>
            </div>
        </div>

        <!-- Customer Home Page -->
        <div id="customerHome" class="hidden">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
                <!-- Store Info Banner -->
                <div class="bg-gradient-to-r from-amber-600 to-amber-700 rounded-xl p-6 mb-8 text-white">
                    <div class="text-center">
                        <h2 class="text-2xl font-bold mb-2" style="font-family: 'Poppins', sans-serif;">Welcome to Our Exclusive Collection</h2>
                        <div class="grid grid-cols-2 md:grid-cols-4 gap-4 text-sm" style="font-family: 'Roboto', sans-serif;">
                            <div>✨ Since 2007</div>
                            <div>👑 Custom Designs</div>
                            <div>🔧 Repair Services</div>
                            <div>🎉 5000+ Happy Customers</div>
                        </div>
                    </div>
                </div>

                <!-- Categories Grid -->
                <div class="mb-8">
                    <h3 class="text-2xl font-bold text-amber-900 mb-6 text-center" style="font-family: 'Poppins', sans-serif;">Browse by Category</h3>
                    <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-6">
                        <div class="category-card bg-white rounded-xl shadow-lg p-6 text-center" onclick="filterByCategory('Necklace')">
                            <div class="text-4xl mb-3">📿</div>
                            <h4 class="font-semibold text-amber-900" style="font-family: 'Poppins', sans-serif;">Necklace</h4>
                        </div>
                        <div class="category-card bg-white rounded-xl shadow-lg p-6 text-center" onclick="filterByCategory('Chains')">
                            <div class="text-4xl mb-3">⛓️</div>
                            <h4 class="font-semibold text-amber-900" style="font-family: 'Poppins', sans-serif;">Chains</h4>
                        </div>
                        <div class="category-card bg-white rounded-xl shadow-lg p-6 text-center" onclick="filterByCategory('Bangles')">
                            <div class="text-4xl mb-3">💍</div>
                            <h4 class="font-semibold text-amber-900" style="font-family: 'Poppins', sans-serif;">Bangles</h4>
                        </div>
                        <div class="category-card bg-white rounded-xl shadow-lg p-6 text-center" onclick="filterByCategory('Bracelet')">
                            <div class="text-4xl mb-3">🔗</div>
                            <h4 class="font-semibold text-amber-900" style="font-family: 'Poppins', sans-serif;">Bracelet</h4>
                        </div>
                        <div class="category-card bg-white rounded-xl shadow-lg p-6 text-center" onclick="filterByCategory('Rings')">
                            <div class="text-4xl mb-3">💎</div>
                            <h4 class="font-semibold text-amber-900" style="font-family: 'Poppins', sans-serif;">Rings</h4>
                        </div>
                        <div class="category-card bg-white rounded-xl shadow-lg p-6 text-center" onclick="filterByCategory('Tops')">
                            <div class="text-4xl mb-3">👑</div>
                            <h4 class="font-semibold text-amber-900" style="font-family: 'Poppins', sans-serif;">Tops</h4>
                        </div>
                        <div class="category-card bg-white rounded-xl shadow-lg p-6 text-center" onclick="filterByCategory('Earrings')">
                            <div class="text-4xl mb-3">👂</div>
                            <h4 class="font-semibold text-amber-900" style="font-family: 'Poppins', sans-serif;">Earrings</h4>
                        </div>
                        <div class="category-card bg-white rounded-xl shadow-lg p-6 text-center" onclick="filterByCategory('Pendant')">
                            <div class="text-4xl mb-3">🔸</div>
                            <h4 class="font-semibold text-amber-900" style="font-family: 'Poppins', sans-serif;">Pendant</h4>
                        </div>
                        <div class="category-card bg-white rounded-xl shadow-lg p-6 text-center" onclick="filterByCategory('Mangalsutra')">
                            <div class="text-4xl mb-3">🌟</div>
                            <h4 class="font-semibold text-amber-900" style="font-family: 'Poppins', sans-serif;">Mangalsutra</h4>
                        </div>
                    </div>
                </div>

                <!-- Featured Products -->
                <div class="mb-8">
                    <h3 class="text-2xl font-bold text-amber-900 mb-6 text-center" style="font-family: 'Poppins', sans-serif;">Featured Products</h3>
                    <div id="featuredProducts" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                        <div class="text-center py-12 text-gray-500 col-span-full">
                            <p>Our exclusive collection is being curated. Please check back soon!</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Customer Collection View -->
        <div id="customerCollection" class="hidden">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
                <!-- Filter Header -->
                <div class="bg-white rounded-xl shadow-lg p-6 mb-8">
                    <div class="flex flex-wrap items-center justify-between gap-4">
                        <div>
                            <h2 class="text-2xl font-bold text-amber-900" style="font-family: 'Poppins', sans-serif;">Collection</h2>
                            <p id="categoryTitle" class="text-amber-700" style="font-family: 'Roboto', sans-serif;">All Products</p>
                        </div>
                        <div class="flex flex-wrap gap-4">
                            <select id="categoryFilter" class="px-4 py-2 border border-amber-200 rounded-lg focus:ring-2 focus:ring-amber-500">
                                <option value="">All Categories</option>
                                <option value="Necklace">Necklace</option>
                                <option value="Chains">Chains</option>
                                <option value="Bangles">Bangles</option>
                                <option value="Bracelet">Bracelet</option>
                                <option value="Rings">Rings</option>
                                <option value="Tops">Tops</option>
                                <option value="Earrings">Earrings</option>
                                <option value="Pendant">Pendant</option>
                                <option value="Mangalsutra">Mangalsutra</option>
                            </select>
                            <select id="purityFilter" class="px-4 py-2 border border-amber-200 rounded-lg focus:ring-2 focus:ring-amber-500">
                                <option value="">All Purity</option>
                                <option value="24K">24K Gold</option>
                                <option value="22K">22K Gold</option>
                                <option value="20K">20K Gold</option>
                                <option value="18K">18K Gold</option>
                            </select>
                            <button id="clearFilters" class="px-4 py-2 bg-gray-200 text-gray-700 rounded-lg hover:bg-gray-300 transition-colors">Clear Filters</button>
                        </div>
                    </div>
                </div>

                <!-- Products Grid -->
                <div id="productsGrid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                    <div class="text-center py-12 text-gray-500 col-span-full">
                        <p>Our exclusive collection is being curated. Please check back soon!</p>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Product Modal -->
    <div id="productModal" class="hidden fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center p-4 z-50">
        <div class="bg-white rounded-xl max-w-4xl w-full max-h-[90vh] overflow-y-auto">
            <div class="p-6">
                <div class="flex justify-between items-center mb-4">
                    <h3 id="modalProductName" class="text-2xl font-playfair font-bold text-amber-900"></h3>
                    <button id="closeModal" class="text-gray-500 hover:text-gray-700 text-2xl">&times;</button>
                </div>
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                    <div>
                        <div id="modalImageContainer" class="zoom-container rounded-lg overflow-hidden mb-4 relative">
                            <img id="modalMainImage" class="zoom-image w-full h-96 object-contain bg-gray-100" src="" alt="">
                            <button id="prevImageBtn" class="absolute left-2 top-1/2 transform -translate-y-1/2 bg-black bg-opacity-50 text-white p-2 rounded-full hover:bg-opacity-70 transition-all">
                                <svg width="20" height="20" fill="currentColor" viewBox="0 0 24 24">
                                    <path d="M15.41 7.41L14 6l-6 6 6 6 1.41-1.41L10.83 12z"/>
                                </svg>
                            </button>
                            <button id="nextImageBtn" class="absolute right-2 top-1/2 transform -translate-y-1/2 bg-black bg-opacity-50 text-white p-2 rounded-full hover:bg-opacity-70 transition-all">
                                <svg width="20" height="20" fill="currentColor" viewBox="0 0 24 24">
                                    <path d="M8.59 16.59L10 18l6-6-6-6-1.41 1.41L13.17 12z"/>
                                </svg>
                            </button>
                        </div>
                        <div id="modalImageThumbs" class="flex space-x-2 overflow-x-auto"></div>
                    </div>
                    <div>
                        <div id="modalProductDetails" class="space-y-4"></div>
                        <button id="inquiryBtn" class="w-full mt-6 bg-green-500 hover:bg-green-600 text-white py-3 px-6 rounded-lg font-semibold transition-colors flex items-center justify-center space-x-2">
                            <span>💬</span>
                            <span>WhatsApp Inquiry</span>
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Firebase Configuration
       const firebaseConfig = {
  apiKey: "AIzaSyCiKd02-kuJJSZW_gyfBYjzkr4fQF7DQ7U",
  authDomain: "svj-website-246aa.firebaseapp.com",
  databaseURL: "https://svj-website-246aa-default-rtdb.firebaseio.com",
  projectId: "svj-website-246aa",
  storageBucket: "svj-website-246aa.firebasestorage.app",
  messagingSenderId: "623018580365",
  appId: "1:623018580365:web:68bfb9da011f8a3a1c05c4",
  measurementId: "G-J3105HRZCQ"
};
        

        // Initialize Firebase
        firebase.initializeApp(firebaseConfig);
        const database = firebase.database();
        const storage = firebase.storage();

        // Global state
        let currentUser = null;
        let products = [];
        let customers = [];
        let currentProductImages = [];
        let currentImageIndex = 0;
        let currentProductForModal = null;
        let currentPage = 'home';
        let currentCategory = '';
        let currentPurity = '';

        // Firebase helper functions
        async function uploadImageToFirebase(file, path) {
            try {
                const storageRef = storage.ref().child(path);
                const snapshot = await storageRef.put(file);
                const downloadURL = await snapshot.ref.getDownloadURL();
                return downloadURL;
            } catch (error) {
                console.error('Error uploading image:', error);
                throw error;
            }
        }

        function loadProductsFromFirebase() {
            database.ref('products').on('value', (snapshot) => {
                const data = snapshot.val();
                products = data ? Object.keys(data).map(key => ({ id: key, ...data[key] })) : [];
                
                // Update UI if needed
                if (currentUser && currentUser.role === 'owner') {
                    renderOwnerProducts();
                } else if (currentUser && currentUser.role === 'customer') {
                    if (currentPage === 'home') {
                        renderFeaturedProducts();
                    } else {
                        renderCustomerProducts();
                    }
                }
            });
        }

        function loadCustomersFromFirebase() {
            database.ref('customers').on('value', (snapshot) => {
                const data = snapshot.val();
                customers = data ? Object.keys(data).map(key => ({ id: key, ...data[key] })) : [];
                
                // Update UI if needed
                if (currentUser && currentUser.role === 'owner') {
                    renderCustomersList();
                }
            });
        }

        async function saveProductToFirebase(product) {
            try {
                const productRef = database.ref('products').push();
                await productRef.set({
                    ...product,
                    id: productRef.key,
                    addedAt: firebase.database.ServerValue.TIMESTAMP
                });
                return productRef.key;
            } catch (error) {
                console.error('Error saving product:', error);
                throw error;
            }
        }

        async function saveCustomerToFirebase(customer) {
            try {
                const customerRef = database.ref('customers').push();
                await customerRef.set({
                    ...customer,
                    id: customerRef.key,
                    registeredAt: firebase.database.ServerValue.TIMESTAMP
                });
                return customerRef.key;
            } catch (error) {
                console.error('Error saving customer:', error);
                throw error;
            }
        }

        async function removeProductFromFirebase(productId) {
            try {
                await database.ref(`products/${productId}`).remove();
            } catch (error) {
                console.error('Error removing product:', error);
                throw error;
            }
        }

        // Initialize app
        document.addEventListener('DOMContentLoaded', function() {
            // Load data from Firebase
            loadProductsFromFirebase();
            loadCustomersFromFirebase();

            // Show loading screen while Firebase loads data
            let dataLoaded = false;
            let minLoadingTime = false;
            
            // Minimum loading time of 3 seconds
            setTimeout(() => {
                minLoadingTime = true;
                if (dataLoaded) {
                    hideLoadingScreen();
                }
            }, 3000);
            
            // Check if data is loaded
            const checkDataLoaded = () => {
                // Consider data loaded when we have initial Firebase connection
                dataLoaded = true;
                if (minLoadingTime) {
                    hideLoadingScreen();
                }
            };
            
            // Wait a bit for Firebase to initialize
            setTimeout(checkDataLoaded, 1000);
            
            function hideLoadingScreen() {
                document.getElementById('loadingScreen').classList.add('fade-out');
                setTimeout(() => {
                    document.getElementById('loadingScreen').style.display = 'none';
                    document.getElementById('authScreen').classList.remove('hidden');
                }, 500);
            }

            initializeEventListeners();
        });

        function initializeEventListeners() {
            // Auth tabs
            document.getElementById('loginTab').addEventListener('click', () => switchAuthTab('login'));
            document.getElementById('signupTab').addEventListener('click', () => switchAuthTab('signup'));

            // Auth forms
            document.getElementById('loginForm').addEventListener('submit', handleLogin);
            document.getElementById('signupForm').addEventListener('submit', handleSignup);

            // Dashboard tabs
            document.getElementById('productsTab').addEventListener('click', () => switchDashboardTab('products'));
            document.getElementById('customersTab').addEventListener('click', () => switchDashboardTab('customers'));
            document.getElementById('addProductTab').addEventListener('click', () => switchDashboardTab('addProduct'));

            // Add product form
            document.getElementById('addProductForm').addEventListener('submit', handleAddProduct);
            document.getElementById('productImages').addEventListener('change', handleImageSelection);

            // Logout
            document.getElementById('logoutBtn').addEventListener('click', handleLogout);

            // Modal
            document.getElementById('closeModal').addEventListener('click', closeProductModal);
            document.getElementById('prevImageBtn').addEventListener('click', showPreviousImage);
            document.getElementById('nextImageBtn').addEventListener('click', showNextImage);

            // Page navigation
            document.getElementById('homePage').addEventListener('click', () => switchPage('home'));
            document.getElementById('collectionPage').addEventListener('click', () => switchPage('collection'));

            // Filters
            document.getElementById('categoryFilter').addEventListener('change', applyFilters);
            document.getElementById('purityFilter').addEventListener('change', applyFilters);
            document.getElementById('clearFilters').addEventListener('click', clearFilters);
        }

        function switchAuthTab(tab) {
            const loginTab = document.getElementById('loginTab');
            const signupTab = document.getElementById('signupTab');
            const loginForm = document.getElementById('loginForm');
            const signupForm = document.getElementById('signupForm');

            if (tab === 'login') {
                loginTab.classList.add('border-amber-500', 'text-amber-800');
                loginTab.classList.remove('border-transparent', 'text-amber-600');
                signupTab.classList.add('border-transparent', 'text-amber-600');
                signupTab.classList.remove('border-amber-500', 'text-amber-800');
                loginForm.classList.remove('hidden');
                signupForm.classList.add('hidden');
            } else {
                signupTab.classList.add('border-amber-500', 'text-amber-800');
                signupTab.classList.remove('border-transparent', 'text-amber-600');
                loginTab.classList.add('border-transparent', 'text-amber-600');
                loginTab.classList.remove('border-amber-500', 'text-amber-800');
                signupForm.classList.remove('hidden');
                loginForm.classList.add('hidden');
            }
        }

        function handleLogin(e) {
            e.preventDefault();
            const email = document.getElementById('loginEmail').value;
            const password = document.getElementById('loginPassword').value;

            // Check owner credentials
            if (email === 'siddhivinayakjewellers91@gmail.com' && password === 'Siddhi@123') {
                currentUser = { email, role: 'owner', name: 'Owner' };
                showMainApp();
                showOwnerDashboard();
            } else {
                // Check customer credentials (simulate database check)
                const customer = customers.find(c => c.email === email && c.password === password);
                if (customer) {
                    currentUser = { ...customer, role: 'customer' };
                    showMainApp();
                    showCustomerCollection();
                } else {
                    alert('Invalid credentials. Please check your email and password.');
                }
            }
        }

        async function handleSignup(e) {
            e.preventDefault();
            const name = document.getElementById('signupName').value;
            const email = document.getElementById('signupEmail').value;
            const phone = document.getElementById('signupPhone').value;
            const password = document.getElementById('signupPassword').value;

            // Check if customer already exists
            if (customers.find(c => c.email === email)) {
                alert('An account with this email already exists.');
                return;
            }

            try {
                // Show loading
                const submitBtn = e.target.querySelector('button[type="submit"]');
                const originalText = submitBtn.textContent;
                submitBtn.textContent = 'Creating Account...';
                submitBtn.disabled = true;

                // Add new customer
                const newCustomer = {
                    name,
                    email,
                    phone,
                    password
                };

                // Save to Firebase
                const customerId = await saveCustomerToFirebase(newCustomer);
                
                // Auto login
                currentUser = { ...newCustomer, id: customerId, role: 'customer' };
                showMainApp();
                showCustomerCollection();

                alert('Account created successfully!');
            } catch (error) {
                console.error('Error creating account:', error);
                alert('Error creating account. Please try again.');
                
                // Reset button
                const submitBtn = e.target.querySelector('button[type="submit"]');
                submitBtn.textContent = 'Create Account';
                submitBtn.disabled = false;
            }
        }

        function showMainApp() {
            document.getElementById('authScreen').classList.add('fade-out');
            setTimeout(() => {
                document.getElementById('authScreen').style.display = 'none';
                document.getElementById('mainApp').classList.remove('hidden');
                document.getElementById('userGreeting').textContent = `Welcome, ${currentUser.name}`;
            }, 500);
        }

        function showOwnerDashboard() {
            document.getElementById('ownerDashboard').classList.remove('hidden');
            document.getElementById('customerCollection').classList.add('hidden');
            renderOwnerProducts();
            renderCustomersList();
        }

        function showCustomerCollection() {
            document.getElementById('customerHome').classList.remove('hidden');
            document.getElementById('customerCollection').classList.add('hidden');
            document.getElementById('ownerDashboard').classList.add('hidden');
            document.getElementById('pageNavigation').classList.remove('hidden');
            currentPage = 'home';
            renderFeaturedProducts();
        }

        function switchPage(page) {
            const homeBtn = document.getElementById('homePage');
            const collectionBtn = document.getElementById('collectionPage');
            
            if (page === 'home') {
                document.getElementById('customerHome').classList.remove('hidden');
                document.getElementById('customerCollection').classList.add('hidden');
                homeBtn.classList.add('bg-amber-600', 'text-white');
                homeBtn.classList.remove('bg-gray-200', 'text-gray-700');
                collectionBtn.classList.add('bg-gray-200', 'text-gray-700');
                collectionBtn.classList.remove('bg-amber-600', 'text-white');
                currentPage = 'home';
                renderFeaturedProducts();
            } else {
                document.getElementById('customerCollection').classList.remove('hidden');
                document.getElementById('customerHome').classList.add('hidden');
                collectionBtn.classList.add('bg-amber-600', 'text-white');
                collectionBtn.classList.remove('bg-gray-200', 'text-gray-700');
                homeBtn.classList.add('bg-gray-200', 'text-gray-700');
                homeBtn.classList.remove('bg-amber-600', 'text-white');
                currentPage = 'collection';
                renderCustomerProducts();
            }
        }

        function filterByCategory(category) {
            currentCategory = category;
            switchPage('collection');
            document.getElementById('categoryFilter').value = category;
            applyFilters();
        }

        function switchDashboardTab(tab) {
            // Update tab buttons
            document.querySelectorAll('[id$="Tab"]').forEach(btn => {
                btn.classList.remove('bg-amber-500', 'text-white');
                btn.classList.add('bg-gray-200', 'text-gray-700');
            });
            document.getElementById(tab + 'Tab').classList.add('bg-amber-500', 'text-white');
            document.getElementById(tab + 'Tab').classList.remove('bg-gray-200', 'text-gray-700');

            // Show/hide panels
            document.getElementById('productsPanel').classList.add('hidden');
            document.getElementById('customersPanel').classList.add('hidden');
            document.getElementById('addProductPanel').classList.add('hidden');
            
            document.getElementById(tab + 'Panel').classList.remove('hidden');
            document.getElementById(tab + 'Panel').classList.add('fade-in');
        }

        function handleImageSelection(e) {
            const files = Array.from(e.target.files);
            currentProductImages = [];
            
            files.forEach(file => {
                const reader = new FileReader();
                reader.onload = function(e) {
                    currentProductImages.push(e.target.result);
                };
                reader.readAsDataURL(file);
            });
        }

        async function handleAddProduct(e) {
            e.preventDefault();
            
            try {
                // Show loading
                const submitBtn = e.target.querySelector('button[type="submit"]');
                const originalText = submitBtn.textContent;
                submitBtn.textContent = 'Adding Product...';
                submitBtn.disabled = true;

                const productName = document.getElementById('productName').value;
                const productCode = document.getElementById('productCode').value;
                
                // Upload images to Firebase Storage
                const imageFiles = document.getElementById('productImages').files;
                const imageUrls = [];
                
                if (imageFiles.length > 0) {
                    submitBtn.textContent = 'Uploading Images...';
                    
                    for (let i = 0; i < imageFiles.length; i++) {
                        const file = imageFiles[i];
                        const imagePath = `products/${productCode}_${Date.now()}_${i}`;
                        const imageUrl = await uploadImageToFirebase(file, imagePath);
                        imageUrls.push(imageUrl);
                    }
                }

                submitBtn.textContent = 'Saving Product...';

                const product = {
                    name: productName,
                    code: productCode,
                    weight: document.getElementById('productWeight').value,
                    category: document.getElementById('productCategory').value,
                    purity: document.getElementById('productPurity').value,
                    images: imageUrls
                };

                // Save to Firebase
                await saveProductToFirebase(product);
                
                // Reset form
                document.getElementById('addProductForm').reset();
                currentProductImages = [];
                
                // Switch to products tab and refresh
                switchDashboardTab('products');
                
                alert('Product added successfully!');
                
                // Reset button
                submitBtn.textContent = originalText;
                submitBtn.disabled = false;
                
            } catch (error) {
                console.error('Error adding product:', error);
                alert('Error adding product. Please try again.');
                
                // Reset button
                const submitBtn = e.target.querySelector('button[type="submit"]');
                submitBtn.textContent = 'Add Product to Collection';
                submitBtn.disabled = false;
            }
        }

        function renderOwnerProducts() {
            const container = document.getElementById('ownerProductsList');
            
            if (products.length === 0) {
                container.innerHTML = `
                    <div class="text-center py-12 text-gray-500 col-span-full">
                        <p>No products added yet. Click "Add Product" to get started.</p>
                    </div>
                `;
                return;
            }

            container.innerHTML = products.map(product => `
                <div class="product-card bg-white rounded-xl shadow-lg overflow-hidden">
                    <div class="zoom-container h-48">
                        <img src="${product.images && product.images[0] ? product.images[0] : 'data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMzAwIiBoZWlnaHQ9IjIwMCIgdmlld0JveD0iMCAwIDMwMCAyMDAiIGZpbGw9Im5vbmUiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+CjxyZWN0IHdpZHRoPSIzMDAiIGhlaWdodD0iMjAwIiBmaWxsPSIjRjNGNEY2Ii8+CjxwYXRoIGQ9Ik0xNTAgMTAwTDEyNSA3NUgxNzVMMTUwIDEwMFoiIGZpbGw9IiM5Q0EzQUYiLz4KPHBhdGggZD0iTTE1MCA5MEwxMzUgMTA1SDE2NUwxNTAgOTBaIiBmaWxsPSIjOUNBM0FGIi8+Cjx0ZXh0IHg9IjE1MCIgeT0iMTMwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmaWxsPSIjNjM3Mzg0IiBmb250LWZhbWlseT0iQXJpYWwiIGZvbnQtc2l6ZT0iMTQiPk5vIEltYWdlPC90ZXh0Pgo8L3N2Zz4='}" alt="${product.name}" class="zoom-image w-full h-full object-cover">
                    </div>
                    <div class="p-4">
                        <h4 class="font-semibold text-amber-900 mb-2">${product.name}</h4>
                        <div class="text-sm text-gray-600 space-y-1">
                            <p><span class="font-medium">Code:</span> ${product.code}</p>
                            <p><span class="font-medium">Category:</span> ${product.category}</p>
                            <p><span class="font-medium">Weight:</span> ${product.weight}g</p>
                            <p><span class="font-medium">Purity:</span> ${product.purity}</p>
                        </div>
                        <div class="mt-4 flex space-x-2">
                            <button onclick="viewProduct('${product.id}')" class="flex-1 bg-amber-500 hover:bg-amber-600 text-white py-2 px-4 rounded-lg text-sm font-medium transition-colors">
                                View
                            </button>
                            <button onclick="removeProduct('${product.id}')" class="bg-red-500 hover:bg-red-600 text-white py-2 px-4 rounded-lg text-sm font-medium transition-colors">
                                Remove
                            </button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        function renderFeaturedProducts() {
            const container = document.getElementById('featuredProducts');
            
            if (products.length === 0) {
                container.innerHTML = `
                    <div class="text-center py-12 text-gray-500 col-span-full">
                        <p>Our exclusive collection is being curated. Please check back soon!</p>
                    </div>
                `;
                return;
            }

            // Show first 6 products as featured
            const featuredProducts = products.slice(0, 6);
            container.innerHTML = featuredProducts.map(product => `
                <div class="product-card bg-white rounded-xl shadow-lg overflow-hidden cursor-pointer" onclick="viewProduct('${product.id}')">
                    <div class="zoom-container h-64">
                        <img src="${product.images && product.images[0] ? product.images[0] : 'data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMzAwIiBoZWlnaHQ9IjI1MCIgdmlld0JveD0iMCAwIDMwMCAyNTAiIGZpbGw9Im5vbmUiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+CjxyZWN0IHdpZHRoPSIzMDAiIGhlaWdodD0iMjUwIiBmaWxsPSIjRjNGNEY2Ii8+CjxwYXRoIGQ9Ik0xNTAgMTI1TDEyNSAxMDBIMTc1TDE1MCAxMjVaIiBmaWxsPSIjOUNBM0FGIi8+CjxwYXRoIGQ9Ik0xNTAgMTE1TDEzNSAxMzBIMTY1TDE1MCAxMTVaIiBmaWxsPSIjOUNBM0FGIi8+Cjx0ZXh0IHg9IjE1MCIgeT0iMTU1IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmaWxsPSIjNjM3Mzg0IiBmb250LWZhbWlseT0iQXJpYWwiIGZvbnQtc2l6ZT0iMTQiPk5vIEltYWdlPC90ZXh0Pgo8L3N2Zz4='}" alt="${product.name}" class="zoom-image w-full h-full object-contain bg-gray-100">
                    </div>
                    <div class="p-6">
                        <h3 class="text-xl font-playfair font-semibold text-amber-900 mb-2">${product.name}</h3>
                        <div class="text-sm text-gray-600 space-y-1 mb-4">
                            <p><span class="font-medium">Category:</span> ${product.category}</p>
                            <p><span class="font-medium">Weight:</span> ${product.weight}g</p>
                            <p><span class="font-medium">Purity:</span> ${product.purity}</p>
                        </div>
                        <button onclick="event.stopPropagation(); openWhatsApp('${product.name}')" class="w-full bg-green-500 hover:bg-green-600 text-white py-2 px-4 rounded-lg font-medium transition-colors flex items-center justify-center space-x-2">
                            <span>💬</span>
                            <span>Inquire on WhatsApp</span>
                        </button>
                    </div>
                </div>
            `).join('');
        }

        function renderCustomerProducts() {
            const container = document.getElementById('productsGrid');
            let filteredProducts = [...products];

            // Apply filters
            if (currentCategory) {
                filteredProducts = filteredProducts.filter(p => p.category === currentCategory);
            }
            if (currentPurity) {
                filteredProducts = filteredProducts.filter(p => p.purity === currentPurity);
            }

            // Update category title
            const categoryTitle = document.getElementById('categoryTitle');
            if (currentCategory && currentPurity) {
                categoryTitle.textContent = `${currentCategory} - ${currentPurity}`;
            } else if (currentCategory) {
                categoryTitle.textContent = currentCategory;
            } else if (currentPurity) {
                categoryTitle.textContent = `${currentPurity} Products`;
            } else {
                categoryTitle.textContent = 'All Products';
            }
            
            if (filteredProducts.length === 0) {
                container.innerHTML = `
                    <div class="text-center py-12 text-gray-500 col-span-full">
                        <p>No products found matching your filters.</p>
                    </div>
                `;
                return;
            }

            container.innerHTML = filteredProducts.map(product => `
                <div class="product-card bg-white rounded-xl shadow-lg overflow-hidden cursor-pointer" onclick="viewProduct('${product.id}')">
                    <div class="zoom-container h-64">
                        <img src="${product.images && product.images[0] ? product.images[0] : 'data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMzAwIiBoZWlnaHQ9IjI1MCIgdmlld0JveD0iMCAwIDMwMCAyNTAiIGZpbGw9Im5vbmUiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+CjxyZWN0IHdpZHRoPSIzMDAiIGhlaWdodD0iMjUwIiBmaWxsPSIjRjNGNEY2Ii8+CjxwYXRoIGQ9Ik0xNTAgMTI1TDEyNSAxMDBIMTc1TDE1MCAxMjVaIiBmaWxsPSIjOUNBM0FGIi8+CjxwYXRoIGQ9Ik0xNTAgMTE1TDEzNSAxMzBIMTY1TDE1MCAxMTVaIiBmaWxsPSIjOUNBM0FGIi8+Cjx0ZXh0IHg9IjE1MCIgeT0iMTU1IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmaWxsPSIjNjM3Mzg0IiBmb250LWZhbWlseT0iQXJpYWwiIGZvbnQtc2l6ZT0iMTQiPk5vIEltYWdlPC90ZXh0Pgo8L3N2Zz4='}" alt="${product.name}" class="zoom-image w-full h-full object-contain bg-gray-100">
                    </div>
                    <div class="p-6">
                        <h3 class="text-xl font-playfair font-semibold text-amber-900 mb-2">${product.name}</h3>
                        <div class="text-sm text-gray-600 space-y-1 mb-4">
                            <p><span class="font-medium">Category:</span> ${product.category}</p>
                            <p><span class="font-medium">Weight:</span> ${product.weight}g</p>
                            <p><span class="font-medium">Purity:</span> ${product.purity}</p>
                        </div>
                        <button onclick="event.stopPropagation(); openWhatsApp('${product.name}')" class="w-full bg-green-500 hover:bg-green-600 text-white py-2 px-4 rounded-lg font-medium transition-colors flex items-center justify-center space-x-2">
                            <span>💬</span>
                            <span>Inquire on WhatsApp</span>
                        </button>
                    </div>
                </div>
            `).join('');
        }

        function applyFilters() {
            currentCategory = document.getElementById('categoryFilter').value;
            currentPurity = document.getElementById('purityFilter').value;
            renderCustomerProducts();
        }

        function clearFilters() {
            currentCategory = '';
            currentPurity = '';
            document.getElementById('categoryFilter').value = '';
            document.getElementById('purityFilter').value = '';
            renderCustomerProducts();
        }

        function renderCustomersList() {
            const container = document.getElementById('customersList');
            
            if (customers.length === 0) {
                container.innerHTML = `
                    <div class="text-center py-12 text-gray-500">
                        <p>No customers registered yet.</p>
                    </div>
                `;
                return;
            }

            container.innerHTML = customers.map(customer => `
                <div class="bg-gray-50 rounded-lg p-4 flex justify-between items-center">
                    <div>
                        <h4 class="font-semibold text-amber-900">${customer.name}</h4>
                        <p class="text-sm text-gray-600">${customer.email}</p>
                        <p class="text-sm text-gray-600">${customer.phone}</p>
                    </div>
                    <div class="text-right">
                        <p class="text-xs text-gray-500">Registered</p>
                        <p class="text-xs text-gray-500">${customer.registeredAt ? new Date(customer.registeredAt).toLocaleDateString() : 'Recently'}</p>
                    </div>
                </div>
            `).join('');
        }

        function viewProduct(productId) {
            const product = products.find(p => p.id === productId);
            if (!product) return;

            currentProductForModal = product;
            currentImageIndex = 0;

            document.getElementById('modalProductName').textContent = product.name;
            updateModalImage();
            
            // Product details
            document.getElementById('modalProductDetails').innerHTML = `
                <div class="space-y-3">
                    <div class="flex justify-between">
                        <span class="font-medium text-amber-800">Product Code:</span>
                        <span class="text-gray-700">${product.code}</span>
                    </div>
                    <div class="flex justify-between">
                        <span class="font-medium text-amber-800">Category:</span>
                        <span class="text-gray-700">${product.category}</span>
                    </div>
                    <div class="flex justify-between">
                        <span class="font-medium text-amber-800">Weight:</span>
                        <span class="text-gray-700">${product.weight} grams</span>
                    </div>
                    <div class="flex justify-between">
                        <span class="font-medium text-amber-800">Purity:</span>
                        <span class="text-gray-700">${product.purity}</span>
                    </div>
                    <div class="flex justify-between">
                        <span class="font-medium text-amber-800">Added:</span>
                        <span class="text-gray-700">${product.addedAt ? new Date(product.addedAt).toLocaleDateString() : 'Recently'}</span>
                    </div>
                </div>
            `;

            // Image thumbnails
            if (product.images && product.images.length > 1) {
                document.getElementById('modalImageThumbs').innerHTML = product.images.map((img, index) => `
                    <img src="${img}" alt="Product ${index + 1}" class="w-16 h-16 object-cover rounded cursor-pointer border-2 ${index === currentImageIndex ? 'border-amber-500' : 'border-transparent'} hover:border-amber-500" onclick="changeMainImage(${index})">
                `).join('');
            } else {
                document.getElementById('modalImageThumbs').innerHTML = '';
            }

            // Show/hide navigation arrows
            const prevBtn = document.getElementById('prevImageBtn');
            const nextBtn = document.getElementById('nextImageBtn');
            if (product.images && product.images.length > 1) {
                prevBtn.style.display = 'block';
                nextBtn.style.display = 'block';
            } else {
                prevBtn.style.display = 'none';
                nextBtn.style.display = 'none';
            }

            // Set up inquiry button
            document.getElementById('inquiryBtn').onclick = () => openWhatsApp(product.name);

            document.getElementById('productModal').classList.remove('hidden');
        }

        function updateModalImage() {
            if (!currentProductForModal) return;
            
            const img = (currentProductForModal.images && currentProductForModal.images[currentImageIndex]) ? 
                currentProductForModal.images[currentImageIndex] : 
                'data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDAwIiBoZWlnaHQ9IjQwMCIgdmlld0JveD0iMCAwIDQwMCA0MDAiIGZpbGw9Im5vbmUiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+CjxyZWN0IHdpZHRoPSI0MDAiIGhlaWdodD0iNDAwIiBmaWxsPSIjRjNGNEY2Ii8+CjxwYXRoIGQ9Ik0yMDAgMjAwTDE3NSAxNzVIMjI1TDIwMCAyMDBaIiBmaWxsPSIjOUNBM0FGIi8+CjxwYXRoIGQ9Ik0yMDAgMTkwTDE4NSAyMDVIMjE1TDIwMCAxOTBaIiBmaWxsPSIjOUNBM0FGIi8+Cjx0ZXh0IHg9IjIwMCIgeT0iMjMwIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmaWxsPSIjNjM3Mzg0IiBmb250LWZhbWlseT0iQXJpYWwiIGZvbnQtc2l6ZT0iMTYiPk5vIEltYWdlPC90ZXh0Pgo8L3N2Zz4=';
            
            document.getElementById('modalMainImage').src = img;
            
            // Update thumbnail highlights
            const thumbs = document.querySelectorAll('#modalImageThumbs img');
            thumbs.forEach((thumb, index) => {
                if (index === currentImageIndex) {
                    thumb.classList.add('border-amber-500');
                    thumb.classList.remove('border-transparent');
                } else {
                    thumb.classList.add('border-transparent');
                    thumb.classList.remove('border-amber-500');
                }
            });
        }

        function showPreviousImage() {
            if (!currentProductForModal || !currentProductForModal.images || currentProductForModal.images.length <= 1) return;
            currentImageIndex = currentImageIndex > 0 ? currentImageIndex - 1 : currentProductForModal.images.length - 1;
            updateModalImage();
        }

        function showNextImage() {
            if (!currentProductForModal || !currentProductForModal.images || currentProductForModal.images.length <= 1) return;
            currentImageIndex = currentImageIndex < currentProductForModal.images.length - 1 ? currentImageIndex + 1 : 0;
            updateModalImage();
        }

        function changeMainImage(index) {
            currentImageIndex = index;
            updateModalImage();
        }

        function closeProductModal() {
            document.getElementById('productModal').classList.add('hidden');
        }

        async function removeProduct(productId) {
            if (confirm('Are you sure you want to remove this product?')) {
                try {
                    await removeProductFromFirebase(productId);
                    alert('Product removed successfully!');
                } catch (error) {
                    console.error('Error removing product:', error);
                    alert('Error removing product. Please try again.');
                }
            }
        }

        function openWhatsApp(productName) {
            const message = `Hi! I'm interested in the ${productName} from Siddhi Vinayak Jewellers. Could you please provide more details?`;
            const whatsappUrl = `https://wa.me/919975624544?text=${encodeURIComponent(message)}`;
            window.open(whatsappUrl, '_blank');
        }

        function handleLogout() {
            if (confirm('Are you sure you want to logout?')) {
                currentUser = null;
                document.getElementById('mainApp').classList.add('hidden');
                document.getElementById('ownerDashboard').classList.add('hidden');
                document.getElementById('customerCollection').classList.add('hidden');
                document.getElementById('authScreen').classList.remove('hidden');
                document.getElementById('authScreen').style.display = 'flex';
                
                // Reset forms
                document.getElementById('loginForm').reset();
                document.getElementById('signupForm').reset();
                switchAuthTab('login');
            }
        }
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'95cf8af463bb0e6e',t:'MTc1MjE0NTA3My4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
