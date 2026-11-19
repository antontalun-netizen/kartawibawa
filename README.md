<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Inventarisasi Gugus Eyang Kartawibawa</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 min-h-screen flex flex-col">

    <!-- MODAL NOTIFIKASI CUSTOM (Menggantikan alert/confirm) -->
    <div id="customModal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/50 backdrop-blur-sm hidden">
        <div class="bg-white rounded-2xl p-6 max-w-md w-full mx-4 shadow-2xl transform transition-all">
            <div id="modalIconContainer" class="w-12 h-12 rounded-full flex items-center justify-center mb-4 mx-auto text-xl"></div>
            <h3 id="modalTitle" class="text-lg font-bold text-center text-slate-900 mb-2"></h3>
            <p id="modalMessage" class="text-sm text-slate-600 text-center mb-6"></p>
            <div id="modalActionButtons" class="flex justify-center gap-3">
                <button id="modalBtnOk" class="px-5 py-2.5 bg-indigo-600 text-white rounded-xl font-medium hover:bg-indigo-700 transition shadow-sm">OK</button>
            </div>
        </div>
    </div>

    <!-- HALAMAN LOGIN (Custom Password Access) -->
    <div id="loginPage" class="fixed inset-0 z-40 bg-gradient-to-br from-slate-900 via-indigo-950 to-slate-900 flex items-center justify-center p-4">
        <div class="bg-white/10 backdrop-blur-md border border-white/20 rounded-3xl p-8 max-w-md w-full shadow-2xl text-white">
            <div class="text-center mb-8">
                <div class="w-16 h-16 bg-indigo-600 rounded-2xl mx-auto flex items-center justify-center shadow-lg shadow-indigo-500/30 mb-4 text-2xl font-bold">
                    <i class="fa-solid fa-graduation-cap"></i>
                </div>
                <h1 class="text-2xl font-bold tracking-tight">Gugus Eyang Kartawibawa</h1>
                <p class="text-xs text-indigo-200 mt-1">Sistem Inventarisasi Administrasi & Dokumentasi Kegiatan</p>
            </div>
            
            <form id="loginForm" onsubmit="handleLogin(event)" class="space-y-4">
                <div>
                    <label class="block text-xs font-semibold uppercase tracking-wider text-indigo-200 mb-1">Username / ID Akses</label>
                    <div class="relative">
                        <span class="absolute inset-y-0 left-0 pl-3.5 flex items-center text-slate-400">
                            <i class="fa-solid fa-user"></i>
                        </span>
                        <input type="text" id="loginUsername" required placeholder="Contoh: admin / k3s / guru" class="w-full pl-10 pr-4 py-3 bg-white/5 border border-white/10 rounded-xl text-white placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-sm">
                    </div>
                </div>
                <div>
                    <label class="block text-xs font-semibold uppercase tracking-wider text-indigo-200 mb-1">Password Custom</label>
                    <div class="relative">
                        <span class="absolute inset-y-0 left-0 pl-3.5 flex items-center text-slate-400">
                            <i class="fa-solid fa-lock"></i>
                        </span>
                        <input type="password" id="loginPassword" required placeholder="Masukkan password..." class="w-full pl-10 pr-4 py-3 bg-white/5 border border-white/10 rounded-xl text-white placeholder-slate-400 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-sm">
                    </div>
                </div>
                <button type="submit" id="btnLoginSubmit" class="w-full py-3.5 bg-indigo-600 hover:bg-indigo-500 rounded-xl font-semibold transition shadow-lg shadow-indigo-600/30 flex items-center justify-center gap-2">
                    <span>Masuk Sistem</span>
                    <i class="fa-solid fa-arrow-right-to-bracket"></i>
                </button>
            </form>

            <div class="mt-6 pt-6 border-t border-white/10 text-center text-xs text-slate-400">
                <p>Akun Demo Bawaan: <span class="text-indigo-300 font-mono">admin / karta2026</span></p>
            </div>
        </div>
    </div>

    <!-- APLIKASI UTAMA (DASHBOARD & DATA) -->
    <div id="appContainer" class="hidden flex-1 flex flex-col">
        <!-- Top Navbar -->
        <header class="bg-white border-b border-slate-200 sticky top-0 z-30 shadow-xs">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 bg-indigo-600 rounded-xl flex items-center justify-center text-white font-bold shadow-sm">
                        <i class="fa-solid fa-school"></i>
                    </div>
                    <div>
                        <h2 class="text-base font-bold text-slate-900 leading-tight">Gugus Eyang Kartawibawa</h2>
                        <p class="text-xs text-slate-500">Inventaris & Dokumentasi Terpusat</p>
                    </div>
                </div>
                
                <div class="flex items-center gap-3">
                    <div class="hidden sm:flex flex-col text-right">
                        <span id="navUserName" class="text-xs font-semibold text-slate-800">Administrator</span>
                        <span id="navUserRole" class="text-[10px] text-indigo-600 font-medium">Admin</span>
                    </div>
                    <button onclick="logout()" class="p-2.5 rounded-xl bg-slate-100 hover:bg-rose-50 hover:text-rose-600 text-slate-600 transition" title="Keluar">
                        <i class="fa-solid fa-power-off"></i>
                    </button>
                </div>
            </div>
        </header>

        <!-- Main Content Layout -->
        <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 space-y-6">
            
            <!-- Navigation Tabs -->
            <div class="flex flex-wrap gap-2 border-b border-slate-200 pb-3">
                <button onclick="switchTab('dashboard')" id="tabBtnDashboard" class="px-4 py-2 rounded-xl text-sm font-semibold transition bg-indigo-600 text-white shadow-sm flex items-center gap-2">
                    <i class="fa-solid fa-chart-pie"></i> Dashboard
                </button>
                <button onclick="switchTab('kegiatan')" id="tabBtnKegiatan" class="px-4 py-2 rounded-xl text-sm font-semibold transition bg-white text-slate-600 hover:bg-slate-100 flex items-center gap-2 border border-slate-200">
                    <i class="fa-solid fa-folder-open"></i> Inventaris Kegiatan
                </button>
                <button onclick="switchTab('petunjuk')" id="tabBtnPetunjuk" class="px-4 py-2 rounded-xl text-sm font-semibold transition bg-white text-slate-600 hover:bg-slate-100 flex items-center gap-2 border border-slate-200">
                    <i class="fa-solid fa-circle-question"></i> Petunjuk Penggunaan
                </button>
            </div>

            <!-- TAB 1: DASHBOARD -->
            <div id="tabContentDashboard" class="space-y-6">
                <!-- Stat Cards -->
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                    <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-xs flex items-center justify-between">
                        <div>
                            <p class="text-xs font-medium text-slate-500 uppercase">Total Kegiatan</p>
                            <h4 id="statTotal" class="text-2xl font-bold text-slate-900 mt-1">0</h4>
                        </div>
                        <div class="w-12 h-12 rounded-2xl bg-indigo-50 text-indigo-600 flex items-center justify-center text-xl">
                            <i class="fa-solid fa-calendar-days"></i>
                        </div>
                    </div>
                    <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-xs flex items-center justify-between">
                        <div>
                            <p class="text-xs font-medium text-slate-500 uppercase">Kelompok K3S</p>
                            <h4 id="statK3S" class="text-2xl font-bold text-slate-900 mt-1">0</h4>
                        </div>
                        <div class="w-12 h-12 rounded-2xl bg-emerald-50 text-emerald-600 flex items-center justify-center text-xl">
                            <i class="fa-solid fa-users-gear"></i>
                        </div>
                    </div>
                    <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-xs flex items-center justify-between">
                        <div>
                            <p class="text-xs font-medium text-slate-500 uppercase">Kelas 1 s.d 6</p>
                            <h4 id="statKelas" class="text-2xl font-bold text-slate-900 mt-1">0</h4>
                        </div>
                        <div class="w-12 h-12 rounded-2xl bg-amber-50 text-amber-600 flex items-center justify-center text-xl">
                            <i class="fa-solid fa-child-reaching"></i>
                        </div>
                    </div>
                    <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-xs flex items-center justify-between">
                        <div>
                            <p class="text-xs font-medium text-slate-500 uppercase">PAI & PJOK</p>
                            <h4 id="statMapel" class="text-2xl font-bold text-slate-900 mt-1">0</h4>
                        </div>
                        <div class="w-12 h-12 rounded-2xl bg-purple-50 text-purple-600 flex items-center justify-center text-xl">
                            <i class="fa-solid fa-medal"></i>
                        </div>
                    </div>
                </div>

                <!-- Quick Activity Distribution -->
                <div class="bg-white rounded-2xl border border-slate-200 p-6 shadow-xs">
                    <h3 class="text-base font-bold text-slate-900 mb-4 flex items-center gap-2">
                        <i class="fa-solid fa-layer-group text-indigo-600"></i> Rekapitulasi Berdasarkan Kelompok
                    </h3>
                    <div id="groupStatsContainer" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-9 gap-3">
                        <!-- Dynamic Group Cards -->
                    </div>
                </div>

                <!-- Recent Activities -->
                <div class="bg-white rounded-2xl border border-slate-200 p-6 shadow-xs">
                    <h3 class="text-base font-bold text-slate-900 mb-4 flex items-center gap-2">
                        <i class="fa-solid fa-clock-rotate-left text-indigo-600"></i> Aktivitas Terbaru
                    </h3>
                    <div id="recentActivitiesList" class="divide-y divide-slate-100">
                        <p class="text-sm text-slate-500 py-4 text-center">Memuat data...</p>
                    </div>
                </div>
            </div>

            <!-- TAB 2: INVENTARIS KEGIATAN -->
            <div id="tabContentKegiatan" class="hidden space-y-6">
                <!-- Action Controls Header -->
                <div class="flex flex-col sm:flex-row justify-between items-stretch sm:items-center gap-4 bg-white p-4 rounded-2xl border border-slate-200 shadow-xs">
                    <div class="flex flex-wrap items-center gap-3 flex-1">
                        <!-- Search Bar -->
                        <div class="relative flex-1 min-w-[220px]">
                            <span class="absolute inset-y-0 left-0 pl-3.5 flex items-center text-slate-400">
                                <i class="fa-solid fa-search"></i>
                            </span>
                            <input type="text" id="searchInput" oninput="filterData()" placeholder="Cari lokasi, jenis kegiatan..." class="w-full pl-10 pr-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                        </div>
                        <!-- Filter Kelompok -->
                        <select id="filterKelompok" onchange="filterData()" class="px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                            <option value="">Semua Kelompok</option>
                            <option value="Kelompok kerja K3S">Kelompok kerja K3S</option>
                            <option value="KELAS 1">KELAS 1</option>
                            <option value="KELAS 2">KELAS 2</option>
                            <option value="KELAS 3">KELAS 3</option>
                            <option value="KELAS 4">KELAS 4</option>
                            <option value="KELAS 5">KELAS 5</option>
                            <option value="KELAS 6">KELAS 6</option>
                            <option value="PAI">PAI</option>
                            <option value="PJOK">PJOK</option>
                        </select>
                    </div>
                    <div class="flex items-center gap-2">
                        <button onclick="fetchData()" class="px-4 py-2.5 bg-slate-100 hover:bg-slate-200 text-slate-700 rounded-xl text-sm font-semibold transition flex items-center gap-2" title="Refresh Data">
                            <i class="fa-solid fa-rotate"></i>
                            <span class="hidden sm:inline">Refresh</span>
                        </button>
                        <button onclick="openAddModal()" class="px-5 py-2.5 bg-indigo-600 hover:bg-indigo-700 text-white rounded-xl text-sm font-semibold transition shadow-sm flex items-center gap-2">
                            <i class="fa-solid fa-plus"></i>
                            <span>Tambah Kegiatan</span>
                        </button>
                    </div>
                </div>

                <!-- Table Data Kegiatan -->
                <div class="bg-white rounded-2xl border border-slate-200 shadow-xs overflow-hidden">
                    <div class="overflow-x-auto">
                        <table class="w-full text-left border-collapse">
                            <thead>
                                <tr class="bg-slate-50 border-b border-slate-200 text-slate-600 text-xs font-bold uppercase tracking-wider">
                                    <th class="py-3.5 px-4">Kelompok & Kegiatan</th>
                                    <th class="py-3.5 px-4">Tanggal & Lokasi</th>
                                    <th class="py-3.5 px-4">Administrasi</th>
                                    <th class="py-3.5 px-4">Dokumentasi</th>
                                    <th class="py-3.5 px-4 text-center">Aksi</th>
                                </tr>
                            </thead>
                            <tbody id="kegiatanTableBody" class="divide-y divide-slate-100 text-sm">
                                <tr>
                                    <td colspan="5" class="py-8 text-center text-slate-500">Memuat data dari Google Spreadsheet...</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- TAB 3: PETUNJUK PENGGUNAAN -->
            <div id="tabContentPetunjuk" class="hidden space-y-6">
                <div class="bg-white rounded-2xl border border-slate-200 p-6 sm:p-8 shadow-xs space-y-6">
                    <div>
                        <div class="inline-block p-3 bg-indigo-50 text-indigo-600 rounded-2xl text-xl mb-3">
                            <i class="fa-solid fa-book-bookmark"></i>
                        </div>
                        <h2 class="text-xl font-bold text-slate-900">Petunjuk Penggunaan Aplikasi</h2>
                        <p class="text-sm text-slate-500 mt-1">Panduan lengkap tata cara pengelolaan inventarisasi administrasi dan dokumentasi Gugus Eyang Kartawibawa.</p>
                    </div>

                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6 pt-4 border-t border-slate-100">
                        <div class="space-y-3">
                            <div class="flex items-start gap-3">
                                <div class="w-7 h-7 rounded-full bg-indigo-600 text-white flex items-center justify-center font-bold text-xs shrink-0 mt-0.5">1</div>
                                <div>
                                    <h4 class="text-sm font-bold text-slate-900">Login & Hak Akses</h4>
                                    <p class="text-xs text-slate-600 mt-0.5">Masuk menggunakan username dan password custom yang telah terdaftar pada database Google Apps Script (Sheet Users).</p>
                                </div>
                            </div>
                            <div class="flex items-start gap-3">
                                <div class="w-7 h-7 rounded-full bg-indigo-600 text-white flex items-center justify-center font-bold text-xs shrink-0 mt-0.5">2</div>
                                <div>
                                    <h4 class="text-sm font-bold text-slate-900">Inventarisasi Kelompok</h4>
                                    <p class="text-xs text-slate-600 mt-0.5">Pilih kategori kelompok kegiatan secara spesifik: K3S, Kelas 1 s.d 6, PAI, atau PJOK untuk memudahkan pengelompokan arsip.</p>
                                </div>
                            </div>
                        </div>
                        <div class="space-y-3">
                            <div class="flex items-start gap-3">
                                <div class="w-7 h-7 rounded-full bg-indigo-600 text-white flex items-center justify-center font-bold text-xs shrink-0 mt-0.5">3</div>
                                <div>
                                    <h4 class="text-sm font-bold text-slate-900">Tautan Administrasi & Dokumentasi</h4>
                                    <p class="text-xs text-slate-600 mt-0.5">Lampirkan tautan Google Drive (Spreadsheet, Doc, PDF, Folder Foto/Video) pada form input agar mudah diakses oleh pengurus gugus.</p>
                                </div>
                            </div>
                            <div class="flex items-start gap-3">
                                <div class="w-7 h-7 rounded-full bg-indigo-600 text-white flex items-center justify-center font-bold text-xs shrink-0 mt-0.5">4</div>
                                <div>
                                    <h4 class="text-sm font-bold text-slate-900">Sinkronisasi Database</h4>
                                    <p class="text-xs text-slate-600 mt-0.5">Seluruh data tersimpan secara otomatis dan terpusat di Google Spreadsheet yang terhubung melalui Google Apps Script Web App.</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

        </main>
    </div>

    <!-- MODAL TAMBAH / EDIT KEGIATAN -->
    <div id="kegiatanModal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/50 backdrop-blur-sm hidden p-4">
        <div class="bg-white rounded-2xl max-w-lg w-full max-h-[90vh] overflow-y-auto shadow-2xl">
            <div class="p-6 border-b border-slate-100 flex items-center justify-between sticky top-0 bg-white z-10">
                <h3 id="modalKegiatanTitle" class="text-lg font-bold text-slate-900">Tambah Kegiatan Baru</h3>
                <button onclick="closeKegiatanModal()" class="w-8 h-8 rounded-full bg-slate-100 text-slate-500 hover:bg-slate-200 flex items-center justify-center transition">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>
            
            <form id="kegiatanForm" onsubmit="saveKegiatan(event)" class="p-6 space-y-4">
                <input type="hidden" id="editKegiatanId">
                
                <div>
                    <label class="block text-xs font-semibold uppercase tracking-wider text-slate-600 mb-1">Nama Kelompok</label>
                    <select id="inputKelompok" required class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                        <option value="">-- Pilih Kelompok Kegiatan --</option>
                        <option value="Kelompok kerja K3S">Kelompok kerja K3S</option>
                        <option value="KELAS 1">KELAS 1</option>
                        <option value="KELAS 2">KELAS 2</option>
                        <option value="KELAS 3">KELAS 3</option>
                        <option value="KELAS 4">KELAS 4</option>
                        <option value="KELAS 5">KELAS 5</option>
                        <option value="KELAS 6">KELAS 6</option>
                        <option value="PAI">PAI</option>
                        <option value="PJOK">PJOK</option>
                    </select>
                </div>

                <div>
                    <label class="block text-xs font-semibold uppercase tracking-wider text-slate-600 mb-1">Jenis Kegiatan</label>
                    <input type="text" id="inputJenis" required placeholder="Contoh: Rapat Koordinasi Kurikulum / Workshop KKG" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-semibold uppercase tracking-wider text-slate-600 mb-1">Tanggal Kegiatan</label>
                        <input type="date" id="inputTanggal" required class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold uppercase tracking-wider text-slate-600 mb-1">Lokasi Kegiatan</label>
                        <input type="text" id="inputLokasi" required placeholder="Contoh: SDN 1 Eyang Kartawibawa" class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-semibold uppercase tracking-wider text-slate-600 mb-1">Tautan Administrasi (Google Drive / Doc / Spreadsheet)</label>
                    <input type="url" id="inputAdminUrl" placeholder="https://docs.google.com/..." class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                    <input type="text" id="inputAdminKet" placeholder="Keterangan singkat administrasi (opsional)" class="w-full mt-2 px-4 py-2 bg-slate-50 border border-slate-200 rounded-xl text-xs focus:outline-none focus:ring-2 focus:ring-indigo-500">
                </div>

                <div>
                    <label class="block text-xs font-semibold uppercase tracking-wider text-slate-600 mb-1">Tautan Dokumentasi (Foto / Video / Folder Drive)</label>
                    <input type="url" id="inputDokUrl" placeholder="https://drive.google.com/drive/folders/..." class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                    <input type="text" id="inputDokKet" placeholder="Keterangan singkat dokumentasi (opsional)" class="w-full mt-2 px-4 py-2 bg-slate-50 border border-slate-200 rounded-xl text-xs focus:outline-none focus:ring-2 focus:ring-indigo-500">
                </div>

                <div class="pt-4 flex items-center justify-end gap-3 border-t border-slate-100">
                    <button type="button" onclick="closeKegiatanModal()" class="px-5 py-2.5 bg-slate-100 hover:bg-slate-200 text-slate-700 rounded-xl text-sm font-semibold transition">Batal</button>
                    <button type="submit" id="btnSaveKegiatan" class="px-5 py-2.5 bg-indigo-600 hover:bg-indigo-700 text-white rounded-xl text-sm font-semibold transition shadow-sm">Simpan Data</button>
                </div>
            </form>
        </div>
    </div>

    <!-- JavaScript Client Logic -->
    <script>
        // CONFIG: Masukkan URL Web App Google Apps Script Anda setelah di-deploy
        // Contoh: const API_URL = "https://script.google.com/macros/s/AKfycbz.../exec";
        // Jika dijalankan secara mandiri/lokal dengan mock data, gunakan empty string untuk simulasi lokal.
        const API_URL = ""; 

        let globalData = [];
        let currentUser = null;

        // Inisialisasi saat load
        window.addEventListener("DOMContentLoaded", () => {
            const savedUser = localStorage.getItem("gugus_user");
            if (savedUser) {
                currentUser = JSON.parse(savedUser);
                showApp();
            }
        });

        // Handler Login Custom Password
        async function handleLogin(e) {
            e.preventDefault();
            const username = document.getElementById("loginUsername").value.trim();
            const password = document.getElementById("loginPassword").value.trim();
            const btn = document.getElementById("btnLoginSubmit");
            
            btn.innerHTML = `<i class="fa-solid fa-spinner animate-spin"></i> Memverifikasi...`;
            btn.disabled = true;

            if (API_URL) {
                try {
                    const response = await fetch(API_URL, {
                        method: "POST",
                        body: JSON.stringify({ action: "login", username, password })
                    });
                    const res = await response.json();
                    if (res.status === "success") {
                        currentUser = res.user;
                        localStorage.setItem("gugus_user", JSON.stringify(currentUser));
                        showApp();
                    } else {
                        showModal("Login Gagal", res.message || "Username atau password salah.", "error");
                    }
                } catch (err) {
                    showModal("Kesalahan Koneksi", "Gagal terhubung ke Google Apps Script backend.", "error");
                }
            } else {
                // Mock Authentication untuk Demo/Testing langsung
                if ((username === "admin" && password === "karta2026") || 
                    (username === "k3s" && password === "k3s123") || 
                    (username === "guru" && password === "guru123")) {
                    currentUser = {
                        username: username,
                        role: username === "admin" ? "Admin" : "Operator",
                        namaLengkap: username === "admin" ? "Administrator Gugus" : "Pengguna Gugus"
                    };
                    localStorage.setItem("gugus_user", JSON.stringify(currentUser));
                    showApp();
                } else {
                    // Fallback universal untuk kemudahan pengujian user
                    currentUser = { username: username, role: "Operator", namaLengkap: username };
                    localStorage.setItem("gugus_user", JSON.stringify(currentUser));
                    showApp();
                }
            }

            btn.innerHTML = `<span>Masuk Sistem</span><i class="fa-solid fa-arrow-right-to-bracket"></i>`;
            btn.disabled = false;
        }

        function logout() {
            localStorage.removeItem("gugus_user");
            currentUser = null;
            document.getElementById("appContainer").classList.add("hidden");
            document.getElementById("loginPage").classList.remove("hidden");
        }

        function showApp() {
            document.getElementById("loginPage").classList.add("hidden");
            document.getElementById("appContainer").classList.remove("hidden");
            document.getElementById("navUserName").innerText = currentUser.namaLengkap;
            document.getElementById("navUserRole").innerText = currentUser.role;
            fetchData();
        }

        // Navigasi Tab
        function switchTab(tabName) {
            ["dashboard", "kegiatan", "petunjuk"].forEach(t => {
                document.getElementById(`tabContent${t.charAt(0).toUpperCase() + t.slice(1)}`).classList.add("hidden");
                document.getElementById(`tabBtn${t.charAt(0).toUpperCase() + t.slice(1)}`).className = "px-4 py-2 rounded-xl text-sm font-semibold transition bg-white text-slate-600 hover:bg-slate-100 flex items-center gap-2 border border-slate-200";
            });

            document.getElementById(`tabContent${tabName.charAt(0).toUpperCase() + tabName.slice(1)}`).classList.remove("hidden");
            document.getElementById(`tabBtn${tabName.charAt(0).toUpperCase() + tabName.slice(1)}`).className = "px-4 py-2 rounded-xl text-sm font-semibold transition bg-indigo-600 text-white shadow-sm flex items-center gap-2";
        }

        // Mengambil Data dari Backend atau Mock Storage
        async function fetchData() {
            if (API_URL) {
                try {
                    const res = await fetch(`${API_URL}?action=getData`);
                    const data = await res.json();
                    if (data.status === "success") {
                        globalData = data.kegiatan;
                        renderDashboard();
                        renderTable(globalData);
                    }
                } catch (err) {
                    console.error("Gagal memuat data", err);
                }
            } else {
                // Mock Data untuk Local Simulation
                const local = localStorage.getItem("gugus_kegiatan_mock");
                if (local) {
                    globalData = JSON.parse(local);
                } else {
                    globalData = [
                        {
                            id: "KEG-1",
                            kelompok: "Kelompok kerja K3S",
                            jenisKegiatan: "Rapat Koordinasi Kepala Sekolah Gugus",
                            tanggalKegiatan: "2026-03-10",
                            lokasiKegiatan: "SDN 1 Eyang Kartawibawa",
                            administrasiUrl: "https://docs.google.com",
                            keteranganAdmin: "Notulen & Daftar Hadir",
                            dokumentasiUrl: "https://drive.google.com",
                            keteranganDok: "Foto Kegiatan Rapat",
                            createdAt: "2026-03-10T08:00:00.000Z"
                        },
                        {
                            id: "KEG-2",
                            kelompok: "KELAS 1",
                            jenisKegiatan: "Workshop Pembelajaran Tematik Kelas 1",
                            tanggalKegiatan: "2026-03-12",
                            lokasiKegiatan: "SDN 2 Eyang Kartawibawa",
                            administrasiUrl: "https://docs.google.com",
                            keteranganAdmin: "Modul Ajar Kurikulum Merdeka",
                            dokumentasiUrl: "https://drive.google.com",
                            keteranganDok: "Dokumentasi Praktik Baik",
                            createdAt: "2026-03-12T09:30:00.000Z"
                        },
                        {
                            id: "KEG-3",
                            kelompok: "PJOK",
                            jenisKegiatan: "Festival Olahraga Antar Siswa Gugus",
                            tanggalKegiatan: "2026-03-15",
                            lokasiKegiatan: "Lapangan Utama Gugus",
                            administrasiUrl: "https://docs.google.com",
                            keteranganAdmin: "Jadwal & SK Panitia",
                            dokumentasiUrl: "https://drive.google.com",
                            keteranganDok: "Album Pertandingan",
                            createdAt: "2026-03-15T07:00:00.000Z"
                        }
                    ];
                    localStorage.setItem("gugus_kegiatan_mock", JSON.stringify(globalData));
                }
                renderDashboard();
                renderTable(globalData);
            }
        }

        // Render Dashboard Statistics & Group Cards
        function renderDashboard() {
            document.getElementById("statTotal").innerText = globalData.length;

            let countK3S = 0;
            let countKelas = 0;
            let countMapel = 0;

            const groupCounts = {
                "Kelompok kerja K3S": 0,
                "KELAS 1": 0,
                "KELAS 2": 0,
                "KELAS 3": 0,
                "KELAS 4": 0,
                "KELAS 5": 0,
                "KELAS 6": 0,
                "PAI": 0,
                "PJOK": 0
            };

            globalData.forEach(item => {
                if (groupCounts[item.kelompok] !== undefined) {
                    groupCounts[item.kelompok]++;
                }
                if (item.kelompok === "Kelompok kerja K3S") countK3S++;
                else if (item.kelompok.startsWith("KELAS")) countKelas++;
                else if (item.kelompok === "PAI" || item.kelompok === "PJOK") countMapel++;
            });

            document.getElementById("statK3S").innerText = countK3S;
            document.getElementById("statKelas").innerText = countKelas;
            document.getElementById("statMapel").innerText = countMapel;

            // Render Group Breakdown Cards
            const groupContainer = document.getElementById("groupStatsContainer");
            groupContainer.innerHTML = "";
            Object.keys(groupCounts).forEach(group => {
                groupContainer.innerHTML += `
                    <div class="bg-slate-50 border border-slate-200 rounded-xl p-3 text-center">
                        <span class="text-[11px] font-semibold text-slate-500 uppercase block truncate">${group}</span>
                        <span class="text-lg font-bold text-indigo-600 mt-1 block">${groupCounts[group]}</span>
                    </div>
                `;
            });

            // Render Recent Activities
            const recentContainer = document.getElementById("recentActivitiesList");
            recentContainer.innerHTML = "";
            const sorted = [...globalData].sort((a, b) => new Date(b.createdAt || 0) - new Date(a.createdAt || 0)).slice(0, 5);
            
            if (sorted.length === 0) {
                recentContainer.innerHTML = `<p class="text-sm text-slate-500 py-4 text-center">Belum ada aktivitas tercatat.</p>`;
                return;
            }

            sorted.forEach(item => {
                recentContainer.innerHTML += `
                    <div class="py-3 flex items-center justify-between">
                        <div class="flex items-start gap-3">
                            <div class="w-10 h-10 rounded-xl bg-indigo-50 text-indigo-600 flex items-center justify-center font-bold text-sm shrink-0">
                                <i class="fa-solid fa-file-lines"></i>
                            </div>
                            <div>
                                <h4 class="text-sm font-semibold text-slate-900">${item.jenisKegiatan}</h4>
                                <p class="text-xs text-slate-500">${item.kelompok} • <i class="fa-solid fa-location-dot ml-1"></i> ${item.lokasiKegiatan}</p>
                            </div>
                        </div>
                        <span class="text-xs font-medium text-slate-400 bg-slate-100 px-2.5 py-1 rounded-lg">${item.tanggalKegiatan}</span>
                    </div>
                `;
            });
        }

        // Render Table Kegiatan
        function renderTable(data) {
            const tbody = document.getElementById("kegiatanTableBody");
            tbody.innerHTML = "";

            if (data.length === 0) {
                tbody.innerHTML = `<tr><td colspan="5" class="py-8 text-center text-slate-500">Tidak ada data kegiatan ditemukan.</td></tr>`;
                return;
            }

            data.forEach(item => {
                tbody.innerHTML += `
                    <tr class="hover:bg-slate-50/80 transition border-b border-slate-100">
                        <td class="py-4 px-4">
                            <span class="inline-block px-2.5 py-0.5 rounded-md text-[10px] font-bold bg-indigo-50 text-indigo-700 uppercase mb-1">${item.kelompok}</span>
                            <div class="font-bold text-slate-900 text-sm">${item.jenisKegiatan}</div>
                        </td>
                        <td class="py-4 px-4">
                            <div class="text-slate-800 text-xs font-semibold"><i class="fa-regular fa-calendar mr-1 text-slate-400"></i> ${item.tanggalKegiatan}</div>
                            <div class="text-slate-500 text-xs mt-0.5"><i class="fa-solid fa-location-dot mr-1 text-slate-400"></i> ${item.lokasiKegiatan}</div>
                        </td>
                        <td class="py-4 px-4">
                            ${item.administrasiUrl ? `<a href="${item.administrasiUrl}" target="_blank" class="inline-flex items-center gap-1.5 px-3 py-1.5 rounded-lg bg-emerald-50 text-emerald-700 hover:bg-emerald-100 text-xs font-semibold transition"><i class="fa-solid fa-link"></i> ${item.keteranganAdmin || 'Buka Arsip'}</a>` : '<span class="text-xs text-slate-400 italic">Tidak ada</span>'}
                        </td>
                        <td class="py-4 px-4">
                            ${item.dokumentasiUrl ? `<a href="${item.dokumentasiUrl}" target="_blank" class="inline-flex items-center gap-1.5 px-3 py-1.5 rounded-lg bg-blue-50 text-blue-700 hover:bg-blue-100 text-xs font-semibold transition"><i class="fa-solid fa-images"></i> ${item.keteranganDok || 'Buka Foto/Video'}</a>` : '<span class="text-xs text-slate-400 italic">Tidak ada</span>'}
                        </td>
                        <td class="py-4 px-4 text-center">
                            <div class="flex items-center justify-center gap-1.5">
                                <button onclick="openEditModal('${item.id}')" class="p-2 rounded-lg bg-slate-100 text-slate-600 hover:bg-indigo-50 hover:text-indigo-600 transition" title="Edit">
                                    <i class="fa-solid fa-pen-to-square text-xs"></i>
                                </button>
                                <button onclick="deleteKegiatanItem('${item.id}')" class="p-2 rounded-lg bg-slate-100 text-slate-600 hover:bg-rose-50 hover:text-rose-600 transition" title="Hapus">
                                    <i class="fa-solid fa-trash text-xs"></i>
                                </button>
                            </div>
                        </td>
                    </tr>
                `;
            });
        }

        // Filter Data Kegiatan
        function filterData() {
            const keyword = document.getElementById("searchInput").value.toLowerCase();
            const groupFilter = document.getElementById("filterKelompok").value;

            const filtered = globalData.filter(item => {
                const matchKeyword = item.jenisKegiatan.toLowerCase().includes(keyword) || item.lokasiKegiatan.toLowerCase().includes(keyword);
                const matchGroup = groupFilter === "" || item.kelompok === groupFilter;
                return matchKeyword && matchGroup;
            });

            renderTable(filtered);
        }

        // Modal Kegiatan Handler
        function openAddModal() {
            document.getElementById("modalKegiatanTitle").innerText = "Tambah Kegiatan Baru";
            document.getElementById("kegiatanForm").reset();
            document.getElementById("editKegiatanId").value = "";
            document.getElementById("kegiatanModal").classList.remove("hidden");
        }

        function openEditModal(id) {
            const item = globalData.find(d => d.id === id);
            if (!item) return;

            document.getElementById("modalKegiatanTitle").innerText = "Edit Data Kegiatan";
            document.getElementById("editKegiatanId").value = item.id;
            document.getElementById("inputKelompok").value = item.kelompok;
            document.getElementById("inputJenis").value = item.jenisKegiatan;
            document.getElementById("inputTanggal").value = item.tanggalKegiatan;
            document.getElementById("inputLokasi").value = item.lokasiKegiatan;
            document.getElementById("inputAdminUrl").value = item.administrasiUrl || "";
            document.getElementById("inputAdminKet").value = item.keteranganAdmin || "";
            document.getElementById("inputDokUrl").value = item.dokumentasiUrl || "";
            document.getElementById("inputDokKet").value = item.keteranganDok || "";

            document.getElementById("kegiatanModal").classList.remove("hidden");
        }

        function closeKegiatanModal() {
            document.getElementById("kegiatanModal").classList.add("hidden");
        }

        // Save Kegiatan (Add / Update)
        async function saveKegiatan(e) {
            e.preventDefault();
            const id = document.getElementById("editKegiatanId").value;
            const payload = {
                id: id || ("KEG-" + new Date().getTime()),
                kelompok: document.getElementById("inputKelompok").value,
                jenisKegiatan: document.getElementById("inputJenis").value,
                tanggalKegiatan: document.getElementById("inputTanggal").value,
                lokasiKegiatan: document.getElementById("inputLokasi").value,
                administrasiUrl: document.getElementById("inputAdminUrl").value,
                keteranganAdmin: document.getElementById("inputAdminKet").value,
                dokumentasiUrl: document.getElementById("inputDokUrl").value,
                keteranganDok: document.getElementById("inputDokKet").value,
                updatedBy: currentUser.namaLengkap,
                createdAt: new Date().toISOString()
            };

            const btn = document.getElementById("btnSaveKegiatan");
            btn.innerHTML = `<i class="fa-solid fa-spinner animate-spin"></i> Menyimpan...`;
            btn.disabled = true;

            if (API_URL) {
                try {
                    const actionType = id ? "updateKegiatan" : "addKegiatan";
                    const response = await fetch(API_URL, {
                        method: "POST",
                        body: JSON.stringify({ action: actionType, payload })
                    });
                    const res = await response.json();
                    if (res.status === "success") {
                        closeKegiatanModal();
                        fetchData();
                        showModal("Berhasil", res.message, "success");
                    } else {
                        showModal("Gagal", res.message, "error");
                    }
                } catch (err) {
                    showModal("Kesalahan", "Gagal menyimpan data ke server.", "error");
                }
            } else {
                // Mock Storage Handling
                if (id) {
                    const idx = globalData.findIndex(d => d.id === id);
                    if (idx !== -1) {
                        payload.createdAt = globalData[idx].createdAt; // keep original
                        globalData[idx] = payload;
                    }
                } else {
                    globalData.push(payload);
                }
                localStorage.setItem("gugus_kegiatan_mock", JSON.stringify(globalData));
                closeKegiatanModal();
                renderDashboard();
                renderTable(globalData);
                showModal("Berhasil", "Data kegiatan berhasil disimpan secara lokal.", "success");
            }

            btn.innerHTML = `Simpan Data`;
            btn.disabled = false;
        }

        // Delete Kegiatan
        async function deleteKegiatanItem(id) {
            // Konfirmasi custom modal sederhana
            if (confirm("Apakah Anda yakin ingin menghapus data kegiatan ini?")) {
                if (API_URL) {
                    try {
                        const response = await fetch(API_URL, {
                            method: "POST",
                            body: JSON.stringify({ action: "deleteKegiatan", id })
                        });
                        const res = await response.json();
                        if (res.status === "success") {
                            fetchData();
                            showModal("Berhasil", res.message, "success");
                        } else {
                            showModal("Gagal", res.message, "error");
                        }
                    } catch (err) {
                        showModal("Kesalahan", "Gagal menghapus data.", "error");
                    }
                } else {
                    globalData = globalData.filter(d => d.id !== id);
                    localStorage.setItem("gugus_kegiatan_mock", JSON.stringify(globalData));
                    renderDashboard();
                    renderTable(globalData);
                    showModal("Berhasil", "Data kegiatan berhasil dihapus.", "success");
                }
            }
        }

        // Custom Modal UI Utility
        function showModal(title, message, type = "success") {
            const modal = document.getElementById("customModal");
            const iconContainer = document.getElementById("modalIconContainer");
            
            document.getElementById("modalTitle").innerText = title;
            document.getElementById("modalMessage").innerText = message;

            if (type === "success") {
                iconContainer.className = "w-12 h-12 rounded-full bg-emerald-50 text-emerald-600 flex items-center justify-center mb-4 mx-auto text-xl";
                iconContainer.innerHTML = `<i class="fa-solid fa-check"></i>`;
            } else {
                iconContainer.className = "w-12 h-12 rounded-full bg-rose-50 text-rose-600 flex items-center justify-center mb-4 mx-auto text-xl";
                iconContainer.innerHTML = `<i class="fa-solid fa-triangle-exclamation"></i>`;
            }

            modal.classList.remove("hidden");
            document.getElementById("modalBtnOk").onclick = () => modal.classList.add("hidden");
        }
    </script>
</body>
</html>
