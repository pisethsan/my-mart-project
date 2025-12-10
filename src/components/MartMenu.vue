<template>
  <!-- Main container for the entire menu app -->
  <div id="mart-menu">
    <header>
      <h1>E-Mart</h1>
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
          <p class="modal-price">${{ selectedItem.price.toFixed(2) }}/ {{ selectedItem.unit }}</p>
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
      { id: 101, name: 'Avocados', price: 3.75, unit: 'kg', imageUrl: 'https://i.postimg.cc/RVPZNXbh/avocados.jpg', description: '' },
      { id: 102, name: 'Passion', price: 3.00, unit: 'kg', imageUrl: 'https://i.postimg.cc/L4tTKZQq/passion.jpg', description: '' },
      { id: 103, name: 'Beetroot', price: 2.00, unit: 'kg', imageUrl: 'https://i.postimg.cc/D0p3yYf5/beetroot.jpg', description: '' },
      { id: 104, name: 'Yellow Watermelon', price: 0.75, unit: 'kg', imageUrl: 'https://i.postimg.cc/15yK95bb/Yellow_Watermelon.jpg', description: '' },
      { id: 105, name: 'Red Watermelon', price: 0.50, unit: 'kg', imageUrl: 'https://i.postimg.cc/qMpGkMWx/Red_Watermelon.jpg', description: '' },
      { id: 106, name: 'Dragon Fruit', price: 1.75, unit: 'kg', imageUrl: 'https://i.postimg.cc/L5p3gH53/dragon_fruit.jpg', description: '' },
      { id: 107, name: 'Papaya', price: 1.25, unit: 'kg', imageUrl: 'https://i.postimg.cc/br9HQKS7/papaya.jpg', description: '' },
      { id: 108, name: 'Seedless Guava', price: 2.00, unit: 'kg', imageUrl: 'https://i.postimg.cc/tC9zXC8L/Seedless_guava.jpg', description: '' },
      { id: 109, name: 'Guava Kimjo', price: 2.00, unit: 'kg', imageUrl: 'https://i.postimg.cc/prJYDg58/guavas_kimjo.jpg', description: '' },
    ]
  },
  {
    name: 'Vegetables',
    items: [
      { id: 201, name: 'Carrot', price: 2.00, unit: 'kg', imageUrl: 'https://i.postimg.cc/mkyNQ01Q/carrot.jpg', description: '' },
      { id: 202, name: 'Potatoes', price: 2.00, unit: 'kg', imageUrl: 'https://i.postimg.cc/QNQbGWYF/Potatoes.jpg', description: '' },
      { id: 203, name: 'Japanese Sweet Potatoes', price: 2.00, unit: 'kg', imageUrl: 'https://i.postimg.cc/wMQchSRP/Japanese_sweet_potatoes.jpg', description: '' },
      { id: 204, name: 'Red Onions', price: 2.00, unit: 'kg', imageUrl: 'https://i.postimg.cc/jqHX0J36/Red_onions.jpg', description: '' },
      { id: 205, name: 'Yellow Onions', price: 2.00, unit: 'kg', imageUrl: 'https://i.postimg.cc/ncn1HcNS/Yellow_onion.jpg', description: '' },
      { id: 206, name: 'cabbage', price: 2.00, unit: 'kg', imageUrl: 'https://i.postimg.cc/65xmCXgm/cabbage.jpg', description: '' },
      { id: 207, name: 'Napa_cabbage', price: 2.00, unit: 'kg', imageUrl: 'https://i.postimg.cc/mZYwfFpg/Napa_cabbage.jpg', description: '' },
    ]
  },
  {
    name: 'Fresh Meat',
    items: [
      { id: 301, name: 'Farm Chicken', price: 7.25, unit: 'kg', imageUrl: 'https://i.postimg.cc/nrGYv69Y/Farm_chicken.jpg', description: '' },
    ]
  },
  {
    name: 'Soap',
    items: [
      { id: 401, name: '1Clean Laundry Detergent Blue', price: 2.50, unit: 'bottle', imageUrl: 'https://i.postimg.cc/d3RVHgCc/1Clean_Laundry_Detergent_Blue.jpg', description: '' },
      { id: 402, name: '1Clean Laundry Detergent Pink', price: 2.50, unit: 'bottle', imageUrl: 'https://i.postimg.cc/598y2QR6/1Clean_Laundry_Detergent_Pink.jpg', description: '' },
      { id: 403, name: '1Clean Laundry Detergent Red', price: 2.50, unit: 'bottle', imageUrl: 'https://i.postimg.cc/G3vtmT5y/1Clean_Laundry_Detergent_Red.jpg', description: '' },
      { id: 404, name: '1Clean Fragrant Water Blue', price: 3.00, unit: 'bottle', imageUrl: 'https://i.postimg.cc/gjb0KGnv/1Clean_Fragrant_Water_Blue.jpg', description: '' },
      { id: 405, name: '1Clean Fragrant Water Purple', price: 8.00, unit: 'bottle', imageUrl: 'https://i.postimg.cc/L5k8CG1N/1Clean_Fragrant_Water_Purple.jpg', description: '' },
      { id: 406, name: '1Clean Dish Soap', price: 0.65, unit: 'bottle', imageUrl: 'https://i.postimg.cc/FzXKxh7J/1Clean_Dish_Soap.jpg', description: '' },
      { id: 407, name: '1Clean Dishwashing Liquid Green', price: 0.65, unit: 'bottle', imageUrl: 'https://i.postimg.cc/PJnqMdPW/1Clean_Dishwashing_Liquid_Green.jpg', description: '' },
      { id: 408, name: '1Clean Dishwashing Liquid White', price: 0.65, unit: 'bottle', imageUrl: 'https://i.postimg.cc/sxC2J3v7/1Clean_Dishwashing_Liquid_White.jpg', description: '' },
      { id: 409, name: 'Ora Laundry Detergent', price: 1.75, unit: 'bottle', imageUrl: 'https://i.postimg.cc/h4VvtQ5r/Ora_Laundry_Detergent.jpg', description: '' },
      { id: 410, name: 'Ora Fragrant Water', price: 2.50, unit: 'bottle', imageUrl: 'https://i.postimg.cc/L4j58gbj/Ora_Fragrant_Water.jpg', description: '' },
      { id: 411, name: 'Ora Dish Soap', price: 0.65, unit: 'bottle', imageUrl: 'https://i.postimg.cc/wxDMjRGk/Ora_Dish_Soap.jpg', description: '' },
      { id: 412, name: 'Ora Toilet Water', price: 1.5, unit: 'bottle', imageUrl: 'https://i.postimg.cc/CMb5xnQm/Ora_Toilet_Water.jpg', description: '' },
      { id: 413, name: 'Klen Shampoo', price: 3.50, unit: 'bottle', imageUrl: 'https://i.postimg.cc/J7j0zH2Z/Klen_Shampoo.jpg', description: '' },
    ]
  },
  {
    name: 'Herbs',
    items: [
      { id: 501, name: 'Black Sesame Cola oil', price: 7.25, unit: 'bottle', imageUrl: 'https://i.postimg.cc/mg9Vkw7t/Black_Sesame_Cola_oil.jpg', description: '' },
      { id: 502, name: 'Balsam Cola oil', price: 7.25, unit: 'bottle', imageUrl: 'https://i.postimg.cc/tRSHNXw2/Balsam-Cola-oil.jpg', description: '' },
      { id: 503, name: 'Ginger and Moringa oil', price: 7.25, unit: 'bottle', imageUrl: 'https://i.postimg.cc/cL3TCmf1/Ginger_and_Moringa_oil.jpg', description: '' },
      { id: 504, name: 'Ginseng oil', price: 7.25, unit: 'bottle', imageUrl: 'https://i.postimg.cc/152JyTVm/Ginseng_oil.jpg', description: '' },
    ]
  },
  {
    name: 'Spices',
    items: [
      { id: 501, name: 'កាពិ', price: 1.13, unit: 'ដប', imageUrl: 'https://i.postimg.cc/9fDV0YdK/SS1.jpg', description: '' },
      { id: 502, name: 'ណាំវ៉ា', price: 10.00, unit: 'ប្រអប់', imageUrl: 'https://i.postimg.cc/ncFyM1Zw/SS2.jpg', description: '3in1, 4in1 ,5in1 =1ប្រអប់40000' },
      { id: 503, name: 'ម្សៅបំពងស្រួយ', price: 0.63, unit: 'កញ្ជប់', imageUrl: 'https://i.postimg.cc/m2LvhSRq/SS3.jpg', description: 'ម្សៅបំពងស្រួយ 1kg.7000 ម្សៅបំពងស្រួយ 150g.2500' },
      { id: 504, name: 'ស្ករស', price: 1.50, unit: 'គីឡូក្រាម', imageUrl: 'https://i.postimg.cc/fyT8WBXV/SS4.jpg', description: '' },
      { id: 505, name: 'អំបិលម៉ត់', price: 1.25, unit: 'គីឡូក្រាម', imageUrl: 'https://i.postimg.cc/D0yxfBsL/SS5.jpg', description: '' },
      { id: 506, name: 'ប៊ីចេង', price: 1.50, unit: 'កញ្ជប់', imageUrl: 'https://i.postimg.cc/d3y941b2/SS6.jpg', description: '' },
      { id: 507, name: 'ម្សៅស៊ុបបន្លែ', price: 5.50, unit: 'កញ្ជប់', imageUrl: 'https://i.postimg.cc/Wzk7X3Br/SS7.jpg', description: '' },
      { id: 508, name: 'ម្សៅស៊ុបឆ្នះ', price: 1.00, unit: 'កញ្ជប់', imageUrl: 'https://i.postimg.cc/nrD10zyD/SS8.jpg', description: 'ម្សៅស៊ុបឆ្នះ 200g.4000 ម្សៅស៊ុបឆ្នះ 400g.8000' },
      { id: 509, name: 'ម្សៅស៊ុប គ្រឿងហ្លួង', price: 2.00, unit: 'កញ្ជប់', imageUrl: 'https://i.postimg.cc/hvd0pjk0/SS9.jpg', description: '' },
      { id: 510, name: 'ម្សៅស៊ុប រ៉ាទី', price: 2.00, unit: 'កញ្ជប់', imageUrl: 'https://i.postimg.cc/3NvZnNMM/SS10.jpg', description: '' }
    ]
  },
  {
    name: 'Sauce',
    items: [
      { id: 601, name: 'ប្រេងឆា រំដួល', price: 2.50, unit: 'ដប', imageUrl: 'https://i.postimg.cc/Nj81KR42/S20.jpg', description: '' },
      { id: 602, name: 'ទឹកត្រីកញ្ចាញ់ច្រាស', price: 0.75, unit: 'ដប', imageUrl: 'https://i.postimg.cc/59syYhxY/S1.jpg', description: '' },
      { id: 603, name: 'ទឹកត្រីដបថ្ម', price: 3.75, unit: 'ដប', imageUrl: 'https://i.postimg.cc/PfSJpgtL/S2.jpg', description: '' },
      { id: 604, name: 'ទឹកត្រីបណ្ដូលបាយ ', price: 0.88, unit: 'ដប', imageUrl: 'https://i.postimg.cc/sfTxQkV1/S3.jpg', description: '' },
      { id: 605, name: 'ទឹកត្រីបណ្ដូលបាយ ២ឆ្នាំ', price: 3.00, unit: 'ដប', imageUrl: 'https://i.postimg.cc/Kc0jk6Zj/S4.jpg', description: '' },
    ]
  },
  {
    name: 'Sauces 2',
    items: [
      
      { id: 606, name: 'ទឹកស៊ីអ៊ីវ 365', price: 0.88, unit: 'ដប', imageUrl: 'https://i.postimg.cc/NfLM1dM6/S6.jpg', description: '' },
      { id: 607, name: 'ទឹកស៊ីអ៊ីវចំការដូង', price: 0.75, unit: 'ដប', imageUrl: 'https://i.postimg.cc/G3Xt8Wcj/S7.jpg', description: '' },
      { id: 608, name: 'ទឹកស៊ីអ៊ីវលាង ឡេង', price: 0.75, unit: 'ដប', imageUrl: 'https://i.postimg.cc/MKXTy9Tt/S8.jpg', description: '' },
      { id: 609, name: 'ទឹកម្ទេស', price: 0.50, unit: 'ដប', imageUrl: 'https://i.postimg.cc/zX3BnxB9/S9.jpg', description: '' },
      { id: 610, name: 'ទឹកម្ទេសចំការដូង', price: 0.50, unit: 'ដប', imageUrl: 'https://i.postimg.cc/kXD4xT4z/S10.jpg', description: '' },
      { id: 611, name: 'ទឹកម្ទេសផ្អែម', price: 1.50, unit: 'ដប', imageUrl: 'https://i.postimg.cc/MKXTy9H8/S11.jpg', description: '' },
      { id: 612, name: 'ទឹកប៉េងប៉ោះ', price: 1.50, unit: 'ដប', imageUrl: 'https://i.postimg.cc/J4tnbp08/S12.jpg', description: '' },
      { id: 613, name: 'ទឹកសៀងចំការដូង', price: 0.63, unit: 'ដប', imageUrl: 'https://i.postimg.cc/bNMvL1SX/S13.jpg', description: '' },
      { id: 614, name: 'ទឹកសៀង', price: 0.63, unit: 'ដប', imageUrl: 'https://i.postimg.cc/65SQzC46/S14.jpg', description: '' },
    ]
  },
  {
    name: 'Sauces 3',
    items: [
      
      { id: 615, name: 'ទឹកប្រេងខ្យង', price: 2.50, unit: 'ដប', imageUrl: 'https://i.postimg.cc/DwQqS1gV/S15.jpg', description: '' },
      { id: 616, name: 'ប្រេងខ្យង ដបថ្ម', price: 1.50, unit: 'ដប', imageUrl: 'https://i.postimg.cc/bw0QZ1TP/S16.jpg', description: '' },
      { id: 617, name: 'ប្រេងខ្យងកាន 5L', price: 4.75, unit: 'ដប', imageUrl: 'https://i.postimg.cc/Hk0Qrb2d/S17.jpg', description: '' },
      { id: 618, name: 'ប្រេងខ្យងកាន 1L', price: 1.13, unit: 'ដប', imageUrl: 'https://i.postimg.cc/3xjgk2BQ/S18.jpg', description: '' },
      { id: 619, name: 'ប្រេងខ្យងចំការដូង', price: 1.13, unit: 'ដប', imageUrl: 'https://i.postimg.cc/525wX83j/S19.jpg', description: '' },
    ]
  },
  {
    name: 'Grains',
    items: [
      { id: 701, name: 'ស្វាយចន្ទី 280g', price: 4.50, unit: 'កញ្ជប់', imageUrl: 'https://i.postimg.cc/rF0GZFbF/Cashew_Nuts_280g.jpg', description: '' },
      { id: 702, name: 'ស្វាយចន្ទី 500g', price: 8.00, unit: 'កញ្ជប់', imageUrl: 'https://i.postimg.cc/ZKBPsKgY/Cashew_Nuts_500g.jpg', description: '' },
      { id: 703, name: 'ស្វាយចន្ទីចំហៀង 500g', price: 5.50, unit: 'កញ្ជប់', imageUrl: 'https://i.postimg.cc/FsdbCsq9/Cashew_Nuts_Slide_500g.jpg', description: '' },
      { id: 704, name: 'ស្វាយចន្ទី​ 500g', price: 7.50, unit: 'កញ្ជប់', imageUrl: 'https://i.postimg.cc/ZKG8GztJ/Cashew_Nuts_500g.jpg', description: '' },
    ]
  },
  {
    name: 'Candy',
    items: [
      { id: 601, name: 'ដំណាប់ចេក', price: 1.50, unit: 'កញ្ជប់', imageUrl: 'https://i.postimg.cc/7YG1tYcv/Banana_jam.jpg', description: '' },
      { id: 602, name: 'ស្ករគ្រាប់ដូង', price: 2.00, unit: 'កញ្ជប់', imageUrl: 'https://i.postimg.cc/dtb2bcPk/Coconut_candy.jpg', description: '' },
      { id: 603, name: 'ស្ករគ្រាប់ដូងប្រអប់', price: 1.25, unit: 'ប្រអប់', imageUrl: 'https://i.postimg.cc/Qx2Q2ZGV/Coconut_candy_box.jpg', description: '' },
    ]
  },
  {
    name: 'Wine',
    items: [
      { id: 301, name: 'ស្រាវីស្គីចេក', price: 11.25, unit: 'ដប', imageUrl: 'https://i.postimg.cc/Kj5vB79X/W1.jpg', description: '' },
      { id: 302, name: 'ស្រាចេកតាអុង', price: 11.25, unit: 'ដប', imageUrl: 'https://i.postimg.cc/HntsX0Sp/W2.jpg', description: '' },
      { id: 303, name: 'ស្រាចេកហនុមាន', price: 11.25, unit: 'ដប', imageUrl: 'https://i.postimg.cc/SRGNCLZp/W3.jpg', description: '' },
      { id: 304, name: 'ស្រាចដកក្រហម', price: 8.75, unit: 'ដប', imageUrl: 'https://i.postimg.cc/Fzxs3bP4/W4.jpg', description: '' },
      { id: 305, name: 'ស្រាចេកបៃតង', price: 8.75, unit: 'ដប', imageUrl: 'https://i.postimg.cc/mkS2C3dP/W5.jpg', description: '' },
    ]
  },
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
  aspect-ratio: 1 / 1; /* Keep thumbnails perfectly square */
  height: auto;
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
  height: auto;
  max-height: 100vh; /* Keep it within the viewport */
  object-fit: contain; /* Show full image without cropping */
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