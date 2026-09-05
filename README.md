<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Brankas Aris & Fifi 💖</title>
    
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>

    <style>
        body { font-family: 'Plus Jakarta Sans', sans-serif; background-color: #fff1f2; }
        .glass-card { background: rgba(255, 255, 255, 0.95); border-radius: 24px; box-shadow: 0 10px 25px rgba(225, 29, 72, 0.05); border: 1px solid #ffe4e6; }
        .pink-gradient { background: linear-gradient(135deg, #f43f5e 0%, #fb7185 100%); }
        .tab-active { background: #f43f5e; color: white; }
        .tab-inactive { background: #ffe4e6; color: #fda4af; }
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: #fda4af; border-radius: 10px; }
    </style>
</head>
<body class="text-slate-800 antialiased h-screen overflow-hidden flex flex-col">

    <!-- LOGIN SCREEN -->
    <div id="loginScreen" class="fixed inset-0 z-50 pink-gradient flex flex-col items-center justify-center p-4">
        <div class="bg-white w-full max-w-sm rounded-3xl p-8 text-center shadow-2xl">
            <div class="w-20 h-20 bg-rose-100 rounded-full flex items-center justify-center mx-auto mb-4 shadow-inner">
                <i class="fas fa-heart text-4xl text-rose-500"></i>
            </div>
            <h1 class="text-2xl font-bold text-slate-800 mb-1">Brankas Cinta</h1>
            <p class="text-sm text-slate-500 mb-6">Masukkan PIN rahasia Aris & Fifi</p>
            <input type="password" id="pinInput" placeholder="••••••" class="w-full text-center text-3xl tracking-widest px-4 py-3 rounded-2xl border-2 border-rose-100 focus:border-rose-500 focus:outline-none mb-6 font-bold text-rose-600 bg-rose-50">
            <button onclick="window.cekPin()" class="w-full pink-gradient text-white font-bold py-3.5 rounded-2xl shadow-lg hover:shadow-xl transition-all">Buka Brankas <i class="fas fa-unlock ml-2"></i></button>
        </div>
    </div>

    <!-- MAIN APP -->
    <div id="mainApp" class="hidden h-full flex flex-col">
        <!-- Navbar -->
        <nav class="bg-white px-6 py-4 flex justify-between items-center border-b border-rose-100 z-10 shadow-sm">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 pink-gradient rounded-xl flex items-center justify-center text-white shadow-md font-bold text-lg">AF</div>
                <div>
                    <h1 class="font-bold text-lg leading-tight text-rose-600">Aris & Fifi</h1>
                    <p id="statusKoneksi" class="text-xs font-bold text-amber-500">Connecting...</p>
                </div>
            </div>
            <div class="flex gap-2">
                <button onclick="window.exportExcel()" class="bg-rose-50 text-rose-600 hover:bg-rose-100 p-2 md:px-4 rounded-xl text-sm font-bold transition-all"><i class="fas fa-file-excel"></i> <span class="hidden md:inline ml-1">Excel</span></button>
                <button onclick="window.logout()" class="bg-slate-50 text-slate-500 hover:bg-slate-100 p-2 md:px-4 rounded-xl text-sm font-bold transition-all"><i class="fas fa-sign-out-alt"></i></button>
            </div>
        </nav>

        <div class="flex-1 overflow-y-auto p-4 md:p-6 lg:p-8">
            <div class="max-w-6xl mx-auto space-y-6">
                
                <!-- SUMMARY & TARGET -->
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <div class="pink-gradient rounded-3xl p-6 text-white shadow-lg relative overflow-hidden md:col-span-2 flex flex-col justify-center">
                        <i class="fas fa-piggy-bank absolute -right-4 -bottom-4 text-8xl opacity-10"></i>
                        <div class="flex justify-between items-end mb-4">
                            <div>
                                <p class="text-rose-100 text-sm font-medium mb-1">Total Tabungan Kita</p>
                                <h2 id="totalSaldo" class="text-4xl lg:text-5xl font-extrabold truncate drop-shadow-md">Rp 0</h2>
                            </div>
                        </div>
                        <div class="mt-2">
                            <div class="flex justify-between text-xs font-medium text-rose-100 mb-1">
                                <span>Target: Rp 10 Juta</span>
                                <span id="persenTarget" class="font-bold">0%</span>
                            </div>
                            <div class="w-full bg-rose-900/30 rounded-full h-2.5 mb-1.5">
                                <div id="barTarget" class="bg-white h-2.5 rounded-full transition-all duration-1000" style="width: 0%"></div>
                            </div>
                            <p id="sisaTarget" class="text-xs text-rose-100 font-medium text-right"></p>
                        </div>
                    </div>

                    <div class="grid grid-rows-2 gap-4">
                        <div class="bg-white rounded-3xl p-5 border border-rose-50 shadow-sm flex items-center justify-between">
                            <div>
                                <p class="text-slate-400 text-xs font-bold uppercase tracking-wider mb-1">Masuk Bulan Ini</p>
                                <h2 id="masukBulanIni" class="text-xl font-extrabold text-emerald-500">Rp 0</h2>
                            </div>
                            <div class="w-10 h-10 rounded-full bg-emerald-50 text-emerald-500 flex items-center justify-center"><i class="fas fa-arrow-down"></i></div>
                        </div>
                        <div class="bg-white rounded-3xl p-5 border border-rose-50 shadow-sm flex items-center justify-between">
                            <div>
                                <p class="text-slate-400 text-xs font-bold uppercase tracking-wider mb-1">Ditarik Bulan Ini</p>
                                <h2 id="keluarBulanIni" class="text-xl font-extrabold text-rose-500">Rp 0</h2>
                            </div>
                            <div class="w-10 h-10 rounded-full bg-rose-50 text-rose-500 flex items-center justify-center"><i class="fas fa-arrow-up"></i></div>
                        </div>
                    </div>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
                    <!-- FORM INPUT -->
                    <div class="lg:col-span-5 glass-card p-6 lg:p-8">
                        <div class="flex p-1 bg-rose-50 rounded-xl mb-6">
                            <button id="tabNabung" onclick="window.switchTab('nabung')" class="flex-1 py-2.5 text-sm font-bold rounded-lg tab-active transition-all">Nabung (+)</button>
                            <button id="tabTarik" onclick="window.switchTab('tarik')" class="flex-1 py-2.5 text-sm font-bold rounded-lg tab-inactive transition-all">Tarik (-)</button>
                        </div>

                        <form id="formTransaksi" class="space-y-5">
                            <input type="hidden" id="jenisTransaksi" value="nabung">
                            
                            <div class="grid grid-cols-2 gap-4">
                                <div>
                                    <label class="block text-xs font-bold text-slate-500 mb-1.5">Tanggal</label>
                                    <input type="date" id="tanggal" required class="w-full px-4 py-3 rounded-xl border border-rose-100 bg-white focus:border-rose-400 focus:ring-2 focus:ring-rose-100 outline-none text-sm font-medium transition-all">
                                </div>
                                <div>
                                    <label id="labelPelaku" class="block text-xs font-bold text-slate-500 mb-1.5">Oleh Siapa?</label>
                                    <select id="pelaku" required class="w-full px-4 py-3 rounded-xl border border-rose-100 bg-white focus:border-rose-400 focus:ring-2 focus:ring-rose-100 outline-none text-sm font-medium transition-all appearance-none">
                                        <option value="Aris">Aris</option>
                                        <option value="Fifi">Fifi</option>
                                        <option value="Aris & Fifi">Berdua</option>
                                    </select>
                                </div>
                            </div>

                            <div>
                                <label class="block text-xs font-bold text-slate-500 mb-1.5">Kategori</label>
                                <select id="kategori" required class="w-full px-4 py-3 rounded-xl border border-rose-100 bg-white focus:border-rose-400 focus:ring-2 focus:ring-rose-100 outline-none text-sm font-medium transition-all appearance-none">
                                    <option value="Sisa Uang Jajan">Sisa Uang Jajan</option>
                                    <option value="Gaji / Pendapatan">Gaji / Pendapatan</option>
                                    <option value="Hadiah / Bonus">Hadiah / Bonus</option>
                                    <option value="Lainnya">Lainnya</option>
                                </select>
                            </div>

                            <div>
                                <label class="block text-xs font-bold text-slate-500 mb-1.5">Nominal Uang</label>
                                <div class="grid grid-cols-4 gap-2 mb-2">
                                    <button type="button" onclick="window.tambahNominal(10000)" class="bg-rose-50 text-rose-600 text-xs font-bold py-2 rounded-lg hover:bg-rose-100 transition-colors">+10k</button>
                                    <button type="button" onclick="window.tambahNominal(20000)" class="bg-rose-50 text-rose-600 text-xs font-bold py-2 rounded-lg hover:bg-rose-100 transition-colors">+20k</button>
                                    <button type="button" onclick="window.tambahNominal(50000)" class="bg-rose-50 text-rose-600 text-xs font-bold py-2 rounded-lg hover:bg-rose-100 transition-colors">+50k</button>
                                    <button type="button" onclick="window.tambahNominal(100000)" class="bg-rose-50 text-rose-600 text-xs font-bold py-2 rounded-lg hover:bg-rose-100 transition-colors">+100k</button>
                                </div>
                                <div class="relative">
                                    <span class="absolute left-4 top-1/2 -translate-y-1/2 text-slate-400 font-bold">Rp</span>
                                    <input type="text" id="nominal" required placeholder="0" class="w-full pl-12 pr-4 py-3.5 rounded-xl border border-rose-100 bg-white focus:border-rose-400 focus:ring-2 focus:ring-rose-100 outline-none text-xl font-extrabold text-slate-800 transition-all">
                                </div>
                                <p id="teksTerbilang" class="text-xs font-semibold text-rose-500 mt-2 italic min-h-[20px]"></p>
                            </div>

                            <!-- Area Transfer Khusus Nabung -->
                            <div id="areaBank" class="grid grid-cols-2 gap-4">
                                <div>
                                    <label class="block text-xs font-bold text-slate-500 mb-1.5">Transfer Via</label>
                                    <input type="text" id="via" required placeholder="Cth: BCA / Dana" class="w-full px-4 py-3 rounded-xl border border-rose-100 bg-white focus:border-rose-400 focus:ring-2 focus:ring-rose-100 outline-none text-sm font-medium transition-all">
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-500 mb-1.5">Ke Rekening</label>
                                    <input type="text" id="rekening" required placeholder="Cth: BNI Fifi" class="w-full px-4 py-3 rounded-xl border border-rose-100 bg-white focus:border-rose-400 focus:ring-2 focus:ring-rose-100 outline-none text-sm font-medium transition-all">
                                </div>
                            </div>

                            <div>
                                <label class="block text-xs font-bold text-slate-500 mb-1.5">Catatan Tambahan (Opsional)</label>
                                <input type="text" id="catatan" placeholder="Cth: Buat DP rumah masa depan / Makan" class="w-full px-4 py-3 rounded-xl border border-rose-100 bg-white focus:border-rose-400 focus:ring-2 focus:ring-rose-100 outline-none text-sm font-medium transition-all">
                            </div>

                            <button type="submit" id="btnSubmit" class="w-full pink-gradient text-white font-extrabold py-4 rounded-xl mt-4 shadow-lg shadow-rose-200 hover:shadow-xl hover:scale-[1.02] transition-all disabled:opacity-50 flex items-center justify-center gap-2">
                                <i class="fas fa-save"></i> Simpan Tabungan
                            </button>
                        </form>
                    </div>

                    <!-- RIWAYAT LIST -->
                    <div class="lg:col-span-7 glass-card p-6 lg:p-8 flex flex-col h-[650px]">
                        <h3 class="text-lg font-bold text-slate-800 mb-4 border-b border-rose-100 pb-4 flex items-center gap-2">
                            <i class="fas fa-history text-rose-400"></i> Detail Riwayat Tabungan
                        </h3>
                        
                        <div id="riwayatKosong" class="flex-1 flex flex-col items-center justify-center text-rose-300 hidden">
                            <i class="fas fa-box-open text-5xl mb-3 opacity-50"></i>
                            <p class="font-bold">Belum ada catatan.</p>
                            <p class="text-sm">Yuk mulai isi brankas cinta kalian! 💖</p>
                        </div>

                        <div id="daftarRiwayat" class="space-y-4 overflow-y-auto pr-2 flex-1">
                            <!-- Render by Firebase -->
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
        import { getFirestore, collection, addDoc, onSnapshot, deleteDoc, doc, query, orderBy } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

        // ==========================================
        // FIREBASE CONFIG MILIKMU
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
        const TARGET_TABUNGAN = 10000000; // Rp 10 Juta
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

        const isBulanIni = (dateString) => {
            const date = new Date(dateString); const now = new Date();
            return date.getMonth() === now.getMonth() && date.getFullYear() === now.getFullYear();
        };

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
            
            document.getElementById('statusKoneksi').innerText = "✅ Connect";
            document.getElementById('statusKoneksi').className = "text-xs font-bold text-emerald-500";
            updateUI();
        }, (error) => {
            document.getElementById('statusKoneksi').innerText = "❌ Disconnect";
            document.getElementById('statusKoneksi').className = "text-xs font-bold text-rose-500";
        });

        const updateUI = () => {
            const list = document.getElementById('daftarRiwayat');
            list.innerHTML = '';
            let total = 0, masukBulan = 0, keluarBulan = 0;

            if(dbTransaksi.length === 0) {
                document.getElementById('riwayatKosong').classList.remove('hidden');
            } else {
                document.getElementById('riwayatKosong').classList.add('hidden');
                
                dbTransaksi.forEach(item => {
                    if(item.jenis === 'nabung') {
                        total += item.nominal;
                        if(isBulanIni(item.tanggal)) masukBulan += item.nominal;
                    } else {
                        total -= item.nominal;
                        if(isBulanIni(item.tanggal)) keluarBulan += item.nominal;
                    }

                    const isNabung = item.jenis === 'nabung';
                    const colorClass = isNabung ? 'text-emerald-500 bg-emerald-50 border-emerald-100' : 'text-rose-500 bg-rose-50 border-rose-100';
                    const icon = isNabung ? 'fa-arrow-down' : 'fa-arrow-up';
                    
                    const catatanDetail = item.catatan ? `• ${item.catatan}` : '';
                    const ruteTransfer = (isNabung && item.via && item.rekening) ? `<br><span class="inline-block mt-1 bg-rose-50 text-rose-500 px-2 py-0.5 rounded text-[10px] font-bold"><i class="fas fa-university"></i> ${item.via} ➔ ${item.rekening}</span>` : '';
                    
                    list.innerHTML += `
                        <div class="flex items-center justify-between p-4 rounded-2xl bg-white border shadow-sm hover:shadow-md transition-all group">
                            <div class="flex items-center gap-4 overflow-hidden">
                                <div class="w-12 h-12 rounded-full flex-shrink-0 flex items-center justify-center text-lg border ${colorClass}">
                                    <i class="fas ${icon}"></i>
                                </div>
                                <div class="min-w-0">
                                    <p class="font-bold text-slate-800 truncate">${item.pelaku} <span class="text-xs font-normal text-slate-500 bg-slate-100 px-2 py-0.5 rounded-full ml-1">${item.kategori}</span></p>
                                    <p class="text-xs text-slate-500 truncate mt-1 leading-relaxed">${formatTgl(item.tanggal)} ${catatanDetail} ${ruteTransfer}</p>
                                </div>
                            </div>
                            <div class="flex items-center gap-3">
                                <span class="font-extrabold text-base whitespace-nowrap ${isNabung ? 'text-emerald-500' : 'text-rose-500'}">
                                    ${isNabung ? '+' : '-'} ${formatRp(item.nominal)}
                                </span>
                                <button onclick="window.hapusData('${item.id}')" class="text-slate-300 hover:text-red-500 p-2 rounded-full hover:bg-red-50 opacity-0 group-hover:opacity-100 transition-all"><i class="fas fa-trash-alt"></i></button>
                            </div>
                        </div>`;
                });
            }

            document.getElementById('totalSaldo').innerText = formatRp(total);
            document.getElementById('masukBulanIni').innerText = formatRp(masukBulan);
            document.getElementById('keluarBulanIni').innerText = formatRp(keluarBulan);

            // Progress & Sisa Target
            let persentase = (total / TARGET_TABUNGAN) * 100;
            if(persentase > 100) persentase = 100;
            if(persentase < 0) persentase = 0;
            
            document.getElementById('barTarget').style.width = persentase.toFixed(1) + '%';
            document.getElementById('persenTarget').innerText = persentase.toFixed(1) + '%';
            
            const sisaTabungan = TARGET_TABUNGAN - total;
            if(sisaTabungan > 0) {
                document.getElementById('sisaTarget').innerHTML = `Kurang <span class="font-bold drop-shadow-sm">${formatRp(sisaTabungan)}</span> lagi buat capai target! 🔥`;
            } else {
                document.getElementById('sisaTarget').innerHTML = `Yeay! Target kalian sudah tercapai! 🎉`;
            }
        };

        document.getElementById('formTransaksi').addEventListener('submit', async (e) => {
            e.preventDefault();
            const btn = document.getElementById('btnSubmit');
            btn.disabled = true; btn.innerHTML = "<i class='fas fa-spinner fa-spin'></i> Menyimpan...";

            const jenis = document.getElementById('jenisTransaksi').value;
            const nominalBersih = parseInt(nominalInput.value.replace(/[^0-9]/g, ''));
            
            if(!nominalBersih || nominalBersih <= 0) {
                Swal.fire('Ups!', 'Nominal uang tidak valid.', 'warning');
                btn.disabled = false; btn.innerHTML = "<i class='fas fa-save'></i> Simpan Tabungan"; return;
            }

            const totalSaatIni = dbTransaksi.reduce((acc, curr) => curr.jenis === 'nabung' ? acc + curr.nominal : acc - curr.nominal, 0);
            if(jenis === 'tarik' && nominalBersih > totalSaatIni) {
                Swal.fire('Saldo Kurang', 'Saldo tabungan tidak cukup untuk ditarik!', 'error');
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
                if(jenis === 'tarik') window.switchTab('tarik'); // Reset form tarik tanpa memunculkan area bank
                
                Swal.fire({icon: 'success', title: 'Tersimpan!', toast: true, position: 'top-end', showConfirmButton: false, timer: 1500});
            } catch (error) {
                Swal.fire('Error', 'Gagal menyimpan ke database.', 'error');
            }
            btn.disabled = false; 
            btn.innerHTML = jenis === 'nabung' ? "<i class='fas fa-save'></i> Simpan Tabungan" : "<i class='fas fa-hand-holding-usd'></i> Catat Pengeluaran";
        });

        window.cekPin = () => {
            if(document.getElementById('pinInput').value === PIN_RAHASIA) {
                sessionStorage.setItem('auth', '1');
                document.getElementById('loginScreen').classList.add('hidden');
                document.getElementById('mainApp').classList.remove('hidden');
            } else { Swal.fire('Error', 'PIN Salah!', 'error'); }
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
                btnNabung.className = "flex-1 py-2.5 text-sm font-bold rounded-lg tab-active transition-all shadow-md";
                btnTarik.className = "flex-1 py-2.5 text-sm font-bold rounded-lg tab-inactive transition-all hover:bg-rose-100";
                document.getElementById('labelPelaku').innerText = "Oleh Siapa?";
                btnSubmit.className = "w-full pink-gradient text-white font-extrabold py-4 rounded-xl mt-4 shadow-lg shadow-rose-200 hover:shadow-xl hover:scale-[1.02] transition-all flex items-center justify-center gap-2";
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
                btnTarik.className = "flex-1 py-2.5 text-sm font-bold rounded-lg tab-active transition-all shadow-md";
                btnNabung.className = "flex-1 py-2.5 text-sm font-bold rounded-lg tab-inactive transition-all hover:bg-rose-100";
                document.getElementById('labelPelaku').innerText = "Siapa yang Tarik?";
                btnSubmit.className = "w-full bg-rose-600 text-white font-extrabold py-4 rounded-xl mt-4 shadow-lg shadow-rose-200 hover:shadow-xl hover:scale-[1.02] transition-all flex items-center justify-center gap-2";
                btnSubmit.innerHTML = "<i class='fas fa-hand-holding-usd'></i> Catat Pengeluaran";
                
                areaBank.classList.add('hidden');
                inputVia.required = false; inputRekening.required = false;

                katSelect.innerHTML = `
                    <option value="Kencan / Jalan">Kencan / Jalan</option>
                    <option value="Makan / Jajan">Makan / Jajan</option>
                    <option value="Belanja Kebutuhan">Belanja Kebutuhan</option>
                    <option value="Transportasi">Transportasi</option>
                    <option value="Darurat">Darurat</option>
                `;
            }
        };

        window.hapusData = async (id) => {
            Swal.fire({
                title: 'Hapus riwayat?', text: "Data tidak bisa dikembalikan!", icon: 'warning',
                showCancelButton: true, confirmButtonColor: '#f43f5e', cancelButtonColor: '#cbd5e1',
                confirmButtonText: 'Ya, hapus!'
            }).then(async (result) => {
                if (result.isConfirmed) { await deleteDoc(doc(db, "tabungan_kita", id)); }
            });
        };

        window.exportExcel = () => {
            if(!dbTransaksi.length) return Swal.fire('Kosong', 'Belum ada data untuk diunduh', 'info');
            const dataSheet = dbTransaksi.map((d, i) => ({
                "No": i+1, "Tanggal": formatTgl(d.tanggal), "Jenis": d.jenis.toUpperCase(),
                "Pelaku": d.pelaku, "Kategori": d.kategori, "Nominal": d.nominal, 
                "Transfer Via": d.jenis === 'nabung' ? d.via : '-', 
                "Tujuan Rekening": d.jenis === 'nabung' ? d.rekening : '-', 
                "Catatan": d.catatan
            }));
            const ws = XLSX.utils.json_to_sheet(dataSheet); const wb = XLSX.utils.book_new();
            XLSX.utils.book_append_sheet(wb, ws, "Tabungan Cinta");
            XLSX.writeFile(wb, `Brankas_Cinta_AF.xlsx`);
        };

        if(sessionStorage.getItem('auth') === '1') {
            document.getElementById('loginScreen').classList.add('hidden');
            document.getElementById('mainApp').classList.remove('hidden');
        }
    </script>
</body>
</html>
