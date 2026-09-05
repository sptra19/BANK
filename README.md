<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title> Aris & Fifi 💖</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>

    <style>
        body { 
            font-family: 'Plus Jakarta Sans', sans-serif; 
            background-color: #fdf2f8; 
            -webkit-tap-highlight-color: transparent;
        }
        .glass-card { 
            background: #ffffff; 
            border-radius: 24px; 
            box-shadow: 0 10px 40px -10px rgba(225, 29, 72, 0.1); 
            border: 1px solid #ffe4e6; 
        }
        .pink-gradient { 
            background: linear-gradient(135deg, #e11d48 0%, #fb7185 100%); 
        }
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: #fda4af; border-radius: 10px; }
        .swal2-html-container { margin: 1em 0 0 0 !important; }
        
        /* Custom Tab */
        .tab-btn { transition: all 0.3s ease; }
        .tab-active { background: #e11d48; color: white; box-shadow: 0 4px 12px rgba(225, 29, 72, 0.3); }
        .tab-inactive { background: #fff1f2; color: #fb7185; }
    </style>
</head>
<body class="text-slate-800 antialiased min-h-screen flex flex-col">

    <!-- LOGIN SCREEN -->
    <div id="loginScreen" class="fixed inset-0 z-50 pink-gradient flex flex-col items-center justify-center p-6">
        <div class="bg-white w-full max-w-sm rounded-[32px] p-8 text-center shadow-2xl">
            <div class="w-20 h-20 bg-rose-50 rounded-full flex items-center justify-center mx-auto mb-6">
                <i class="fas fa-lock text-4xl text-rose-500"></i>
            </div>
            <h1 class="text-2xl font-extrabold text-slate-800 mb-2">Akses Brankas</h1>
            <p class="text-sm text-slate-500 mb-8 font-medium">Masukkan PIN Keamanan</p>
            
            <input type="password" id="pinInput" placeholder="••••••" class="w-full text-center text-3xl tracking-[0.5em] px-4 py-4 rounded-2xl bg-slate-50 border-2 border-transparent focus:border-rose-400 focus:bg-white outline-none mb-6 font-bold text-rose-600 transition-all">
            
            <button onclick="window.cekPin()" class="w-full pink-gradient text-white font-bold py-4 rounded-2xl shadow-lg hover:shadow-xl active:scale-95 transition-all text-lg">
                Masuk <i class="fas fa-arrow-right ml-2"></i>
            </button>
        </div>
    </div>

    <!-- MAIN DASHBOARD -->
    <div id="mainApp" class="hidden flex-1 flex flex-col relative h-screen overflow-hidden">
        
        <!-- Navbar -->
        <nav class="bg-white px-6 py-4 flex justify-between items-center border-b border-rose-100 z-10 shadow-sm flex-shrink-0">
            <div class="flex items-center gap-4">
                <div class="w-12 h-12 pink-gradient rounded-xl flex items-center justify-center text-white shadow-md font-bold text-xl">AF</div>
                <div>
                    <h1 class="font-extrabold text-xl text-slate-800 tracking-tight">Aris & Fifi</h1>
                    <p id="statusKoneksi" class="text-xs font-bold text-amber-500 mt-0.5"><i class="fas fa-circle-notch fa-spin"></i> Menghubungkan...</p>
                </div>
            </div>
            <div class="flex gap-2">
                <button onclick="window.exportExcel()" class="bg-emerald-50 text-emerald-600 hover:bg-emerald-100 p-2 md:px-4 rounded-xl text-sm font-bold transition-all flex items-center gap-2"><i class="fas fa-file-excel"></i> <span class="hidden md:inline">Unduh Rekap</span></button>
                <button onclick="window.logout()" class="bg-slate-50 text-slate-500 hover:bg-slate-100 p-2 md:px-4 rounded-xl text-sm font-bold transition-all"><i class="fas fa-sign-out-alt"></i></button>
            </div>
        </nav>

        <!-- Scrollable Content -->
        <div class="flex-1 overflow-y-auto p-4 md:p-8">
            <div class="max-w-7xl mx-auto space-y-6">
                
                <!-- SUMMARY WIDGETS -->
                <div class="grid grid-cols-1 md:grid-cols-12 gap-6">
                    
                    <!-- Total Balance -->
                    <div class="md:col-span-8 pink-gradient rounded-[28px] p-8 text-white shadow-xl relative overflow-hidden flex flex-col justify-center min-h-[200px]">
                        <i class="fas fa-wallet absolute -right-6 -bottom-6 text-9xl opacity-10 transform -rotate-12"></i>
                        <p class="text-rose-100 font-semibold mb-2 tracking-wide uppercase text-sm">Total Tabungan Terkumpul</p>
                        <h2 id="totalSaldo" class="text-4xl md:text-6xl font-extrabold truncate drop-shadow-md mb-6">Rp 0</h2>
                        
                        <div>
                            <div class="flex justify-between text-sm font-bold text-rose-50 mb-2">
                                <span>Target: Rp 10 Juta</span>
                                <span id="persenTarget">0%</span>
                            </div>
                            <div class="w-full bg-black/20 rounded-full h-3 mb-2">
                                <div id="barTarget" class="bg-white h-3 rounded-full transition-all duration-1000 shadow-sm" style="width: 0%"></div>
                            </div>
                            <p id="sisaTarget" class="text-xs text-rose-100 font-medium"></p>
                        </div>
                    </div>

                    <!-- Mini Stats -->
                    <div class="md:col-span-4 grid grid-cols-2 md:grid-cols-1 gap-4">
                        <div class="bg-white rounded-[24px] p-6 border border-rose-50 shadow-sm flex flex-col justify-center">
                            <div class="w-10 h-10 rounded-full bg-emerald-50 text-emerald-500 flex items-center justify-center mb-3"><i class="fas fa-arrow-down"></i></div>
                            <p class="text-slate-400 text-xs font-bold uppercase tracking-wider mb-1">Masuk Bulan Ini</p>
                            <h2 id="masukBulanIni" class="text-xl md:text-2xl font-extrabold text-emerald-500 truncate">Rp 0</h2>
                        </div>
                        <div class="bg-white rounded-[24px] p-6 border border-rose-50 shadow-sm flex flex-col justify-center">
                            <div class="w-10 h-10 rounded-full bg-rose-50 text-rose-500 flex items-center justify-center mb-3"><i class="fas fa-arrow-up"></i></div>
                            <p class="text-slate-400 text-xs font-bold uppercase tracking-wider mb-1">Ditarik Bulan Ini</p>
                            <h2 id="keluarBulanIni" class="text-xl md:text-2xl font-extrabold text-rose-500 truncate">Rp 0</h2>
                        </div>
                    </div>
                </div>

                <!-- MAIN WORKSPACE -->
                <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                    
                    <!-- FORM COLUMN -->
                    <div class="lg:col-span-5 glass-card p-6 md:p-8">
                        <div class="flex p-1.5 bg-rose-50 rounded-xl mb-6">
                            <button id="tabNabung" onclick="window.switchTab('nabung')" class="flex-1 py-3 text-sm font-bold rounded-lg tab-active tab-btn">Nabung (+)</button>
                            <button id="tabTarik" onclick="window.switchTab('tarik')" class="flex-1 py-3 text-sm font-bold rounded-lg tab-inactive tab-btn">Tarik (-)</button>
                        </div>

                        <form id="formTransaksi" class="space-y-5">
                            <input type="hidden" id="jenisTransaksi" value="nabung">
                            
                            <div class="grid grid-cols-2 gap-4">
                                <div>
                                    <label class="block text-xs font-bold text-slate-500 mb-2">TANGGAL</label>
                                    <input type="date" id="tanggal" required class="w-full px-4 py-3.5 rounded-xl border border-slate-200 bg-slate-50 focus:bg-white focus:border-rose-400 focus:ring-2 focus:ring-rose-100 outline-none text-sm font-bold text-slate-700 transition-all">
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-500 mb-2">OLEH SIAPA?</label>
                                    <select id="pelaku" required class="w-full px-4 py-3.5 rounded-xl border border-slate-200 bg-slate-50 focus:bg-white focus:border-rose-400 focus:ring-2 focus:ring-rose-100 outline-none text-sm font-bold text-slate-700 transition-all cursor-pointer">
                                        <option value="Aris">Aris</option>
                                        <option value="Fifi">Fifi</option>
                                        <option value="Aris & Fifi">Berdua</option>
                                    </select>
                                </div>
                            </div>

                            <div>
                                <label class="block text-xs font-bold text-slate-500 mb-2">KATEGORI</label>
                                <select id="kategori" required class="w-full px-4 py-3.5 rounded-xl border border-slate-200 bg-slate-50 focus:bg-white focus:border-rose-400 focus:ring-2 focus:ring-rose-100 outline-none text-sm font-bold text-slate-700 transition-all cursor-pointer">
                                    <option value="Sisa Uang Jajan">Sisa Uang Jajan</option>
                                    <option value="Gaji / Pendapatan">Gaji / Pendapatan</option>
                                    <option value="Hadiah / Bonus">Hadiah / Bonus</option>
                                    <option value="Lainnya">Lainnya</option>
                                </select>
                            </div>

                            <div>
                                <label class="block text-xs font-bold text-slate-500 mb-2">NOMINAL (RP)</label>
                                <div class="relative mb-3">
                                    <span class="absolute left-4 top-1/2 -translate-y-1/2 text-slate-400 font-extrabold text-lg">Rp</span>
                                    <input type="text" id="nominal" required placeholder="0" class="w-full pl-12 pr-4 py-4 rounded-xl border border-slate-200 bg-slate-50 focus:bg-white focus:border-rose-400 focus:ring-2 focus:ring-rose-100 outline-none text-2xl font-extrabold text-slate-800 transition-all">
                                </div>
                                <div class="grid grid-cols-4 gap-2 mb-1">
                                    <button type="button" onclick="window.tambahNominal(10000)" class="bg-rose-50 text-rose-600 text-xs font-bold py-2.5 rounded-lg hover:bg-rose-100 active:scale-95 transition-all">+10k</button>
                                    <button type="button" onclick="window.tambahNominal(20000)" class="bg-rose-50 text-rose-600 text-xs font-bold py-2.5 rounded-lg hover:bg-rose-100 active:scale-95 transition-all">+20k</button>
                                    <button type="button" onclick="window.tambahNominal(50000)" class="bg-rose-50 text-rose-600 text-xs font-bold py-2.5 rounded-lg hover:bg-rose-100 active:scale-95 transition-all">+50k</button>
                                    <button type="button" onclick="window.tambahNominal(100000)" class="bg-rose-50 text-rose-600 text-xs font-bold py-2.5 rounded-lg hover:bg-rose-100 active:scale-95 transition-all">+100k</button>
                                </div>
                                <p id="teksTerbilang" class="text-xs font-bold text-rose-500 text-right h-4"></p>
                            </div>

                            <div id="areaBank" class="grid grid-cols-2 gap-4">
                                <div>
                                    <label class="block text-xs font-bold text-slate-500 mb-2">SUMBER DANA</label>
                                    <input type="text" id="via" required placeholder="Cth: DANA Fifi" class="w-full px-4 py-3.5 rounded-xl border border-slate-200 bg-slate-50 focus:bg-white focus:border-rose-400 focus:ring-2 focus:ring-rose-100 outline-none text-sm font-bold text-slate-700 transition-all">
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-500 mb-2">MASUK KE</label>
                                    <select id="rekening" required class="w-full px-4 py-3.5 rounded-xl border border-rose-200 bg-rose-50 focus:border-rose-400 focus:ring-2 focus:ring-rose-100 outline-none text-xs font-extrabold text-rose-600 transition-all cursor-pointer">
                                        <option value="SeaBank 901061919099">SeaBank 901061919099</option>
                                        <option value="Celengan Tunai">Celengan Tunai</option>
                                    </select>
                                </div>
                            </div>

                            <div>
                                <label class="block text-xs font-bold text-slate-500 mb-2">KETERANGAN</label>
                                <input type="text" id="catatan" placeholder="Tulis rincian di sini..." class="w-full px-4 py-3.5 rounded-xl border border-slate-200 bg-slate-50 focus:bg-white focus:border-rose-400 focus:ring-2 focus:ring-rose-100 outline-none text-sm font-bold text-slate-700 transition-all">
                            </div>

                            <button type="submit" id="btnSubmit" class="w-full pink-gradient text-white font-extrabold py-4 rounded-xl mt-4 shadow-lg shadow-rose-200 hover:shadow-xl active:scale-95 transition-all disabled:opacity-50 flex items-center justify-center gap-2 text-lg">
                                <i class="fas fa-save"></i> Simpan Tabungan
                            </button>
                        </form>
                    </div>

                    <!-- HISTORY COLUMN -->
                    <div class="lg:col-span-7 glass-card p-6 md:p-8 flex flex-col min-h-[600px]">
                        
                        <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center mb-6 gap-4">
                            <h3 class="text-xl font-extrabold text-slate-800 flex items-center gap-2">
                                <i class="fas fa-history text-rose-400"></i> Rekam Jejak
                            </h3>
                            <div class="flex gap-2 w-full sm:w-auto">
                                <button onclick="window.tampilkanStatistik()" class="flex-1 sm:flex-none bg-slate-800 hover:bg-slate-700 text-white text-xs font-bold px-4 py-2.5 rounded-xl transition-colors flex items-center justify-center gap-2 shadow-md">
                                    <i class="fas fa-chart-pie"></i> Rekap Bulan
                                </button>
                                <select id="filterRiwayat" onchange="window.updateUI()" class="flex-1 sm:flex-none text-xs bg-rose-50 border border-rose-100 text-rose-600 rounded-xl px-4 py-2.5 font-extrabold outline-none cursor-pointer text-center">
                                    <option value="30">30 Hari Terakhir</option>
                                    <option value="bulan">Bulan Ini</option>
                                    <option value="semua">Semua Waktu</option>
                                </select>
                            </div>
                        </div>
                        
                        <div id="riwayatKosong" class="flex-1 flex flex-col items-center justify-center text-slate-300 hidden">
                            <i class="fas fa-inbox text-6xl mb-4 text-slate-200"></i>
                            <p class="font-bold text-lg text-slate-400">Belum Ada Transaksi</p>
                        </div>

                        <div id="daftarRiwayat" class="space-y-4 overflow-y-auto pr-2 flex-1 pb-4">
                            <!-- Render by Firebase -->
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
        import { getFirestore, collection, addDoc, onSnapshot, query, orderBy } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

        // ==========================================
        // CONFIG FIREBASE
        // ==========================================
        const firebaseConfig = {
            apiKey: "AIzaSyBiaz5nbZxEQtTzSCLaqD66m7aIK9mxSug",
            authDomain: "tabungan-36e48.firebaseapp.com",
            projectId: "tabungan-36e48",
            storageBucket: "tabungan-36e48.firebasestorage.app",
            messagingSenderId: "246286895008",
            appId: "1:246286895008:web:6392b3ac73e265299ec6c5",
            measurementId: "G-0FJ7Z2RJGD"
        };
        // ==========================================

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        
        const PIN_RAHASIA = "140826"; 
        const TARGET_TABUNGAN = 10000000; 
        let dbTransaksi = [];
        
        document.getElementById('tanggal').valueAsDate = new Date();

        const formatRp = (angka) => new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR', minimumFractionDigits: 0 }).format(angka);
        const formatAngkaTitik = (angka) => new Intl.NumberFormat('id-ID').format(angka);
        const formatTgl = (tgl) => new Date(tgl).toLocaleDateString('id-ID', { day: 'numeric', month: 'short', year: 'numeric' });
        
        function terbilang(angka) {
            angka = Math.abs(angka);
            const bil = ["", "Satu", "Dua", "Tiga", "Empat", "Lima", "Enam", "Tujuh", "Delapan", "Sembilan", "Sepuluh", "Sebelas"];
            if (angka < 12) return bil[angka];
            if (angka < 20) return terbilang(angka - 10) + " Belas";
            if (angka < 100) return terbilang(Math.floor(angka / 10)) + " Puluh " + terbilang(angka % 10);
            if (angka < 200) return "Seratus " + terbilang(angka - 100);
            if (angka < 1000) return terbilang(Math.floor(angka / 100)) + " Ratus " + terbilang(angka % 100);
            if (angka < 2000) return "Seribu " + terbilang(angka - 1000);
            if (angka < 1000000) return terbilang(Math.floor(angka / 1000)) + " Ribu " + terbilang(angka % 1000);
            if (angka < 1000000000) return terbilang(Math.floor(angka / 1000000)) + " Juta " + terbilang(angka % 1000000);
            return "";
        }

        const nominalInput = document.getElementById('nominal');
        const teksTerbilang = document.getElementById('teksTerbilang');

        nominalInput.addEventListener('input', function(e) {
            let val = this.value.replace(/[^0-9]/g, '');
            if(val === '') { this.value = ''; teksTerbilang.innerText = ''; return; }
            this.value = formatAngkaTitik(val);
            teksTerbilang.innerText = terbilang(parseInt(val)) + " Rupiah";
        });

        window.tambahNominal = (val) => {
            let currentVal = parseInt(nominalInput.value.replace(/[^0-9]/g, '')) || 0;
            let newVal = currentVal + val;
            nominalInput.value = formatAngkaTitik(newVal);
            teksTerbilang.innerText = terbilang(newVal) + " Rupiah";
        };

        const q = query(collection(db, "tabungan_kita"), orderBy("createdAt", "desc"));
        onSnapshot(q, (snapshot) => {
            dbTransaksi = [];
            snapshot.forEach((doc) => { dbTransaksi.push({ id: doc.id, ...doc.data() }); });
            
            document.getElementById('statusKoneksi').innerHTML = "<i class='fas fa-check-circle'></i> Online";
            document.getElementById('statusKoneksi').className = "text-xs font-bold text-emerald-500 mt-0.5";
            window.updateUI();
        }, (error) => {
            document.getElementById('statusKoneksi').innerHTML = "<i class='fas fa-exclamation-triangle'></i> Offline";
            document.getElementById('statusKoneksi').className = "text-xs font-bold text-rose-500 mt-0.5";
        });

        window.updateUI = () => {
            const list = document.getElementById('daftarRiwayat');
            const filterVal = document.getElementById('filterRiwayat').value;
            const now = new Date();
            list.innerHTML = '';
            
            let total = 0, masukBulan = 0, keluarBulan = 0;

            dbTransaksi.forEach(item => {
                const itemDate = new Date(item.tanggal);
                const isBulanIni = itemDate.getMonth() === now.getMonth() && itemDate.getFullYear() === now.getFullYear();
                
                if(item.jenis === 'nabung') {
                    total += item.nominal;
                    if(isBulanIni) masukBulan += item.nominal;
                } else {
                    total -= item.nominal;
                    if(isBulanIni) keluarBulan += item.nominal;
                }
            });

            let dataDitampilkan = dbTransaksi.filter(item => {
                if(filterVal === 'semua') return true;
                const itemDate = new Date(item.tanggal);
                if(filterVal === 'bulan') {
                    return itemDate.getMonth() === now.getMonth() && itemDate.getFullYear() === now.getFullYear();
                }
                if(filterVal === '30') {
                    const diffTime = Math.abs(now - itemDate);
                    const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24)); 
                    return diffDays <= 30;
                }
                return true;
            });

            if(dataDitampilkan.length === 0) {
                document.getElementById('riwayatKosong').classList.remove('hidden');
            } else {
                document.getElementById('riwayatKosong').classList.add('hidden');
                
                dataDitampilkan.forEach(item => {
                    const isNabung = item.jenis === 'nabung';
                    const icon = isNabung ? 'fa-arrow-down text-emerald-600' : 'fa-arrow-up text-rose-600';
                    const bgIcon = isNabung ? 'bg-emerald-100' : 'bg-rose-100';
                    const sign = isNabung ? '+' : '-';
                    const colorSign = isNabung ? 'text-emerald-500' : 'text-rose-500';
                    
                    const catatanDetail = item.catatan ? `<br><span class="text-slate-500 font-medium">${item.catatan}</span>` : '';
                    const ruteTransfer = (isNabung && item.via && item.rekening) ? `<br><span class="inline-block mt-1.5 bg-slate-100 text-slate-500 px-2 py-1 rounded-md text-[10px] font-bold uppercase border border-slate-200"><i class="fas fa-university"></i> ${item.via} ➔ ${item.rekening}</span>` : '';
                    
                    list.innerHTML += `
                        <div class="bg-white p-5 rounded-2xl border border-slate-100 shadow-sm flex items-start gap-4 hover:border-rose-200 transition-colors">
                            <div class="w-12 h-12 rounded-full flex-shrink-0 flex items-center justify-center ${bgIcon}">
                                <i class="fas ${icon} text-lg"></i>
                            </div>
                            <div class="flex-1 min-w-0">
                                <div class="flex justify-between items-start mb-1">
                                    <p class="font-extrabold text-slate-800 text-sm md:text-base truncate">${item.pelaku} <span class="font-bold text-slate-400 text-xs ml-1">• ${item.kategori}</span></p>
                                    <span class="font-extrabold ${colorSign} whitespace-nowrap ml-2 text-sm md:text-base">${sign} ${formatRp(item.nominal)}</span>
                                </div>
                                <p class="text-xs text-slate-400 leading-relaxed">${formatTgl(item.tanggal)} ${catatanDetail} ${ruteTransfer}</p>
                            </div>
                        </div>`;
                });
            }

            document.getElementById('totalSaldo').innerText = formatRp(total);
            document.getElementById('masukBulanIni').innerText = formatRp(masukBulan);
            document.getElementById('keluarBulanIni').innerText = formatRp(keluarBulan);

            let persentase = (total / TARGET_TABUNGAN) * 100;
            if(persentase > 100) persentase = 100;
            if(persentase < 0) persentase = 0;
            
            document.getElementById('barTarget').style.width = persentase.toFixed(1) + '%';
            document.getElementById('persenTarget').innerText = persentase.toFixed(1) + '%';
            
            const sisaTabungan = TARGET_TABUNGAN - total;
            if(sisaTabungan > 0) {
                document.getElementById('sisaTarget').innerHTML = `Sisa <span class="font-bold">${formatRp(sisaTabungan)}</span> menuju 10 Juta 🔥`;
            } else {
                document.getElementById('sisaTarget').innerHTML = `Target Tercapai! 🎉`;
            }
        };

        window.tampilkanStatistik = () => {
            if(dbTransaksi.length === 0) return Swal.fire('Kosong', 'Belum ada data untuk direkap.', 'info');
            
            let rekapBulanan = {};
            dbTransaksi.forEach(item => {
                const tgl = new Date(item.tanggal);
                const bulanTahun = tgl.toLocaleString('id-ID', { month: 'long', year: 'numeric' });
                
                if(!rekapBulanan[bulanTahun]) rekapBulanan[bulanTahun] = { masuk: 0, keluar: 0 };
                
                if(item.jenis === 'nabung') rekapBulanan[bulanTahun].masuk += item.nominal;
                else rekapBulanan[bulanTahun].keluar += item.nominal;
            });

            let htmlRekap = '<div class="space-y-4 text-left max-h-[400px] overflow-y-auto px-1">';
            for(let bln in rekapBulanan) {
                let saldoBulan = rekapBulanan[bln].masuk - rekapBulanan[bln].keluar;
                htmlRekap += `
                    <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm">
                        <p class="font-extrabold text-slate-800 border-b border-slate-100 pb-3 mb-3 text-sm"><i class="far fa-calendar-alt text-rose-500 mr-2"></i> ${bln}</p>
                        <div class="flex justify-between text-xs mb-2"><span class="text-slate-500 font-bold">Uang Masuk:</span> <span class="font-extrabold text-emerald-500">+ ${formatRp(rekapBulanan[bln].masuk)}</span></div>
                        <div class="flex justify-between text-xs mb-2"><span class="text-slate-500 font-bold">Uang Keluar:</span> <span class="font-extrabold text-rose-500">- ${formatRp(rekapBulanan[bln].keluar)}</span></div>
                        <div class="flex justify-between text-sm mt-3 pt-3 border-t border-slate-100"><span class="font-extrabold text-slate-800">Saldo Akhir:</span> <span class="font-extrabold text-blue-600">${formatRp(saldoBulan)}</span></div>
                    </div>
                `;
            }
            htmlRekap += '</div>';

            Swal.fire({
                title: '<span class="text-xl font-extrabold text-slate-800">Rekapitulasi Tabungan</span>',
                html: htmlRekap,
                confirmButtonColor: '#e11d48',
                confirmButtonText: 'Tutup',
                background: '#f8fafc',
                customClass: { popup: 'rounded-[32px]' }
            });
        };

        document.getElementById('formTransaksi').addEventListener('submit', async (e) => {
            e.preventDefault();
            const btn = document.getElementById('btnSubmit');
            btn.disabled = true; btn.innerHTML = "<i class='fas fa-spinner fa-spin'></i> Memproses...";

            const jenis = document.getElementById('jenisTransaksi').value;
            const nominalBersih = parseInt(nominalInput.value.replace(/[^0-9]/g, ''));
            
            if(!nominalBersih || nominalBersih <= 0) {
                Swal.fire('Oops', 'Nominal tidak valid.', 'warning');
                btn.disabled = false; btn.innerHTML = "<i class='fas fa-save'></i> Simpan Tabungan"; return;
            }

            const totalSaatIni = dbTransaksi.reduce((acc, curr) => curr.jenis === 'nabung' ? acc + curr.nominal : acc - curr.nominal, 0);
            if(jenis === 'tarik' && nominalBersih > totalSaatIni) {
                Swal.fire('Saldo Kurang', 'Saldo tabungan tidak cukup.', 'error');
                btn.disabled = false; btn.innerHTML = "<i class='fas fa-save'></i> Catat Pengeluaran"; return;
            }

            try {
                await addDoc(collection(db, "tabungan_kita"), {
                    createdAt: Date.now(),
                    jenis: jenis,
                    tanggal: document.getElementById('tanggal').value,
                    pelaku: document.getElementById('pelaku').value,
                    kategori: document.getElementById('kategori').value,
                    nominal: nominalBersih,
                    catatan: document.getElementById('catatan').value,
                    via: jenis === 'nabung' ? document.getElementById('via').value : '-',
                    rekening: jenis === 'nabung' ? document.getElementById('rekening').value : '-'
                });
                
                e.target.reset(); 
                document.getElementById('tanggal').valueAsDate = new Date();
                teksTerbilang.innerText = '';
                if(jenis === 'tarik') window.switchTab('tarik'); 
                
                Swal.fire({icon: 'success', title: 'Tersimpan!', toast: true, position: 'top', showConfirmButton: false, timer: 1500});
            } catch (error) {
                Swal.fire('Error', 'Sistem gagal menyimpan data.', 'error');
            }
            btn.disabled = false; 
            btn.innerHTML = jenis === 'nabung' ? "<i class='fas fa-save'></i> Simpan Tabungan" : "<i class='fas fa-hand-holding-usd'></i> Catat Pengeluaran";
        });

        window.cekPin = () => {
            if(document.getElementById('pinInput').value === PIN_RAHASIA) {
                sessionStorage.setItem('auth', '1');
                document.getElementById('loginScreen').classList.add('hidden');
                document.getElementById('mainApp').classList.remove('hidden');
            } else { Swal.fire({icon: 'error', title: 'Akses Ditolak', text: 'PIN Salah!', confirmButtonColor: '#e11d48'}); }
        };

        window.logout = () => { sessionStorage.removeItem('auth'); location.reload(); };

        window.switchTab = (jenis) => {
            document.getElementById('jenisTransaksi').value = jenis;
            const btnNabung = document.getElementById('tabNabung'); 
            const btnTarik = document.getElementById('tabTarik');
            const katSelect = document.getElementById('kategori');
            const btnSubmit = document.getElementById('btnSubmit');
            const areaBank = document.getElementById('areaBank');
            const inputVia = document.getElementById('via');
            const inputRekening = document.getElementById('rekening');

            if(jenis === 'nabung') {
                btnNabung.className = "flex-1 py-3 text-sm font-bold rounded-lg tab-active tab-btn";
                btnTarik.className = "flex-1 py-3 text-sm font-bold rounded-lg tab-inactive tab-btn";
                btnSubmit.className = "w-full pink-gradient text-white font-extrabold py-4 rounded-xl mt-4 shadow-lg shadow-rose-200 hover:shadow-xl active:scale-95 transition-all flex items-center justify-center gap-2 text-lg";
                btnSubmit.innerHTML = "<i class='fas fa-save'></i> Simpan Tabungan";
                
                areaBank.classList.remove('hidden');
                inputVia.required = true; inputRekening.required = true;

                katSelect.innerHTML = `
                    <option value="Sisa Uang Jajan">Sisa Uang Jajan</option>
                    <option value="Gaji / Pendapatan">Gaji / Pendapatan</option>
                    <option value="Hadiah / Bonus">Hadiah / Bonus</option>
                    <option value="Lainnya">Lainnya</option>
                `;
            } else {
                btnTarik.className = "flex-1 py-3 text-sm font-bold rounded-lg tab-active tab-btn bg-slate-800 shadow-slate-300";
                btnNabung.className = "flex-1 py-3 text-sm font-bold rounded-lg tab-inactive tab-btn";
                btnSubmit.className = "w-full bg-slate-800 text-white font-extrabold py-4 rounded-xl mt-4 shadow-lg shadow-slate-200 hover:shadow-xl active:scale-95 transition-all flex items-center justify-center gap-2 text-lg";
                btnSubmit.innerHTML = "<i class='fas fa-hand-holding-usd'></i> Catat Pengeluaran";
                
                areaBank.classList.add('hidden');
                inputVia.required = false; inputRekening.required = false;

                katSelect.innerHTML = `
                    <option value="Kencan / Jalan">Kencan / Jalan</option>
                    <option value="Makan / Jajan">Makan / Jajan</option>
                    <option value="Belanja Kebutuhan">Belanja Kebutuhan</option>
                    <option value="Transportasi">Transportasi</option>
                    <option value="Darurat">Darurat</option>
                    <option value="Koreksi Salah Input">Koreksi Salah Input</option>
                `;
            }
        };

        window.exportExcel = () => {
            if(!dbTransaksi.length) return Swal.fire('Kosong', 'Tidak ada data untuk diunduh.', 'info');
            const dataSheet = dbTransaksi.map((d, i) => ({
                "No": i+1, "Tanggal": formatTgl(d.tanggal), "Jenis": d.jenis.toUpperCase(),
                "Pelaku": d.pelaku, "Kategori": d.kategori, "Nominal": d.nominal, 
                "Transfer Via": d.jenis === 'nabung' ? d.via : '-', 
                "Tujuan Rekening": d.jenis === 'nabung' ? d.rekening : '-', 
                "Keterangan": d.catatan
            }));
            const ws = XLSX.utils.json_to_sheet(dataSheet); const wb = XLSX.utils.book_new();
            XLSX.utils.book_append_sheet(wb, ws, "Data Tabungan");
            XLSX.writeFile(wb, `Riwayat_Tabungan.xlsx`);
        };

        if(sessionStorage.getItem('auth') === '1') {
            document.getElementById('loginScreen').classList.add('hidden');
            document.getElementById('mainApp').classList.remove('hidden');
        }
    </script>
</body>
</html>
