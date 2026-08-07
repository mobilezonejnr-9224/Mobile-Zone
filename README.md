<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Universal Mobile Finder Pro</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <style>
        body { background-color: #0f172a; color: #f8fafc; font-family: system-ui, -apple-system, sans-serif; }
        .tab-active { background-color: #f59e0b !important; color: #000 !important; font-weight: bold; }
    </style>
</head>
<body class="min-h-screen p-4 sm:p-8">
    <div class="max-w-4xl mx-auto">
        <header class="text-center mb-6">
            <h1 class="text-2xl sm:text-4xl font-extrabold text-amber-400 mb-2">📱 Model Finder Pro</h1>
            <p class="text-slate-400 text-sm">यूनिवर्सल डिस्प्ले और बैटरी सर्च करें</p>
        </header>

        <!-- Search Bar -->
        <div class="mb-6">
            <input type="text" id="searchInput" placeholder="मॉडल नाम सर्च करें (जैसे: Y20, C55, A53)..." 
                class="w-full px-4 py-3 rounded-xl bg-slate-800 border border-slate-700 text-white placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-amber-400"
                onkeyup="updateView()">
        </div>

        <!-- Tabs -->
        <div class="flex gap-2 mb-6 overflow-x-auto pb-2" id="tabContainer">
            <button onclick="setTab('All')" id="tab-All" class="tab-btn tab-active px-4 py-2 rounded-lg bg-slate-800 text-sm whitespace-nowrap">All Items</button>
            <button onclick="setTab('Display')" id="tab-Display" class="tab-btn px-4 py-2 rounded-lg bg-slate-800 text-sm whitespace-nowrap">Display / Combo</button>
            <button onclick="setTab('Battery')" id="tab-Battery" class="tab-btn px-4 py-2 rounded-lg bg-slate-800 text-sm whitespace-nowrap">Batteries</button>
        </div>

        <!-- Results Container -->
        <div id="resultsContainer" class="grid grid-cols-1 gap-4"></div>
    </div>

    <script>
        const database = [
            { category: "Display", title: "Realme 8i New Universal Combo", models: ["Realme 8i", "Realme 9i", "Oppo A96", "Narzo 50", "Oppo K10", "Realme 9 Pro 5G", "Realme 9 Pro", "1+ Nord CE 2 Lite", "Oppo A76", "Oppo A36", "Oppo A53", "Oppo A53s", "Oppo A53 (2020)", "Oppo A54 4G", "Oppo A55 4G", "Oppo A11s", "Narzo 20", "Realme 7i", "Oppo A33", "Oppo A54", "Oppo A32", "Realme C17", "OnePlus Nord N100"] },
            { category: "Display", title: "Realme 13+ 5G Universal Combo Model", models: ["Oppo K12x", "1+ nord ce4 lite 5G", "Oppo F27 5G", "Oppo Reno 12F", "Oppo Reno 12FS", "Oppo Reno 12FS 5G", "Oppo Reno 12F 5G", "Realme 12", "Realme 13+5G", "Narzo 70 Turbo", "Realme 13 Pro", "Realme 13 4G", "Oppo Reno 13F", "Oppo Reno 13F 5G"] },
            { category: "Display", title: "Oppo F19 Pro Screen Display", models: ["Oppo Reno 8 Lite 5G", "Oppo Reno 7 Lite", "Oppo Reno 7Z", "Oppo Reno 8Z", "Oppo Reno 6 Lite", "Oppo Reno 5 Lite", "Oppo Reno 5F", "Oppo Reno 5Z", "Oppo Reno 6Z", "Oppo Reno 4 SE 5G", "Oppo A74 4G", "Oppo A94 4G", "Oppo A94 5G", "Oppo A95 4G", "Oppo A95 5G", "Oppo A96 5G", "Oppo F19", "Oppo F19 Pro", "Oppo F19S", "Oppo F21 Pro 5G", "OnePlus Nord N20 5G", "Realme 8 4G", "Realme 8 Pro", "Realme 7 Pro", "Realme X7", "Realme X7 5G", "Realme Q2 5G", "Realme V15"] },
            { category: "Display", title: "Realme C55 New Update / All-in-One Universal Combo", models: ["Realme C55", "Realme N55", "Realme C67", "Realme 11 5G", "Realme 11x 5G", "Narzo 60x", "Oppo A58 4G", "Oppo A98 5G", "Oppo A79 5G", "Oppo F23 5G", "1+ Nord N30", "1+ Nord CE3 Lite", "Realme 12 5G", "Realme 12x 5G", "Realme 13 5G", "Narzo 70x 5G", "Narzo 80x 5G", "Realme C67 5G", "Realme C75 4G", "Realme PX3 5G", "Realme C65 4G", "Realme N65 4G", "Realme C63 5G", "Realme C73 5G", "Realme C75 5G", "Realme C75x 4G", "Realme 14x 5G", "Narzo 30 Lite", "Realme P3 Lite", "Oppo A3", "Oppo A3x", "Oppo A3 Pro", "Oppo A5", "Oppo A5x", "Oppo A5 Pro", "Oppo A60", "Oppo A40", "Oppo A80", "Oppo K12x 5G", "Oppo K13x 5G"] },
            { category: "Display", title: "Vivo Y20 Universal Display", models: ["Vivo Y21", "Vivo Y21s", "Vivo Y21a", "Vivo Y21e", "Vivo Y21t", "Vivo Y21g", "Vivo Y02s", "Vivo Y16", "Vivo Y30 5G", "Vivo Y15a", "Vivo Y15c", "Vivo Y32", "Vivo Y33 5G", "Vivo Y20", "Vivo Y20g", "Vivo Y12g", "Vivo Y01", "Vivo Y22", "Vivo Y17s", "Vivo Y22s", "Vivo Y22 New", "Vivo Y28 5G", "Vivo Y33t", "Vivo Y36i", "Vivo Y36", "Vivo Y12 New", "Vivo T1x", "Vivo Y33s", "Vivo Y51 2020", "Vivo Y31 2020", "Vivo Y73", "Vivo Y72s", "Vivo Y53s", "iQOO Z3", "iQOO U3", "iQOO U3x", "Vivo Y31", "Vivo T2x", "Vivo Y72 5G", "Vivo Y55s 5G", "Vivo Y76 5G", "Vivo Y76s 5G", "Vivo Y75 5G", "Vivo Y77 5G", "Vivo Y56 5G", "Vivo Y20a", "Vivo Y20t", "Vivo Y20s", "Vivo Y20i", "Vivo Y12s", "Vivo Y12a", "Vivo Y20e", "Vivo Y30g", "Vivo Y31s", "Vivo Y11s", "Vivo Y12d", "Vivo Y15s", "iQOO U1"] },
            { category: "Display", title: "Vivo Y03 Universal Display", models: ["Vivo Y03", "Vivo Y18", "Vivo T3 Lite 5G", "Vivo Y28s 5G", "Vivo Y18e", "Vivo Y18i", "Vivo Y18s", "Vivo Z9 Lite 5G", "Vivo Y28e 5G", "Vivo Y03t"] },
            { category: "Display", title: "Vivo Y39 5G Universal Display", models: ["Vivo Y38 5G", "Vivo Y20 4G", "Vivo Y29 4G", "Vivo Y40 5G", "Vivo Y55 5G", "Vivo Y19S", "Vivo Y19S Pro", "Vivo Y15C", "Vivo Y58 5G", "Vivo T3X 5G", "iQOO Z9X", "iQOO Z10K"] },
            { category: "Battery", title: "Battery BLPA83", models: ["Realme C61", "Realme C63", "Realme C63 5G", "Realme 10 Pro"] },
            { category: "Battery", title: "Battery BLPA77", models: ["Oppo K12x 5G", "Oppo A3x", "Oppo A3x 5G", "Oppo A3 Pro", "Oppo A4 5G", "Oppo A40", "Oppo A80 5G", "Oppo A40m"] },
            { category: "Battery", title: "Battery BLP19 / BLP21", models: ["Oppo A38", "Oppo A58 4G", "Oppo A59 5G", "Oppo A79 5G", "Oppo A18", "Oppo A2x", "Oppo A2m"] },
            { category: "Battery", title: "Battery BLP851", models: ["Oppo A54", "Oppo A74 5G", "Oppo A94 5G", "Oppo A95", "Oppo F19", "Oppo F19s"] },
            { category: "Battery", title: "Battery BLP877", models: ["Realme C30", "Realme C30s", "Realme C31", "Realme C33", "Realme C35", "Narzo 50", "Narzo 50 Prime", "Realme 8i", "Realme V20", "Realme V30"] },
            { category: "Battery", title: "Battery BLP673", models: ["Oppo A3s", "Oppo A5", "Oppo A5s", "Oppo A7", "Oppo A7n", "Oppo A8", "Oppo A11k", "Oppo A12", "Oppo A12s", "Oppo A31", "Realme 2", "Realme C1"] },
            { category: "Battery", title: "Battery BLP923", models: ["Oppo A57", "Oppo A77", "Oppo A78 5G", "Oppo A77 5G", "Oppo K10 5G", "Oppo A97", "Oppo A56 5G", "Realme C51", "OnePlus Nord N20 SE", "OnePlus Nord N300"] },
            { category: "Battery", title: "Battery BLP799", models: ["Realme 7 Pro", "Realme X7 5G", "Realme V15", "Realme Q2 Pro", "Realme X7 Pro", "Realme X3 Pro", "Narzo 20 Pro"] },
            { category: "Battery", title: "Battery BLP727 / BLP729", models: ["Oppo A5 2020", "Oppo A9 2020", "Oppo A11", "Oppo A11x", "Realme 5", "Realme 5i", "Realme 5s", "Realme 6i", "Realme C3", "Realme C11", "Realme C11 2021", "Realme C20", "Realme C20A", "Realme C25Y", "Realme Narzo 10A", "Realme Narzo 20A", "Realme Narzo 50i", "Realme V3", "Realme 10s 5G"] },
            { category: "Battery", title: "Battery BLP803", models: ["Realme 8 5G", "Realme 8s 5G", "Narzo 30 5G", "Realme V13 5G", "Realme 03 5G", "Realme 7i", "Oppo A54", "Oppo A16s", "Oppo A53", "Oppo A73", "Oppo A32", "Oppo A33", "Oppo A53s", "Realme C17", "Oppo A16"] }
        ];

        let currentTab = 'All';

        function setTab(tab) {
            currentTab = tab;
            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('tab-active'));
            document.getElementById('tab-' + tab).classList.add('tab-active');
            updateView();
        }

        function updateView() {
            const query = document.getElementById("searchInput").value.toLowerCase();
            const container = document.getElementById("resultsContainer");
            container.innerHTML = "";

            const filtered = database.filter(item => {
                const matchesTab = (currentTab === 'All' || item.category === currentTab);
                const matchesQuery = (item.title.toLowerCase().includes(query) || item.models.some(m => m.toLowerCase().includes(query)));
                return matchesTab && matchesQuery;
            });

            filtered.forEach(item => {
                const card = document.createElement("div");
                card.className = "bg-slate-800 border border-slate-700 rounded-xl p-5 hover:border-amber-400";
                card.innerHTML = `
                    <h2 class="text-amber-300 font-bold mb-2">${item.title}</h2>
                    <div class="flex flex-wrap gap-1">${item.models.map(m => `<span class="bg-slate-700 text-slate-200 text-xs px-2 py-1 rounded-md">${m}</span>`).join("")}</div>
                `;
                container.appendChild(card);
            });
        }

        updateView();
    </script>
</body>
</html>
