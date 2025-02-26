<template>
  <div class="container py-4">
    <h1 class="text-center mb-4">Warehouse Inventory</h1>
    
    <!-- Form untuk menambah/edit produk -->
    <div class="card p-3 mb-3">
      <form @submit.prevent="isEditing ? updateProduct() : addProduct()" class="row g-3">
        <div class="col-md-6">
          <input v-model="currentProduct.name" class="form-control" placeholder="Product Name" required />
        </div>
        <div class="col-md-6">
          <input v-model="currentProduct.sku" class="form-control" placeholder="SKU" required />
        </div>
        <div class="col-md-4">
          <input v-model="currentProduct.quantity" type="number" class="form-control" placeholder="Quantity" required />
        </div>
        <div class="col-md-4">
          <input v-model="currentProduct.location" class="form-control" placeholder="Location" required />
        </div>
        <div class="col-md-4">
          <input v-model="currentProduct.status" class="form-control" placeholder="Status" required />
        </div>
        <div class="col-md-12 d-flex gap-2">
          <button type="submit" class="btn btn-primary flex-grow-1">
            {{ isEditing ? 'Update Product' : 'Add Product' }}
          </button>
          <button v-if="isEditing" @click.prevent="cancelEdit" class="btn btn-secondary">
            Cancel
          </button>
        </div>
      </form>
    </div>
    
    <!-- Tabel produk -->
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Name</th>
          <th>SKU</th>
          <th>Quantity</th>
          <th>Location</th>
          <th>Status</th>
          <th class="text-center">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in products" :key="product.id">
          <td>{{ product.name }}</td>
          <td>{{ product.sku }}</td>
          <td>{{ product.quantity }}</td>
          <td>{{ product.location }}</td>
          <td>{{ product.status }}</td>
          <td class="text-center">
            <div class="btn-group">
              <button @click="editProduct(product)" class="btn btn-sm btn-warning me-1">
                <i class="bi bi-pencil"></i> Edit
              </button>
              <button @click="deleteProduct(product.id)" class="btn btn-sm btn-danger">
                <i class="bi bi-trash"></i> Delete
              </button>
            </div>
          </td>
        </tr>
        <tr v-if="products.length === 0">
          <td colspan="6" class="text-center">No products found</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

const products = ref([]);
const isEditing = ref(false);
const currentProduct = ref({
  id: null,
  name: "",
  sku: "",
  quantity: 0,
  location: "",
  status: "",
});

const fetchProducts = async () => {
  try {
    const response = await axios.get("http://localhost:8080/products");
    products.value = response.data;
  } catch (error) {
    console.error("Error fetching products:", error);
    alert("Failed to load products");
  }
};

const resetForm = () => {
  currentProduct.value = {
    id: null,
    name: "",
    sku: "",
    quantity: 0,
    location: "",
    status: "",
  };
  isEditing.value = false;
};

const addProduct = async () => {
  try {
    await axios.post("http://localhost:8080/products", currentProduct.value);
    resetForm();
    fetchProducts();
  } catch (error) {
    console.error("Error adding product:", error);
    alert("Failed to add product");
  }
};

const editProduct = (product) => {
  currentProduct.value = { ...product };
  isEditing.value = true;
  // Scroll to form
  window.scrollTo({ top: 0, behavior: 'smooth' });
};

const updateProduct = async () => {
  try {
    await axios.put(`http://localhost:8080/products/${currentProduct.value.id}`, currentProduct.value);
    resetForm();
    fetchProducts();
  } catch (error) {
    console.error("Error updating product:", error);
    alert("Failed to update product");
  }
};

const cancelEdit = () => {
  resetForm();
};

const deleteProduct = async (id) => {
  try {
    if (confirm("Are you sure you want to delete this product?")) {
      await axios.delete(`http://localhost:8080/products/${id}`);
      
      products.value = products.value.filter(product => product.id !== id);
      
      fetchProducts();
    }
  } catch (error) {
    console.error("Error deleting product:", error);
    alert("Failed to delete product");
  }
};


onMounted(fetchProducts);
</script>