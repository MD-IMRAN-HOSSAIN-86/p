<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tasmia Cottage Meal Management</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        emerald: {
                            600: '#059669',
                            700: '#047857',
                        }
                    }
                }
            }
        }
    </script>
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
            min-height: 100vh;
        }
        
        .card {
            background: rgba(30, 41, 59, 0.8);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(71, 85, 105, 0.3);
        }
        
        .btn-primary {
            background: linear-gradient(135deg, #059669 0%, #047857 100%);
            border: none;
            color: white;
            font-weight: 600;
            padding: 10px 20px;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(5, 150, 105, 0.3);
        }
        
        .meal-badge {
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
        }
        
        .meal-on {
            background: rgba(5, 150, 105, 0.2);
            color: #10b981;
            border: 1px solid rgba(5, 150, 105, 0.3);
        }
        
        .meal-off {
            background: rgba(239, 68, 68, 0.2);
            color: #f87171;
            border: 1px solid rgba(239, 68, 68, 0.3);
        }
        
        .tab-button {
            padding: 12px 24px;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .tab-button.active {
            background: #059669;
            color: white;
            box-shadow: 0 2px 8px rgba(5, 150, 105, 0.3);
        }
        
        .tab-button:not(.active) {
            background: transparent;
            color: #94a3b8;
        }
        
        .tab-button:not(.active):hover {
            background: rgba(71, 85, 105, 0.3);
            color: #cbd5e1;
        }
        
        input, textarea, select {
            background: rgba(15, 23, 42, 0.7);
            border: 1px solid rgba(71, 85, 105, 0.5);
            color: white;
            padding: 10px 14px;
            border-radius: 8px;
            outline: none;
            transition: all 0.3s ease;
        }
        
        input:focus, textarea:focus, select:focus {
            border-color: #059669;
            box-shadow: 0 0 0 2px rgba(5, 150, 105, 0.2);
        }
        
        /* Toast notifications */
        .toast {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 16px 24px;
            border-radius: 8px;
            z-index: 1000;
            animation: slideIn 0.3s ease-out;
            max-width: 400px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
        }
        
        .toast-success {
            background: linear-gradient(135deg, #059669 0%, #047857 100%);
            color: white;
        }
        
        .toast-error {
            background: linear-gradient(135deg, #dc2626 0%, #b91c1c 100%);
            color: white;
        }
        
        .toast-info {
            background: linear-gradient(135deg, #3b82f6 0%, #1d4ed8 100%);
            color: white;
        }
        
        @keyframes slideIn {
            from {
                transform: translateX(100%);
                opacity: 0;
            }
            to {
                transform: translateX(0);
                opacity: 1;
            }
        }
        
        @keyframes slideOut {
            from {
                transform: translateX(0);
                opacity: 1;
            }
            to {
                transform: translateX(100%);
                opacity: 0;
            }
        }
        
        /* Meal counter styles */
        .counter-badge {
            background: rgba(139, 92, 246, 0.2);
            border: 1px solid rgba(139, 92, 246, 0.3);
            color: #a78bfa;
            padding: 2px 8px;
            border-radius: 12px;
            font-size: 11px;
            font-weight: 600;
        }
        
        .counter-total {
            background: rgba(59, 130, 246, 0.2);
            border: 1px solid rgba(59, 130, 246, 0.3);
            color: #60a5fa;
            padding: 3px 10px;
            border-radius: 15px;
            font-size: 12px;
            font-weight: 700;
        }
        
        .monthly-stats {
            background: linear-gradient(135deg, rgba(30, 41, 59, 0.9) 0%, rgba(15, 23, 42, 0.9) 100%);
            border: 1px solid rgba(139, 92, 246, 0.3);
        }
        
        /* Pulse animation for live indicator */
        .pulse {
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0% {
                opacity: 0.6;
            }
            50% {
                opacity: 1;
            }
            100% {
                opacity: 0.6;
            }
        }
        
        /* Toggle switch for admin */
        .toggle-switch {
            position: relative;
            display: inline-block;
            width: 50px;
            height: 26px;
        }
        
        .toggle-switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }
        
        .toggle-slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #dc2626;
            transition: .4s;
            border-radius: 34px;
        }
        
        .toggle-slider:before {
            position: absolute;
            content: "";
            height: 18px;
            width: 18px;
            left: 4px;
            bottom: 4px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }
        
        input:checked + .toggle-slider {
            background-color: #059669;
        }
        
        input:checked + .toggle-slider:before {
            transform: translateX(24px);
        }
        
        /* Scrollbar styling */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        
        ::-webkit-scrollbar-track {
            background: rgba(30, 41, 59, 0.5);
            border-radius: 4px;
        }
        
        ::-webkit-scrollbar-thumb {
            background: rgba(71, 85, 105, 0.7);
            border-radius: 4px;
        }
        
        ::-webkit-scrollbar-thumb:hover {
            background: rgba(100, 116, 139, 0.8);
        }
    </style>
</head>
<body class="text-gray-200">
    <div id="app"></div>
    
    <!-- Toast container -->
    <div id="toast-container"></div>

    <script>
        // ======================= UTILITY FUNCTIONS =======================
        function showToast(message, type = 'info', duration = 3000) {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            toast.className = `toast toast-${type}`;
            toast.innerHTML = `
                <div class="flex items-center gap-3">
                    <i class="fas ${type === 'success' ? 'fa-check-circle' : type === 'error' ? 'fa-exclamation-circle' : 'fa-info-circle'}"></i>
                    <span>${message}</span>
                </div>
            `;
            container.appendChild(toast);
            
            setTimeout(() => {
                toast.style.animation = 'slideOut 0.3s ease-out forwards';
                setTimeout(() => toast.remove(), 300);
            }, duration);
        }

        function formatDate(date) {
            const d = new Date(date);
            const day = String(d.getDate()).padStart(2, '0');
            const month = String(d.getMonth() + 1).padStart(2, '0');
            const year = d.getFullYear();
            return `${year}-${month}-${day}`;
        }

        function getToday() {
            return formatDate(new Date());
        }

        function getTomorrow() {
            const tomorrow = new Date();
            tomorrow.setDate(tomorrow.getDate() + 1);
            return formatDate(tomorrow);
        }

        function getCurrentMonth() {
            const now = new Date();
            return now.getMonth() + 1; // 1-12
        }

        function getCurrentYear() {
            return new Date().getFullYear();
        }

        // ======================= STORAGE MANAGER =======================
        const StorageManager = {
            STORAGE_KEY: 'tasmia_cottage_data_v4',
            
            saveData(data) {
                try {
                    const dataToSave = {
                        ...data,
                        lastUpdated: new Date().toISOString(),
                        version: '4.0'
                    };
                    localStorage.setItem(this.STORAGE_KEY, JSON.stringify(dataToSave));
                    return true;
                } catch (error) {
                    console.error('Save error:', error);
                    showToast('Error saving data', 'error');
                    return false;
                }
            },
            
            loadData() {
                try {
                    const saved = localStorage.getItem(this.STORAGE_KEY);
                    if (saved) {
                        const data = JSON.parse(saved);
                        
                        // Ensure data has required structure
                        if (!data.members || !Array.isArray(data.members)) {
                            data.members = [];
                        }
                        
                        // Ensure each member has monthlyMealCount
                        data.members.forEach(member => {
                            if (!member.monthlyMealCount) {
                                member.monthlyMealCount = {};
                            }
                            
                            // Initialize current month if not exists
                            const monthYear = `${getCurrentYear()}-${getCurrentMonth()}`;
                            if (!member.monthlyMealCount[monthYear]) {
                                member.monthlyMealCount[monthYear] = {
                                    breakfast: 0,
                                    lunch: 0,
                                    dinner: 0,
                                    guestBreakfast: 0,
                                    guestLunch: 0,
                                    guestDinner: 0
                                };
                            }
                        });
                        
                        if (!data.settings) {
                            data.settings = {
                                adminCanAlwaysEdit: true // Admin can always edit by default
                            };
                        }
                        
                        return data;
                    }
                } catch (error) {
                    console.error('Load error:', error);
                }
                
                // Return default data structure
                return {
                    members: [],
                    settings: {
                        adminCanAlwaysEdit: true
                    },
                    lastUpdated: null,
                    version: '4.0'
                };
            },
            
            clearData() {
                localStorage.removeItem(this.STORAGE_KEY);
                showToast('All data cleared', 'info');
            },
            
            exportData(data) {
                try {
                    const exportData = {
                        members: data.members,
                        settings: data.settings,
                        exportDate: new Date().toISOString(),
                        totalMembers: data.members.length
                    };
                    
                    const dataStr = JSON.stringify(exportData, null, 2);
                    const blob = new Blob([dataStr], { type: 'application/json' });
                    const url = URL.createObjectURL(blob);
                    
                    const a = document.createElement('a');
                    a.href = url;
                    a.download = `tasmia-cottage-backup-${formatDate(new Date())}.json`;
                    document.body.appendChild(a);
                    a.click();
                    document.body.removeChild(a);
                    URL.revokeObjectURL(url);
                    
                    showToast('Data exported successfully', 'success');
                } catch (error) {
                    console.error('Export error:', error);
                    showToast('Error exporting data', 'error');
                }
            },
            
            async importData(file) {
                return new Promise((resolve, reject) => {
                    const reader = new FileReader();
                    
                    reader.onload = (e) => {
                        try {
                            const imported = JSON.parse(e.target.result);
                            
                            if (!imported.members || !Array.isArray(imported.members)) {
                                throw new Error('Invalid data format');
                            }
                            
                            showToast(`Imported ${imported.members.length} members`, 'success');
                            resolve(imported);
                        } catch (error) {
                            showToast('Invalid file format', 'error');
                            reject(error);
                        }
                    };
                    
                    reader.onerror = () => {
                        showToast('Error reading file', 'error');
                        reject(new Error('File read error'));
                    };
                    
                    reader.readAsText(file);
                });
            }
        };

        // ======================= APP STATE =======================
        let appState = {
            activeTab: 'dashboard',
            selectedDate: getToday(),
            members: [],
            isEditing: false,
            currentEditId: null,
            searchTerm: '',
            viewMode: 'daily', // 'daily' or 'monthly'
            selectedMonth: getCurrentMonth(),
            selectedYear: getCurrentYear()
        };

        let storageData = StorageManager.loadData();

        // ======================= MEAL COUNT MANAGEMENT =======================
        function updateMealCount(memberId, date, mealType, status, guestCount = 0) {
            const member = storageData.members.find(m => m.id === memberId);
            if (!member) return;
            
            const currentDate = new Date(date);
            const monthYear = `${currentDate.getFullYear()}-${currentDate.getMonth() + 1}`;
            
            // Initialize monthly meal count if not exists
            if (!member.monthlyMealCount) {
                member.monthlyMealCount = {};
            }
            
            if (!member.monthlyMealCount[monthYear]) {
                member.monthlyMealCount[monthYear] = {
                    breakfast: 0,
                    lunch: 0,
                    dinner: 0,
                    guestBreakfast: 0,
                    guestLunch: 0,
                    guestDinner: 0
                };
            }
            
            // Get current month's data
            const monthData = member.monthlyMealCount[monthYear];
            
            // Check if we already counted this meal for today
            const todayKey = `counted_${date}_${mealType}`;
            const guestKey = `counted_${date}_guest_${mealType}`;
            
            if (!member.dailyCounted) {
                member.dailyCounted = {};
            }
            
            // Count member meal
            if (status && !member.dailyCounted[todayKey]) {
                monthData[mealType] += 1;
                member.dailyCounted[todayKey] = true;
            } else if (!status && member.dailyCounted[todayKey]) {
                monthData[mealType] = Math.max(0, monthData[mealType] - 1);
                delete member.dailyCounted[todayKey];
            }
            
            // Count guest meals
            const currentGuestCount = member.dailyCounted[guestKey] || 0;
            if (guestCount > currentGuestCount) {
                const diff = guestCount - currentGuestCount;
                monthData[`guest${mealType.charAt(0).toUpperCase() + mealType.slice(1)}`] += diff;
                member.dailyCounted[guestKey] = guestCount;
            } else if (guestCount < currentGuestCount) {
                const diff = currentGuestCount - guestCount;
                monthData[`guest${mealType.charAt(0).toUpperCase() + mealType.slice(1)}`] = 
                    Math.max(0, monthData[`guest${mealType.charAt(0).toUpperCase() + mealType.slice(1)}`] - diff);
                member.dailyCounted[guestKey] = guestCount;
            }
            
            StorageManager.saveData(storageData);
        }

        function getMonthlyStats(memberId, month, year) {
            const member = storageData.members.find(m => m.id === memberId);
            if (!member || !member.monthlyMealCount) {
                return {
                    breakfast: 0,
                    lunch: 0,
                    dinner: 0,
                    guestBreakfast: 0,
                    guestLunch: 0,
                    guestDinner: 0,
                    total: 0,
                    totalWithGuests: 0
                };
            }
            
            const monthYear = `${year}-${month}`;
            const monthData = member.monthlyMealCount[monthYear] || {
                breakfast: 0,
                lunch: 0,
                dinner: 0,
                guestBreakfast: 0,
                guestLunch: 0,
                guestDinner: 0
            };
            
            const totalMemberMeals = monthData.breakfast + monthData.lunch + monthData.dinner;
            const totalGuestMeals = monthData.guestBreakfast + monthData.guestLunch + monthData.guestDinner;
            
            return {
                ...monthData,
                total: totalMemberMeals,
                totalWithGuests: totalMemberMeals + totalGuestMeals
            };
        }

        function getAllMembersMonthlyStats(month, year) {
            const stats = {
                totalBreakfast: 0,
                totalLunch: 0,
                totalDinner: 0,
                totalGuestBreakfast: 0,
                totalGuestLunch: 0,
                totalGuestDinner: 0,
                members: []
            };
            
            storageData.members.forEach(member => {
                const memberStats = getMonthlyStats(member.id, month, year);
                stats.totalBreakfast += memberStats.breakfast;
                stats.totalLunch += memberStats.lunch;
                stats.totalDinner += memberStats.dinner;
                stats.totalGuestBreakfast += memberStats.guestBreakfast;
                stats.totalGuestLunch += memberStats.guestLunch;
                stats.totalGuestDinner += memberStats.guestDinner;
                
                stats.members.push({
                    name: member.name,
                    room: member.room,
                    ...memberStats
                });
            });
            
            stats.totalMeals = stats.totalBreakfast + stats.totalLunch + stats.totalDinner;
            stats.totalWithGuests = stats.totalMeals + stats.totalGuestBreakfast + stats.totalGuestLunch + stats.totalGuestDinner;
            
            return stats;
        }

        // ======================= MEMBER MANAGEMENT =======================
        function addMember(name, room, phone) {
            const newMember = {
                id: Date.now().toString(),
                name: name.trim(),
                room: room.trim(),
                phone: phone.trim(),
                isActive: true,
                dailyData: {},
                monthlyMealCount: {},
                dailyCounted: {},
                createdAt: new Date().toISOString()
            };
            
            // Initialize current month meal count
            const monthYear = `${getCurrentYear()}-${getCurrentMonth()}`;
            newMember.monthlyMealCount[monthYear] = {
                breakfast: 0,
                lunch: 0,
                dinner: 0,
                guestBreakfast: 0,
                guestLunch: 0,
                guestDinner: 0
            };
            
            storageData.members.push(newMember);
            StorageManager.saveData(storageData);
            appState.members = storageData.members;
            showToast(`Member "${name}" added successfully`, 'success');
            renderApp();
        }

        function updateMember(id, updates) {
            const index = storageData.members.findIndex(m => m.id === id);
            if (index !== -1) {
                storageData.members[index] = { ...storageData.members[index], ...updates };
                StorageManager.saveData(storageData);
                appState.members = storageData.members;
                showToast('Member updated successfully', 'success');
                renderApp();
            }
        }

        function deleteMember(id) {
            if (confirm('Are you sure you want to delete this member?')) {
                const memberIndex = storageData.members.findIndex(m => m.id === id);
                if (memberIndex !== -1) {
                    const memberName = storageData.members[memberIndex].name;
                    storageData.members.splice(memberIndex, 1);
                    StorageManager.saveData(storageData);
                    appState.members = storageData.members;
                    showToast(`Member "${memberName}" deleted`, 'info');
                    renderApp();
                }
            }
        }

        function toggleMemberStatus(id) {
            const member = storageData.members.find(m => m.id === id);
            if (member) {
                member.isActive = !member.isActive;
                StorageManager.saveData(storageData);
                appState.members = storageData.members;
                showToast(`Member ${member.isActive ? 'activated' : 'deactivated'}`, 'success');
                renderApp();
            }
        }

        function updateMealStatus(memberId, date, mealType, status) {
            const member = storageData.members.find(m => m.id === memberId);
            if (!member) return;
            
            if (!member.dailyData[date]) {
                member.dailyData[date] = {
                    breakfast: false,
                    lunch: false,
                    dinner: false,
                    guestBreakfast: 0,
                    guestLunch: 0,
                    guestDinner: 0,
                    note: ''
                };
            }
            
            // Get current guest count
            const guestKey = `guest${mealType.charAt(0).toUpperCase() + mealType.slice(1)}`;
            const guestCount = member.dailyData[date][guestKey] || 0;
            
            // Update meal status
            member.dailyData[date][mealType] = status;
            
            // Update meal count
            updateMealCount(memberId, date, mealType, status, guestCount);
            
            StorageManager.saveData(storageData);
            renderApp();
        }

        function updateGuestMeals(memberId, date, mealType, delta) {
            const member = storageData.members.find(m => m.id === memberId);
            if (!member) return;
            
            if (!member.dailyData[date]) {
                member.dailyData[date] = {
                    breakfast: false,
                    lunch: false,
                    dinner: false,
                    guestBreakfast: 0,
                    guestLunch: 0,
                    guestDinner: 0,
                    note: ''
                };
            }
            
            const guestKey = `guest${mealType.charAt(0).toUpperCase() + mealType.slice(1)}`;
            const current = member.dailyData[date][guestKey] || 0;
            const newValue = Math.max(0, current + delta);
            member.dailyData[date][guestKey] = newValue;
            
            // Get current meal status
            const mealStatus = member.dailyData[date][mealType];
            
            // Update meal count with new guest count
            updateMealCount(memberId, date, mealType, mealStatus, newValue);
            
            StorageManager.saveData(storageData);
            renderApp();
        }

        function updateNote(memberId, date, note) {
            const member = storageData.members.find(m => m.id === memberId);
            if (!member) return;
            
            if (!member.dailyData[date]) {
                member.dailyData[date] = {
                    breakfast: false,
                    lunch: false,
                    dinner: false,
                    guestBreakfast: 0,
                    guestLunch: 0,
                    guestDinner: 0,
                    note: ''
                };
            }
            
            member.dailyData[date].note = note;
            StorageManager.saveData(storageData);
        }

        function getMealStats(date) {
            const stats = {
                breakfast: { members: 0, guests: 0, total: 0 },
                lunch: { members: 0, guests: 0, total: 0 },
                dinner: { members: 0, guests: 0, total: 0 }
            };
            
            storageData.members.forEach(member => {
                const dayData = member.dailyData[date];
                
                if (member.isActive) {
                    // Active members count as 1 for each meal if no data or if turned on
                    if (!dayData || dayData.breakfast) {
                        stats.breakfast.members += 1;
                    }
                    if (!dayData || dayData.lunch) {
                        stats.lunch.members += 1;
                    }
                    if (!dayData || dayData.dinner) {
                        stats.dinner.members += 1;
                    }
                }
                
                // Add guest meals
                if (dayData) {
                    stats.breakfast.guests += dayData.guestBreakfast || 0;
                    stats.lunch.guests += dayData.guestLunch || 0;
                    stats.dinner.guests += dayData.guestDinner || 0;
                }
            });
            
            // Calculate totals
            stats.breakfast.total = stats.breakfast.members + stats.breakfast.guests;
            stats.lunch.total = stats.lunch.members + stats.lunch.guests;
            stats.dinner.total = stats.dinner.members + stats.dinner.guests;
            
            return stats;
        }

        // ======================= ADMIN SETTINGS =======================
        function toggleAdminEditMode() {
            storageData.settings.adminCanAlwaysEdit = !storageData.settings.adminCanAlwaysEdit;
            StorageManager.saveData(storageData);
            showToast(`Admin ${storageData.settings.adminCanAlwaysEdit ? 'can always edit' : 'follows 11 PM rule'}`, 'info');
            renderApp();
        }

        function isAdminEditable() {
            // If admin can always edit setting is on, return true
            if (storageData.settings.adminCanAlwaysEdit) {
                return true;
            }
            
            // Otherwise follow the 11 PM rule
            const isTomorrow = appState.selectedDate === getTomorrow();
            const now = new Date();
            return !isTomorrow || now.getHours() < 23;
        }

        // ======================= UI COMPONENTS =======================
        function Header() {
            return `
                <header class="card mb-8 p-6">
                    <div class="flex flex-col md:flex-row md:items-center justify-between gap-4">
                        <div>
                            <h1 class="text-3xl font-bold text-white mb-2">
                                <i class="fas fa-utensils text-emerald-500 mr-3"></i>
                                Tasmia Cottage Meal Management
                            </h1>
                            <p class="text-gray-400">
                                Admin can always edit meals • Monthly meal counting enabled
                            </p>
                        </div>
                        <div class="flex items-center gap-3">
                            <div class="flex items-center gap-2 px-3 py-2 rounded-lg bg-emerald-900/30 border border-emerald-700/50">
                                <div class="w-2 h-2 bg-emerald-400 rounded-full pulse"></div>
                                <span class="text-sm font-medium text-emerald-300">Auto-save ON</span>
                            </div>
                            <button onclick="handleManualSave()" class="btn-primary flex items-center gap-2">
                                <i class="fas fa-save"></i>
                                <span class="hidden md:inline">Save Now</span>
                            </button>
                        </div>
                    </div>
                </header>
            `;
        }

        function Tabs() {
            return `
                <div class="card mb-8 p-2">
                    <div class="flex space-x-2">
                        <button 
                            onclick="switchTab('dashboard')"
                            class="tab-button flex-1 flex items-center justify-center gap-2 ${appState.activeTab === 'dashboard' ? 'active' : ''}"
                        >
                            <i class="fas fa-tachometer-alt"></i>
                            Dashboard
                        </button>
                        <button 
                            onclick="switchTab('admin')"
                            class="tab-button flex-1 flex items-center justify-center gap-2 ${appState.activeTab === 'admin' ? 'active' : ''}"
                        >
                            <i class="fas fa-user-cog"></i>
                            Admin Panel
                        </button>
                        <button 
                            onclick="switchTab('monthly')"
                            class="tab-button flex-1 flex items-center justify-center gap-2 ${appState.activeTab === 'monthly' ? 'active' : ''}"
                        >
                            <i class="fas fa-chart-bar"></i>
                            Monthly Report
                        </button>
                        <button 
                            onclick="switchTab('data')"
                            class="tab-button flex-1 flex items-center justify-center gap-2 ${appState.activeTab === 'data' ? 'active' : ''}"
                        >
                            <i class="fas fa-database"></i>
                            Data
                        </button>
                    </div>
                </div>
            `;
        }

        function DateSelector() {
            return `
                <div class="card mb-8 p-6">
                    <div class="flex flex-col md:flex-row md:items-center justify-between gap-4">
                        <div>
                            <h2 class="text-xl font-bold text-white mb-2 flex items-center gap-2">
                                <i class="fas fa-calendar text-emerald-500"></i>
                                ${appState.viewMode === 'monthly' ? 'Monthly Report' : 'Select Date'}
                            </h2>
                            <p class="text-gray-400 text-sm">
                                ${isAdminEditable() ? 
                                    '<span class="text-emerald-400"><i class="fas fa-user-shield mr-2"></i>Admin edit mode: Always enabled</span>' : 
                                    (appState.selectedDate === getTomorrow() ? 
                                        '<span class="text-amber-400"><i class="fas fa-exclamation-triangle mr-2"></i>Past 11 PM - Viewing tomorrow\'s meals</span>' : 
                                        `Viewing meals for ${appState.selectedDate}`)
                                }
                            </p>
                        </div>
                        <div class="flex gap-2">
                            ${appState.viewMode === 'daily' ? `
                                <button 
                                    onclick="setDate('${getToday()}')"
                                    class="px-4 py-2 rounded-lg bg-slate-800 hover:bg-slate-700 border border-slate-700 transition-colors ${appState.selectedDate === getToday() ? 'ring-2 ring-emerald-500' : ''}"
                                >
                                    Today
                                </button>
                                <button 
                                    onclick="setDate('${getTomorrow()}')"
                                    class="px-4 py-2 rounded-lg bg-slate-800 hover:bg-slate-700 border border-slate-700 transition-colors ${appState.selectedDate === getTomorrow() ? 'ring-2 ring-emerald-500' : ''}"
                                >
                                    Tomorrow
                                </button>
                                <input 
                                    type="date" 
                                    value="${appState.selectedDate}"
                                    onchange="setDate(this.value)"
                                    class="px-4 py-2 rounded-lg bg-slate-900 border border-slate-700"
                                >
                            ` : `
                                <div class="flex gap-2">
                                    <select 
                                        value="${appState.selectedMonth}"
                                        onchange="setMonth(this.value)"
                                        class="px-4 py-2 rounded-lg bg-slate-900 border border-slate-700"
                                    >
                                        ${Array.from({length: 12}, (_, i) => i + 1).map(m => `
                                            <option value="${m}" ${m == appState.selectedMonth ? 'selected' : ''}>
                                                ${new Date(2000, m-1).toLocaleString('default', {month: 'long'})}
                                            </option>
                                        `).join('')}
                                    </select>
                                    <select 
                                        value="${appState.selectedYear}"
                                        onchange="setYear(this.value)"
                                        class="px-4 py-2 rounded-lg bg-slate-900 border border-slate-700"
                                    >
                                        ${Array.from({length: 5}, (_, i) => getCurrentYear() - 2 + i).map(y => `
                                            <option value="${y}" ${y == appState.selectedYear ? 'selected' : ''}>${y}</option>
                                        `).join('')}
                                    </select>
                                    <button 
                                        onclick="setViewMode('daily')"
                                        class="px-4 py-2 rounded-lg bg-slate-800 hover:bg-slate-700 border border-slate-700 transition-colors"
                                    >
                                        <i class="fas fa-calendar-day mr-2"></i>Daily View
                                    </button>
                                </div>
                            `}
                        </div>
                    </div>
                </div>
            `;
        }

        function AdminSettings() {
            return `
                <div class="card p-6 mb-6">
                    <div class="flex items-center justify-between">
                        <div>
                            <h3 class="text-lg font-bold text-white mb-2 flex items-center gap-2">
                                <i class="fas fa-user-shield text-emerald-500"></i>
                                Admin Settings
                            </h3>
                            <p class="text-sm text-gray-400">
                                Control admin editing permissions
                            </p>
                        </div>
                        <div class="flex items-center gap-3">
                            <span class="text-sm ${storageData.settings.adminCanAlwaysEdit ? 'text-emerald-400' : 'text-amber-400'}">
                                ${storageData.settings.adminCanAlwaysEdit ? 
                                    'Admin can always edit' : 
                                    'Admin follows 11 PM rule'}
                            </span>
                            <label class="toggle-switch">
                                <input 
                                    type="checkbox" 
                                    ${storageData.settings.adminCanAlwaysEdit ? 'checked' : ''}
                                    onchange="toggleAdminEditMode()"
                                >
                                <span class="toggle-slider"></span>
                            </label>
                        </div>
                    </div>
                </div>
            `;
        }

        function StatsCard() {
            const stats = getMealStats(appState.selectedDate);
            const editable = isAdminEditable();
            
            return `
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
                    <div class="card p-6 border-l-4 border-l-emerald-500">
                        <div class="flex items-center justify-between mb-4">
                            <div class="flex items-center gap-3">
                                <div class="p-3 rounded-lg bg-emerald-900/30">
                                    <i class="fas fa-coffee text-emerald-400 text-xl"></i>
                                </div>
                                <div>
                                    <h3 class="font-semibold text-gray-300">Breakfast</h3>
                                    <p class="text-3xl font-bold text-white mt-1">${stats.breakfast.total}</p>
                                </div>
                            </div>
                            <div class="text-right">
                                <p class="text-sm text-gray-400">${stats.breakfast.members} members</p>
                                <p class="text-sm text-gray-400">${stats.breakfast.guests} guests</p>
                            </div>
                        </div>
                        <div class="text-sm ${editable ? 'text-emerald-400' : 'text-amber-400'}">
                            ${editable ? 
                                '<i class="fas fa-edit mr-2"></i>Editable (Admin Mode)' : 
                                '<i class="fas fa-lock mr-2"></i>Read-only'}
                        </div>
                    </div>
                    
                    <div class="card p-6 border-l-4 border-l-blue-500">
                        <div class="flex items-center justify-between mb-4">
                            <div class="flex items-center gap-3">
                                <div class="p-3 rounded-lg bg-blue-900/30">
                                    <i class="fas fa-utensils text-blue-400 text-xl"></i>
                                </div>
                                <div>
                                    <h3 class="font-semibold text-gray-300">Lunch</h3>
                                    <p class="text-3xl font-bold text-white mt-1">${stats.lunch.total}</p>
                                </div>
                            </div>
                            <div class="text-right">
                                <p class="text-sm text-gray-400">${stats.lunch.members} members</p>
                                <p class="text-sm text-gray-400">${stats.lunch.guests} guests</p>
                            </div>
                        </div>
                        <div class="text-sm ${editable ? 'text-emerald-400' : 'text-amber-400'}">
                            ${editable ? 
                                '<i class="fas fa-edit mr-2"></i>Editable (Admin Mode)' : 
                                '<i class="fas fa-lock mr-2"></i>Read-only'}
                        </div>
                    </div>
                    
                    <div class="card p-6 border-l-4 border-l-purple-500">
                        <div class="flex items-center justify-between mb-4">
                            <div class="flex items-center gap-3">
                                <div class="p-3 rounded-lg bg-purple-900/30">
                                    <i class="fas fa-drumstick-bite text-purple-400 text-xl"></i>
                                </div>
                                <div>
                                    <h3 class="font-semibold text-gray-300">Dinner</h3>
                                    <p class="text-3xl font-bold text-white mt-1">${stats.dinner.total}</p>
                                </div>
                            </div>
                            <div class="text-right">
                                <p class="text-sm text-gray-400">${stats.dinner.members} members</p>
                                <p class="text-sm text-gray-400">${stats.dinner.guests} guests</p>
                            </div>
                        </div>
                        <div class="text-sm ${editable ? 'text-emerald-400' : 'text-amber-400'}">
                            ${editable ? 
                                '<i class="fas fa-edit mr-2"></i>Editable (Admin Mode)' : 
                                '<i class="fas fa-lock mr-2"></i>Read-only'}
                        </div>
                    </div>
                </div>
            `;
        }

        function MemberList() {
            const filteredMembers = appState.members.filter(member => 
                member.name.toLowerCase().includes(appState.searchTerm.toLowerCase()) ||
                member.room.toLowerCase().includes(appState.searchTerm.toLowerCase()) ||
                member.phone.includes(appState.searchTerm)
            );
            
            if (filteredMembers.length === 0) {
                return `
                    <div class="card p-8 text-center">
                        <i class="fas fa-users text-5xl text-gray-600 mb-4"></i>
                        <h3 class="text-xl font-semibold text-gray-400 mb-2">No members found</h3>
                        <p class="text-gray-500">
                            ${appState.searchTerm ? 'Try a different search term' : 'Add your first member in the Admin Panel'}
                        </p>
                    </div>
                `;
            }
            
            const editable = isAdminEditable();
            
            return `
                <div class="space-y-4">
                    <div class="flex items-center justify-between mb-6">
                        <h2 class="text-xl font-bold text-white flex items-center gap-2">
                            <i class="fas fa-users"></i>
                            Members (${filteredMembers.length})
                            <span class="text-sm font-normal text-gray-400">
                                • Showing monthly meal counts
                            </span>
                        </h2>
                        <div class="relative">
                            <i class="fas fa-search absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400"></i>
                            <input 
                                type="text" 
                                placeholder="Search members..." 
                                value="${appState.searchTerm}"
                                oninput="setSearchTerm(this.value)"
                                class="pl-10 pr-4 py-2 rounded-lg bg-slate-900 border border-slate-700 w-64"
                            >
                        </div>
                    </div>
                    
                    <div class="grid grid-cols-1 lg:grid-cols-2 gap-4">
                        ${filteredMembers.map(member => {
                            const dayData = member.dailyData[appState.selectedDate];
                            const isActive = member.isActive;
                            const monthlyStats = getMonthlyStats(member.id, getCurrentMonth(), getCurrentYear());
                            
                            return `
                                <div class="card p-5 hover:border-slate-600 transition-colors">
                                    <div class="flex items-start justify-between mb-4">
                                        <div class="flex items-center gap-3">
                                            <div class="p-2 rounded-lg ${isActive ? 'bg-emerald-900/30' : 'bg-red-900/30'}">
                                                <i class="fas fa-user ${isActive ? 'text-emerald-400' : 'text-red-400'}"></i>
                                            </div>
                                            <div>
                                                <h3 class="font-bold text-white">${member.name}</h3>
                                                <p class="text-sm text-gray-400">
                                                    Room ${member.room} • ${member.phone}
                                                </p>
                                            </div>
                                        </div>
                                        <div class="flex items-center gap-2">
                                            <span class="meal-badge ${isActive ? 'meal-on' : 'meal-off'}">
                                                ${isActive ? 'Active' : 'Inactive'}
                                            </span>
                                        </div>
                                    </div>
                                    
                                    <!-- Monthly Count Badges -->
                                    <div class="flex flex-wrap gap-2 mb-4">
                                        <span class="counter-badge">
                                            <i class="fas fa-coffee mr-1"></i>B: ${monthlyStats.breakfast}
                                        </span>
                                        <span class="counter-badge">
                                            <i class="fas fa-utensils mr-1"></i>L: ${monthlyStats.lunch}
                                        </span>
                                        <span class="counter-badge">
                                            <i class="fas fa-drumstick-bite mr-1"></i>D: ${monthlyStats.dinner}
                                        </span>
                                        <span class="counter-total">
                                            <i class="fas fa-calculator mr-1"></i>Total: ${monthlyStats.total}
                                        </span>
                                        ${monthlyStats.totalWithGuests > monthlyStats.total ? `
                                            <span class="counter-total" style="background: rgba(245, 158, 11, 0.2); border-color: rgba(245, 158, 11, 0.3); color: #fbbf24;">
                                                <i class="fas fa-user-plus mr-1"></i>+Guests: ${monthlyStats.totalWithGuests}
                                            </span>
                                        ` : ''}
                                    </div>
                                    
                                    <div class="grid grid-cols-3 gap-2 mb-4">
                                        ${['breakfast', 'lunch', 'dinner'].map(meal => {
                                            const isOn = isActive && (!dayData || dayData[meal]);
                                            const guestCount = dayData ? dayData[`guest${meal.charAt(0).toUpperCase() + meal.slice(1)}`] || 0 : 0;
                                            
                                            return `
                                                <div class="p-3 rounded-lg ${isOn ? 'bg-emerald-900/20 border border-emerald-800' : 'bg-red-900/20 border border-red-800'}">
                                                    <div class="flex items-center justify-between mb-2">
                                                        <span class="text-sm text-gray-300 capitalize">${meal}</span>
                                                        <span class="text-xs font-bold px-2 py-1 rounded ${isOn ? 'bg-emerald-900 text-emerald-300' : 'bg-red-900 text-red-300'}">
                                                            ${isOn ? 'ON' : 'OFF'}
                                                        </span>
                                                    </div>
                                                    <div class="flex items-center justify-between">
                                                        <span class="text-xs text-gray-400">Guests:</span>
                                                        <span class="text-sm font-medium ${guestCount > 0 ? 'text-amber-400' : 'text-gray-400'}">
                                                            ${guestCount > 0 ? `+${guestCount}` : 'None'}
                                                        </span>
                                                    </div>
                                                </div>
                                            `;
                                        }).join('')}
                                    </div>
                                    
                                    ${dayData && dayData.note ? `
                                        <div class="p-3 bg-slate-900/50 rounded-lg">
                                            <p class="text-sm text-gray-300">
                                                <span class="font-medium text-gray-400">Note:</span> ${dayData.note}
                                            </p>
                                        </div>
                                    ` : ''}
                                </div>
                            `;
                        }).join('')}
                    </div>
                </div>
            `;
        }

        function MonthlyReportView() {
            const monthlyStats = getAllMembersMonthlyStats(appState.selectedMonth, appState.selectedYear);
            const monthName = new Date(2000, appState.selectedMonth - 1).toLocaleString('default', {month: 'long'});
            
            return `
                <div class="space-y-6">
                    <div class="card p-6">
                        <div class="flex items-center justify-between mb-6">
                            <div>
                                <h3 class="text-xl font-bold text-white mb-2 flex items-center gap-2">
                                    <i class="fas fa-chart-bar text-purple-500"></i>
                                    Monthly Meal Report - ${monthName} ${appState.selectedYear}
                                </h3>
                                <p class="text-gray-400">
                                    Total meals served this month with guest counts
                                </p>
                            </div>
                            <div class="text-right">
                                <div class="text-3xl font-bold text-white">${monthlyStats.totalWithGuests}</div>
                                <div class="text-sm text-gray-400">Total meals served</div>
                            </div>
                        </div>
                        
                        <!-- Monthly Summary -->
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
                            <div class="bg-emerald-900/20 p-4 rounded-lg border border-emerald-800/30">
                                <div class="flex items-center justify-between">
                                    <div>
                                        <div class="text-sm text-emerald-300">Breakfast</div>
                                        <div class="text-2xl font-bold text-white">${monthlyStats.totalBreakfast}</div>
                                    </div>
                                    <div class="text-right">
                                        <div class="text-sm text-gray-400">+${monthlyStats.totalGuestBreakfast} guests</div>
                                        <div class="text-lg font-semibold text-emerald-400">
                                            ${monthlyStats.totalBreakfast + monthlyStats.totalGuestBreakfast} total
                                        </div>
                                    </div>
                                </div>
                            </div>
                            
                            <div class="bg-blue-900/20 p-4 rounded-lg border border-blue-800/30">
                                <div class="flex items-center justify-between">
                                    <div>
                                        <div class="text-sm text-blue-300">Lunch</div>
                                        <div class="text-2xl font-bold text-white">${monthlyStats.totalLunch}</div>
                                    </div>
                                    <div class="text-right">
                                        <div class="text-sm text-gray-400">+${monthlyStats.totalGuestLunch} guests</div>
                                        <div class="text-lg font-semibold text-blue-400">
                                            ${monthlyStats.totalLunch + monthlyStats.totalGuestLunch} total
                                        </div>
                                    </div>
                                </div>
                            </div>
                            
                            <div class="bg-purple-900/20 p-4 rounded-lg border border-purple-800/30">
                                <div class="flex items-center justify-between">
                                    <div>
                                        <div class="text-sm text-purple-300">Dinner</div>
                                        <div class="text-2xl font-bold text-white">${monthlyStats.totalDinner}</div>
                                    </div>
                                    <div class="text-right">
                                        <div class="text-sm text-gray-400">+${monthlyStats.totalGuestDinner} guests</div>
                                        <div class="text-lg font-semibold text-purple-400">
                                            ${monthlyStats.totalDinner + monthlyStats.totalGuestDinner} total
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        
                        <!-- Member-wise Details -->
                        <div class="overflow-x-auto">
                            <table class="w-full">
                                <thead>
                                    <tr class="border-b border-slate-800">
                                        <th class="text-left py-3 px-4 text-gray-400 font-semibold">Member</th>
                                        <th class="text-center py-3 px-4 text-gray-400 font-semibold">Breakfast</th>
                                        <th class="text-center py-3 px-4 text-gray-400 font-semibold">Lunch</th>
                                        <th class="text-center py-3 px-4 text-gray-400 font-semibold">Dinner</th>
                                        <th class="text-center py-3 px-4 text-gray-400 font-semibold">Guests</th>
                                        <th class="text-center py-3 px-4 text-gray-400 font-semibold">Total</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    ${monthlyStats.members.map(member => `
                                        <tr class="border-b border-slate-800/50 hover:bg-slate-800/30">
                                            <td class="py-3 px-4">
                                                <div class="font-medium text-white">${member.name}</div>
                                                <div class="text-sm text-gray-400">Room ${member.room}</div>
                                            </td>
                                            <td class="text-center py-3 px-4">
                                                <div class="text-white font-medium">${member.breakfast}</div>
                                                ${member.guestBreakfast > 0 ? `
                                                    <div class="text-xs text-amber-400">+${member.guestBreakfast}</div>
                                                ` : ''}
                                            </td>
                                            <td class="text-center py-3 px-4">
                                                <div class="text-white font-medium">${member.lunch}</div>
                                                ${member.guestLunch > 0 ? `
                                                    <div class="text-xs text-amber-400">+${member.guestLunch}</div>
                                                ` : ''}
                                            </td>
                                            <td class="text-center py-3 px-4">
                                                <div class="text-white font-medium">${member.dinner}</div>
                                                ${member.guestDinner > 0 ? `
                                                    <div class="text-xs text-amber-400">+${member.guestDinner}</div>
                                                ` : ''}
                                            </td>
                                            <td class="text-center py-3 px-4">
                                                <div class="text-amber-400 font-medium">
                                                    ${member.guestBreakfast + member.guestLunch + member.guestDinner}
                                                </div>
                                            </td>
                                            <td class="text-center py-3 px-4">
                                                <div class="text-emerald-400 font-bold text-lg">
                                                    ${member.totalWithGuests}
                                                </div>
                                            </td>
                                        </tr>
                                    `).join('')}
                                </tbody>
                                <tfoot>
                                    <tr class="bg-slate-900/50">
                                        <td class="py-3 px-4 font-bold text-white">TOTAL</td>
                                        <td class="text-center py-3 px-4 font-bold text-white">
                                            ${monthlyStats.totalBreakfast}
                                            ${monthlyStats.totalGuestBreakfast > 0 ? `
                                                <div class="text-xs text-amber-400">+${monthlyStats.totalGuestBreakfast}</div>
                                            ` : ''}
                                        </td>
                                        <td class="text-center py-3 px-4 font-bold text-white">
                                            ${monthlyStats.totalLunch}
                                            ${monthlyStats.totalGuestLunch > 0 ? `
                                                <div class="text-xs text-amber-400">+${monthlyStats.totalGuestLunch}</div>
                                            ` : ''}
                                        </td>
                                        <td class="text-center py-3 px-4 font-bold text-white">
                                            ${monthlyStats.totalDinner}
                                            ${monthlyStats.totalGuestDinner > 0 ? `
                                                <div class="text-xs text-amber-400">+${monthlyStats.totalGuestDinner}</div>
                                            ` : ''}
                                        </td>
                                        <td class="text-center py-3 px-4 font-bold text-amber-400">
                                            ${monthlyStats.totalGuestBreakfast + monthlyStats.totalGuestLunch + monthlyStats.totalGuestDinner}
                                        </td>
                                        <td class="text-center py-3 px-4 font-bold text-emerald-400 text-xl">
                                            ${monthlyStats.totalWithGuests}
                                        </td>
                                    </tr>
                                </tfoot>
                            </table>
                        </div>
                        
                        <!-- Export Monthly Report -->
                        <div class="mt-6 flex justify-end">
                            <button 
                                onclick="exportMonthlyReport()"
                                class="btn-primary flex items-center gap-2"
                            >
                                <i class="fas fa-file-excel"></i>
                                Export Monthly Report
                            </button>
                        </div>
                    </div>
                </div>
            `;
        }

        function AddMemberForm() {
            return `
                <div class="card p-6 mb-8">
                    <h3 class="text-xl font-bold text-white mb-4 flex items-center gap-2">
                        <i class="fas fa-user-plus text-emerald-500"></i>
                        Add New Member
                    </h3>
                    
                    <form onsubmit="handleAddMember(event)" class="space-y-4">
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-400 mb-2">Full Name *</label>
                                <input 
                                    type="text" 
                                    id="new-member-name"
                                    required
                                    placeholder="Enter full name"
                                    class="w-full"
                                >
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-400 mb-2">Room Number *</label>
                                <input 
                                    type="text" 
                                    id="new-member-room"
                                    required
                                    placeholder="e.g., 101"
                                    class="w-full"
                                >
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-400 mb-2">Phone Number</label>
                                <input 
                                    type="tel" 
                                    id="new-member-phone"
                                    placeholder="e.g., +8801XXXXXXXXX"
                                    class="w-full"
                                >
                            </div>
                        </div>
                        
                        <div class="flex justify-end">
                            <button type="submit" class="btn-primary flex items-center gap-2">
                                <i class="fas fa-plus"></i>
                                Add Member
                            </button>
                        </div>
                    </form>
                </div>
            `;
        }

        function MemberManagement() {
            const editable = isAdminEditable();
            
            return `
                <div class="space-y-6">
                    <div class="card p-6">
                        <div class="flex items-center justify-between mb-6">
                            <h3 class="text-xl font-bold text-white flex items-center gap-2">
                                <i class="fas fa-users-cog text-emerald-500"></i>
                                Manage Members (${appState.members.length})
                                ${editable ? 
                                    '<span class="text-sm font-normal text-emerald-400"><i class="fas fa-user-shield ml-2"></i>Admin Edit Mode</span>' : 
                                    '<span class="text-sm font-normal text-amber-400"><i class="fas fa-clock ml-2"></i>11 PM Rule Active</span>'
                                }
                            </h3>
                            <div class="flex items-center gap-2">
                                <div class="text-sm ${editable ? 'text-emerald-400' : 'text-amber-400'}">
                                    ${editable ? 
                                        '<i class="fas fa-edit mr-2"></i>Can edit anytime' : 
                                        '<i class="fas fa-lock mr-2"></i>Follows 11 PM rule'}
                                </div>
                            </div>
                        </div>
                        
                        ${appState.members.length === 0 ? `
                            <div class="text-center py-8">
                                <i class="fas fa-user-slash text-4xl text-gray-600 mb-4"></i>
                                <p class="text-gray-400">No members added yet. Add your first member above.</p>
                            </div>
                        ` : `
                            <div class="space-y-4">
                                ${appState.members.map(member => {
                                    const dayData = member.dailyData[appState.selectedDate];
                                    const monthlyStats = getMonthlyStats(member.id, getCurrentMonth(), getCurrentYear());
                                    
                                    return `
                                        <div class="bg-slate-900/50 p-4 rounded-lg border border-slate-800">
                                            <div class="flex items-center justify-between mb-4">
                                                <div class="flex items-center gap-3">
                                                    <div class="p-2 rounded-lg ${member.isActive ? 'bg-emerald-900/30' : 'bg-red-900/30'}">
                                                        <i class="fas fa-user ${member.isActive ? 'text-emerald-400' : 'text-red-400'}"></i>
                                                    </div>
                                                    <div>
                                                        <h4 class="font-bold text-white">${member.name}</h4>
                                                        <p class="text-sm text-gray-400">Room ${member.room}</p>
                                                    </div>
                                                    <div class="ml-4 flex flex-wrap gap-1">
                                                        <span class="counter-badge text-xs">
                                                            B:${monthlyStats.breakfast}
                                                        </span>
                                                        <span class="counter-badge text-xs">
                                                            L:${monthlyStats.lunch}
                                                        </span>
                                                        <span class="counter-badge text-xs">
                                                            D:${monthlyStats.dinner}
                                                        </span>
                                                        <span class="counter-total text-xs">
                                                            T:${monthlyStats.total}
                                                        </span>
                                                    </div>
                                                </div>
                                                <div class="flex items-center gap-2">
                                                    <button 
                                                        onclick="toggleMemberStatus('${member.id}')"
                                                        class="px-3 py-1 rounded text-sm font-medium ${member.isActive ? 'bg-red-600/20 hover:bg-red-600/30 text-red-400' : 'bg-emerald-600/20 hover:bg-emerald-600/30 text-emerald-400'}"
                                                    >
                                                        ${member.isActive ? 'Deactivate' : 'Activate'}
                                                    </button>
                                                    <button 
                                                        onclick="deleteMember('${member.id}')"
                                                        class="px-3 py-1 rounded text-sm font-medium bg-red-600/20 hover:bg-red-600/30 text-red-400"
                                                    >
                                                        <i class="fas fa-trash"></i>
                                                    </button>
                                                </div>
                                            </div>
                                            
                                            <div class="grid grid-cols-3 gap-3 mb-4">
                                                ${['breakfast', 'lunch', 'dinner'].map(meal => {
                                                    const isOn = member.isActive && (!dayData || dayData[meal]);
                                                    const guestCount = dayData ? dayData[`guest${meal.charAt(0).toUpperCase() + meal.slice(1)}`] || 0 : 0;
                                                    const monthlyMealCount = monthlyStats[meal];
                                                    
                                                    return `
                                                        <div class="bg-slate-800/50 p-3 rounded-lg">
                                                            <div class="flex items-center justify-between mb-3">
                                                                <div>
                                                                    <span class="text-sm text-gray-300 capitalize flex items-center gap-2">
                                                                        ${meal === 'breakfast' ? '<i class="fas fa-coffee"></i>' : 
                                                                          meal === 'lunch' ? '<i class="fas fa-utensils"></i>' : 
                                                                          '<i class="fas fa-drumstick-bite"></i>'}
                                                                        ${meal}
                                                                    </span>
                                                                    <div class="text-xs text-gray-500 mt-1">
                                                                        Month: ${monthlyMealCount}
                                                                    </div>
                                                                </div>
                                                                <button 
                                                                    onclick="updateMealStatus('${member.id}', '${appState.selectedDate}', '${meal}', ${!isOn})"
                                                                    disabled="${!editable || !member.isActive}"
                                                                    class="px-3 py-1 rounded text-sm font-bold ${isOn ? 'bg-red-900/30 text-red-400 hover:bg-red-900/40' : 'bg-emerald-900/30 text-emerald-400 hover:bg-emerald-900/40'} ${(!editable || !member.isActive) ? 'opacity-50 cursor-not-allowed' : ''}"
                                                                >
                                                                    ${isOn ? 'Turn OFF' : 'Turn ON'}
                                                                </button>
                                                            </div>
                                                            
                                                            <div class="flex items-center justify-between">
                                                                <span class="text-sm text-gray-400">Guest meals:</span>
                                                                <div class="flex items-center gap-2">
                                                                    <button 
                                                                        onclick="updateGuestMeals('${member.id}', '${appState.selectedDate}', '${meal}', -1)"
                                                                        disabled="${!editable || guestCount <= 0}"
                                                                        class="w-8 h-8 flex items-center justify-center rounded bg-slate-700 hover:bg-slate-600 ${!editable || guestCount <= 0 ? 'opacity-50 cursor-not-allowed' : ''}"
                                                                    >
                                                                        <i class="fas fa-minus text-sm"></i>
                                                                    </button>
                                                                    <span class="text-white font-medium min-w-[2rem] text-center">${guestCount}</span>
                                                                    <button 
                                                                        onclick="updateGuestMeals('${member.id}', '${appState.selectedDate}', '${meal}', 1)"
                                                                        disabled="${!editable}"
                                                                        class="w-8 h-8 flex items-center justify-center rounded bg-slate-700 hover:bg-slate-600 ${!editable ? 'opacity-50 cursor-not-allowed' : ''}"
                                                                    >
                                                                        <i class="fas fa-plus text-sm"></i>
                                                                    </button>
                                                                </div>
                                                            </div>
                                                        </div>
                                                    `;
                                                }).join('')}
                                            </div>
                                            
                                            <div>
                                                <textarea 
                                                    placeholder="Add note for this day..."
                                                    onchange="updateNote('${member.id}', '${appState.selectedDate}', this.value)"
                                                    ${!editable ? 'disabled' : ''}
                                                    class="w-full h-20 ${!editable ? 'opacity-50 cursor-not-allowed' : ''}"
                                                >${dayData && dayData.note ? dayData.note : ''}</textarea>
                                            </div>
                                        </div>
                                    `;
                                }).join('')}
                            </div>
                        `}
                    </div>
                </div>
            `;
        }

        function DataManagement() {
            return `
                <div class="space-y-6">
                    <div class="card p-6">
                        <h3 class="text-xl font-bold text-white mb-6 flex items-center gap-2">
                            <i class="fas fa-database text-emerald-500"></i>
                            Data Management
                        </h3>
                        
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <div class="bg-slate-900/50 p-6 rounded-lg border border-slate-800">
                                <div class="flex items-center gap-3 mb-4">
                                    <div class="p-3 rounded-lg bg-blue-900/30">
                                        <i class="fas fa-download text-blue-400 text-xl"></i>
                                    </div>
                                    <div>
                                        <h4 class="font-bold text-white">Export Data</h4>
                                        <p class="text-sm text-gray-400">Download all data as JSON file</p>
                                    </div>
                                </div>
                                <p class="text-sm text-gray-300 mb-4">
                                    Save a backup of all member data, meal records, and monthly counts.
                                </p>
                                <button 
                                    onclick="exportData()"
                                    class="btn-primary w-full flex items-center justify-center gap-2"
                                >
                                    <i class="fas fa-file-export"></i>
                                    Export All Data
                                </button>
                            </div>
                            
                            <div class="bg-slate-900/50 p-6 rounded-lg border border-slate-800">
                                <div class="flex items-center gap-3 mb-4">
                                    <div class="p-3 rounded-lg bg-purple-900/30">
                                        <i class="fas fa-upload text-purple-400 text-xl"></i>
                                    </div>
                                    <div>
                                        <h4 class="font-bold text-white">Import Data</h4>
                                        <p class="text-sm text-gray-400">Restore from backup file</p>
                                    </div>
                                </div>
                                <div class="space-y-4">
                                    <input 
                                        type="file" 
                                        id="import-file"
                                        accept=".json"
                                        class="w-full"
                                        onchange="handleImportFileChange(this)"
                                    >
                                    <button 
                                        onclick="importData()"
                                        id="import-btn"
                                        disabled
                                        class="btn-primary w-full flex items-center justify-center gap-2 opacity-50 cursor-not-allowed"
                                    >
                                        <i class="fas fa-file-import"></i>
                                        Import Data
                                    </button>
                                </div>
                            </div>
                        </div>
                        
                        <div class="mt-8 pt-6 border-t border-slate-800">
                            <div class="flex items-center justify-between">
                                <div>
                                    <h4 class="font-bold text-white mb-2">Danger Zone</h4>
                                    <p class="text-sm text-gray-400">
                                        Clear all data from your browser. This cannot be undone!
                                    </p>
                                </div>
                                <button 
                                    onclick="clearAllData()"
                                    class="px-6 py-3 rounded-lg bg-red-600 hover:bg-red-700 text-white font-semibold transition-colors flex items-center gap-2"
                                >
                                    <i class="fas fa-trash"></i>
                                    Clear All Data
                                </button>
                            </div>
                        </div>
                        
                        <div class="mt-6 p-4 bg-slate-900/30 rounded-lg">
                            <h4 class="font-semibold text-gray-300 mb-2">Current Statistics</h4>
                            <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
                                <div>
                                    <p class="text-sm text-gray-400">Total Members</p>
                                    <p class="text-2xl font-bold text-white">${appState.members.length}</p>
                                </div>
                                <div>
                                    <p class="text-sm text-gray-400">Active Members</p>
                                    <p class="text-2xl font-bold text-emerald-400">${appState.members.filter(m => m.isActive).length}</p>
                                </div>
                                <div>
                                    <p class="text-sm text-gray-400">This Month's Meals</p>
                                    <p class="text-2xl font-bold text-purple-400">
                                        ${getAllMembersMonthlyStats(getCurrentMonth(), getCurrentYear()).totalWithGuests}
                                    </p>
                                </div>
                                <div>
                                    <p class="text-sm text-gray-400">Last Updated</p>
                                    <p class="text-sm text-white">
                                        ${storageData.lastUpdated ? new Date(storageData.lastUpdated).toLocaleString() : 'Never'}
                                    </p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            `;
        }

        function DashboardView() {
            return `
                ${DateSelector()}
                ${AdminSettings()}
                ${StatsCard()}
                ${MemberList()}
            `;
        }

        function AdminView() {
            return `
                ${DateSelector()}
                ${AdminSettings()}
                ${AddMemberForm()}
                ${MemberManagement()}
            `;
        }

        function MonthlyView() {
            return `
                ${DateSelector()}
                ${MonthlyReportView()}
            `;
        }

        function DataView() {
            return DataManagement();
        }

        function Footer() {
            return `
                <footer class="mt-12 pt-6 border-t border-slate-800">
                    <div class="flex flex-col md:flex-row md:items-center justify-between gap-4">
                        <div>
                            <div class="text-xs font-bold text-slate-500 uppercase tracking-wider">
                                TASMIA COTTAGE MEAL MANAGEMENT v4.0
                            </div>
                            <p class="text-xs text-slate-600 mt-1">
                                Admin always edit • Monthly meal counting • Auto-save
                            </p>
                        </div>
                        <div class="text-xs text-slate-600 flex items-center gap-4">
                            <span>© ${new Date().getFullYear()}</span>
                            <span>•</span>
                            <span>Current Month: ${getAllMembersMonthlyStats(getCurrentMonth(), getCurrentYear()).totalWithGuests} meals</span>
                        </div>
                    </div>
                </footer>
            `;
        }

        // ======================= EVENT HANDLERS =======================
        function switchTab(tab) {
            appState.activeTab = tab;
            appState.viewMode = tab === 'monthly' ? 'monthly' : 'daily';
            renderApp();
        }

        function setDate(date) {
            appState.selectedDate = date;
            appState.viewMode = 'daily';
            renderApp();
        }

        function setMonth(month) {
            appState.selectedMonth = parseInt(month);
            renderApp();
        }

        function setYear(year) {
            appState.selectedYear = parseInt(year);
            renderApp();
        }

        function setViewMode(mode) {
            appState.viewMode = mode;
            renderApp();
        }

        function setSearchTerm(term) {
            appState.searchTerm = term;
            renderApp();
        }

        function handleAddMember(event) {
            event.preventDefault();
            const name = document.getElementById('new-member-name').value;
            const room = document.getElementById('new-member-room').value;
            const phone = document.getElementById('new-member-phone').value;
            
            if (name && room) {
                addMember(name, room, phone);
                document.getElementById('new-member-name').value = '';
                document.getElementById('new-member-room').value = '';
                document.getElementById('new-member-phone').value = '';
            }
        }

        function handleManualSave() {
            StorageManager.saveData(storageData);
            showToast('Data saved successfully', 'success');
        }

        function exportData() {
            StorageManager.exportData(storageData);
        }

        function exportMonthlyReport() {
            const monthlyStats = getAllMembersMonthlyStats(appState.selectedMonth, appState.selectedYear);
            const monthName = new Date(2000, appState.selectedMonth - 1).toLocaleString('default', {month: 'long'});
            
            // Create CSV content
            let csv = `Tasmia Cottage Monthly Meal Report - ${monthName} ${appState.selectedYear}\n\n`;
            csv += 'Member,Room,Breakfast,Lunch,Dinner,Guests,Total\n';
            
            monthlyStats.members.forEach(member => {
                const totalGuests = member.guestBreakfast + member.guestLunch + member.guestDinner;
                csv += `"${member.name}",${member.room},${member.breakfast},${member.lunch},${member.dinner},${totalGuests},${member.totalWithGuests}\n`;
            });
            
            csv += `\nTOTAL,,${monthlyStats.totalBreakfast},${monthlyStats.totalLunch},${monthlyStats.totalDinner},`;
            csv += `${monthlyStats.totalGuestBreakfast + monthlyStats.totalGuestLunch + monthlyStats.totalGuestDinner},`;
            csv += `${monthlyStats.totalWithGuests}\n`;
            
            csv += `\nBreakfast with guests: ${monthlyStats.totalBreakfast + monthlyStats.totalGuestBreakfast}\n`;
            csv += `Lunch with guests: ${monthlyStats.totalLunch + monthlyStats.totalGuestLunch}\n`;
            csv += `Dinner with guests: ${monthlyStats.totalDinner + monthlyStats.totalGuestDinner}\n`;
            csv += `Total meals served: ${monthlyStats.totalWithGuests}\n`;
            csv += `Generated on: ${new Date().toLocaleString()}\n`;
            
            // Download CSV
            const blob = new Blob([csv], { type: 'text/csv' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `tasmia-monthly-report-${monthName}-${appState.selectedYear}.csv`;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
            
            showToast('Monthly report exported as CSV', 'success');
        }

        function handleImportFileChange(input) {
            const importBtn = document.getElementById('import-btn');
            if (input.files.length > 0) {
                importBtn.disabled = false;
                importBtn.classList.remove('opacity-50', 'cursor-not-allowed');
            } else {
                importBtn.disabled = true;
                importBtn.classList.add('opacity-50', 'cursor-not-allowed');
            }
        }

        async function importData() {
            const fileInput = document.getElementById('import-file');
            if (!fileInput.files.length) {
                showToast('Please select a file first', 'error');
                return;
            }
            
            if (!confirm('Importing will replace all current data. Continue?')) {
                return;
            }
            
            try {
                const importedData = await StorageManager.importData(fileInput.files[0]);
                storageData.members = importedData.members || [];
                storageData.settings = importedData.settings || { adminCanAlwaysEdit: true };
                StorageManager.saveData(storageData);
                appState.members = storageData.members;
                fileInput.value = '';
                handleImportFileChange(fileInput);
                renderApp();
            } catch (error) {
                console.error('Import failed:', error);
            }
        }

        function clearAllData() {
            if (confirm('Are you sure? This will delete ALL data permanently!')) {
                StorageManager.clearData();
                storageData = StorageManager.loadData();
                appState.members = [];
                renderApp();
            }
        }

        // ======================= RENDER APP =======================
        function renderApp() {
            // Load fresh data
            storageData = StorageManager.loadData();
            appState.members = storageData.members;
            
            const app = document.getElementById('app');
            
            let mainContent;
            switch (appState.activeTab) {
                case 'admin':
                    mainContent = AdminView();
                    break;
                case 'monthly':
                    mainContent = MonthlyView();
                    break;
                case 'data':
                    mainContent = DataView();
                    break;
                default:
                    mainContent = DashboardView();
            }
            
            app.innerHTML = `
                <div class="max-w-7xl mx-auto px-4 py-6">
                    ${Header()}
                    ${Tabs()}
                    ${mainContent}
                    ${Footer()}
                </div>
            `;
        }

        // ======================= AUTO-SAVE =======================
        function setupAutoSave() {
            // Auto-save every 30 seconds
            setInterval(() => {
                if (storageData.members.length > 0) {
                    StorageManager.saveData(storageData);
                }
            }, 30000);
            
            // Save before page unload
            window.addEventListener('beforeunload', () => {
                if (storageData.members.length > 0) {
                    StorageManager.saveData(storageData);
                }
            });
            
            // Initial save if no data exists
            if (storageData.members.length === 0) {
                // Add some sample data for first-time users
                setTimeout(() => {
                    if (confirm('Welcome! Would you like to add some sample members to get started?')) {
                        addMember('John Doe', '101', '+8801712345678');
                        addMember('Jane Smith', '102', '+8801812345678');
                        addMember('Bob Johnson', '103', '+8801912345678');
                        showToast('Sample members added. Monthly meal counting is active.', 'info');
                    }
                }, 1000);
            }
        }

        // ======================= INITIALIZE =======================
        function init() {
            // Load initial data
            storageData = StorageManager.loadData();
            appState.members = storageData.members;
            
            // Setup auto-save
            setupAutoSave();
            
            // Initial render
            renderApp();
            
            // Show welcome message for first-time users
            if (storageData.members.length === 0 && !localStorage.getItem('welcome_shown_v4')) {
                setTimeout(() => {
                    showToast('Welcome! Admin can always edit meals. Monthly meal counting is enabled.', 'info', 5000);
                    localStorage.setItem('welcome_shown_v4', 'true');
                }, 1000);
            }
        }

        // Start the app
        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>
