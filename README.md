
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Brankas Aris & Fifi 💖</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>

    <style>
        body { 
            font-family: 'Plus Jakarta Sans', sans-serif; 
            background-color: #fdf2f8; /* pink-50 */
            -webkit-tap-highlight-color: transparent;
        }
        /* Desain Kartu ATM */
        .atm-card {
            background: linear-gradient(135deg, #f43f5e 0%, #fb7185 100%);
            box-shadow: 0 15px 30px -5px rgba(225, 29, 72, 0.3);
        }
        /* Sembunyikan scrollbar tapi tetap bisa scroll */
        ::-webkit-scrollbar { display: none; }
        .ios-segment { background-color: #ffe4e6; padding: 4px; border-radius: 14px; display: flex; }
        .ios-tab { flex: 1; text-align: center; padding: 8px 0; font-size: 14px; font-weight: 700; border-radius: 10px; cursor: pointer; transition: all 0.3s; color: #fda4af; }
        .ios-tab.active { background-color: #f43f5e; color: white; box-shadow: 0 2px 8px rgba(225, 29, 72, 0.2); }
        .swal2-container { z-index: 10000 !important; }
    </style>
</head>
<body class="text-slate-800 antialiased pb-20">

    <!-- ================= LOGIN SCREEN ================= -->
    <div id="loginScreen" class="fixed inset-0 z-50 bg-[#fdf2f8] flex flex-col items-center justify-center p-6">
        <div class="w-full max-w-sm flex flex-col items-center text-center animation-fade-in">
            <div class="w-24 h-24 bg-white rounded-full flex items-center justify-center shadow-xl shadow-rose-200 mb-6">
                <i class="fas fa-heart text-5xl text-rose-500 animate-pulse"></i>
            </div>
            <h1 class="text-3xl font-extrabold text-slate-800 mb-2">Our Brankas</h1>
            <p class="text-slate-500 mb-8 font-medium">Aris & Fifi's Private Vault</p>
            
            <div class="w-full bg-white p-6 rounded-3xl shadow-sm border border-rose-50">
                <input type="password" id="pinInput" placeholder="PIN Rahasia" class="w-full text-center text-2xl tracking-[0.5em] px-4 py-4 rounded-2xl bg-rose-50 border-none focus:ring-2 focus:ring-rose-400 outline-none mb-6 font-bold text-rose-600 transition-all">
                <button onclick="window.cekPin()" class="w-full atm-card text-white font-bold py-4 rounded-2xl active:scale-95 transition-transform text-lg">Buka Brankas</button>
            </div>
        </div>
    </div>

    <!-- ================= MAIN APP ================= -->
    <div id="mainApp" class="hidden w-full max-w-md mx-auto relative pt-4 px-4 min-h-screen">
        
        <!-- Header -->
        <div class="flex justify-between items-center mb-6">
            <div class="flex items-center gap-3">
                <div class="w-12 h-12 rounded-full atm-card flex items-center justify-center text-white font-bold text-xl border-2 border-white shadow-md">AF</div>
                <div>
                    <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">Halo Kesayangan,</p>
                    <h1 class="font-extrabold text-lg text-slate-800">Aris & Fifi 👋</h1>
                </div>
            </div>
            <button onclick="window.logout()" class="w-10 h-10 bg-white rounded-full flex items-center justify-center text-rose-400 shadow-sm border border-rose-50 active:scale-95"><i class="fas fa-sign-out-alt"></i></button>
        </div>

        <!-- Kartu Saldo (ATM Style) -->
        <div class="atm-card rounded-[28px] p-6 text-white mb-6 relative overflow-hidden">
            <i class="fas fa-leaf absolute -right-6 -bottom-6 text-9xl opacity-10"></i>
            <div class="flex justify-between items-center mb-1">
                <p class="text-rose-100 text-sm font-medium">Total Tabungan</p>
                <span id="statusKoneksi" class="text-[10px] font-bold bg-white/20 px-2 py-1 rounded-full"><i class="fas fa-wifi text-emerald-300"></i> Sync</span>
            </div>
            <h2 id="totalSaldo" class="text-4xl font-extrabold mb-6 tracking-tight">Rp 0</h2>
            
            <!-- Progress Bar -->
            <div>
                <div class="flex justify-between text-xs font-medium text-rose-100 mb-1.5">
                    <span>Target: 10 Juta</span>
                    <span id="persenTarget" class="font-bold">0%</span>
                </div>
                <div class="w-full bg-black/10 rounded-full h-2">
                    <div id="barTarget" class="bg-white h-2 rounded-full transition-all duration-1000" style="width: 0%"></div>
                </div>
            </div>
        </div>

        <!-- Masuk & Keluar Cepat -->
        <div class="grid grid-cols-2 gap-4 mb-6">
            <div class="bg-white p-4 rounded-2xl shadow-sm border border-rose-50 flex items-center gap-3">
                <div class="w-10 h-10 rounded-full bg-emerald-50 flex items-center justify-center text-emerald-500"><i class="fas fa-arrow-down"></i></div>
                <div>
                    <p class="text-[10px] font-bold text-slate-400 uppercase">Bulan Ini Masuk</p>
                    <p id="masukBulanIni" class="font-extrabold text-slate-800 text-sm">Rp 0</p>
                </div>
            </div>
            <div class="bg-white p-4 rounded-2xl shadow-sm border border-rose-50 flex items-center gap-3">
                <div class="w-10 h-10 rounded-full bg-rose-50 flex items-center justify-center text-rose-500"><i class="fas fa-arrow-up"></i></div>
                <div>
                    <p class="text-[10px] font-bold text-slate-400 uppercase">Bulan Ini Ditarik</p>
                    <p id="keluarBulanIni" class="font-extrabold text-slate-800 text-sm">Rp 0</p>
                </div>
            </div>
        </div>

        <!-- Form Input Tabungan -->
        <div class="bg-white rounded-3xl p-5 shadow-sm border border-rose-50 mb-8">
            <div class="ios-segment mb-5">
                <div id="tabNabung" class="ios-tab active" onclick="window.switchTab('nabung')">Nabung (+)</div id="tabNabung">
                <div id="tabTarik" class="ios-tab" onclick="window.switchTab('tarik')">Tarik (-)</div>
            </div>

            <form id="formTransaksi" class="space-y-4">
                <input type="hidden" id="jenisTransaksi" value="nabung">
                
                <div class="grid grid-cols-2 gap-3">
                    <div class="bg-slate-50 rounded-xl p-2 border border-slate-100 focus-within:border-rose-400 transition-colors">
                        <label class="block text-[10px] font-bold text-slate-400 ml-1 mb-0.5">TANGGAL</label>
                        <input type="date" id="tanggal" required class="w-full bg-transparent outline-none text-sm font-bold text-slate-700 px-1">
                    </div>
                    <div class="bg-slate-50 rounded-xl p-2 border border-slate-100 focus-within:border-rose-400 transition-colors">
                        <label class="block text-[10px] font-bold text-slate-400 ml-1 mb-0.5">OLEH SIAPA?</label>
                        <select id="pelaku" required class="w-full bg-transparent outline-none text-sm font-bold text-slate-700 px-1 appearance-none">
                            <option value="Aris">Aris</option>
                            <option value="Fifi">Fifi</option>
                            <option value="Aris & Fifi">Berdua</option>
                        </select>
                    </div>
                </div>

                <div class="bg-slate-50 rounded-xl p-3 border border-slate-100 focus-within:border-rose-400 transition-colors">
                    <label class="block text-[10px] font-bold text-slate-400 ml-1 mb-1">NOMINAL (RP)</label>
                    <input type="text" id="nominal" required placeholder="0" class="w-full bg-transparent outline-none text-3xl font-extrabold text-slate-800 px-1">
                    <p id="teksTerbilang" class="text-xs font-semibold text-rose-500 mt-1 ml-1 h-4"></p>
                </div>
                
                <!-- Tombol Nominal Cepat -->
                <div class="grid grid-cols-4 gap-2">
                    <button type="button" onclick="window.tambahNominal(10000)" class="bg-rose-50 text-rose-600 text-xs font-bold py-2 rounded-lg active:scale-95 transition-transform">+10k</button>
                    <button type="button" onclick="window.tambahNominal(20000)" class="bg-rose-50 text-rose-600 text-xs font-bold py-2 rounded-lg active:scale-95 transition-transform">+20k</button>
                    <button type="button" onclick="window.tambahNominal(50000)" class="bg-rose-50 text-rose-600 text-xs font-bold py-2 rounded-lg active:scale-95 transition-transform">+50k</button>
                    <button type="button" onclick="window.tambahNominal(100000)" class="bg-rose-50 text-rose-600 text-xs font-bold py-2 rounded-lg active:scale-95 transition-transform">+100k</button>
                </div>

                <div class="bg-slate-50 rounded-xl p-2 border border-slate-100 focus-within:border-rose-400 transition-colors">
                    <label class="block text-[10px] font-bold text-slate-400 ml-1 mb-0.5">KATEGORI</label>
                    <select id="kategori" required class="w-full bg-transparent outline-none text-sm font-bold text-slate-700 px-1 appearance-none">
                        <option value="Sisa Uang Jajan">Sisa Uang Jajan</option>
                        <option value="Gaji / Pendapatan">Gaji / Pendapatan</option>
                        <option value="Hadiah / Bonus">Hadiah / Bonus</option>
                        <option value="Lainnya">Lainnya</option>
                    </select>
                </div>

                <div id="areaBank" class="grid grid-cols-2 gap-3">
                    <div class="bg-slate-50 rounded-xl p-2 border border-slate-100 focus-within:border-rose-400 transition-colors">
                        <label class="block text-[10px] font-bold text-slate-400 ml-1 mb-0.5">DARI (VIA)</label>
                        <input type="text" id="via" required placeholder="Cth: BCA Aris" class="w-full bg-transparent outline-none text-sm font-bold text-slate-700 px-1">
                    </div>
                    <div class="bg-rose-50 rounded-xl p-2 border border-rose-100 transition-colors">
                        <label class="block text-[10px] font-bold text-rose-400 ml-1 mb-0.5">MASUK KE</label>
                        <select id="rekening" required class="w-full bg-transparent outline-none text-xs font-bold text-rose-700 px-1 appearance-none">
                            <option value="SeaBank 901061919099">SeaBank 901061919099</option>
                            <option value="Celengan Tunai">Celengan / Tunai</option>
                        </select>
                    </div>
                </div>

                <div class="bg-slate-50 rounded-xl p-2 border border-slate-100 focus-within:border-rose-400 transition-colors">
                    <label class="block text-[10px] font-bold text-slate-400 ml-1 mb-0.5">KETERANGAN</label>
                    <input type="text" id="catatan" placeholder="Tulis keterangan di sini..." class="w-full bg-transparent outline-none text-sm font-bold text-slate-700 px-1">
                </div>

                <button type="submit" id="btnSubmit" class="w-full atm-card text-white font-extrabold py-3.5 rounded-xl mt-2 shadow-lg active:scale-95 transition-transform flex items-center justify-center gap-2">
                    <i class="fas fa-save"></i> Simpan Tabungan
                </button>
            </form>
        </div>

        <!-- Riwayat Transaksi -->
        <div class="mb-10">
            <div class="flex justify-between items-end mb-4">
                <div>
                    <h3 class="text-lg font-extrabold text-slate-800">Riwayat Terakhir</h3>
                    <select id="filterRiwayat" onchange="window.updateUI()" class="text-xs bg-transparent text-rose-500 font-bold outline-none appearance-none mt-1">
                        <option value="30">30 Hari Terakhir ▾</option>
                        <option value="bulan">Bulan Ini ▾</option>
                        <option value="semua">Semua Waktu ▾</option>
                    </select>
                </div>
                <div class="flex gap-2">
                    <button onclick="window.tampilkanStatistik()" class="w-10 h-10 bg-white rounded-full text-rose-500 shadow-sm border border-rose-50 flex items-center justify-center active:scale-95"><i class="fas fa-chart-bar"></i></button>
                    <button onclick="window.exportExcel()" class="w-10 h-10 bg-emerald-50 rounded-full text-emerald-500 shadow-sm border border-emerald-100 flex items-center justify-center active:scale-95"><i class="fas fa-file-excel"></i></button>
                </div>
            </div>
            
            <div id="riwayatKosong" class="bg-white rounded-3xl p-8 text-center border border-rose-50 shadow-sm hidden">
                <i class="fas fa-wind text-4xl text-rose-200 mb-2"></i>
                <p class="font-bold text-slate-500">Belum ada catatan.</p>
            </div>

            <div id="daftarRiwayat" class="space-y-3 pb-8">
                <!-- Render by Firebase -->
            </div>
        </div>

    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
        import { getFirestore, collection, addDoc, onSnapshot, query, orderBy } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

        // ==========================================
        // FIREBASE CONFIG (JANGAN DIUBAH)
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
            
            document.getElementById('statusKoneksi').innerHTML = "<i class='fas fa-wifi text-emerald-300'></i> Sync";
            window.updateUI();
        }, (error) => {
            document.getElementById('statusKoneksi').innerHTML = "<i class='fas fa-exclamation-triangle text-rose-300'></i> Error";
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
                    const icon = isNabung ? 'fa-arrow-down text-emerald-500' : 'fa-arrow-up text-rose-500';
                    const bgIcon = isNabung ? 'bg-emerald-50' : 'bg-rose-50';
                    const sign = isNabung ? '+' : '-';
                    const colorSign = isNabung ? 'text-emerald-500' : 'text-rose-500';
                    
                    const catatanDetail = item.catatan ? `<br><span class="text-slate-400">${item.catatan}</span>` : '';
                    const ruteTransfer = (isNabung && item.via && item.rekening) ? `<br><span class="inline-block mt-1 bg-slate-100 text-slate-500 px-2 py-0.5 rounded text-[9px] font-bold uppercase"><i class="fas fa-university"></i> ${item.via} ➔ ${item.rekening}</span>` : '';
                    
                    list.innerHTML += `
                        <div class="bg-white p-4 rounded-2xl border border-rose-50 shadow-sm flex items-start gap-4">
                            <div class="w-10 h-10 rounded-full flex-shrink-0 flex items-center justify-center ${bgIcon}">
                                <i class="fas ${icon}"></i>
                            </div>
                            <div class="flex-1 min-w-0">
                                <div class="flex justify-between items-start mb-0.5">
                                    <p class="font-extrabold text-slate-800 text-sm truncate">${item.pelaku} <span class="font-medium text-slate-400 text-xs ml-1">• ${item.kategori}</span></p>
                                    <span class="font-extrabold ${colorSign} whitespace-nowrap ml-2">${sign} ${formatRp(item.nominal)}</span>
                                </div>
                                <p class="text-xs text-slate-500 leading-tight">${formatTgl(item.tanggal)} ${catatanDetail} ${ruteTransfer}</p>
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
                document.getElementById('sisaTarget').innerHTML = `Sisa <span class="font-bold">${formatRp(sisaTabungan)}</span> menuju target 🔥`;
            } else {
                document.getElementById('sisaTarget').innerHTML = `Target 10 Juta Tercapai! 🎉`;
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

            let htmlRekap = '<div class="space-y-3 text-left max-h-[400px] overflow-y-auto">';
            for(let bln in rekapBulanan) {
                let saldoBulan = rekapBulanan[bln].masuk - rekapBulanan[bln].keluar;
                htmlRekap += `
                    <div class="bg-rose-50 p-4 rounded-2xl border border-rose-100">
                        <p class="font-extrabold text-slate-800 border-b border-rose-200 pb-2 mb-2 text-sm"><i class="far fa-calendar-alt text-rose-500 mr-1"></i> ${bln}</p>
                        <div class="flex justify-between text-xs mb-1"><span class="text-slate-600 font-medium">Uang Masuk:</span> <span class="font-bold text-emerald-500">+ ${formatRp(rekapBulanan[bln].masuk)}</span></div>
                        <div class="flex justify-between text-xs mb-1"><span class="text-slate-600 font-medium">Uang Keluar:</span> <span class="font-bold text-rose-500">- ${formatRp(rekapBulanan[bln].keluar)}</span></div>
                        <div class="flex justify-between text-sm mt-2 pt-2 border-t border-rose-200"><span class="font-extrabold text-slate-800">Saldo:</span> <span class="font-extrabold text-blue-600">${formatRp(saldoBulan)}</span></div>
                    </div>
                `;
            }
            htmlRekap += '</div>';

            Swal.fire({
                title: '<span class="text-lg font-bold text-slate-800">Rangkuman Bulanan</span>',
                html: htmlRekap,
                confirmButtonColor: '#f43f5e',
                confirmButtonText: 'Tutup',
                background: '#fff',
                border: '1px solid #ffe4e6',
                customClass: { popup: 'rounded-3xl' }
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
                Swal.fire('Saldo Kurang', 'Saldo tabungan tidak cukup untuk ditarik.', 'error');
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
                
                Swal.fire({icon: 'success', title: 'Berhasil Disimpan!', toast: true, position: 'top', showConfirmButton: false, timer: 1500});
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
            } else { Swal.fire({icon: 'error', title: 'Akses Ditolak', text: 'PIN Salah!', confirmButtonColor: '#f43f5e'}); }
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
                btnNabung.className = "ios-tab active";
                btnTarik.className = "ios-tab";
                btnSubmit.className = "w-full atm-card text-white font-extrabold py-3.5 rounded-xl mt-2 shadow-lg active:scale-95 transition-transform flex items-center justify-center gap-2";
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
                btnTarik.className = "ios-tab active";
                btnNabung.className = "ios-tab";
                btnSubmit.className = "w-full bg-slate-800 text-white font-extrabold py-3.5 rounded-xl mt-2 shadow-lg active:scale-95 transition-transform flex items-center justify-center gap-2";
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
