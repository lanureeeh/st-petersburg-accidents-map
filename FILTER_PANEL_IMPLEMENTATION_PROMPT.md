# Complete Filter Panel Implementation Guide for map2.html

## Overview
Implement a comprehensive filter panel system for `map2.html` that allows users to filter traffic accident data by multiple criteria. The panel should match the functionality of the Russian interface but be implemented in English.

---

## 1. FILTER PANEL STRUCTURE

### 1.1 Required Filters

The filter panel should include the following sections (in order):

1. **Data Period** - Date range picker
2. **Participants** - Multi-select participant categories
3. **Health Harm** - Severity filter (checkboxes)
4. **Filters** - Collapsible section containing:
   - Accident Types (Category filter)
   - Traffic Violations
   - Nearby Objects
   - Road Conditions
   - Streets
   - Weather
5. **Hide/Show Toggle** - Button to collapse/expand the panel

---

## 2. HTML STRUCTURE

### 2.1 Filter Panel Container

Add this HTML structure before the closing `</body>` tag in `map2.html`:

```html
<!-- Filter Panel -->
<div class="filter-panel" id="filterPanel">
    <div class="filter-panel-content">
        <!-- Data Period Section -->
        <div class="filter-item">
            <p class="subtitle2">Data Period</p>
            <div class="date-filter-container">
                <input 
                    type="text" 
                    id="dateRangeInput" 
                    class="date-input" 
                    placeholder="01.06.2021 - 30.06.2025"
                    readonly
                />
                <button class="date-dropdown-btn" id="dateDropdownBtn">
                    <span>▼</span>
                </button>
                <div class="date-menu" id="dateMenu" style="display: none;">
                    <div class="date-value" data-range="last-month">Last Month</div>
                    <div class="date-value" data-range="two-months-ago">Two Months Ago</div>
                    <div class="date-value" data-range="2025">2025</div>
                    <div class="date-value" data-range="2024">2024</div>
                    <div class="date-value" data-range="2023">2023</div>
                    <div class="date-value" data-range="2022">2022</div>
                    <div class="date-value" data-range="2021">2021</div>
                    <div class="date-value" data-range="all-time">All Time</div>
                </div>
            </div>
            <div class="date-error" id="dateError" style="display: none;"></div>
        </div>

        <!-- Participants Section -->
        <div class="filter-item">
            <p class="subtitle2">Accident Participants</p>
            <div class="participant-filter">
                <button class="participant-item" data-participant="all" data-selected="true">
                    <span class="participant-icon">👥</span>
                    <p class="subtitle3">All Participants</p>
                </button>
                <button class="participant-item" data-participant="pedestrian" data-selected="false">
                    <span class="participant-icon">🚶</span>
                    <p class="subtitle3">Pedestrians</p>
                </button>
                <button class="participant-item" data-participant="cyclist" data-selected="false">
                    <span class="participant-icon">🚴</span>
                    <p class="subtitle3">Cyclists</p>
                </button>
                <button class="participant-item" data-participant="motorcyclist" data-selected="false">
                    <span class="participant-icon">🏍️</span>
                    <p class="subtitle3">Motorcyclists</p>
                </button>
                <button class="participant-item" data-participant="public_transport" data-selected="false">
                    <span class="participant-icon">🚌</span>
                    <p class="subtitle3">Public Transport</p>
                </button>
                <button class="participant-item" data-participant="children" data-selected="false">
                    <span class="participant-icon">👶</span>
                    <p class="subtitle3">Children</p>
                </button>
            </div>
        </div>

        <!-- Health Harm Section -->
        <div class="filter-item">
            <p class="subtitle2">Health Harm</p>
            <div class="severity-filter">
                <label class="severity-item">
                    <input type="checkbox" data-severity="Easy" checked>
                    <span class="checkmark">✓</span>
                    <span class="severity-color" style="background: #FFB81F;"></span>
                    <span class="body1">Easy</span>
                </label>
                <label class="severity-item">
                    <input type="checkbox" data-severity="Heavy" checked>
                    <span class="checkmark">✓</span>
                    <span class="severity-color" style="background: #FF7F24;"></span>
                    <span class="body1">Heavy</span>
                </label>
                <label class="severity-item">
                    <input type="checkbox" data-severity="With the dead" checked>
                    <span class="checkmark">✓</span>
                    <span class="severity-color" style="background: #FF001A;"></span>
                    <span class="body1">With the dead</span>
                </label>
            </div>
        </div>

        <!-- Filters Section (Collapsible) -->
        <div class="filter-item">
            <p class="subtitle2">Filters</p>
            <div class="category-filter-container">
                <!-- Accident Types -->
                <button class="category-tag" data-filter-type="category" id="categoryFilterBtn">
                    <span>Accident Types</span>
                    <span class="filter-count" id="categoryCount">0</span>
                </button>
                
                <!-- Traffic Violations -->
                <button class="category-tag" data-filter-type="violations" id="violationsFilterBtn">
                    <span>Traffic Violations</span>
                    <span class="filter-count" id="violationsCount">0</span>
                </button>
                
                <!-- Nearby Objects -->
                <button class="category-tag" data-filter-type="nearby" id="nearbyFilterBtn">
                    <span>Nearby Objects</span>
                    <span class="filter-count" id="nearbyCount">0</span>
                </button>
                
                <!-- Road Conditions -->
                <button class="category-tag" data-filter-type="road_conditions" id="roadConditionsFilterBtn">
                    <span>Road Conditions</span>
                    <span class="filter-count" id="roadConditionsCount">0</span>
                </button>
                
                <!-- Streets -->
                <button class="category-tag" data-filter-type="street" id="streetFilterBtn">
                    <span>Streets</span>
                    <span class="filter-count" id="streetCount">0</span>
                </button>
                
                <!-- Weather -->
                <button class="category-tag" data-filter-type="weather" id="weatherFilterBtn">
                    <span>Weather</span>
                    <span class="filter-count" id="weatherCount">0</span>
                </button>
            </div>
        </div>
    </div>

    <!-- Hide/Show Toggle Button -->
    <button class="btn-hideFilter" id="hideFilterBtn">
        <span>▼</span>
        <span>Hide</span>
    </button>
</div>

<!-- Category Filter Modal/Panel -->
<div class="category-filter-panel" id="categoryFilterPanel" style="display: none;">
    <div class="category-filter-header">
        <button class="back-btn" id="backBtn">← Back</button>
        <h3 id="categoryFilterTitle">Filter</h3>
        <input 
            type="text" 
            class="search-input" 
            id="categorySearchInput" 
            placeholder="Search..."
        />
    </div>
    <div class="category-filter-content" id="categoryFilterContent">
        <!-- Dynamically populated with filter options -->
    </div>
</div>
```

---

## 3. CSS STYLING

Add these styles to the `<style>` section in `map2.html`:

```css
/* Filter Panel Styles */
.filter-panel {
    position: absolute;
    top: 20px;
    left: 20px;
    width: 400px;
    max-width: calc(100% - 40px);
    background: white;
    box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    z-index: 1000;
    pointer-events: all;
    max-height: calc(100vh - 40px);
    display: flex;
    flex-direction: column;
    overflow: hidden;
}

.filter-panel-content {
    padding: 20px;
    overflow-y: auto;
    flex: 1;
}

.filter-panel.hidden {
    width: 300px;
}

.filter-panel.hidden .filter-panel-content {
    display: none;
}

.filter-item {
    margin-bottom: 20px;
}

.filter-item:last-child {
    margin-bottom: 0;
}

.subtitle2 {
    font-size: 14px;
    font-weight: 500;
    color: #18334a;
    margin-bottom: 12px;
    line-height: 20px;
}

/* Date Filter Styles */
.date-filter-container {
    display: flex;
    gap: 8px;
    align-items: center;
}

.date-input {
    flex: 1;
    padding: 8px 12px;
    border: 1px solid #d4dadd;
    border-radius: 4px;
    font-size: 14px;
    color: #18334a;
    background: white;
    cursor: pointer;
}

.date-input:focus {
    outline: none;
    border-color: #18334a;
}

.date-dropdown-btn {
    padding: 8px 12px;
    border: 1px solid #d4dadd;
    border-radius: 4px;
    background: white;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
}

.date-dropdown-btn:hover {
    background: #f5f5f5;
}

.date-menu {
    position: absolute;
    background: white;
    border: 1px solid #d4dadd;
    border-radius: 4px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    margin-top: 4px;
    max-height: 300px;
    overflow-y: auto;
    z-index: 1001;
}

.date-value {
    padding: 10px 16px;
    cursor: pointer;
    font-size: 14px;
    color: #18334a;
    border-bottom: 1px solid #f0f0f0;
}

.date-value:last-child {
    border-bottom: none;
}

.date-value:hover {
    background: #f5f5f5;
}

.date-error {
    margin-top: 8px;
    color: #FF001A;
    font-size: 12px;
}

/* Participant Filter Styles */
.participant-filter {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
    grid-auto-rows: 65px;
}

.participant-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 8px 4px;
    background: #f5f5f5;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background 0.3s ease;
    font-size: 10px;
}

.participant-item:hover {
    background: #e8e8e8;
}

.participant-item.active {
    background: #18334a;
    color: white;
}

.participant-item.active .subtitle3 {
    color: white;
}

.participant-icon {
    font-size: 24px;
    margin-bottom: 4px;
}

.participant-item .subtitle3 {
    font-size: 10px;
    font-weight: 500;
    color: #18334a;
    text-align: center;
}

/* Severity Filter Styles */
.severity-filter {
    display: flex;
    flex-direction: column;
    gap: 8px;
}

.severity-item {
    display: flex;
    align-items: center;
    gap: 12px;
    cursor: pointer;
    padding: 8px;
    border-radius: 4px;
    transition: background 0.2s;
}

.severity-item:hover {
    background: #f5f5f5;
}

.severity-item input[type="checkbox"] {
    display: none;
}

.checkmark {
    width: 20px;
    height: 20px;
    border: 2px solid #d4dadd;
    border-radius: 4px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    font-size: 12px;
    flex-shrink: 0;
}

.severity-item input[type="checkbox"]:checked + .checkmark {
    background: #18334a;
    border-color: #18334a;
}

.severity-color {
    width: 16px;
    height: 16px;
    border-radius: 50%;
    flex-shrink: 0;
}

.severity-item .body1 {
    font-size: 14px;
    color: #18334a;
    flex: 1;
}

/* Category Filter Tags */
.category-filter-container {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
}

.category-tag {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 6px 12px;
    background: #f5f5f5;
    border: 1px solid #d4dadd;
    border-radius: 4px;
    cursor: pointer;
    font-size: 14px;
    color: #18334a;
    transition: all 0.2s;
}

.category-tag:hover {
    background: #e8e8e8;
    border-color: #18334a;
}

.category-tag.active {
    background: #18334a;
    color: white;
    border-color: #18334a;
}

.filter-count {
    background: rgba(255, 255, 255, 0.3);
    padding: 2px 6px;
    border-radius: 10px;
    font-size: 12px;
    font-weight: 600;
}

.category-tag.active .filter-count {
    background: rgba(255, 255, 255, 0.2);
}

/* Hide/Show Button */
.btn-hideFilter {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    width: 100%;
    padding: 12px;
    background: #f5f5f5;
    border: none;
    border-top: 1px solid #d4dadd;
    cursor: pointer;
    font-size: 14px;
    color: #18334a;
    transition: background 0.2s;
}

.btn-hideFilter:hover {
    background: #e8e8e8;
}

/* Category Filter Panel (Modal) */
.category-filter-panel {
    position: absolute;
    top: 20px;
    left: 20px;
    width: 400px;
    max-width: calc(100% - 40px);
    max-height: calc(100vh - 40px);
    background: white;
    box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    z-index: 1001;
    display: flex;
    flex-direction: column;
    overflow: hidden;
}

.category-filter-header {
    padding: 16px 20px;
    border-bottom: 1px solid #d4dadd;
    display: flex;
    align-items: center;
    gap: 12px;
}

.back-btn {
    background: none;
    border: none;
    font-size: 18px;
    cursor: pointer;
    color: #18334a;
    padding: 4px 8px;
}

.back-btn:hover {
    background: #f5f5f5;
    border-radius: 4px;
}

.category-filter-header h3 {
    flex: 1;
    font-size: 16px;
    font-weight: 600;
    color: #18334a;
    margin: 0;
}

.search-input {
    flex: 1;
    padding: 8px 12px;
    border: 1px solid #d4dadd;
    border-radius: 4px;
    font-size: 14px;
}

.category-filter-content {
    flex: 1;
    overflow-y: auto;
    padding: 20px;
}

.category-value {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 10px;
    cursor: pointer;
    border-radius: 4px;
    transition: background 0.2s;
}

.category-value:hover {
    background: #f5f5f5;
}

.category-value input[type="checkbox"] {
    width: 18px;
    height: 18px;
    cursor: pointer;
}

.category-value label {
    flex: 1;
    cursor: pointer;
    font-size: 14px;
    color: #18334a;
}
```

---

## 4. JAVASCRIPT IMPLEMENTATION

### 4.1 Filter State Management

Add this to your JavaScript section in `map2.html`:

```javascript
// ============================================
// FILTER STATE MANAGEMENT
// ============================================
const filterState = {
    dateRange: {
        start: null,
        end: null
    },
    participants: ['all'], // Default: all participants
    severity: ['Easy', 'Heavy', 'With the dead'], // All selected by default
    category: [],
    violations: [],
    nearby: [],
    roadConditions: [],
    streets: [],
    weather: []
};

// Extract unique values from GeoJSON for filter options
let filterOptions = {
    categories: new Set(),
    violations: new Set(),
    nearby: new Set(),
    roadConditions: new Set(),
    streets: new Set(),
    weather: new Set()
};

// Initialize filter options from data
function initializeFilterOptions() {
    allAccidents.forEach(feature => {
        const props = feature.properties;
        
        // Categories
        if (props.category) {
            filterOptions.categories.add(props.category);
        }
        
        // Nearby objects (array)
        if (props.nearby && Array.isArray(props.nearby)) {
            props.nearby.forEach(item => filterOptions.nearby.add(item));
        }
        
        // Road conditions (array)
        if (props.road_conditions && Array.isArray(props.road_conditions)) {
            props.road_conditions.forEach(item => filterOptions.roadConditions.add(item));
        }
        
        // Weather (array)
        if (props.weather && Array.isArray(props.weather)) {
            props.weather.forEach(item => filterOptions.weather.add(item));
        }
        
        // Streets (from address)
        if (props.address) {
            // Extract street name (simplified - you may need more sophisticated parsing)
            const streetMatch = props.address.match(/([A-Za-z\s]+(?:street|st|avenue|ave|road|rd|boulevard|blvd|lane|ln))/i);
            if (streetMatch) {
                filterOptions.streets.add(streetMatch[1].trim());
            }
        }
        
        // Violations (extract from participants/vehicles)
        if (props.participants && Array.isArray(props.participants)) {
            props.participants.forEach(participant => {
                if (typeof participant === 'string') {
                    try {
                        const parsed = JSON.parse(participant);
                        if (parsed.violations && Array.isArray(parsed.violations)) {
                            parsed.violations.forEach(v => filterOptions.violations.add(v));
                        }
                    } catch (e) {
                        // Not JSON, skip
                    }
                }
            });
        }
    });
    
    // Convert Sets to sorted Arrays
    filterOptions.categories = Array.from(filterOptions.categories).sort();
    filterOptions.violations = Array.from(filterOptions.violations).sort();
    filterOptions.nearby = Array.from(filterOptions.nearby).sort();
    filterOptions.roadConditions = Array.from(filterOptions.roadConditions).sort();
    filterOptions.streets = Array.from(filterOptions.streets).sort();
    filterOptions.weather = Array.from(filterOptions.weather).sort();
}

// Set default date range (all available data)
function setDefaultDateRange() {
    if (allAccidents.length === 0) return;
    
    let minDate = new Date(allAccidents[0].properties.datetime);
    let maxDate = new Date(allAccidents[0].properties.datetime);
    
    allAccidents.forEach(feature => {
        const date = new Date(feature.properties.datetime);
        if (date < minDate) minDate = date;
        if (date > maxDate) maxDate = date;
    });
    
    filterState.dateRange.start = minDate.toISOString().split('T')[0];
    filterState.dateRange.end = maxDate.toISOString().split('T')[0];
    
    updateDateRangeDisplay();
}
```

### 4.2 Date Filter Implementation

```javascript
// Date Filter Functions
function formatDateRange(start, end) {
    const startDate = new Date(start);
    const endDate = new Date(end);
    const format = (date) => {
        const day = String(date.getDate()).padStart(2, '0');
        const month = String(date.getMonth() + 1).padStart(2, '0');
        const year = date.getFullYear();
        return `${day}.${month}.${year}`;
    };
    return `${format(startDate)} - ${format(endDate)}`;
}

function updateDateRangeDisplay() {
    const input = document.getElementById('dateRangeInput');
    if (filterState.dateRange.start && filterState.dateRange.end) {
        input.value = formatDateRange(filterState.dateRange.start, filterState.dateRange.end);
    }
}

function setDateRange(range) {
    const now = new Date();
    const currentYear = now.getFullYear();
    
    switch(range) {
        case 'last-month':
            const lastMonth = new Date(now.getFullYear(), now.getMonth() - 1, 1);
            const lastMonthEnd = new Date(now.getFullYear(), now.getMonth(), 0);
            filterState.dateRange.start = lastMonth.toISOString().split('T')[0];
            filterState.dateRange.end = lastMonthEnd.toISOString().split('T')[0];
            break;
        case 'two-months-ago':
            const twoMonthsAgo = new Date(now.getFullYear(), now.getMonth() - 2, 1);
            const twoMonthsAgoEnd = new Date(now.getFullYear(), now.getMonth() - 1, 0);
            filterState.dateRange.start = twoMonthsAgo.toISOString().split('T')[0];
            filterState.dateRange.end = twoMonthsAgoEnd.toISOString().split('T')[0];
            break;
        case '2025':
        case '2024':
        case '2023':
        case '2022':
        case '2021':
            const year = parseInt(range);
            filterState.dateRange.start = `${year}-01-01`;
            filterState.dateRange.end = `${year}-12-31`;
            break;
        case 'all-time':
            // Set to full range of data
            setDefaultDateRange();
            return;
    }
    
    updateDateRangeDisplay();
    applyFilters();
}

// Date filter event listeners
document.addEventListener('DOMContentLoaded', function() {
    const dateDropdownBtn = document.getElementById('dateDropdownBtn');
    const dateMenu = document.getElementById('dateMenu');
    
    dateDropdownBtn.addEventListener('click', function(e) {
        e.stopPropagation();
        dateMenu.style.display = dateMenu.style.display === 'none' ? 'block' : 'none';
    });
    
    // Close menu when clicking outside
    document.addEventListener('click', function(e) {
        if (!dateDropdownBtn.contains(e.target) && !dateMenu.contains(e.target)) {
            dateMenu.style.display = 'none';
        }
    });
    
    // Date menu item clicks
    const dateValues = dateMenu.querySelectorAll('.date-value');
    dateValues.forEach(item => {
        item.addEventListener('click', function() {
            const range = this.getAttribute('data-range');
            setDateRange(range);
            dateMenu.style.display = 'none';
        });
    });
});
```

### 4.3 Participant Filter Implementation

```javascript
// Participant Filter Functions
function initializeParticipantFilter() {
    const participantItems = document.querySelectorAll('.participant-item');
    
    participantItems.forEach(item => {
        item.addEventListener('click', function() {
            const participant = this.getAttribute('data-participant');
            
            if (participant === 'all') {
                // If "All" is clicked, deselect all others and select "All"
                participantItems.forEach(i => {
                    i.setAttribute('data-selected', 'false');
                    i.classList.remove('active');
                });
                this.setAttribute('data-selected', 'true');
                this.classList.add('active');
                filterState.participants = ['all'];
            } else {
                // Toggle individual participant
                const isSelected = this.getAttribute('data-selected') === 'true';
                
                if (isSelected) {
                    this.setAttribute('data-selected', 'false');
                    this.classList.remove('active');
                    filterState.participants = filterState.participants.filter(p => p !== participant);
                } else {
                    this.setAttribute('data-selected', 'true');
                    this.classList.add('active');
                    filterState.participants.push(participant);
                }
                
                // If any specific participant is selected, deselect "All"
                if (filterState.participants.length > 0 && filterState.participants[0] !== 'all') {
                    const allBtn = document.querySelector('[data-participant="all"]');
                    allBtn.setAttribute('data-selected', 'false');
                    allBtn.classList.remove('active');
                    filterState.participants = filterState.participants.filter(p => p !== 'all');
                }
                
                // If no specific participants selected, select "All"
                if (filterState.participants.length === 0) {
                    const allBtn = document.querySelector('[data-participant="all"]');
                    allBtn.setAttribute('data-selected', 'true');
                    allBtn.classList.add('active');
                    filterState.participants = ['all'];
                }
            }
            
            applyFilters();
        });
    });
    
    // Set default: "All" selected
    const allBtn = document.querySelector('[data-participant="all"]');
    if (allBtn) {
        allBtn.classList.add('active');
    }
}
```

### 4.4 Severity Filter Implementation

```javascript
// Severity Filter Functions
function initializeSeverityFilter() {
    const severityCheckboxes = document.querySelectorAll('[data-severity]');
    
    severityCheckboxes.forEach(checkbox => {
        checkbox.addEventListener('change', function() {
            const severity = this.getAttribute('data-severity');
            const isChecked = this.checked;
            
            if (isChecked) {
                if (!filterState.severity.includes(severity)) {
                    filterState.severity.push(severity);
                }
            } else {
                filterState.severity = filterState.severity.filter(s => s !== severity);
            }
            
            applyFilters();
        });
    });
}
```

### 4.5 Category Filter Modal Implementation

```javascript
// Category Filter Modal Functions
let currentFilterType = null;

function openCategoryFilter(type) {
    currentFilterType = type;
    const panel = document.getElementById('categoryFilterPanel');
    const title = document.getElementById('categoryFilterTitle');
    const content = document.getElementById('categoryFilterContent');
    const searchInput = document.getElementById('categorySearchInput');
    
    // Set title based on type
    const titles = {
        'category': 'Accident Types',
        'violations': 'Traffic Violations',
        'nearby': 'Nearby Objects',
        'road_conditions': 'Road Conditions',
        'street': 'Streets',
        'weather': 'Weather'
    };
    title.textContent = titles[type] || 'Filter';
    
    // Populate content
    populateCategoryFilterContent(type, content);
    
    // Show panel
    panel.style.display = 'flex';
    
    // Focus search input
    searchInput.value = '';
    searchInput.focus();
    
    // Setup search
    searchInput.oninput = function() {
        filterCategoryOptions(this.value, content, type);
    };
}

function populateCategoryFilterContent(type, container) {
    container.innerHTML = '';
    
    let options = [];
    switch(type) {
        case 'category':
            options = filterOptions.categories;
            break;
        case 'violations':
            options = filterOptions.violations;
            break;
        case 'nearby':
            options = filterOptions.nearby;
            break;
        case 'road_conditions':
            options = filterOptions.roadConditions;
            break;
        case 'street':
            options = filterOptions.streets;
            break;
        case 'weather':
            options = filterOptions.weather;
            break;
    }
    
    const selectedValues = filterState[type] || [];
    
    options.forEach(option => {
        const isSelected = selectedValues.includes(option);
        const item = document.createElement('div');
        item.className = 'category-value';
        item.innerHTML = `
            <input type="checkbox" id="${type}-${option}" ${isSelected ? 'checked' : ''}>
            <label for="${type}-${option}">${option}</label>
        `;
        
        const checkbox = item.querySelector('input');
        checkbox.addEventListener('change', function() {
            updateCategoryFilter(type, option, this.checked);
        });
        
        container.appendChild(item);
    });
}

function filterCategoryOptions(searchTerm, container, type) {
    const items = container.querySelectorAll('.category-value');
    const term = searchTerm.toLowerCase();
    
    items.forEach(item => {
        const label = item.querySelector('label').textContent.toLowerCase();
        item.style.display = label.includes(term) ? 'flex' : 'none';
    });
}

function updateCategoryFilter(type, value, isSelected) {
    if (!filterState[type]) {
        filterState[type] = [];
    }
    
    if (isSelected) {
        if (!filterState[type].includes(value)) {
            filterState[type].push(value);
        }
    } else {
        filterState[type] = filterState[type].filter(v => v !== value);
    }
    
    updateFilterCounts();
    applyFilters();
}

function updateFilterCounts() {
    document.getElementById('categoryCount').textContent = filterState.category.length;
    document.getElementById('violationsCount').textContent = filterState.violations.length;
    document.getElementById('nearbyCount').textContent = filterState.nearby.length;
    document.getElementById('roadConditionsCount').textContent = filterState.roadConditions.length;
    document.getElementById('streetCount').textContent = filterState.streets.length;
    document.getElementById('weatherCount').textContent = filterState.weather.length;
    
    // Update active state of buttons
    ['category', 'violations', 'nearby', 'road_conditions', 'street', 'weather'].forEach(type => {
        const btn = document.getElementById(`${type}FilterBtn`);
        if (btn) {
            if (filterState[type] && filterState[type].length > 0) {
                btn.classList.add('active');
            } else {
                btn.classList.remove('active');
            }
        }
    });
}

// Category filter button event listeners
document.addEventListener('DOMContentLoaded', function() {
    const categoryButtons = document.querySelectorAll('.category-tag');
    categoryButtons.forEach(btn => {
        btn.addEventListener('click', function() {
            const type = this.getAttribute('data-filter-type');
            openCategoryFilter(type);
        });
    });
    
    // Back button
    document.getElementById('backBtn').addEventListener('click', function() {
        document.getElementById('categoryFilterPanel').style.display = 'none';
    });
});
```

### 4.6 Hide/Show Toggle

```javascript
// Hide/Show Filter Panel
document.addEventListener('DOMContentLoaded', function() {
    const hideBtn = document.getElementById('hideFilterBtn');
    const filterPanel = document.getElementById('filterPanel');
    
    hideBtn.addEventListener('click', function() {
        const isHidden = filterPanel.classList.contains('hidden');
        
        if (isHidden) {
            filterPanel.classList.remove('hidden');
            this.querySelector('span:first-child').textContent = '▼';
            this.querySelector('span:last-child').textContent = 'Hide';
        } else {
            filterPanel.classList.add('hidden');
            this.querySelector('span:first-child').textContent = '▲';
            this.querySelector('span:last-child').textContent = 'Show';
        }
    });
});
```

### 4.7 Enhanced Filter Application Function

Replace your existing `applyFilters()` function with this enhanced version:

```javascript
function applyFilters() {
    filteredAccidents = allAccidents.filter(feature => {
        const props = feature.properties;
        
        // Date range filter
        if (filterState.dateRange.start && filterState.dateRange.end) {
            const accidentDate = new Date(props.datetime);
            const startDate = new Date(filterState.dateRange.start);
            const endDate = new Date(filterState.dateRange.end);
            endDate.setHours(23, 59, 59, 999); // Include entire end date
            
            if (accidentDate < startDate || accidentDate > endDate) {
                return false;
            }
        }
        
        // Severity filter
        if (filterState.severity.length > 0) {
            if (!filterState.severity.includes(props.severity)) {
                return false;
            }
        }
        
        // Category filter
        if (filterState.category.length > 0) {
            if (!filterState.category.includes(props.category)) {
                return false;
            }
        }
        
        // Participant filter
        if (filterState.participants.length > 0 && !filterState.participants.includes('all')) {
            // Check if any participant matches
            let hasMatch = false;
            
            // Check participant_categories property
            if (props.participant_categories && Array.isArray(props.participant_categories)) {
                hasMatch = filterState.participants.some(p => {
                    const participantMap = {
                        'pedestrian': 'Pedestrian',
                        'cyclist': 'Cyclist',
                        'motorcyclist': 'Motorcyclist',
                        'public_transport': 'Public transport',
                        'children': 'Children'
                    };
                    return props.participant_categories.includes(participantMap[p]);
                });
            }
            
            // Also check participants array
            if (!hasMatch && props.participants && Array.isArray(props.participants)) {
                props.participants.forEach(participant => {
                    if (typeof participant === 'string') {
                        try {
                            const parsed = JSON.parse(participant);
                            if (parsed.role) {
                                const role = parsed.role.toLowerCase();
                                if (filterState.participants.includes(role) || 
                                    (role === 'passenger' && filterState.participants.includes('public_transport'))) {
                                    hasMatch = true;
                                }
                            }
                        } catch (e) {
                            // Not JSON, skip
                        }
                    }
                });
            }
            
            if (!hasMatch) {
                return false;
            }
        }
        
        // Nearby objects filter
        if (filterState.nearby.length > 0) {
            const nearby = props.nearby || [];
            const hasMatch = filterState.nearby.some(filterNearby => 
                Array.isArray(nearby) && nearby.includes(filterNearby)
            );
            if (!hasMatch) {
                return false;
            }
        }
        
        // Road conditions filter
        if (filterState.roadConditions.length > 0) {
            const roadConditions = props.road_conditions || [];
            const hasMatch = filterState.roadConditions.some(filterRC => 
                Array.isArray(roadConditions) && roadConditions.includes(filterRC)
            );
            if (!hasMatch) {
                return false;
            }
        }
        
        // Weather filter
        if (filterState.weather.length > 0) {
            const weather = props.weather || [];
            const hasMatch = filterState.weather.some(filterWeather => 
                Array.isArray(weather) && weather.includes(filterWeather)
            );
            if (!hasMatch) {
                return false;
            }
        }
        
        // Street filter
        if (filterState.streets.length > 0) {
            const address = props.address || '';
            const hasMatch = filterState.streets.some(street =>
                address.toLowerCase().includes(street.toLowerCase())
            );
            if (!hasMatch) {
                return false;
            }
        }
        
        // Violations filter (check in participants/vehicles)
        if (filterState.violations.length > 0) {
            let hasViolation = false;
            
            // Check participants
            if (props.participants && Array.isArray(props.participants)) {
                props.participants.forEach(participant => {
                    if (typeof participant === 'string') {
                        try {
                            const parsed = JSON.parse(participant);
                            if (parsed.violations && Array.isArray(parsed.violations)) {
                                if (parsed.violations.some(v => filterState.violations.includes(v))) {
                                    hasViolation = true;
                                }
                            }
                        } catch (e) {
                            // Not JSON
                        }
                    }
                });
            }
            
            // Check vehicles
            if (!hasViolation && props.vehicles && Array.isArray(props.vehicles)) {
                props.vehicles.forEach(vehicle => {
                    if (typeof vehicle === 'string') {
                        try {
                            const parsed = JSON.parse(vehicle);
                            if (parsed.participants && Array.isArray(parsed.participants)) {
                                parsed.participants.forEach(p => {
                                    if (p.violations && Array.isArray(p.violations)) {
                                        if (p.violations.some(v => filterState.violations.includes(v))) {
                                            hasViolation = true;
                                        }
                                    }
                                });
                            }
                        } catch (e) {
                            // Not JSON
                        }
                    }
                });
            }
            
            if (!hasViolation) {
                return false;
            }
        }
        
        return true;
    });
    
    // Update visualization with filtered data
    updateVisualization();
    updateStatistics();
    updateFilterCounts();
}
```

### 4.8 Initialization

Add this to your initialization code:

```javascript
// Initialize filters after data is loaded
async function initializeFilters() {
    // Wait for data to load
    await loadGeoJSON();
    
    // Initialize filter options
    initializeFilterOptions();
    
    // Set default date range
    setDefaultDateRange();
    
    // Initialize filter UI
    initializeParticipantFilter();
    initializeSeverityFilter();
    
    // Apply initial filters
    applyFilters();
}

// Update your DOMContentLoaded event listener
document.addEventListener('DOMContentLoaded', async function() {
    // Initialize filters first
    await initializeFilters();
    
    // Then initialize map and visualization
    loadStateFromURL();
    updateVisualization();
    updateStatistics();
    
    // Set up event listeners
    map.on('moveend', updateURL);
    map.on('zoomend', function() {
        updateVisualization();
        updateURL();
    });
});
```

---

## 5. URL SYNCHRONIZATION

Update your `updateURL()` and `loadStateFromURL()` functions to include filter state:

```javascript
function updateURL() {
    const url = new URL(window.location);
    
    // Map center and zoom
    const center = map.getCenter();
    url.searchParams.set('center', `${center.lat}:${center.lng}`);
    url.searchParams.set('zoom', map.getZoom());
    
    // Date range
    if (filterState.dateRange.start && filterState.dateRange.end) {
        url.searchParams.set('start_date', filterState.dateRange.start);
        url.searchParams.set('end_date', filterState.dateRange.end);
    }
    
    // Participants
    if (filterState.participants.length > 0 && !filterState.participants.includes('all')) {
        url.searchParams.set('participants', filterState.participants.join(';'));
    }
    
    // Severity
    if (filterState.severity.length < 3) { // Only save if not all selected
        url.searchParams.set('severity', filterState.severity.join(';'));
    }
    
    // Category filters
    ['category', 'violations', 'nearby', 'roadConditions', 'streets', 'weather'].forEach(type => {
        if (filterState[type] && filterState[type].length > 0) {
            url.searchParams.set(type, filterState[type].join(';'));
        }
    });
    
    window.history.pushState(null, '', url);
}

function loadStateFromURL() {
    const url = new URL(window.location);
    
    // Map center and zoom
    const centerStr = url.searchParams.get('center');
    if (centerStr) {
        const [lat, lng] = centerStr.split(':').map(Number);
        const zoom = Number(url.searchParams.get('zoom')) || 12;
        map.setView([lat, lng], zoom);
    }
    
    // Load filters from URL
    const startDate = url.searchParams.get('start_date');
    const endDate = url.searchParams.get('end_date');
    if (startDate && endDate) {
        filterState.dateRange.start = startDate;
        filterState.dateRange.end = endDate;
        updateDateRangeDisplay();
    }
    
    const participants = url.searchParams.get('participants');
    if (participants) {
        filterState.participants = participants.split(';');
        // Update UI
        document.querySelectorAll('.participant-item').forEach(item => {
            const participant = item.getAttribute('data-participant');
            if (filterState.participants.includes(participant)) {
                item.setAttribute('data-selected', 'true');
                item.classList.add('active');
            }
        });
    }
    
    const severity = url.searchParams.get('severity');
    if (severity) {
        filterState.severity = severity.split(';');
        // Update checkboxes
        document.querySelectorAll('[data-severity]').forEach(checkbox => {
            checkbox.checked = filterState.severity.includes(checkbox.getAttribute('data-severity'));
        });
    }
    
    ['category', 'violations', 'nearby', 'roadConditions', 'streets', 'weather'].forEach(type => {
        const value = url.searchParams.get(type);
        if (value) {
            filterState[type] = value.split(';');
        }
    });
    
    updateFilterCounts();
    applyFilters();
}
```

---

## 6. TESTING CHECKLIST

- [ ] Date range filter works correctly
- [ ] Participant filter toggles correctly (All vs individual selections)
- [ ] Severity checkboxes work and filter correctly
- [ ] Category filter modal opens and closes
- [ ] Search in category filter modal works
- [ ] All category filters (types, violations, nearby, road conditions, streets, weather) work
- [ ] Filter counts update correctly
- [ ] Hide/Show button toggles panel visibility
- [ ] Filters persist in URL
- [ ] Filters load from URL on page load
- [ ] Filtered data updates map visualization correctly
- [ ] Filtered data updates statistics correctly
- [ ] Multiple filters work together (AND logic)
- [ ] Empty filter selections show all data

---

## 7. NOTES

1. **Participant Filter Logic**: The "All Participants" button should deselect all specific participants. When any specific participant is selected, "All" should be deselected automatically.

2. **Filter Counts**: Display the number of selected items in each category filter button.

3. **Search Functionality**: The category filter modal should have a search input that filters the options as you type.

4. **Default State**: By default, all severity levels should be selected, "All Participants" should be selected, and the date range should cover all available data.

5. **Performance**: For large datasets, consider debouncing the filter application or using web workers for filtering.

6. **Data Extraction**: The code extracts filter options from the GeoJSON data. Make sure your `english_clipped.geojson` has the expected property structure.

This implementation provides a complete filter panel system matching the Russian interface functionality but in English.

