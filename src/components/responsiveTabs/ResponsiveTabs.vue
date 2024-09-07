<template>
  <div class="area-editor-responsive-config">
    <div class="area-editor-responsive-options">
      <!-- Default tab (always present and non-removable) -->
      <div class="tab-item" :class="{ active: activeTab === 'default' }" @click="setActiveTab('default')">
        Default ({{ mode }})
      </div>
      <!-- Dynamic tabs -->
      <div
        v-for="(tab, index) in tabs"
        :key="index"
        class="tab-item"
        :class="{ active: activeTab === index }"
        @mouseover="hoveredTab = index"
        @mouseleave="hoveredTab = null"
        @click="setActiveTab(index)"
      >
        {{ tab.name }}
        <!-- Remove Button: Only show on hover -->
        <button
          v-if="hoveredTab === index"
          aria-label="Remove selection"
          class="btn-remove"
          title="Remove Selection"
          @click.stop="removeTab(index)"
        >
          <IconRemove />
        </button>
      </div>
    </div>
    <div class="area-editor-responsive-actions">
      <div @click="addTab">Add device</div>
      <!-- Mobile/ Desktop Toggle -->
      <div @click="toggleMode">{{ mode }}</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

// State to track the mode (Mobile First / Desktop First)
const mode = ref('Mobile First')

// Create a reactive array to store additional tabs (excluding default)
const tabs = ref([])
const hoveredTab = ref<number | null>(null) // Track which tab is hovered
const activeTab = ref<string | number>('default') // Track the active tab

// Function to set the active tab
const setActiveTab = (tab: string | number) => {
  activeTab.value = tab
}

// Function to add a new tab (active tab stays the same unless user clicks new tab)
const addTab = () => {
  const newIndex = tabs.value.length + 1
  tabs.value.push({ name: `Tab ${newIndex}` })
}

// Function to remove a tab (cannot remove the default tab)
const removeTab = (index: number) => {
  tabs.value.splice(index, 1)

  // If the active tab is the one being removed, set the default tab as active
  if (activeTab.value === index) {
    setActiveTab('default')
  }
}

// Function to toggle the mode between Mobile First and Desktop First
const toggleMode = () => {
  mode.value = mode.value === 'Mobile First' ? 'Desktop First' : 'Mobile First'
}
</script>

<style scoped>
.area-editor-responsive-config {
  display: flex;
}

.area-editor-responsive-options {
  flex: 1;
  display: flex;
  overflow-x: auto;
}

.tab-item {
  flex-shrink: 0;
  position: relative;
  padding: 1rem 0.5rem;
  margin-right: 0.5rem;
  background: green;
  border-top-left-radius: 0.5rem;
  border-top-right-radius: 0.5rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
  cursor: pointer;
}

/* Active tab style */
.tab-item.active {
  background: blue;
  color: white;
}

.btn-remove {
  display: none;
  position: absolute;
  top: 0;
  right: 0;
  border: none;
  cursor: pointer;
}

.tab-item:hover .btn-remove {
  display: block;
  width: 30px;
  background: var(--color-remove);
  svg {
    stroke-width: 15px;
  }
}

.area-editor-responsive-actions {
  display: flex;
}

.area-editor-responsive-actions > div {
  background: orange;
  padding: 1rem 0.5rem;
  margin-left: 0.5rem;
  border-top-left-radius: 0.5rem;
  border-top-right-radius: 0.5rem;
  cursor: pointer;
}
</style>
