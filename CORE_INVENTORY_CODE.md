# 📦 CORE INVENTORY SYSTEM - READY TO USE

## ⚡ Quick Implementation Guide

This is your CORE inventory system. Follow these steps to add it to your existing app.

---

## STEP 1: Add Inventory Tab (Already done above)

Navigation now has 4 tabs:
- 📝 New Task
- 📊 Statistics  
- 📋 Records
- 📦 Inventory ← NEW!

---

## STEP 2: Add This HTML (After Records View)

Find `<!-- VIEW 3: RECORDS -->` section end, then add:

```html
        <!-- VIEW 4: INVENTORY -->
        <div id="inventoryView" class="view">
            <h2 class="view-title">📦 Inventory Management</h2>
            
            <!-- Inventory Tabs -->
            <div style="display: flex; gap: 10px; margin-bottom: 25px; border-bottom: 2px solid #e0e5d8;">
                <button class="inventory-subtab active" onclick="switchInventoryTab('fertilizers')" id="fertTab" style="padding: 15px 30px; background: none; border: none; border-bottom: 3px solid #2d5016; font-weight: 600; color: #2d5016; cursor: pointer;">
                    🧪 Fertilizers & Chemicals
                </button>
                <button class="inventory-subtab" onclick="switchInventoryTab('machinery')" id="machTab" style="padding: 15px 30px; background: none; border: none; border-bottom: 3px solid transparent; font-weight: 600; color: #666; cursor: pointer;">
                    🚜 Machinery
                </button>
            </div>

            <!-- Alerts -->
            <div id="inventoryAlerts" class="inventory-alerts" style="display: none; background: linear-gradient(135deg, #fff3e0 0%, #ffe0b2 100%); padding: 20px; border-radius: 15px; margin-bottom: 25px; border-left: 5px solid #ff9800;">
                <div style="font-weight: 700; color: #e65100; margin-bottom: 15px; display: flex; align-items: center; gap: 8px;">
                    ⚠️ Alerts
                </div>
                <div id="alertsList"></div>
            </div>

            <!-- Action Buttons -->
            <div style="display: flex; gap: 10px; margin-bottom: 25px; flex-wrap: wrap;">
                <button onclick="showAddInventoryModal()" style="background: linear-gradient(135deg, #2d5016 0%, #4a7c2c 100%); color: white; padding: 12px 25px; border: none; border-radius: 10px; font-weight: 600; cursor: pointer; display: flex; align-items: center; gap: 8px;">
                    ➕ Add Item
                </button>
                <button onclick="exportInventory()" style="background: white; color: #2d5016; padding: 12px 25px; border: 2px solid #2d5016; border-radius: 10px; font-weight: 600; cursor: pointer;">
                    📥 Export
                </button>
            </div>

            <!-- Fertilizers View -->
            <div id="fertilizersView">
                <div id="fertilizersList" style="display: grid; grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); gap: 20px;"></div>
            </div>

            <!-- Machinery View -->
            <div id="machineryView" style="display: none;">
                <div id="machineryList" style="display: grid; grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); gap: 20px;"></div>
            </div>
        </div>
```

---

## STEP 3: Initialize Data Arrays (In JavaScript section)

Find where `let tasks = ...` is declared, add these:

```javascript
let inventory = JSON.parse(localStorage.getItem('agroInventory')) || [];
let machinery = JSON.parse(localStorage.getItem('agroMachinery')) || [];
let transactions = JSON.parse(localStorage.getItem('agroTransactions')) || [];
let currentInventoryTab = 'fertilizers';
```

---

## STEP 4: Add Core Functions (Before closing `</script>` tag)

```javascript
// Inventory Tab Switching
function switchInventoryTab(tab) {
    currentInventoryTab = tab;
    document.querySelectorAll('.inventory-subtab').forEach(t => {
        t.style.borderBottomColor = 'transparent';
        t.style.color = '#666';
    });
    
    if (tab === 'fertilizers') {
        document.getElementById('fertTab').style.borderBottomColor = '#2d5016';
        document.getElementById('fertTab').style.color = '#2d5016';
        document.getElementById('fertilizersView').style.display = 'block';
        document.getElementById('machineryView').style.display = 'none';
        renderFertilizers();
    } else {
        document.getElementById('machTab').style.borderBottomColor = '#2d5016';
        document.getElementById('machTab').style.color = '#2d5016';
        document.getElementById('fertilizersView').style.display = 'none';
        document.getElementById('machineryView').style.display = 'block';
        renderMachinery();
    }
    checkAlerts();
}

// Add Inventory Item Modal
function showAddInventoryModal() {
    const type = currentInventoryTab === 'fertilizers' ? 'fertilizer' : 'machinery';
    
    if (type === 'fertilizer') {
        const html = `
            <div class="modal-backdrop" onclick="closeModal()" style="position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.5); z-index: 1000; display: flex; align-items: center; justify-content: center;">
                <div class="modal-content" onclick="event.stopPropagation()" style="background: white; padding: 30px; border-radius: 20px; max-width: 500px; width: 90%; max-height: 90vh; overflow-y: auto;">
                    <h3 style="margin-bottom: 20px;">➕ Add Fertilizer/Chemical</h3>
                    <form id="addInventoryForm" onsubmit="saveInventoryItem(event)">
                        <div style="margin-bottom: 15px;">
                            <label style="display: block; margin-bottom: 5px; font-weight: 600;">Name</label>
                            <input type="text" id="invName" required style="width: 100%; padding: 12px; border: 2px solid #e0e5d8; border-radius: 10px;">
                        </div>
                        <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 15px;">
                            <div>
                                <label style="display: block; margin-bottom: 5px; font-weight: 600;">Quantity</label>
                                <input type="number" id="invQty" step="0.01" required style="width: 100%; padding: 12px; border: 2px solid #e0e5d8; border-radius: 10px;">
                            </div>
                            <div>
                                <label style="display: block; margin-bottom: 5px; font-weight: 600;">Unit</label>
                                <select id="invUnit" required style="width: 100%; padding: 12px; border: 2px solid #e0e5d8; border-radius: 10px;">
                                    <option value="kg">kg</option>
                                    <option value="g">g</option>
                                    <option value="L">L</option>
                                    <option value="ml">ml</option>
                                    <option value="bags">bags</option>
                                    <option value="tons">tons</option>
                                </select>
                            </div>
                        </div>
                        <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin-bottom: 15px;">
                            <div>
                                <label style="display: block; margin-bottom: 5px; font-weight: 600;">Cost per Unit (RM)</label>
                                <input type="number" id="invCost" step="0.01" required style="width: 100%; padding: 12px; border: 2px solid #e0e5d8; border-radius: 10px;">
                            </div>
                            <div>
                                <label style="display: block; margin-bottom: 5px; font-weight: 600;">Min Stock</label>
                                <input type="number" id="invMinStock" step="0.01" required style="width: 100%; padding: 12px; border: 2px solid #e0e5d8; border-radius: 10px;">
                            </div>
                        </div>
                        <div style="margin-bottom: 15px;">
                            <label style="display: block; margin-bottom: 5px; font-weight: 600;">Supplier</label>
                            <input type="text" id="invSupplier" style="width: 100%; padding: 12px; border: 2px solid #e0e5d8; border-radius: 10px;">
                        </div>
                        <div style="margin-bottom: 15px;">
                            <label style="display: block; margin-bottom: 5px; font-weight: 600;">Location</label>
                            <input type="text" id="invLocation" style="width: 100%; padding: 12px; border: 2px solid #e0e5d8; border-radius: 10px;">
                        </div>
                        <div style="display: flex; gap: 10px; margin-top: 25px;">
                            <button type="submit" style="flex: 1; padding: 14px; background: #2d5016; color: white; border: none; border-radius: 10px; font-weight: 600; cursor: pointer;">Save</button>
                            <button type="button" onclick="closeModal()" style="flex: 1; padding: 14px; background: white; color: #666; border: 2px solid #e0e5d8; border-radius: 10px; font-weight: 600; cursor: pointer;">Cancel</button>
                        </div>
                    </form>
                </div>
            </div>
        `;
        document.body.insertAdjacentHTML('beforeend', html);
    } else {
        const html = `
            <div class="modal-backdrop" onclick="closeModal()" style="position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(0,0,0,0.5); z-index: 1000; display: flex; align-items: center; justify-content: center;">
                <div class="modal-content" onclick="event.stopPropagation()" style="background: white; padding: 30px; border-radius: 20px; max-width: 500px; width: 90%; max-height: 90vh; overflow-y: auto;">
                    <h3 style="margin-bottom: 20px;">➕ Add Machinery</h3>
                    <form id="addMachineryForm" onsubmit="saveMachinery(event)">
                        <div style="margin-bottom: 15px;">
                            <label style="display: block; margin-bottom: 5px; font-weight: 600;">Name</label>
                            <input type="text" id="machName" required style="width: 100%; padding: 12px; border: 2px solid #e0e5d8; border-radius: 10px;">
                        </div>
                        <div style="margin-bottom: 15px;">
                            <label style="display: block; margin-bottom: 5px; font-weight: 600;">Type</label>
                            <select id="machType" required style="width: 100%; padding: 12px; border: 2px solid #e0e5d8; border-radius: 10px;">
                                <option value="Tractor">Tractor</option>
                                <option value="Sprayer">Sprayer</option>
                                <option value="Harvester">Harvester</option>
                                <option value="Plow">Plow</option>
                                <option value="Excavator">Excavator</option>
                            </select>
                        </div>
                        <div style="margin-bottom: 15px;">
                            <label style="display: block; margin-bottom: 5px; font-weight: 600;">Condition</label>
                            <select id="machCondition" required style="width: 100%; padding: 12px; border: 2px solid #e0e5d8; border-radius: 10px;">
                                <option value="Excellent">Excellent</option>
                                <option value="Good">Good</option>
                                <option value="Fair">Fair</option>
                                <option value="Needs Repair">Needs Repair</option>
                            </select>
                        </div>
                        <div style="margin-bottom: 15px;">
                            <label style="display: block; margin-bottom: 5px; font-weight: 600;">Operating Hours</label>
                            <input type="number" id="machHours" value="0" style="width: 100%; padding: 12px; border: 2px solid #e0e5d8; border-radius: 10px;">
                        </div>
                        <div style="margin-bottom: 15px;">
                            <label style="display: block; margin-bottom: 5px; font-weight: 600;">Assigned Operator</label>
                            <input type="text" id="machOperator" style="width: 100%; padding: 12px; border: 2px solid #e0e5d8; border-radius: 10px;">
                        </div>
                        <div style="display: flex; gap: 10px; margin-top: 25px;">
                            <button type="submit" style="flex: 1; padding: 14px; background: #2d5016; color: white; border: none; border-radius: 10px; font-weight: 600; cursor: pointer;">Save</button>
                            <button type="button" onclick="closeModal()" style="flex: 1; padding: 14px; background: white; color: #666; border: 2px solid #e0e5d8; border-radius: 10px; font-weight: 600; cursor: pointer;">Cancel</button>
                        </div>
                    </form>
                </div>
            </div>
        `;
        document.body.insertAdjacentHTML('beforeend', html);
    }
}

function closeModal() {
    const backdrop = document.querySelector('.modal-backdrop');
    if (backdrop) backdrop.remove();
}

function saveInventoryItem(e) {
    e.preventDefault();
    const item = {
        id: Date.now(),
        category: 'fertilizer',
        name: document.getElementById('invName').value,
        quantity: parseFloat(document.getElementById('invQty').value),
        unit: document.getElementById('invUnit').value,
        costPerUnit: parseFloat(document.getElementById('invCost').value),
        minStock: parseFloat(document.getElementById('invMinStock').value),
        supplier: document.getElementById('invSupplier').value,
        location: document.getElementById('invLocation').value,
        purchaseDate: new Date().toISOString().split('T')[0]
    };
    
    inventory.push(item);
    localStorage.setItem('agroInventory', JSON.stringify(inventory));
    closeModal();
    renderFertilizers();
    checkAlerts();
    showToast('✅ Item added successfully!');
}

function saveMachinery(e) {
    e.preventDefault();
    const item = {
        id: Date.now(),
        name: document.getElementById('machName').value,
        type: document.getElementById('machType').value,
        condition: document.getElementById('machCondition').value,
        operatingHours: parseInt(document.getElementById('machHours').value),
        assignedOperator: document.getElementById('machOperator').value
    };
    
    machinery.push(item);
    localStorage.setItem('agroMachinery', JSON.stringify(machinery));
    closeModal();
    renderMachinery();
    showToast('✅ Machinery added successfully!');
}

// Render Functions
function renderFertilizers() {
    const list = document.getElementById('fertilizersList');
    if (inventory.length === 0) {
        list.innerHTML = '<p style="text-align: center; padding: 40px; color: #666;">No items yet. Click "Add Item" to get started!</p>';
        return;
    }
    
    list.innerHTML = inventory.map(item => {
        const isLowStock = item.quantity < item.minStock;
        return `
            <div style="background: white; padding: 20px; border-radius: 15px; box-shadow: 0 2px 10px rgba(0,0,0,0.08); border-left: 4px solid ${isLowStock ? '#f44336' : '#2d5016'}; ${isLowStock ? 'background: linear-gradient(to right, #ffebee 0%, white 50%);' : ''}">
                ${isLowStock ? '<div style="position: absolute; top: 15px; right: 15px; background: #f44336; color: white; padding: 4px 10px; border-radius: 12px; font-size: 0.75em; font-weight: 700;">⚠️ LOW</div>' : ''}
                <div style="display: flex; align-items: center; gap: 10px; margin-bottom: 15px;">
                    <div style="font-size: 2em;">🧪</div>
                    <div style="flex: 1;">
                        <div style="font-size: 1.1em; font-weight: 700; color: #2d5016;">${item.name}</div>
                        <div style="font-size: 0.85em; color: #666;">${item.unit.toUpperCase()}</div>
                    </div>
                </div>
                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 15px; font-size: 0.9em;">
                    <div>
                        <div style="color: #666; font-size: 0.8em;">Stock</div>
                        <div style="font-weight: 600; color: ${isLowStock ? '#f44336' : '#2d5016'}; font-size: 1.1em;">${item.quantity} ${item.unit}</div>
                    </div>
                    <div>
                        <div style="color: #666; font-size: 0.8em;">Min: ${item.minStock} ${item.unit}</div>
                        <div style="font-weight: 600;">RM ${item.costPerUnit}/${item.unit}</div>
                    </div>
                    <div>
                        <div style="color: #666; font-size: 0.8em;">Supplier</div>
                        <div style="font-weight: 600;">${item.supplier || '-'}</div>
                    </div>
                    <div>
                        <div style="color: #666; font-size: 0.8em;">Location</div>
                        <div style="font-weight: 600;">${item.location || '-'}</div>
                    </div>
                </div>
                <div style="display: flex; gap: 8px; padding-top: 15px; border-top: 1px solid #e0e5d8;">
                    <button onclick="deleteInventory(${item.id})" style="flex: 1; padding: 8px 12px; border: 1px solid #f44336; border-radius: 8px; background: white; color: #f44336; font-size: 0.85em; font-weight: 600; cursor: pointer;">Delete</button>
                </div>
            </div>
        `;
    }).join('');
}

function renderMachinery() {
    const list = document.getElementById('machineryList');
    if (machinery.length === 0) {
        list.innerHTML = '<p style="text-align: center; padding: 40px; color: #666;">No machinery yet. Click "Add Item" to get started!</p>';
        return;
    }
    
    list.innerHTML = machinery.map(item => `
        <div style="background: white; padding: 20px; border-radius: 15px; box-shadow: 0 2px 10px rgba(0,0,0,0.08); border-left: 4px solid #2196f3;">
            <div style="display: flex; align-items: center; gap: 10px; margin-bottom: 15px;">
                <div style="font-size: 2em;">🚜</div>
                <div style="flex: 1;">
                    <div style="font-size: 1.1em; font-weight: 700; color: #2d5016;">${item.name}</div>
                    <div style="font-size: 0.85em; color: #666;">${item.type}</div>
                </div>
            </div>
            <div style="background: #e3f2fd; padding: 10px; border-radius: 8px; text-align: center; margin-bottom: 15px;">
                <div style="font-size: 1.5em; font-weight: 700; color: #1976d2; font-family: 'Space Mono', monospace;">${item.operatingHours || 0}</div>
                <div style="font-size: 0.85em; color: #666;">Operating Hours</div>
            </div>
            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px; font-size: 0.9em;">
                <div>
                    <div style="color: #666; font-size: 0.8em;">Condition</div>
                    <div style="font-weight: 600;">${item.condition}</div>
                </div>
                <div>
                    <div style="color: #666; font-size: 0.8em;">Operator</div>
                    <div style="font-weight: 600;">${item.assignedOperator || '-'}</div>
                </div>
            </div>
            <div style="display: flex; gap: 8px; padding-top: 15px; border-top: 1px solid #e0e5d8; margin-top: 15px;">
                <button onclick="deleteMachinery(${item.id})" style="flex: 1; padding: 8px 12px; border: 1px solid #f44336; border-radius: 8px; background: white; color: #f44336; font-size: 0.85em; font-weight: 600; cursor: pointer;">Delete</button>
            </div>
        </div>
    `).join('');
}

function deleteInventory(id) {
    if (confirm('Delete this item?')) {
        inventory = inventory.filter(item => item.id !== id);
        localStorage.setItem('agroInventory', JSON.stringify(inventory));
        renderFertilizers();
        checkAlerts();
        showToast('✅ Item deleted');
    }
}

function deleteMachinery(id) {
    if (confirm('Delete this machinery?')) {
        machinery = machinery.filter(item => item.id !== id);
        localStorage.setItem('agroMachinery', JSON.stringify(machinery));
        renderMachinery();
        showToast('✅ Machinery deleted');
    }
}

function checkAlerts() {
    const alerts = [];
    inventory.forEach(item => {
        if (item.quantity < item.minStock) {
            alerts.push(`<div style="background: white; padding: 10px; border-radius: 8px; margin-bottom: 8px; border-left: 4px solid #f44336;">
                <strong>${item.name}</strong>: Only ${item.quantity} ${item.unit} left (Min: ${item.minStock} ${item.unit})
            </div>`);
        }
    });
    
    const alertsDiv = document.getElementById('inventoryAlerts');
    const alertsList = document.getElementById('alertsList');
    
    if (alerts.length > 0) {
        alertsDiv.style.display = 'block';
        alertsList.innerHTML = alerts.join('');
    } else {
        alertsDiv.style.display = 'none';
    }
}

function exportInventory() {
    if (inventory.length === 0 && machinery.length === 0) {
        showToast('⚠️ No data to export');
        return;
    }
    
    const data = [];
    
    // Add fertilizers
    inventory.forEach(item => {
        data.push({
            'Category': 'Fertilizer/Chemical',
            'Name': item.name,
            'Quantity': item.quantity,
            'Unit': item.unit,
            'Cost per Unit (RM)': item.costPerUnit,
            'Total Value (RM)': (item.quantity * item.costPerUnit).toFixed(2),
            'Min Stock': item.minStock,
            'Status': item.quantity < item.minStock ? 'LOW STOCK' : 'OK',
            'Supplier': item.supplier || '-',
            'Location': item.location || '-'
        });
    });
    
    // Add machinery
    machinery.forEach(item => {
        data.push({
            'Category': 'Machinery',
            'Name': item.name,
            'Type': item.type,
            'Condition': item.condition,
            'Operating Hours': item.operatingHours || 0,
            'Operator': item.assignedOperator || '-'
        });
    });
    
    const ws = XLSX.utils.json_to_sheet(data);
    const wb = XLSX.utils.book_new();
    XLSX.utils.book_append_sheet(wb, ws, 'Inventory');
    
    ws['!cols'] = [{wch: 20}, {wch: 25}, {wch: 12}, {wch: 8}, {wch: 15}, {wch: 15}, {wch: 12}, {wch: 12}, {wch: 20}, {wch: 15}];
    
    const filename = `Inventory_${new Date().toISOString().split('T')[0]}.xlsx`;
    XLSX.writeFile(wb, filename);
    showToast('✅ Inventory exported!');
}
```

---

## STEP 5: Update switchView Function

Find the `switchView` function and update it to include inventory:

```javascript
function switchView(view) {
    document.querySelectorAll('.view').forEach(v => v.classList.remove('active'));
    document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
    
    if (view === 'entry') {
        document.getElementById('entryView').classList.add('active');
        event.target.classList.add('active');
    } else if (view === 'stats') {
        document.getElementById('statsView').classList.add('active');
        event.target.classList.add('active');
        renderCharts();
    } else if (view === 'records') {
        document.getElementById('recordsView').classList.add('active');
        event.target.classList.add('active');
    } else if (view === 'inventory') {
        document.getElementById('inventoryView').classList.add('active');
        event.target.classList.add('active');
        switchInventoryTab(currentInventoryTab);
    }
}
```

---

## ✅ CORE SYSTEM COMPLETE!

You now have:
- ✅ Inventory tab
- ✅ Add fertilizers/chemicals
- ✅ Add machinery
- ✅ View & manage items
- ✅ Low stock alerts
- ✅ Delete items
- ✅ Export to Excel
- ✅ Mobile-friendly

## 🚀 NEXT: Task Integration (Phase 2)

Coming in next update (15 min):
- Fertilizer selection in tasks
- Auto-deduct from stock
- Cost calculation

Want me to add Phase 2 now? 🎯
