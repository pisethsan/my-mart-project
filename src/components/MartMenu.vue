<template>
  <!-- Main container for the entire menu app -->
  <div id="mart-menu">
    <header>
      <h1>My Mart</h1>
      <p>Your one-stop shop for fresh goods</p>
    </header>

    <!-- 
      This is the main loop. 
      It creates a new <section> for each category in our 'categories' data.
    -->
    <section 
      v-for="category in categories" 
      :key="category.name" 
      class="category-section"
    >
      <!-- Display the category name (e.g., "Fruits", "Vegetables") -->
      <h2>{{ category.name }}</h2>

      <!-- 
        We replaced the <ul> with <TransitionGroup>
        - 'tag="ul"' tells Vue to render it as a <ul> element.
        - 'name="list"' connects it to our new CSS animations.
        - 'class="item-list"' is the same class we had before.
      -->
      <TransitionGroup 
        tag="ul" 
        name="list" 
        class="item-list"
      >
        <li 
          v-for="item in category.items" 
          :key="item.id" 
          class="menu-item"
          @click="showItemDetails(item)"
        >
          <!-- NEW: Image for the item -->
          <img :src="item.imageUrl" :alt="item.name" class="item-image" @error="handleImageError">

          <!-- NEW: Wrapper for the item details -->
          <div class="item-details">
            <span class="item-name">{{ item.name }}</span>
            <div class="price-container">
              <span class="item-price">${{ item.price.toFixed(2) }}</span>
              <span class="item-unit">/ {{ item.unit }}</span>
            </div>
          </div>
        </li>
      </TransitionGroup>
    </section>

    <!-- NEW: Item Detail Modal -->
    <Transition name="modal-fade">
      <div v-if="selectedItem" class="modal-backdrop" @click="closeModal">
        <div class="modal-content" @click.stop>
          <button @click="closeModal" class="close-button">&times;</button>
          <img :src="selectedItem.imageUrl" :alt="selectedItem.name" class="modal-image" @error="handleImageError">
          <h2>{{ selectedItem.name }}</h2>
          <p class="modal-price">${{ selectedItem.price.toFixed(2) }}</p>
          <p class="modal-description">{{ selectedItem.description }}</p>
          <button class="add-to-cart-button">Add to Cart</button>
        </div>
      </div>
    </Transition>

  </div>
</template>

<script setup>
// ... existing code ... */
import { ref } from 'vue';
const selectedItem = ref(null);

const PLACEHOLDER_IMAGE_URL = 'https://placehold.co/280x150/EAECEE/7F8C8D?text=No+Image';

const showItemDetails = (item) => {
  selectedItem.value = item;
};

const closeModal = () => {
  selectedItem.value = null;
};

const handleImageError = (event) => {
  event.target.src = PLACEHOLDER_IMAGE_URL;
};


// This is your "database" for all the products.
// UPDATED: Added an 'imageUrl' to each item.
const categories = ref([
  {
    name: 'Fruits',
    items: [
      { id: 101, name: 'Apple', price: 1.20, unit: 'kg', imageUrl: 'https://www.shutterstock.com/image-photo/red-apple-cut-half-water-600nw-2532255795.jpg', description: 'Crisp, sweet, and perfect for a healthy snack. Sourced from local orchards.' },
      { id: 102, name: 'Banana', price: 0.75, unit: 'kg', imageUrl: 'https://thumbs.dreamstime.com/b/bunch-bananas-6175887.jpg', description: 'A rich source of potassium, great for energy. Sold individually.' },
      { id: 103, name: 'Orange', price: 0.90, unit: 'kg', imageUrl: 'https://previews.123rf.com/images/phayoke/phayoke1606/phayoke160600036/59616610-fresh-orange-fruit-and-orage-half-fruit-on-white-background.jpg', description: 'Packed with Vitamin C, this juicy orange is a perfect immunity booster.' },
      { id: 104, name: 'Mango', price: 2.50, unit: 'kg', imageUrl: 'https://placehold.co/280x150/FDF8E1/F39C12?text=Mango', description: 'Sweet, tropical, and ready to eat. A taste of sunshine.' },
      { id: 105, name: 'Strawberries', price: 3.50, unit: 'kg', imageUrl: 'https://placehold.co/280x150/FEEEEE/E74C3C?text=Strawberries', description: 'Freshly picked, ripe strawberries. Perfect for desserts or on their own.' },
      { id: 106, name: 'Blueberries', price: 4.00, unit: 'kg', imageUrl: 'https://placehold.co/280x150/EBF5FB/3498DB?text=Blueberries', description: 'Sweet and tiny antioxidants. Great for smoothies or cereal.' },
      { id: 107, name: 'Grapes', price: 5.00, unit: 'kg', imageUrl: 'https://placehold.co/280x150/F5EEF8/8E44AD?text=Grapes', description: 'Seedless green grapes, sold by the kilogram. Crisp and delicious.' },
    ]
  },
  {
    name: 'Vegetables',
    items: [
      { id: 201, name: 'Carrot', price: 1.80, unit: 'kg', imageUrl: 'https://placehold.co/280x150/FAE5D3/E67E22?text=Carrot', description: 'Fresh, crunchy carrots. Ideal for roasting, salads, or juicing.' },
      { id: 202, name: 'Broccoli', price: 2.20, unit: 'head', imageUrl: 'https://placehold.co/280x150/E9F7EF/2ECC71?text=Broccoli', description: 'A vibrant green head of broccoli, packed with vitamins. Sold per head.' },
      { id: 203, name: 'Tomato', price: 3.00, unit: 'kg', imageUrl: 'https://placehold.co/280x150/FADBD8/E74C3C?text=Tomato', description: 'Ripe vine tomatoes, perfect for sauces or fresh salads. Sold by the kilogram.' },
      { id: 204, name: 'Spinach', price: 2.75, unit: 'bag', imageUrl: 'https://placehold.co/280x150/D4EFDF/27AE60?text=Spinach', description: 'A large bag of fresh baby spinach. Tender and ready to eat.' },
      { id: 205, name: 'Cucumber', price: 1.00, unit: 'each', imageUrl: 'https://placehold.co/280x150/E8F8F5/1ABC9C?text=Cucumber', description: 'A long English cucumber. Crisp and refreshing for salads or water.' },
      { id: 206, name: 'Bell Pepper (Red)', price: 1.50, unit: 'each', imageUrl: 'https://placehold.co/280x150/FADBD8/C0392B?text=Pepper', description: 'Sweet, crunchy red bell pepper. Perfect for stir-fries or snacking.' },
      { id: 207, name: 'Onion', price: 1.30, unit: 'kg', imageUrl: 'https://placehold.co/280x150/F5EFEF/D7BDE2?text=Onion', description: 'A 1kg bag of versatile yellow onions, the base for any great meal.' },
    ]
  },
  {
    name: 'Wine',
    items: [
      { id: 301, name: 'Red Wine', price: 15.00, unit: 'bottle', imageUrl: 'https://placehold.co/280x150/EAECEE/7B241C?text=Red+Wine', description: 'A full-bodied Cabernet Sauvignon with notes of dark cherry and oak.' },
      { id: 302, name: 'White Wine', price: 14.50, unit: 'bottle', imageUrl: 'https://placehold.co/280x150/FDFEFE/FEF5E7?text=White+Wine', description: 'A crisp, refreshing Sauvignon Blanc with hints of citrus and green apple.' },
      { id: 303, name: 'Rosé Wine', price: 16.00, unit: 'bottle', imageUrl: 'https://placehold.co/280x150/FEF2F2/F5B7B1?text=Rosé+Wine', description: 'A dry, light-bodied Rosé from Provence. Perfect for summer.' },
      { id: 304, name: 'Sparkling Wine', price: 22.00, unit: 'bottle', imageUrl: 'https://placehold.co/280x150/F4FAFE/D6EAF8?text=Sparkling', description: 'A celebratory bottle of Brut sparkling wine. Fine bubbles and a toasty finish.' },
    ]
  },
  {
    name: 'Soap',
    items: [
      { id: 401, name: 'Hand Soap', price: 3.50, unit: 'bottle', imageUrl: 'https://placehold.co/280x150/EBF5FB/3498DB?text=Hand+Soap', description: 'Gentle foaming hand soap with a light lavender scent. 250ml bottle.' },
      { id: 402, name: 'Bar Soap', price: 5.00, unit: '3-pack', imageUrl: 'https://placehold.co/280x150/E8F8F5/1ABC9C?text=Bar+Soap', description: 'A 3-pack of moisturizing shea butter bar soap. Unscented.' },
      { id: 403, name: 'Dish Soap', price: 4.20, unit: 'bottle', imageUrl: 'https://placehold.co/280x150/E8F6F3/16A085?text=Dish+Soap', description: 'Tough on grease, plant-based dish soap with a lemon verbena scent. 500ml.' },
      { id: 404, name: 'Body Wash', price: 6.00, unit: 'bottle', imageUrl: 'https://placehold.co/280x150/F4ECF7/A569BD?text=Body+Wash', description: 'Hydrating body wash with argan oil and a floral scent. 400ml.' },
    ]
  },
  {
    name: 'Spices',
    items: [
      { id: 501, name: 'Cumin', price: 4.00, unit: 'jar', imageUrl: 'https://placehold.co/280x150/FDF2E9/AF601A?text=Cumin', description: 'Ground cumin, 100g. Earthy and warm, essential for many cuisines.' },
      { id: 502, name: 'Paprika', price: 3.75, unit: 'jar', imageUrl: 'https://placehold.co/280x150/FADBD8/C0392B?text=Paprika', description: 'Smoked sweet paprika, 100g. Adds rich color and flavor.' },
      { id: 503, name: 'Turmeric', price: 3.50, unit: 'jar', imageUrl: 'https://placehold.co/280x150/FEF9E7/F39C12?text=Turmeric', description: 'Ground turmeric, 100g. Known for its vibrant color and health benefits.' },
      { id: 504, name: 'Black Pepper', price: 3.00, unit: 'jar', imageUrl: 'https://placehold.co/280x150/EAECEE/839192?text=Pepper', description: 'Whole black peppercorns, 80g. Ready for your grinder.' },
      { id: 505, name: 'Garlic Powder', price: 3.25, unit: 'jar', imageUrl: 'https://placehold.co/280x150/FEF9E7/F7DC6F?text=Garlic', description: 'Convenient garlic powder, 90g. For when you\'re out of fresh garlic.' },
      { id: 506, name: 'Oregano', price: 2.75, unit: 'jar', imageUrl: 'https://placehold.co/280x150/EBF5EE/229954?text=Oregano', description: 'Dried oregano, 50g. The taste of the Mediterranean.' },
    ]
  },
  {
    name: 'Rice',
    items: [
      { id: 601, name: 'Jasmine Rice', price: 12.00, unit: '5kg bag', imageUrl: 'https://placehold.co/280x150/F8F9F9/AAB7B8?text=Jasmine+Rice', description: 'Fragrant jasmine rice from Thailand. A large 5kg bag.' },
      { id: 602, name: 'Brown Rice', price: 4.50, unit: '1kg bag', imageUrl: 'https://placehold.co/280x150/F5EBE0/D5C0A9?text=Brown+Rice', description: 'A healthy, fiber-rich whole grain rice. 1kg bag.' },
      { id: 603, name: 'Basmati Rice', price: 5.50, unit: '1kg bag', imageUrl: 'https://placehold.co/280x150/F8F9F9/AAB7B8?text=Basmati+Rice', description: 'Aromatic long-grain rice, perfect for curries. 1kg bag.' },
      { id: 604, name: 'Arborio Rice', price: 6.00, unit: '500g box', imageUrl: 'https://placehold.co/280x150/F8F9F9/AAB7B8?text=Arborio+Rice', description: 'Creamy Italian rice, ideal for risotto. 500g box.' },
    ]
  },
  {
    name: 'Snacks',
    items: [
      { id: 701, name: 'Potato Chips', price: 2.80, unit: 'bag', imageUrl: 'https://placehold.co/280x150/FEF9E7/F1C40F?text=Chips', description: 'Classic salted potato chips. A large bag for sharing (or not).' },
      { id: 702, name: 'Chocolate Bar', price: 1.90, unit: 'bar', imageUrl: 'https://placehold.co/280x150/EAECEE/7B241C?text=Chocolate', description: 'Rich dark chocolate bar, 70% cocoa. 100g bar.' },
      { id: 703, name: 'Pretzels', price: 3.20, unit: 'bag', imageUrl: 'https://placehold.co/280x150/FDF2E9/E59866?text=Pretzels', description: 'A bag of crunchy, salty pretzels. A classic snack.' },
      { id: 704, name: 'Mixed Nuts', price: 7.00, unit: '250g', imageUrl: 'https://placehold.co/280x150/F6EEE0/BAA482?text=Nuts', description: 'A roasted and salted mix of almonds, cashews, and walnuts. 250g.' },
      { id: 705, name: 'Cookies', price: 4.00, unit: 'pack', imageUrl: 'https://placehold.co/280x150/FDF2E9/E59866?text=Cookies', description: 'Chocolate chip cookies, just like homemade. 12 cookies per pack.' },
      { id: 706, name: 'Popcorn', price: 2.50, unit: 'bag', imageUrl: 'https://placehold.co/280x150/FEF9E7/F7DC6F?text=Popcorn', description: 'Ready-to-eat sweet and salty popcorn. Perfect for movie night.' },
    ]
  }
])
</script>

<style scoped>
/* 'scoped' means these styles will only apply to this component,
  so they won't accidentally affect other parts of your website.
*/

#mart-menu {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  padding: 20px;
  color: #333;
}

header {
  text-align: center;
  margin-bottom: 40px;
  border-bottom: 2px solid #f0f0f0;
  padding-bottom: 20px;
}
/* ... existing styles for header h1 and p ... */
header h1 {
  color: #2c3e50;
  margin: 0;
  font-size: 2.5rem;
}

header p {
  font-size: 1.2rem;
  color: #7f8c8d;
  margin: 5px 0 0;
}

.category-section {
  margin-bottom: 35px;
}

h2 {
  font-size: 2rem;
  color: #42b983; /* Vue's signature green color */
  border-bottom: 2px solid #42b983;
  padding-bottom: 10px;
  margin-bottom: 20px;
}

/* --- NEW: Style for the horizontal list --- */
.item-list {
  list-style-type: none;
  padding: 0;
  margin: 0;
  display: flex; /* Makes the list horizontal */
  overflow-x: auto; /* Enables horizontal scrolling */
  padding-bottom: 20px; /* Adds space for scrollbar (even if hidden) */
  scroll-snap-type: x mandatory; /* Makes the scroll "snap" to items */

  /* --- Hides the scrollbar for a cleaner look --- */
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;  /* Firefox */
}

.item-list::-webkit-scrollbar {
  display: none; /* Chrome, Safari and Opera */
}


/* --- UPDATED: Style for the items in the horizontal list --- */
.menu-item {
  /* display: flex; */ /* This is already set by the container */
  /* justify-content: space-between; */ /* REMOVED */
  /* align-items: center; */ /* REMOVED */
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  transition: all 0.2s ease-in-out;

  /* --- New properties for horizontal layout --- */
  flex: 0 0 auto; /* Prevents items from shrinking */
  width: 220px;  /* Sets a fixed width for each card */
  margin-right: 12px; /* Adds space between the cards */
  scroll-snap-align: start; /* Ensures the card snaps to the start */

  /* --- NEW: Changed for image layout --- */
  padding: 0; /* REMOVED old padding */
  display: flex; /* Keep flex, but change direction */
  flex-direction: column; /* Stack image and text vertically */
  overflow: hidden; /* This makes the image respect the border radius */
  cursor: pointer; /* NEW: Add pointer to show it's clickable */
}

.menu-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

/* --- NEW: Style for the item image --- */
.item-image {
  width: 100%;
  height: 120px;
  object-fit: cover; /* Makes the image cover the area without distortion */
}

/* --- NEW: Style for the item details container --- */
.item-details {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 15px; /* Add padding here instead */
  width: 100%;
}

/* NEW: Container for price and unit */
.price-container {
  white-space: nowrap; /* Prevent price and unit from wrapping */
}

/* --- UPDATED: Style for the item name --- */
.item-name {
  font-size: 1rem;
  font-weight: 500;
  color: #34495e;
  flex: 1; /* Allow name to take up available space */
  white-space: nowrap; /* Prevent wrapping */
  overflow: hidden; /* Hide overflow */
  text-overflow: ellipsis; /* Add ... for long names */
  margin-right: 10px; /* Space between name and price */
}

/* --- UPDATED: Style for the item price --- */
.item-price {
  font-size: 1rem;
  font-weight: 600;
  color: #2c3e50;
}

/* NEW: Style for the item unit */
.item-unit {
  font-size: 0.8rem;
  color: #7f8c8d;
}

/*
 * --- NEW: CSS for the "list" TransitionGroup ---
 * This creates the fade-in and slide animation
*/

/* 1. Define the transition for move, enter, and leave */
.list-move,
.list-enter-active,
.list-leave-active {
  transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
}

/* 2. Define the starting state for an item *entering* */
.list-enter-from {
  opacity: 0;
  transform: translateX(30px); /* Start 30px to the right */
}

/* 3. Define the end state for an item *entering* (not needed, default is fine) */
/* .list-enter-to { opacity: 1; transform: translateX(0); } */

/* 4. Define the end state for an item *leaving* */
.list-leave-to {
  opacity: 0;
  transform: translateX(-30px); /* Leave by sliding to the left */
}

/* 5. Ensure leaving items don't mess up the layout */
.list-leave-active {
  position: absolute;
}

/* --- NEW: All Modal Styles --- */

.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  padding: 20px;
  box-sizing: border-box;
}

.modal-content {
  background: #fff;
  border-radius: 12px;
  padding: 25px;
  max-width: 500px;
  width: 100%;
  position: relative;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
  max-height: 90vh;
  overflow-y: auto;
}

.close-button {
  position: absolute;
  top: 15px;
  right: 15px;
  background: #f1f1f1;
  border: none;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  font-size: 1.2rem;
  font-weight: bold;
  color: #777;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  line-height: 1;
  transition: all 0.2s ease;
}

.close-button:hover {
  background: #e0e0e0;
  color: #333;
}

.modal-image {
  width: 100%;
  height: 250px;
  object-fit: cover;
  border-radius: 8px;
  margin-bottom: 20px;
}

.modal-content h2 {
  font-size: 1.8rem;
  color: #2c3e50;
  margin: 0 0 10px 0;
  border-bottom: none; /* Override category h2 style */
  padding-bottom: 0;
}

.modal-price {
  font-size: 1.5rem;
  font-weight: 600;
  color: #42b983;
  margin-bottom: 15px;
}

.modal-description {
  font-size: 1rem;
  color: #555;
  line-height: 1.6;
  margin-bottom: 25px;
}

.add-to-cart-button {
  background-color: #42b983;
  color: white;
  border: none;
  padding: 12px 20px;
  font-size: 1rem;
  font-weight: 600;
  border-radius: 8px;
  width: 100%;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.add-to-cart-button:hover {
  background-color: #36a473;
}

/* --- NEW: Modal Animation --- */
.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: opacity 0.3s ease;
}

.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
}

.modal-fade-enter-active .modal-content,
.modal-fade-leave-active .modal-content {
  transition: transform 0.3s ease;
}

.modal-fade-enter-from .modal-content,
.modal-fade-leave-to .modal-content {
  transform: scale(0.95) translateY(10px);
}
</style>