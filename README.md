<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Kepanjangan 'IC'?", "id": "Integrated Circuit (Sirkuit Terpadu)." },
  { "en": "Apa Bahan Dasar 'IC'?", "id": "Silikon (Si) Semikonduktor." },
  { "en": "Siapa Penemu 'IC'?", "id": "Jack Kilby Dan Robert Noyce." },
  { "en": "Apa Komponen Utama Dalam 'IC'?", "id": "Transistor." },
  { "en": "Apa Itu 'Wafer'?", "id": "Piringan Silikon Tipis Untuk Membuat IC." },
  { "en": "Apa Itu 'Die'?", "id": "Satu Keping IC Dari Wafer." },
  { "en": "Apa Itu 'Fabrikasi'?", "id": "Proses Pembuatan IC (Integrated Circuit)." },
  { "en": "Apa Itu 'Fotolitografi'?", "id": "Proses Mencetak Pola Sirkuit." },
  { "en": "Apa Itu 'Doping'?", "id": "Menambahkan Impuritas Ke Semikonduktor." },
  { "en": "Apa Itu 'Paket IC (IC Package)'?", "id": "Wadah Yang Melindungi Die." },
  { "en": "Apa Fungsi 'Pin' Pada IC?", "id": "Kaki-kaki Untuk Koneksi Eksternal." },
  { "en": "Apa Itu 'Sirkuit Monolitik'?", "id": "Semua Komponen Dalam Satu Kristal." },
  { "en": "Apa Itu 'Skala Integrasi'?", "id": "Ukuran Kepadatan Komponen." },
  { "en": "Apa Kepanjangan 'SSI (Small-Scale Integration)'?", "id": "Integrasi Skala Kecil." },
  { "en": "Apa Kepanjangan 'MSI (Medium-Scale Integration)'?", "id": "Integrasi Skala Menengah." },
  { "en": "Apa Kepanjangan 'LSI (Large-Scale Integration)'?", "id": "Integrasi Skala Besar." },
  { "en": "Apa Kepanjangan 'VLSI (Very Large-Scale Integration)'?", "id": "Integrasi Skala Sangat Besar." },
  { "en": "Apa Itu 'Hukum Moore'?", "id": "Jumlah Transistor Berlipat Ganda Periodik." },
  { "en": "Apa Itu 'IC Analog'?", "id": "IC (Integrated Circuit) Untuk Sinyal Analog." },
  { "en": "Apa Itu 'IC Digital'?", "id": "IC (Integrated Circuit) Untuk Sinyal Digital." },
  { "en": "Apa Itu 'IC Sinyal Campuran'?", "id": "IC (Integrated Circuit) Dengan Sirkuit Analog-Digital." },
  { "en": "Apa Kepanjangan 'Op-Amp'?", "id": "Operational Amplifier (Penguat Operasional)." },
  { "en": "Apa Itu 'Timer 555'?", "id": "IC (Integrated Circuit) Pewaktu Yang Populer." },
  { "en": "Apa Itu 'Regulator Tegangan'?", "id": "IC (Integrated Circuit) Untuk Menstabilkan Tegangan." },
  { "en": "Apa Itu 'Gerbang Logika'?", "id": "Blok Bangunan Dasar Sirkuit Digital." },
  { "en": "Apa Itu 'Mikroprosesor'?", "id": "CPU (Central Processing Unit) Dalam Satu IC." },
  { "en": "Apa Itu 'Mikrokontroler'?", "id": "Komputer Kecil Dalam Satu IC." },
  { "en": "Apa Itu 'Memori'?", "id": "IC (Integrated Circuit) Untuk Menyimpan Data." },
  { "en": "Apa Kepanjangan 'RAM (Random-Access Memory)'?", "id": "Memori Akses Acak." },
  { "en": "Apa Kepanjangan 'ROM (Read-Only Memory)'?", "id": "Memori Hanya Baca." },
  { "en": "Apa Itu 'Substrat'?", "id": "Material Dasar Wafer IC." },
  { "en": "Apa Itu 'Epitaksi'?", "id": "Pertumbuhan Lapisan Kristal." },
  { "en": "Apa Itu 'Oksidasi'?", "id": "Proses Penumbuhan Lapisan Oksida." },
  { "en": "Apa Itu 'Lapisan Oksida'?", "id": "Silikon Dioksida (SiO₂) Sebagai Isolator." },
  { "en": "Apa Itu 'Etching'?", "id": "Proses Menghilangkan Material." },
  { "en": "Apa Itu 'Deposisi'?", "id": "Proses Menambahkan Lapisan Material." },
  { "en": "Apa Itu 'Metalisasi'?", "id": "Deposisi Logam Untuk Interkoneksi." },
  { "en": "Apa Itu 'Interkoneksi'?", "id": "Jalur Logam Penghubung Komponen." },
  { "en": "Apa Itu 'Pinout'?", "id": "Diagram Konfigurasi Pin IC." },
  { "en": "Apa Kepanjangan 'DIP (Dual In-line Package)'?", "id": "Paket Dual In-line." },
  { "en": "Apa Kepanjangan 'SOP (Small Outline Package)'?", "id": "Paket Garis Kecil." },
  { "en": "Apa Kepanjangan 'QFP (Quad Flat Package)'?", "id": "Paket Datar Empat Sisi." },
  { "en": "Apa Kepanjangan 'BGA (Ball Grid Array)'?", "id": "Array Kisi Bola." },
  { "en": "Apa Itu 'Transistor'?", "id": "Saklar Semikonduktor." },
  { "en": "Apa Itu 'Dioda'?", "id": "Komponen Penyearah Arus." },
  { "en": "Apa Itu 'Resistor'?", "id": "Komponen Penghambat Arus." },
  { "en": "Apa Itu 'Kapasitor'?", "id": "Komponen Penyimpan Muatan Listrik." },
  { "en": "Bisakah 'Resistor' Dibuat Dalam IC?", "id": "Ya, Dengan Mendoping Wilayah Silikon." },
  { "en": "Bisakah 'Kapasitor' Dibuat Dalam IC?", "id": "Ya, Menggunakan Struktur MOS atau Sambungan." },
  { "en": "Apa Itu 'Layout' IC?", "id": "Gambar Fisik Geometris Sirkuit." },
  { "en": "Apa Kepanjangan 'EDA (Electronic Design Automation)'?", "id": "Otomatisasi Desain Elektronik." },
  { "en": "Apa Itu 'Fabless'?", "id": "Perusahaan Desain IC Tanpa Pabrik." },
  { "en": "Apa Itu 'Foundry'?", "id": "Pabrik Pembuat IC Untuk Perusahaan Lain." },
  { "en": "Apa Kepanjangan 'ASIC (Application-Specific IC)'?", "id": "IC (Integrated Circuit) Aplikasi Spesifik." },
  { "en": "Apa Kepanjangan 'FPGA (Field-Programmable Gate Array)'?", "id": "Array Gerbang Terprogram Lapangan." },
  { "en": "Apa Itu 'Ruang Bersih (Cleanroom)'?", "id": "Lingkungan Produksi IC Bebas Debu." },
  { "en": "Mengapa 'Cleanroom' Penting?", "id": "Debu Dapat Merusak Sirkuit Mikro." },
  { "en": "Apa Itu 'Yield'?", "id": "Persentase Chip Fungsional Per Wafer." },
  { "en": "Apa Itu 'Die Sort'?", "id": "Proses Memilah Die Baik Dan Buruk." },
  { "en": "Apa Itu 'Wire Bonding'?", "id": "Menyambung Die Ke Paket Dengan Kawat." },
  { "en": "Apa Itu 'Enkapsulasi'?", "id": "Melindungi Die Dengan Bahan Plastik." },
  { "en": "Apa Itu 'Disipasi Daya'?", "id": "Energi Yang Diubah IC Menjadi Panas." },
  { "en": "Mengapa 'IC' Menjadi Panas?", "id": "Karena Adanya Disipasi Daya." },
  { "en": "Apa Fungsi 'Heatsink'?", "id": "Membantu Mendinginkan IC (Integrated Circuit)." },
  { "en": "Apa Itu 'Keluarga Logika'?", "id": "Grup IC Digital Dengan Teknologi Sama." },
  { "en": "Apa Kepanjangan 'TTL (Transistor-Transistor Logic)'?", "id": "Logika Transistor-Transistor." },
  { "en": "Apa Kepanjangan 'CMOS (Complementary MOS)'?", "id": "MOS (Metal-Oxide-Semiconductor) Komplementer." },
  { "en": "Apa Itu 'Tegangan Suplai'?", "id": "Tegangan Yang Dibutuhkan IC." },
  { "en": "Apa Itu 'Datasheet'?", "id": "Dokumen Spesifikasi Teknis IC." },
  { "en": "Apa Itu 'ESD (Electrostatic Discharge)'?", "id": "Pelepasan Listrik Statis." },
  { "en": "Mengapa 'ESD' Berbahaya Untuk IC?", "id": "Dapat Merusak Struktur Mikro Internal." },
  { "en": "Apa Itu 'IC Linear'?", "id": "Nama Lain Untuk IC Analog." },
  { "en": "Apa Itu 'Latch-up'?", "id": "Kondisi Hubung Singkat Parasit CMOS." },
  { "en": "Apa Itu 'Skematik'?", "id": "Diagram Logis Sebuah Sirkuit." },
  { "en": "Apa Itu 'Node Teknologi'?", "id": "Ukuran Fitur Terkecil Dalam Fabrikasi." },
  { "en": "Contoh 'Node Teknologi'?", "id": "7nm, 5nm, 3nm." },
  { "en": "Apa Itu 'Interkoneksi Tembaga (Copper)'?", "id": "Penggunaan Tembaga Untuk Jalur Logam." },
  { "en": "Apa Itu 'Dielektrik Low-k'?", "id": "Isolator Untuk Mengurangi Tundaan Sinyal." },
  { "en": "Apa Itu 'Sirkuit Hibrida'?", "id": "Sirkuit Dengan Komponen IC Dan Diskrit." },
  { "en": "Apa Itu 'Pengujian Wafer'?", "id": "Menguji IC Saat Masih Berbentuk Wafer." },
  { "en": "Apa Itu 'Pengujian Akhir'?", "id": "Menguji IC Setelah Proses Pengemasan." },
  { "en": "Apa Itu 'Binning'?", "id": "Mengelompokkan IC Berdasarkan Kinerja." },
  { "en": "Apa Itu 'Kloning IC'?", "id": "Proses Meniru Desain IC Ilegal." },
  { "en": "Apa Itu 'Reverse Engineering'?", "id": "Menganalisis Produk Untuk Tahu Desainnya." },
  { "en": "Apa Kepanjangan 'SoC (System on Chip)'?", "id": "Sistem Dalam Sebuah Chip." },
  { "en": "Apa Kepanjangan 'SiP (System in Package)'?", "id": "Sistem Dalam Sebuah Paket." },
  { "en": "Apa Beda 'SoC' Dan 'SiP'?", "id": "SoC Satu Die, SiP Banyak Die." },
  { "en": "Apa Itu 'Chiplet'?", "id": "Die Kecil Modular Untuk Dibangun Sistem." },
  { "en": "Apa Itu 'Integrasi 3D'?", "id": "Menumpuk Die Secara Vertikal." },
  { "en": "Apa Kepanjangan 'TSV (Through-Silicon Via)'?", "id": "Via Melalui Silikon." },
  { "en": "Apa Fungsi 'TSV (Through-Silicon Via)'?", "id": "Koneksi Vertikal Antar Die Bertumpuk." },
  { "en": "Apa Itu 'Passivasi'?", "id": "Lapisan Pelindung Terakhir Di Atas Die." },
  { "en": "Bahan Apa Untuk 'Passivasi'?", "id": "Silikon Nitrida Atau Polimida." },
  { "en": "Apa Itu 'IC Power Management (PMIC)'?", "id": "IC (Integrated Circuit) Pengelola Daya." },
  { "en": "Apa Itu 'IC RF (Radio Frequency)'?", "id": "IC (Integrated Circuit) Untuk Sinyal Frekuensi Radio." },
  { "en": "Apa Itu 'IC MEMS (Microelectromechanical Systems)'?", "id": "IC (Integrated Circuit) Dengan Struktur Mekanis Mikro." },
  { "en": "Contoh 'IC MEMS'?", "id": "Akselerometer Dan Giroskop." },
  { "en": "Apa Itu 'IC Fotonik'?", "id": "IC (Integrated Circuit) Yang Memproses Sinyal Cahaya." },
  { "en": "Apa Itu Desain 'RTL (Register-Transfer Level)'?", "id": "Mendeskripsikan Aliran Data Antar Register." },
  { "en": "Apa Fungsi 'ADC (Analog-to-Digital Converter)'?", "id": "Mengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Fungsi 'DAC (Digital-to-Analog Converter)'?", "id": "Mengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Paket 'PGA (Pin Grid Array)'?", "id": "Paket IC (Integrated Circuit) Dengan Kisi Pin." },
  { "en": "Apa Itu Paket 'LGA (Land Grid Array)'?", "id": "Paket IC (Integrated Circuit) Kontak Datar." },
  { "en": "Apa Kepanjangan 'QFN (Quad Flat No-lead)'?", "id": "Paket Datar Empat Sisi Tanpa Kaki." },
  { "en": "Apa Fungsi 'Flip-Flop'?", "id": "Elemen Dasar Penyimpan Satu Bit." },
  { "en": "Apa Fungsi 'Register'?", "id": "Menyimpan Sekelompok Bit Data." },
  { "en": "Apa Fungsi 'Counter'?", "id": "Menghitung Pulsa Sinyal Clock." },
  { "en": "Apa Kepanjangan 'ALU (Arithmetic Logic Unit)'?", "id": "Unit Aritmetika Dan Logika." },
  { "en": "Apa Itu 'CMP (Chemical-Mechanical Polishing)'?", "id": "Proses Meratakan Permukaan Wafer." },
  { "en": "Apa Beda 'VCC' Dan 'VDD'?", "id": "VCC Bipolar, VDD Untuk MOS." },
  { "en": "Apa Kepanjangan 'GPIO (General Purpose I/O)'?", "id": "Input/Output Tujuan Umum." },
  { "en": "Apa Kepanjangan 'PWM (Pulse Width Modulation)'?", "id": "Modulasi Lebar Pulsa." },
  { "en": "Apa Itu 'EPROM (Erasable PROM)'?", "id": "PROM (Programmable ROM) Yang Bisa Dihapus." },
  { "en": "Bagaimana Cara Menghapus 'EPROM'?", "id": "Dengan Sinar UV (UltraViolet)." },
  { "en": "Apa Kepanjangan 'EEPROM (Electrically EPROM)'?", "id": "EPROM (Erasable PROM) Yang Dihapus Listrik." },
  { "en": "Apa Itu 'Memori Flash'?", "id": "Jenis EEPROM (Electrically EPROM) Cepat." },
  { "en": "Apa Fungsi 'UART (Universal Asynchronous Receiver-Transmitter)'?", "id": "Untuk Komunikasi Serial Asinkron." },
  { "en": "Apa Fungsi 'SPI (Serial Peripheral Interface)'?", "id": "Untuk Komunikasi Serial Sinkron." },
  { "en": "Apa Fungsi 'I2C (Inter-Integrated Circuit)'?", "id": "Bus Komunikasi Dua Kawat." },
  { "en": "Apa Itu 'Sirkuit Hibrida'?", "id": "Gabungan Teknologi IC Dan Diskrit." },
  { "en": "Apa Itu 'Die Attach'?", "id": "Proses Melekatkan Die Ke Paket." },
  { "en": "Apa Itu 'Thermal Pad'?", "id": "Area Pembuang Panas Di Bawah IC." },
  { "en": "Apa Itu 'Solder Ball'?", "id": "Bola Timah Untuk Koneksi BGA." },
  { "en": "Apa Itu 'Proses Czochralski'?", "id": "Metode Menumbuhkan Ingot Silikon (Si)." },
  { "en": "Apa Itu 'Polisilicon'?", "id": "Silikon (Si) Polikristalin." },
  { "en": "Apa Fungsi 'Polisilicon'?", "id": "Sebagai Material Gerbang (Gate)." },
  { "en": "Apa Itu 'Kontak'?", "id": "Koneksi Antara Lapisan Logam Dan Silikon." },
  { "en": "Apa Itu 'Via'?", "id": "Koneksi Vertikal Antar Lapisan Logam." },
  { "en": "Apa Itu 'Mask'?", "id": "Template Pola Untuk Proses Fotolitografi." },
  { "en": "Apa Itu 'Reticle'?", "id": "Nama Lain Untuk Mask." },
  { "en": "Apa Itu 'Stepper'?", "id": "Mesin Fotolitografi Untuk Proyeksi Pola." },
  { "en": "Apa Itu 'Photoresist'?", "id": "Cairan Peka Cahaya Untuk Litografi." },
  { "en": "Apa Itu 'Plasma Etching'?", "id": "Proses Etsa Menggunakan Gas Plasma." },
  { "en": "Apa Itu 'Implantasi Ion'?", "id": "Metode Doping Menembakkan Ion." },
  { "en": "Apa Itu 'Annealing'?", "id": "Pemanasan Untuk Mengaktifkan Dopan." },
  { "en": "Apa Itu 'Antifuse'?", "id": "Elemen Sekali Program Dalam PLD." },
  { "en": "Apa Kepanjangan 'PLD (Programmable Logic Device)'?", "id": "Perangkat Logika Terprogram." },
  { "en": "Apa Kepanjangan 'CPLD (Complex PLD)'?", "id": "PLD (Programmable Logic Device) Kompleks." },
  { "en": "Apa Itu 'SRAM Cell'?", "id": "Elemen Penyimpan Konfigurasi FPGA." },
  { "en": "Apa Itu 'Bitstream'?", "id": "File Konfigurasi Untuk Memprogram FPGA." },
  { "en": "Apa Kepanjangan 'JTAG (Joint Test Action Group)'?", "id": "Standar Untuk Pengujian Dan Debugging." },
  { "en": "Apa Itu 'Buffer'?", "id": "IC (Integrated Circuit) Penguat Sinyal Digital." },
  { "en": "Apa Itu 'Inverter'?", "id": "IC (Integrated Circuit) Yang Membalik Logika." },
  { "en": "Apa Itu 'Transceiver'?", "id": "IC (Integrated Circuit) Pengirim Dan Penerima." },
  { "en": "Apa Itu 'Level Shifter'?", "id": "IC (Integrated Circuit) Pengubah Level Tegangan." },
  { "en": "Apa Itu 'IC Regulator Linier'?", "id": "IC (Integrated Circuit) Penstabil Tegangan Efisiensi Rendah." },
  { "en": "Apa Itu 'IC Regulator Switching'?", "id": "IC (Integrated Circuit) Penstabil Tegangan Efisiensi Tinggi." },
  { "en": "Apa Kepanjangan 'LDO (Low-Dropout)'?", "id": "Regulator Jatuh Tegangan Rendah." },
  { "en": "Apa Itu 'IC Audio Amplifier'?", "id": "IC (Integrated Circuit) Untuk Menguatkan Sinyal Audio." },
  { "en": "Apa Itu 'IC RF Amplifier'?", "id": "IC (Integrated Circuit) Penguat Frekuensi Radio." },
  { "en": "Apa Itu 'IC Mixer'?", "id": "IC (Integrated Circuit) Pencampur Sinyal Frekuensi." },
  { "en": "Apa Itu 'IC Osilator'?", "id": "IC (Integrated Circuit) Pembangkit Frekuensi." },
  { "en": "Apa Kepanjangan 'VCO (Voltage-Controlled Oscillator)'?", "id": "Osilator Yang Dikontrol Tegangan." },
  { "en": "Apa Kepanjangan 'PLL (Phase-Locked Loop)'?", "id": "Lup Terkunci Fasa." },
  { "en": "Apa Itu 'IC Sensor'?", "id": "IC (Integrated Circuit) Dengan Fungsi Penginderaan." },
  { "en": "Apa Itu 'IC Hall Effect'?", "id": "IC (Integrated Circuit) Sensor Medan Magnet." },
  { "en": "Apa Itu 'IC Temperature Sensor'?", "id": "IC (Integrated Circuit) Sensor Suhu." },
  { "en": "Apa Itu 'IC Accelerometer'?", "id": "IC (Integrated Circuit) Sensor Percepatan." },
  { "en": "Apa Itu 'IC Gyroscope'?", "id": "IC (Integrated Circuit) Sensor Rotasi." },
  { "en": "Apa Itu 'Sistem Tertanam (Embedded System)'?", "id": "Sistem Komputer Dengan Fungsi Khusus." },
  { "en": "Apa Itu 'Firmware'?", "id": "Perangkat Lunak Tertanam Dalam IC." },
  { "en": "Apa Itu 'Node Proses'?", "id": "Nama Lain Untuk Node Teknologi." },
  { "en": "Apa Itu 'Power Density'?", "id": "Disipasi Daya Per Satuan Area." },
  { "en": "Apa Itu 'Delay' Sirkuit?", "id": "Waktu Tunda Sinyal Melewati Sirkuit." },
  { "en": "Apa Itu 'Noise Margin'?", "id": "Toleransi Sirkuit Terhadap Derau." },
  { "en": "Apa Itu 'Crosstalk'?", "id": "Gangguan Antar Jalur Sinyal Berdekatan." },
  { "en": "Apa Itu 'Parasitic Capacitance'?", "id": "Kapasitansi Tak Diinginkan Dalam IC." },
  { "en": "Apa Itu 'Parasitic Resistance'?", "id": "Resistansi Tak Diinginkan Dalam IC." },
  { "en": "Apa Itu 'Parasitic Inductance'?", "id": "Induktansi Tak Diinginkan Dalam IC." },
  { "en": "Apa Itu 'Gerbang Universal'?", "id": "Gerbang NAND Atau NOR." },
  { "en": "Apa Itu 'Tabel Kebenaran'?", "id": "Tabel Mendefinisikan Fungsi Logika." },
  { "en": "Apa Itu 'Aljabar Boolean'?", "id": "Matematika Untuk Sirkuit Logika." },
  { "en": "Apa Itu 'Peta Karnaugh'?", "id": "Metode Grafis Penyederhanaan Logika." },
  { "en": "Apa Itu 'Logika Kombinasional'?", "id": "Sirkuit Tanpa Memori." },
  { "en": "Apa Itu 'Logika Sekuensial'?", "id": "Sirkuit Dengan Memori." },
  { "en": "Apa Itu 'State Machine'?", "id": "Model Desain Untuk Logika Sekuensial." },
  { "en": "Apa Itu 'Clock'?", "id": "Sinyal Denyut Untuk Sinkronisasi." },
  { "en": "Apa Itu 'Clock Rate'?", "id": "Frekuensi Sinyal Clock." },
  { "en": "Apa Itu 'Setup Time'?", "id": "Waktu Data Stabil Sebelum Clock." },
  { "en": "Apa Itu 'Hold Time'?", "id": "Waktu Data Stabil Setelah Clock." },
  { "en": "Apa Itu 'Metastabilitas'?", "id": "Keadaan Output Tidak Menentu." },
  { "en": "Apa Itu 'IC Analog Switch'?", "id": "IC (Integrated Circuit) Saklar Untuk Sinyal Analog." },
  { "en": "Apa Itu 'IC Multiplexer Analog'?", "id": "IC (Integrated Circuit) Pemilih Sinyal Analog." },
  { "en": "Apa Itu 'IC Power Driver'?", "id": "IC (Integrated Circuit) Penggerak Beban Daya." },
  { "en": "Apa Itu 'IC Motor Driver'?", "id": "IC (Integrated Circuit) Pengendali Motor." },
  { "en": "Apa Itu 'H-Bridge'?", "id": "Sirkuit Untuk Mengubah Arah Motor." },
  { "en": "Apa Itu 'IC Display Driver'?", "id": "IC (Integrated Circuit) Pengendali Layar." },
  { "en": "Apa Itu 'IC Optocoupler'?", "id": "IC (Integrated Circuit) Isolator Berbasis Cahaya." },
  { "en": "Apa Itu 'IC Real-Time Clock (RTC)'?", "id": "IC (Integrated Circuit) Penyimpan Waktu." },
  { "en": "Apa Itu 'Baterai Cadangan RTC'?", "id": "Baterai Untuk Menjaga Waktu Tetap Aktif." },
  { "en": "Apa Kepanjangan 'MTBF (Mean Time Between Failures)'?", "id": "Waktu Rata-rata Antar Kegagalan." },
  { "en": "Apa Kepanjangan 'MTTF (Mean Time To Failure)'?", "id": "Waktu Rata-rata Hingga Gagal." },
  { "en": "Apa Itu 'Infant Mortality'?", "id": "Tingkat Kegagalan Tinggi Di Awal." },
  { "en": "Apa Itu 'Wear-Out'?", "id": "Kegagalan Akibat Usia Komponen." },
  { "en": "Apa Kepanjangan 'CVD (Chemical Vapor Deposition)'?", "id": "Deposisi Uap Kimia." },
  { "en": "Apa Kepanjangan 'PVD (Physical Vapor Deposition)'?", "id": "Deposisi Uap Fisik." },
  { "en": "Apa Bahan Semikonduktor Selain Silikon?", "id": "GaAs (Gallium Arsenide), GaN (Gallium Nitride)." },
  { "en": "Apa Itu 'Kelas Cleanroom'?", "id": "Standar Kebersihan Ruang Produksi." },
  { "en": "Apa Kepanjangan 'ATE (Automated Test Equipment)'?", "id": "Peralatan Uji Otomatis." },
  { "en": "Apa Kepanjangan 'DUT (Device Under Test)'?", "id": "Perangkat Yang Sedang Diuji." },
  { "en": "Apa Kepanjangan 'LVS (Layout Versus Schematic)'?", "id": "Tata Letak Versus Skematik." },
  { "en": "Apa Kepanjangan 'DRC (Design Rule Check)'?", "id": "Pemeriksaan Aturan Desain." },
  { "en": "Apa Itu 'Simulasi Sirkuit'?", "id": "Memverifikasi Desain Dengan Perangkat Lunak." },
  { "en": "Apa Kepanjangan 'SOIC (Small Outline IC)'?", "id": "IC (Integrated Circuit) Garis Kecil." },
  { "en": "Apa Kepanjangan 'PLCC (Plastic Leaded Chip Carrier)'?", "id": "Pembawa Chip Berplumbum Plastik." },
  { "en": "Apa Kepanjangan 'TSOP (Thin SOP)'?", "id": "SOP (Small Outline Package) Tipis." },
  { "en": "Apa Itu 'Theta-JA'?", "id": "Resistansi Termal Junction-ke-Udara." },
  { "en": "Apa Itu 'Theta-JC'?", "id": "Resistansi Termal Junction-ke-Casing." },
  { "en": "Apa Fungsi 'Multiplexer (MUX)'?", "id": "Memilih Satu Input Dari Banyak." },
  { "en": "Apa Fungsi 'Demultiplexer (DEMUX)'?", "id": "Mengirim Input Ke Satu Output." },
  { "en": "Apa Fungsi 'Encoder'?", "id": "Mengubah Sinyal Input Menjadi Kode." },
  { "en": "Apa Fungsi 'Decoder'?", "id": "Mengubah Kode Menjadi Sinyal Output." },
  { "en": "Apa Itu 'Logika Tri-State'?", "id": "Logika Dengan Keadaan Impedansi Tinggi." },
  { "en": "Apa Kepanjangan 'SDRAM (Synchronous DRAM)'?", "id": "DRAM (Dynamic RAM) Sinkron." },
  { "en": "Apa Kepanjangan 'DDR (Double Data Rate)'?", "id": "Laju Data Ganda." },
  { "en": "Apa Itu 'Memori Cache'?", "id": "Memori Cepat Antara CPU Dan RAM." },
  { "en": "Apa Itu 'Memori Volatil'?", "id": "Memori Yang Hilang Tanpa Daya." },
  { "en": "Apa Itu 'Memori Non-Volatil'?", "id": "Memori Yang Tetap Tanpa Daya." },
  { "en": "Apa Kepanjangan 'SMPS (Switch-Mode Power Supply)'?", "id": "Catu Daya Mode Saklar." },
  { "en": "Apa Itu 'Konverter Buck'?", "id": "SMPS (Switch-Mode Power Supply) Penurun Tegangan." },
  { "en": "Apa Itu 'Konverter Boost'?", "id": "SMPS (Switch-Mode Power Supply) Penaik Tegangan." },
  { "en": "Apa Itu 'Arus Quiescent (Iq)'?", "id": "Arus Yang Dikonsumsi Saat Diam." },
  { "en": "Apa Itu 'Slew Rate'?", "id": "Laju Perubahan Tegangan Output." },
  { "en": "Apa Itu 'Ringing' Sinyal?", "id": "Osilasi Sinyal Tak Diinginkan." },
  { "en": "Apa Itu 'Overshoot'?", "id": "Sinyal Melebihi Level Tegangan Target." },
  { "en": "Apa Itu 'Undershoot'?", "id": "Sinyal Jatuh Di Bawah Level Target." },
  { "en": "Apa Kepanjangan 'JEDEC'?", "id": "Joint Electron Device Engineering Council." },
  { "en": "Apa Kepanjangan 'IEEE'?", "id": "Institute of Electrical and Electronics Engineers." },
  { "en": "Apa Kepanjangan 'IPC'?", "id": "Association Connecting Electronics Industries." },
  { "en": "Apa Itu 'Gerbang NAND'?", "id": "Gerbang AND Yang Diikuti NOT." },
  { "en": "Apa Itu 'Gerbang NOR'?", "id": "Gerbang OR Yang Diikuti NOT." },
  { "en": "Apa Itu 'Gerbang XOR'?", "id": "Gerbang Exclusive OR." },
  { "en": "Apa Itu 'Gerbang XNOR'?", "id": "Gerbang Exclusive NOR." },
  { "en": "Apa Itu 'Die Bonding'?", "id": "Nama Lain Untuk Die Attach." },
  { "en": "Apa Itu 'Flip-Chip'?", "id": "Metode Pemasangan Die Terbalik." },
  { "en": "Apa Itu 'Underfill'?", "id": "Material Pengisi Celah Flip-Chip." },
  { "en": "Apa Itu 'Lead Frame'?", "id": "Kerangka Logam Kaki-kaki Paket." },
  { "en": "Apa Itu 'Pad' Pada Die?", "id": "Area Kontak Logam Pada Die." },
  { "en": "Apa Itu 'Scribe Line'?", "id": "Garis Antar Die Untuk Pemotongan." },
  { "en": "Apa Itu 'Dicing'?", "id": "Proses Memotong Wafer Menjadi Die." },
  { "en": "Apa Itu 'Probe Card'?", "id": "Kartu Jarum Untuk Pengujian Wafer." },
  { "en": "Apa Itu 'Burn-in'?", "id": "Pengujian IC Pada Suhu Tinggi." },
  { "en": "Tujuan 'Burn-in'?", "id": "Mendeteksi Kegagalan Dini." },
  { "en": "Apa Itu 'ESD Wrist Strap'?", "id": "Gelang Anti-statis." },
  { "en": "Apa Itu 'Isolator'?", "id": "Bahan Yang Tidak Menghantarkan Listrik." },
  { "en": "Apa Itu 'Konduktor'?", "id": "Bahan Yang Menghantarkan Listrik." },
  { "en": "Apa Itu 'Semikonduktor'?", "id": "Bahan Antara Konduktor Dan Isolator." },
  { "en": "Apa Itu 'Semikonduktor Intrinsik'?", "id": "Semikonduktor Murni Tanpa Doping." },
  { "en": "Apa Itu 'Semikonduktor Ekstrinsik'?", "id": "Semikonduktor Yang Sudah Didoping." },
  { "en": "Apa Itu 'Tipe-N'?", "id": "Semikonduktor Dengan Kelebihan Elektron." },
  { "en": "Apa Itu 'Tipe-P'?", "id": "Semikonduktor Dengan Kelebihan Lubang (Hole)." },
  { "en": "Apa Itu 'Sambungan P-N'?", "id": "Batas Antara Material Tipe-P Tipe-N." },
  { "en": "Apa Fungsi Dasar 'Sambungan P-N'?", "id": "Membentuk Dioda." },
  { "en": "Apa Kepanjangan 'BJT (Bipolar Junction Transistor)'?", "id": "Transistor Sambungan Bipolar." },
  { "en": "Apa Kepanjangan 'FET (Field-Effect Transistor)'?", "id": "Transistor Efek Medan." },
  { "en": "Apa Beda 'BJT' dan 'FET'?", "id": "BJT Dikontrol Arus, FET Tegangan." },
  { "en": "Apa Kepanjangan 'MOSFET (Metal-Oxide-Semiconductor FET)'?", "id": "FET (Field-Effect Transistor) Oksida-Logam-Semikonduktor." },
  { "en": "Apa Tiga Terminal 'MOSFET'?", "id": "Gate, Drain, Dan Source." },
  { "en": "Apa Tiga Terminal 'BJT'?", "id": "Base, Collector, Dan Emitter." },
  { "en": "Apa Itu 'Wafer Backgrinding'?", "id": "Proses Menipiskan Bagian Belakang Wafer." },
  { "en": "Apa Itu 'IC Kustom'?", "id": "IC (Integrated Circuit) Didesain Fungsi Tertentu." },
  { "en": "Apa Itu 'IC Standar'?", "id": "IC (Integrated Circuit) Fungsi Umum." },
  { "en": "Apa Itu 'Sirkuit Terpadu Hibrida'?", "id": "IC (Integrated Circuit) Dengan Komponen Tambahan." },
  { "en": "Apa Itu 'Pita Frekuensi'?", "id": "Rentang Frekuensi Operasi IC." },
  { "en": "Apa Itu 'Suhu Operasi'?", "id": "Rentang Suhu Kerja IC." },
  { "en": "Apa Itu 'Kelas Komersial'?", "id": "IC (Integrated Circuit) Untuk Suhu 0-70°C." },
  { "en": "Apa Itu 'Kelas Industri'?", "id": "IC (Integrated Circuit) Untuk Suhu -40-85°C." },
  { "en": "Apa Itu 'Kelas Otomotif'?", "id": "IC (Integrated Circuit) Suhu Lebih Tinggi." },
  { "en": "Apa Itu 'Kelas Militer'?", "id": "IC (Integrated Circuit) Suhu Paling Ekstrem." },
  { "en": "Apa Itu 'Derating'?", "id": "Mengoperasikan Komponen Di Bawah Rating." },
  { "en": "Mengapa 'Derating' Penting?", "id": "Untuk Meningkatkan Keandalan." },
  { "en": "Apa Itu 'Papan Evaluasi'?", "id": "Papan Sirkuit Untuk Menguji IC." },
  { "en": "Apa Itu 'Kit Pengembangan'?", "id": "Paket Untuk Pengembangan Produk IC." },
  { "en": "Apa Kepanjangan 'IDE (Integrated Development Environment)'?", "id": "Lingkungan Pengembangan Terpadu." },
  { "en": "Apa Itu 'Compiler'?", "id": "Penerjemah Kode Ke Bahasa Mesin." },
  { "en": "Apa Itu 'Debugger'?", "id": "Alat Untuk Mencari Kesalahan Program." },
  { "en": "Apa Itu 'Emulator In-Circuit (ICE)'?", "id": "Alat Debugging Perangkat Keras." },
  { "en": "Apa Itu 'Logic Analyzer'?", "id": "Alat Menganalisis Sinyal Digital." },
  { "en": "Apa Itu 'Osiloskop'?", "id": "Alat Melihat Bentuk Gelombang Sinyal." },
  { "en": "Apa Itu 'Library' Perangkat Lunak?", "id": "Kumpulan Kode Siap Pakai." },
  { "en": "Apa Kepanjangan 'API (Application Programming Interface)'?", "id": "Antarmuka Pemrograman Aplikasi." },
  { "en": "Apa Itu 'IP Core (Intellectual Property)'?", "id": "Blok Desain Sirkuit Berlisensi." },
  { "en": "Apa Itu 'Hard IP Core'?", "id": "Blok IP (Intellectual Property) Berupa Layout Fisik." },
  { "en": "Apa Itu 'Soft IP Core'?", "id": "Blok IP (Intellectual Property) Berupa Kode HDL." },
  { "en": "Apa Fungsi 'Lapisan Penghalang (Barrier Layer)'?", "id": "Mencegah Difusi Antara Lapisan Logam." },
  { "en": "Mengapa 'Litografi Imersi' Digunakan?", "id": "Untuk Meningkatkan Resolusi Cetak Pola." },
  { "en": "Cairan Apa Yang Digunakan 'Litografi Imersi'?", "id": "Air Deionisasi Sangat Murni." },
  { "en": "Bagaimana 'Kapasitor Trench DRAM' Dibuat?", "id": "Menggali Parit Dalam Ke Silikon (Si)." },
  { "en": "Apa Itu 'Binning' Setelah Pengujian?", "id": "Mengelompokkan Chip Berdasarkan Kecepatan." },
  { "en": "Bagaimana 'HEMT (High-Electron-Mobility Transistor)' Dibuat?", "id": "Dengan Struktur Heterojunction." },
  { "en": "Bagaimana 'IGBT (Insulated-Gate Bipolar Transistor)' Dibuat?", "id": "Gabungan Struktur BJT Dan MOSFET." },
  { "en": "Apa Itu 'Manajemen Termal' IC?", "id": "Mengontrol Panas Yang Dihasilkan Chip." },
  { "en": "Bagaimana 'Sensor Tekanan Kapasitif' Bekerja?", "id": "Mengukur Perubahan Jarak Antar Pelat." },
  { "en": "Apa Itu 'Orientasi Kristal' Wafer?", "id": "Arah Bidang Atom Dalam Kristal." },
  { "en": "Apa Arti Orientasi '<100>'?", "id": "Orientasi Umum Untuk Fabrikasi CMOS." },
  { "en": "Apa Fungsi 'Wafer Flat' Atau 'Notch'?", "id": "Menandai Orientasi Kristal." },
  { "en": "Apa Itu 'Wafer Dicing Tape'?", "id": "Pita Perekat Penahan Die Saat Dipotong." },
  { "en": "Apa Itu 'Die Attach Adhesive'?", "id": "Perekat Khusus Untuk Melekatkan Die." },
  { "en": "Apa Fungsi 'Underfill'?", "id": "Memperkuat Koneksi Bola Solder." },
  { "en": "Apa Itu 'Handler' Pengujian Akhir?", "id": "Mesin Otomatis Penanganan Chip Uji." },
  { "en": "Bagaimana 'Kapasitor Elektrolit' Dibuat?", "id": "Menggulung Foil Anoda, Katoda, Separator." },
  { "en": "Apa Itu 'Anodizing' Foil Aluminium?", "id": "Menumbuhkan Lapisan Oksida Dielektrik." },
  { "en": "Bagaimana 'Resistor Karbon' Dibuat?", "id": "Melapisi Batang Keramik Dengan Karbon." },
  { "en": "Bagaimana 'Resistor Metal Film' Dibuat?", "id": "Deposisi Vakum Lapisan Logam Tipis." },
  { "en": "Apa Itu 'Laser Trimming'?", "id": "Menyesuaikan Nilai Resistor Dengan Laser." },
  { "en": "Bagaimana 'Kapasitor Film' Dibuat?", "id": "Menggulung Lapisan Film Plastik Metalized." },
  { "en": "Bagaimana 'Kabel Koaksial' Dibuat?", "id": "Menyatukan Konduktor, Insulator, Pelindung." },
  { "en": "Apa Itu 'Proses Drawing' Kawat?", "id": "Menarik Logam Melalui Cetakan Kecil." },
  { "en": "Apa Itu 'Annealing' Kawat?", "id": "Melembutkan Kawat Setelah Proses Drawing." },
  { "en": "Bagaimana 'Termokopel' Dibuat?", "id": "Menyambungkan Dua Jenis Kawat Logam." },
  { "en": "Apa Itu 'Sambungan Panas' Termokopel?", "id": "Ujung Yang Mengukur Suhu." },
  { "en": "Apa Itu 'Sambungan Dingin' Termokopel?", "id": "Ujung Referensi Yang Diketahui Suhunya." },
  { "en": "Bagaimana 'Strain Gauge' Dibuat?", "id": "Membentuk Pola Foil Logam Tipis." },
  { "en": "Bagaimana 'Load Cell' Dirakit?", "id": "Menempelkan Strain Gauge Ke Struktur Logam." },
  { "en": "Apa Itu 'Wafer Bumping'?", "id": "Proses Menambahkan Bola Solder Ke Wafer." },
  { "en": "Apa Itu 'Solder Bump'?", "id": "Bola Solder Kecil Untuk Koneksi Flip-Chip." },
  { "en": "Apa Itu 'Ball Grid Array (BGA)'?", "id": "Jenis Paket IC (Integrated Circuit) Bola Solder." },
  { "en": "Bagaimana 'Paket BGA' Dibuat?", "id": "Melekatkan Die Ke Substrat Dengan Bola." },
  { "en": "Apa Itu 'Rework' Dalam Perakitan?", "id": "Proses Memperbaiki Papan Sirkuit." },
  { "en": "Apa Itu 'X-ray Inspection'?", "id": "Inspeksi Sambungan Solder Yang Tersembunyi." },
  { "en": "Mengapa 'X-ray' Digunakan Untuk BGA?", "id": "Untuk Melihat Koneksi Di Bawah Chip." },
  { "en": "Apa Itu 'Pasta Solder Bebas Timbal'?", "id": "Solder Tanpa Kandungan Timbal (Pb)." },
  { "en": "Mengapa Beralih Ke 'Solder Bebas Timbal'?", "id": "Karena Alasan Lingkungan Dan Kesehatan." },
  { "en": "Apa Kepanjangan 'RoHS'?", "id": "Restriction of Hazardous Substances." },
  { "en": "Apa Itu 'Standar IPC'?", "id": "Standar Industri Untuk Perakitan Elektronik." },
  { "en": "Apa Itu 'Proses Manufaktur Aditif'?", "id": "Membangun Komponen Lapis Demi Lapis." },
  { "en": "Apa Itu 'Antena PCB (Printed Circuit Board)'?", "id": "Antena Dibuat Dari Jalur Tembaga." },
  { "en": "Bagaimana 'Osilator MEMS' Dibuat?", "id": "Membuat Struktur Resonator Di Atas Silikon." },
  { "en": "Apa Itu 'Stiction' Dalam MEMS?", "id": "Masalah Pelekatan Antar Struktur Mikro." },
  { "en": "Apa Itu 'Getaran Kisi'?", "id": "Getaran Termal Atom Dalam Kristal." },
  { "en": "Apa Itu 'Fonon'?", "id": "Kuantum Energi Dari Getaran Kisi." },
  { "en": "Bagaimana 'Kristal Sintetis' Dibuat?", "id": "Ditumbuhkan Dalam Kondisi Laboratorium Terkontrol." },
  { "en": "Apa Itu 'Proses Hidrotermal'?", "id": "Metode Menumbuhkan Kristal Kuarsa." },
  { "en": "Apa Itu 'Autoclave'?", "id": "Bejana Tekanan Tinggi Untuk Sintesis." },
  { "en": "Apa Itu 'Seed Crystal'?", "id": "Kristal Kecil Sebagai Titik Awal." },
  { "en": "Bagaimana 'LED (Light Emitting Diode) Putih' Dibuat?", "id": "Melapisi Chip LED Biru Dengan Fosfor." },
  { "en": "Apa Itu 'Fosfor'?", "id": "Bahan Yang Mengubah Panjang Gelombang Cahaya." },
  { "en": "Apa Itu 'Chip-on-Board (COB) LED'?", "id": "Banyak Chip LED Dipasang Langsung." },
  { "en": "Apa Keuntungan 'COB (Chip-on-Board) LED'?", "id": "Cahaya Lebih Merata Dan Padat." },
  { "en": "Apa Itu 'Quantum Dot Enhancement Film (QDEF)'?", "id": "Film Untuk Meningkatkan Warna Layar LCD." },
  { "en": "Apa Itu 'Fabrikasi Semikonduktor Senyawa'?", "id": "Pembuatan Chip Dari Bahan Seperti GaAs." },
  { "en": "Apa Tantangan 'Fabrikasi Semikonduktor Senyawa'?", "id": "Wafer Lebih Rapuh Dan Mahal." },
  { "en": "Apa Itu 'Passivation Layer'?", "id": "Lapisan Pelindung Terakhir Pada Die." },
  { "en": "Apa Fungsi 'Passivation'?", "id": "Melindungi Dari Goresan Dan Kontaminasi." },
  { "en": "Apa Itu 'Wire Sawing'?", "id": "Proses Memotong Ingot Menjadi Wafer." },
  { "en": "Apa Itu 'Slurry'?", "id": "Campuran Partikel Abrasif Dan Cairan." },
  { "en": "Apa Itu 'Lapping' Wafer?", "id": "Proses Menghaluskan Permukaan Wafer." },
  { "en": "Apa Itu 'Edge Grinding'?", "id": "Membulatkan Tepi Wafer." },
  { "en": "Apa Itu 'Wafer Cleaning'?", "id": "Membersihkan Kontaminan Dari Permukaan Wafer." },
  { "en": "Mengapa 'Wafer Cleaning' Penting?", "id": "Kontaminan Dapat Menyebabkan Cacat Sirkuit." },
  { "en": "Apa Itu 'RCA Clean'?", "id": "Prosedur Standar Pembersihan Wafer Silikon." },
  { "en": "Apa Itu 'Mask Aligner'?", "id": "Mesin Litografi Untuk Penjajaran Mask." },
  { "en": "Apa Itu 'Proximity Lithography'?", "id": "Mask Berada Sangat Dekat Dengan Wafer." },
  { "en": "Apa Itu 'Contact Lithography'?", "id": "Mask Menyentuh Langsung Wafer." },
  { "en": "Apa Itu 'Projection Lithography'?", "id": "Pola Masker Diproyesikan Ke Wafer." },
  { "en": "Apa Itu 'Reticle'?", "id": "Nama Lain Untuk Photomask." },
  { "en": "Apa Itu 'Pellicle'?", "id": "Membran Pelindung Yang Dipasang Di Mask." },
  { "en": "Apa Fungsi 'Pellicle'?", "id": "Menjaga Debu Agar Tidak Terfokus." },
  { "en": "Apa Itu 'Critical Dimension (CD)'?", "id": "Ukuran Fitur Terkecil Dalam Desain." },
  { "en": "Apa Kepanjangan 'CD-SEM (Critical Dimension SEM)'?", "id": "Mikroskop Elektron Pengukur Dimensi Kritis." },
  { "en": "Apa Itu 'Overlay'?", "id": "Akurasi Penjajaran Antar Lapisan." },
  { "en": "Apa Itu 'Hard Mask'?", "id": "Lapisan Keras (Bukan Resist) Untuk Etsa." },
  { "en": "Mengapa 'Hard Mask' Digunakan?", "id": "Untuk Mengetsa Fitur Yang Sangat Dalam." },
  { "en": "Apa Itu 'Plasma Ashing'?", "id": "Proses Menghilangkan Photoresist Dengan Plasma." },
  { "en": "Apa Itu 'Wafer Backside Coating'?", "id": "Pelapisan Bagian Belakang Wafer." },
  { "en": "Apa Itu 'Thermal Budget'?", "id": "Total Paparan Suhu Tinggi Selama Proses." },
  { "en": "Mengapa 'Thermal Budget' Penting?", "id": "Suhu Berlebih Dapat Merusak Dopan." },
  { "en": "Apa Itu 'Rapid Thermal Anneal (RTA)'?", "id": "Proses Annealing Dengan Waktu Sangat Singkat." },
  { "en": "Apa Itu 'Furnace Annealing'?", "id": "Annealing Dalam Tungku Untuk Waktu Lama." },
  { "en": "Apa Itu 'Spreading Resistance'?", "id": "Metode Mengukur Profil Resistivitas." },
  { "en": "Apa Kepanjangan 'SIMS (Secondary Ion Mass Spectrometry)'?", "id": "Spektrometri Massa Ion Sekunder." },
  { "en": "Apa Fungsi 'SIMS'?", "id": "Mengukur Profil Konsentrasi Dopan." },
  { "en": "Apa Itu 'Process Integration'?", "id": "Menggabungkan Semua Langkah Proses Fabrikasi." },
  { "en": "Apa Itu 'Process Flow'?", "id": "Urutan Langkah-langkah Dalam Fabrikasi." },
  { "en": "Apa Itu 'Scribe Line'?", "id": "Garis Antar Die Tempat Wafer Dipotong." },
  { "en": "Apa Itu 'Test Structure'?", "id": "Struktur Tes Di Scribe Line." },
  { "en": "Apa Fungsi 'Test Structure'?", "id": "Memantau Kualitas Dan Parameter Proses." },
  { "en": "Apa Itu 'Binning' Chip?", "id": "Mengelompokkan Chip Berdasarkan Hasil Tes." },
  { "en": "Apa Itu 'Tape and Reel'?", "id": "Metode Pengemasan Komponen Untuk Perakitan." },
  { "en": "Apa Penyebab 'Elektromigrasi'?", "id": "Momentum Elektron Mendorong Atom Logam." },
  { "en": "Apa Itu 'Injeksi Pembawa Panas (HCI)'?", "id": "Elektron Energi Tinggi Merusak Oksida." },
  { "en": "Apa Itu 'Latch-up'?", "id": "Hubung Singkat Parasit Dalam CMOS." },
  { "en": "Apa Fungsi 'Through-Silicon Via (TSV)'?", "id": "Koneksi Vertikal Langsung Antar Chip." },
  { "en": "Apa Itu 'Gettering'?", "id": "Proses Menjebak Kontaminan Jauh Dari Perangkat." },
  { "en": "Apa Beda 'Polisilicon' Dan 'Silikon Kristal Tunggal'?", "id": "Polisilicon Terdiri Dari Banyak Butir Kristal." },
  { "en": "Apa Keunggulan 'Galium Arsenida (GaAs)'?", "id": "Mobilitas Elektron Sangat Tinggi." },
  { "en": "Bagaimana 'Filter SAW (Surface Acoustic Wave)' Dibuat?", "id": "Pola Elektroda Di Bahan Piezoelektrik." },
  { "en": "Apa Beda 'SEM' Dan 'TEM'?", "id": "SEM (Scanning Electron Microscope) Lihat Permukaan, TEM Tembus." },
  { "en": "Apa Itu 'Stencil' Dalam Perakitan PCB?", "id": "Lembaran Logam Untuk Aplikasi Pasta Solder." },
  { "en": "Apa Itu 'Profil Reflow'?", "id": "Kurva Suhu-Waktu Untuk Proses Solder." },
  { "en": "Bagaimana 'Resistor Wirewound' Dibuat?", "id": "Melilitkan Kawat Resistif Di Inti Keramik." },
  { "en": "Bagaimana 'Kapasitor Tantalum' Dibuat?", "id": "Menggunakan Butiran Tantalum Dan Oksida." },
  { "en": "Apa Itu 'Cacat Kristal'?", "id": "Ketidaksempurnaan Dalam Susunan Atom Kristal." },
  { "en": "Apa Itu 'Cacat Titik (Point Defect)'?", "id": "Cacat Yang Melibatkan Posisi Atom Tunggal." },
  { "en": "Apa Itu 'Kekosongan (Vacancy)'?", "id": "Posisi Atom Kosong Dalam Kisi." },
  { "en": "Apa Itu 'Interstisial'?", "id": "Atom Tambahan Di Antara Posisi Kisi." },
  { "en": "Apa Itu 'Dislokasi'?", "id": "Cacat Garis Dalam Struktur Kristal." },
  { "en": "Apa Itu 'Wafer Doping Gradien'?", "id": "Konsentrasi Dopan Bervariasi Dengan Kedalaman." },
  { "en": "Bagaimana 'Sensor Optik' Bekerja?", "id": "Mengubah Cahaya Menjadi Sinyal Listrik." },
  { "en": "Bagaimana 'Fototransistor' Dibuat?", "id": "Struktur BJT (Bipolar Junction Transistor) Peka Cahaya." },
  { "en": "Apa Itu 'Quantum Well Infrared Photodetector (QWIP)'?", "id": "Detektor Inframerah Berbasis Struktur Kuantum." },
  { "en": "Apa Itu 'Cryogenic Cooling'?", "id": "Pendinginan Sensor Ke Suhu Sangat Rendah." },
  { "en": "Mengapa Sensor Perlu 'Cryogenic Cooling'?", "id": "Untuk Mengurangi Derau Termal." },
  { "en": "Bagaimana 'Termokopel' Dibuat?", "id": "Mengelas Ujung Dua Jenis Logam." },
  { "en": "Apa Itu 'Manufaktur Aditif Elektronik'?", "id": "Mencetak 3D Komponen Dan Sirkuit." },
  { "en": "Apa Itu 'Tinta Perak'?", "id": "Tinta Mengandung Partikel Perak Untuk Konduksi." },
  { "en": "Apa Itu 'Substrat Keramik'?", "id": "Papan Sirkuit Berbasis Keramik Tahan Panas." },
  { "en": "Mengapa Menggunakan 'Substrat Keramik'?", "id": "Untuk Aplikasi Daya Tinggi Dan Frekuensi Tinggi." },
  { "en": "Apa Itu 'Direct Bonded Copper (DBC)'?", "id": "Tembaga (Cu) Dilekatkan Langsung Ke Keramik." },
  { "en": "Apa Itu 'Modul Daya IGBT'?", "id": "Beberapa Chip IGBT Dalam Satu Modul." },
  { "en": "Apa Itu 'Wirebond-less Packaging'?", "id": "Teknik Pengemasan Tanpa Menggunakan Kawat." },
  { "en": "Apa Itu 'Laser Annealing'?", "id": "Proses Annealing Menggunakan Energi Laser." },
  { "en": "Apa Keuntungan 'Laser Annealing'?", "id": "Pemanasan Sangat Cepat Dan Terlokalisasi." },
  { "en": "Apa Itu 'Etching Selektivitas'?", "id": "Rasio Laju Etsa Antara Dua Material." },
  { "en": "Apa Itu 'Plasma'?", "id": "Gas Terionisasi Yang Terdiri Dari Ion." },
  { "en": "Bagaimana 'Plasma' Diciptakan Dalam Fabrikasi?", "id": "Menerapkan Medan RF (Radio Frequency) Ke Gas." },
  { "en": "Apa Itu 'Sputtering'?", "id": "Deposisi Fisik Dengan Menembakkan Ion." },
  { "en": "Apa Itu 'Target' Dalam Sputtering?", "id": "Material Sumber Yang Akan Dideposisikan." },
  { "en": "Apa Itu 'Wafer Boat'?", "id": "Rak Untuk Memegang Wafer Dalam Tungku." },
  { "en": "Bahan Apa Untuk 'Wafer Boat'?", "id": "Kuarsa Atau Silikon Karbida (SiC)." },
  { "en": "Apa Itu 'Fotodetektor Avalanche'?", "id": "Fotodioda Yang Beroperasi Dalam Mode Avalanche." },
  { "en": "Apa Itu 'Gain' Fotodetektor?", "id": "Peningkatan Arus Akibat Efek Internal." },
  { "en": "Bagaimana 'Dioda Varactor' Dibuat?", "id": "Dengan Profil Doping Khusus." },
  { "en": "Bagaimana 'Dioda Tunnel' Dibuat?", "id": "Dengan Tingkat Doping Sangat Tinggi." },
  { "en": "Apa Itu 'Resistansi Diferensial Negatif'?", "id": "Arus Turun Saat Tegangan Naik." },
  { "en": "Bagaimana 'Thyristor' Diaktifkan?", "id": "Dengan Memberi Pulsa Ke Terminal Gerbang." },
  { "en": "Bagaimana 'Thyristor' Dimatikan?", "id": "Membuat Arus Di Bawah Arus Tahan." },
  { "en": "Apa Itu 'Arus Latching'?", "id": "Arus Minimum Agar Thyristor Tetap On." },
  { "en": "Apa Itu 'Arus Holding'?", "id": "Arus Minimum Menjaga Thyristor On." },
  { "en": "Apa Itu 'dv/dt Rating'?", "id": "Laju Kenaikan Tegangan Maksimum." },
  { "en": "Apa Itu 'di/dt Rating'?", "id": "Laju Kenaikan Arus Maksimum." },
  { "en": "Apa Itu 'Sirkuit Snubber'?", "id": "Sirkuit Pelindung Dari dv/dt Tinggi." },
  { "en": "Bagaimana 'Sirkuit Snubber' Dibuat?", "id": "Dengan Resistor Dan Kapasitor (RC)." },
  { "en": "Bagaimana 'Relay Solid State' Dibuat?", "id": "Menggunakan Optocoupler Dan Thyristor Atau MOSFET." },
  { "en": "Apa Itu 'Zero Crossing Detection'?", "id": "Mendeteksi Saat Sinyal AC Melewati Nol." },
  { "en": "Apa Fungsi 'Zero Crossing'?", "id": "Menyalakan Beban AC Tanpa Lonjakan Arus." },
  { "en": "Apa Itu 'Soldermask Dam'?", "id": "Pemisah Soldermask Antara Kaki IC." },
  { "en": "Apa Itu 'Tenting Vias'?", "id": "Menutup Lubang Via Dengan Soldermask." },
  { "en": "Apa Itu 'Plugging Vias'?", "id": "Mengisi Lubang Via Dengan Material." },
  { "en": "Apa Itu 'Finishing Permukaan PCB'?", "id": "Pelapisan Akhir Pada Pad Tembaga." },
  { "en": "Contoh 'Finishing Permukaan'?", "id": "HASL, ENIG, OSP." },
  { "en": "Apa Kepanjangan 'HASL'?", "id": "Hot Air Solder Leveling." },
  { "en": "Apa Kepanjangan 'ENIG'?", "id": "Electroless Nickel Immersion Gold." },
  { "en": "Apa Itu 'Panelisasi'?", "id": "Menggabungkan Banyak Desain PCB Dalam Panel." },
  { "en": "Apa Itu 'V-Scoring'?", "id": "Membuat Alur Untuk Memisahkan PCB." },
  { "en": "Apa Itu 'Tab Routing'?", "id": "Memisahkan PCB Dengan Tab Kecil." },
  { "en": "Bagaimana 'Konektor' Dibuat?", "id": "Stamping Logam Dan Cetakan Plastik." },
  { "en": "Bagaimana 'Kabel Pita (Ribbon Cable)' Dibuat?", "id": "Menjajarkan Banyak Kawat Dan Melaminasi." },
  { "en": "Bagaimana 'Baterai Koin' Dibuat?", "id": "Menumpuk Elektroda Dan Separator." },
  { "en": "Bagaimana 'Antena Chip' Dibuat?", "id": "Dengan Fabrikasi Keramik Frekuensi Tinggi." },
  { "en": "Apa Itu 'LTCC (Low-Temperature Co-fired Ceramic)'?", "id": "Teknologi Keramik Untuk Modul RF." },
  { "en": "Bagaimana 'Sekring PTC (Positive Temperature Coefficient)' Bekerja?", "id": "Resistansi Naik Drastis Saat Panas." },
  { "en": "Bagaimana 'Sekring PTC' Dibuat?", "id": "Dari Polimer Konduktif." },
  { "en": "Apa Itu 'Superkonduktor Suhu Tinggi'?", "id": "Superkonduktor Di Atas Suhu Nitrogen Cair." },
  { "en": "Bagaimana 'Kawat Superkonduktor' Dibuat?", "id": "Menanam Material Dalam Matriks Logam." },
  { "en": "Apa Itu 'Manufaktur Elektronik Organik'?", "id": "Pembuatan Perangkat Elektronik Berbasis Karbon." },
  { "en": "Apa Keuntungan 'Elektronik Organik'?", "id": "Fleksibel, Transparan, Dan Murah." },
  { "en": "Apa Itu 'Spin Coating'?", "id": "Teknik Melapisi Substrat Dengan Cairan." },
  { "en": "Bagaimana 'Layar Sentuh Resistif' Dibuat?", "id": "Dua Lapisan Konduktif Dengan Pemisah." },
  { "en": "Apa Prinsip 'Layar Resistif'?", "id": "Kontak Antara Dua Lapisan Akibat Tekanan." },
  { "en": "Bagaimana 'Panel Akustik Permukaan (SAW)' Dibuat?", "id": "Dengan Transduser Di Tepi Kaca." },
  { "en": "Bagaimana 'Casing Logam' Dibuat?", "id": "Melalui Proses CNC Atau Stamping." },
  { "en": "Apa Itu 'PVD (Physical Vapor Deposition)' Untuk Coating?", "id": "Memberi Warna Dan Ketahanan Gores." },
  { "en": "Bagaimana 'Kaca Gorilla' Dibuat?", "id": "Melalui Proses Pertukaran Ion Kimia." },
  { "en": "Apa Itu 'Pertukaran Ion'?", "id": "Mengganti Ion Kecil Dengan Ion Besar." },
  { "en": "Apa Efek 'Pertukaran Ion'?", "id": "Menciptakan Tegangan Tekan Di Permukaan." },
  { "en": "Bagaimana 'Serat Zirkonia' Dibuat?", "id": "Proses Elektrospinning Polimer." },
  { "en": "Apa Itu 'Elektrospinning'?", "id": "Membuat Serat Nano Dengan Medan Listrik." },
  { "en": "Bagaimana 'Aerogel' Dibuat?", "id": "Mengeringkan Gel Sambil Jaga Strukturnya." },
  { "en": "Apa Itu 'Pengeringan Superkritis'?", "id": "Metode Kunci Dalam Pembuatan Aerogel." },
  { "en": "Apa Itu 'Fabrikasi Aditif'?", "id": "Nama Lain Untuk Manufaktur Aditif." },
  { "en": "Apa Itu 'Fabrikasi Subtraktif'?", "id": "Nama Lain Untuk Manufaktur Subtraktif." },
  { "en": "Apa Itu 'Sirkuit Logika Kustom'?", "id": "IC (Integrated Circuit) Didesain Fungsi Unik." },
  { "en": "Apa Itu 'Gerbang Logika Statis'?", "id": "Gerbang Yang Menjaga Outputnya Stabil." },
  { "en": "Apa Itu 'Gerbang Logika Dinamis'?", "id": "Gerbang Yang Gunakan Kapasitor Untuk Logika." },
  { "en": "Apa Keuntungan 'Logika Dinamis'?", "id": "Lebih Cepat Dan Ukuran Lebih Kecil." },
  { "en": "Apa Kerugian 'Logika Dinamis'?", "id": "Lebih Kompleks Dan Butuh Clock." },
  { "en": "Apa Itu 'Charge Sharing'?", "id": "Masalah Dalam Logika Dinamis." },
  { "en": "Apa Itu 'Precharge'?", "id": "Tahap Awal Dalam Operasi Logika Dinamis." },
  { "en": "Apa Itu 'Evaluate'?", "id": "Tahap Kedua Dalam Operasi Logika Dinamis." },
  { "en": "Apa Itu 'Logika Domino'?", "id": "Jenis Logika Dinamis Yang Cepat." },
  { "en": "Apa Itu 'Buffer Inverting'?", "id": "Dua Inverter Disusun Seri." },
  { "en": "Apa Itu 'Sistem Bilangan'?", "id": "Cara Untuk Merepresentasikan Angka." },
  { "en": "Apa Itu 'Basis' Atau 'Radix'?", "id": "Jumlah Simbol Unik Dalam Sistem Bilangan." },
  { "en": "Apa Itu 'Konversi Basis'?", "id": "Mengubah Angka Dari Satu Basis Ke Lainnya." },
  { "en": "Apa Itu 'Desain Digital'?", "id": "Proses Merancang Dan Membangun Sistem Digital." },
  { "en": "Apa Itu 'Sintesis Perilaku (Behavioral)'?", "id": "Sintesis Dari Deskripsi Level Sangat Tinggi." },
  { "en": "Apa Itu 'Abstraksi'?", "id": "Menyembunyikan Detail Kompleksitas." },
  { "en": "Apa Itu 'Hierarki' Dalam Desain?", "id": "Membagi Desain Menjadi Blok-blok Kecil." },
  { "en": "Apa Itu 'Modularitas'?", "id": "Desain Dibangun Dari Modul Independen." },
  { "en": "Apa Keuntungan 'Desain Modular'?", "id": "Mudah Dikelola Dan Digunakan Kembali." },
  { "en": "Apa Itu 'Regularitas'?", "id": "Menggunakan Kembali Blok Yang Sama." },
  { "en": "Apa Itu 'Sistem Biner'?", "id": "Sistem Berbasis Dua Nilai (0 dan 1)." },
  { "en": "Apa Itu 'Sirkuit Digital Terintegrasi'?", "id": "Nama Lain Untuk IC Digital." },
  { "en": "Apa Itu 'Tegangan Suplai Nominal'?", "id": "Tegangan Kerja Yang Direkomendasikan." },
  { "en": "Apa Itu 'Karakteristik Transfer Tegangan'?", "id": "Grafik Vout Terhadap Vin." },
  { "en": "Apa Itu 'Derau (Noise)'?", "id": "Sinyal Tak Diinginkan Pengganggu Logika." },
  { "en": "Apa Itu 'Simbol Logika'?", "id": "Simbol Grafis Untuk Gerbang Logika." },
  { "en": "Apa Itu 'Timing Analysis'?", "id": "Analisis Penundaan Sinyal Dalam Sirkuit." },
  { "en": "Apa Itu 'Verifikasi Fungsional'?", "id": "Memastikan Desain Bekerja Sesuai Fungsi." },
  { "en": "Apa Itu 'Test Case'?", "id": "Satu Set Input Untuk Menguji Fungsi." },
  { "en": "Apa Itu 'Delayering'?", "id": "Proses Mengupas Lapisan Chip Untuk Analisis." },
  { "en": "Mengapa 'Gettering' Penting?", "id": "Menyingkirkan Kontaminan Logam Perusak." },
  { "en": "Apa Beda Inspeksi 'Brightfield' Dan 'Darkfield'?", "id": "Brightfield Deteksi Kontras, Darkfield Deteksi Hamburan." },
  { "en": "Apa Fungsi 'FIB (Focused Ion Beam)'?", "id": "Memotong Dan Pencitraan Skala Nano." },
  { "en": "Bagaimana Cara Membuat 'Filter BAW (Bulk Acoustic Wave)'?", "id": "Membuat Resonator Akustik Di Dalam Chip." },
  { "en": "Apa Itu 'Gowning Procedure'?", "id": "Prosedur Mengenakan Pakaian Ruang Bersih." },
  { "en": "Apa Fungsi 'Air Shower'?", "id": "Menghilangkan Partikel Debu Dari Pakaian." },
  { "en": "Apa Itu 'Rheologi' Pasta Solder?", "id": "Studi Sifat Aliran Pasta Solder." },
  { "en": "Apa Itu 'Vapor Phase Soldering'?", "id": "Solder Menggunakan Uap Cairan Inert Panas." },
  { "en": "Apa Jenis 'Conformal Coating'?", "id": "Akrilik, Silikon, Uretan." },
  { "en": "Bagaimana 'Potensiometer' Dibuat?", "id": "Dengan Jalur Resistif Dan Kontak Geser." },
  { "en": "Apa Itu 'Substrate Engineering'?", "id": "Memodifikasi Substrat Untuk Kinerja Lebih Baik." },
  { "en": "Apa Itu 'Wafer Dicing' Menggunakan Laser?", "id": "Memotong Wafer Dengan Sinar Laser Presisi." },
  { "en": "Apa Itu 'Thermal Compression Bonding'?", "id": "Menyambung Chip Menggunakan Panas Dan Tekanan." },
  { "en": "Bagaimana 'Cermin MEMS' Dibuat?", "id": "Dengan Surface Micromachining." },
  { "en": "Apa Fungsi 'Cermin MEMS'?", "id": "Mengarahkan Sinar Laser Dengan Cepat." },
  { "en": "Apa Itu 'Dielectric Constant'?", "id": "Kemampuan Bahan Menyimpan Energi Listrik." },
  { "en": "Mengapa 'Dielektrik Low-k' Digunakan?", "id": "Mengurangi Tundaan Sinyal Antar Jalur." },
  { "en": "Apa Itu 'Copper Pillar Bump'?", "id": "Tonjolan Tembaga (Cu) Untuk Koneksi Flip-Chip." },
  { "en": "Apa Keuntungan 'Copper Pillar'?", "id": "Kinerja Listrik Dan Termal Lebih Baik." },
  { "en": "Apa Itu 'Reliability Testing'?", "id": "Pengujian Ketahanan Komponen Seiring Waktu." },
  { "en": "Apa Kepanjangan 'HTOL (High Temperature Operating Life)'?", "id": "Uji Umur Operasi Suhu Tinggi." },
  { "en": "Apa Itu 'Thermal Shock Test'?", "id": "Pengujian Dengan Perubahan Suhu Ekstrem." },
  { "en": "Apa Itu 'Vibration Test'?", "id": "Pengujian Ketahanan Terhadap Getaran Mekanis." },
  { "en": "Apa Itu 'Proses Manufaktur CMOS'?", "id": "Langkah-langkah Membuat Chip CMOS." },
  { "en": "Apa Itu 'Litho-Etch-Litho-Etch (LELE)'?", "id": "Teknik Double Patterning Untuk Skala Kecil." },
  { "en": "Apa Itu 'Self-Aligned Double Patterning (SADP)'?", "id": "Teknik Double Patterning Lainnya." },
  { "en": "Mengapa 'Double Patterning' Diperlukan?", "id": "Untuk Membuat Fitur Lebih Kecil." },
  { "en": "Apa Itu 'Optical Lithography'?", "id": "Nama Lain Untuk Fotolitografi." },
  { "en": "Apa Itu 'E-beam Lithography'?", "id": "Litografi Tanpa Masker Menggunakan Berkas Elektron." },
  { "en": "Apa Kelemahan 'E-beam Lithography'?", "id": "Sangat Lambat, Tidak Untuk Produksi Massal." },
  { "en": "Apa Itu 'Nanoimprint Lithography (NIL)'?", "id": "Mencetak Pola Nano Seperti Stempel." },
  { "en": "Apa Itu 'Molecular Imprinting'?", "id": "Membuat Cetakan Molekuler Di Polimer." },
  { "en": "Apa Itu 'Sol-Gel Process'?", "id": "Metode Kimia Membuat Oksida Logam." },
  { "en": "Bagaimana 'Kaca' Dibuat?", "id": "Mendinginkan Cairan Silika Dengan Cepat." },
  { "en": "Apa Itu 'Tempering' Kaca?", "id": "Proses Pemanasan-Pendinginan Untuk Perkuat Kaca." },
  { "en": "Bagaimana 'LED (Light Emitting Diode) Inframerah' Dibuat?", "id": "Menggunakan Bahan Seperti Galium Arsenida (GaAs)." },
  { "en": "Bagaimana 'Sensor UV (UltraViolet)' Dibuat?", "id": "Menggunakan Semikonduktor Celah Pita Lebar." },
  { "en": "Bahan Apa Untuk 'Sensor UV'?", "id": "Aluminium Galium Nitrida (AlGaN)." },
  { "en": "Apa Itu 'Photomultiplier Tube (PMT)'?", "id": "Detektor Cahaya Sangat Sensitif." },
  { "en": "Bagaimana 'PMT' Dibuat?", "id": "Merakit Fotokatoda Dan Dynode Dalam Tabung Vakum." },
  { "en": "Apa Itu 'Fotokatoda'?", "id": "Bahan Yang Melepas Elektron Saat Kena Cahaya." },
  { "en": "Apa Itu 'Dynode'?", "id": "Elektroda Yang Melipatgandakan Elektron." },
  { "en": "Apa Itu 'Scintillator'?", "id": "Bahan Yang Berpendar Saat Kena Radiasi." },
  { "en": "Bagaimana 'Detektor Scintillation' Dibuat?", "id": "Menggabungkan Scintillator Dengan Photodetector." },
  { "en": "Bagaimana 'Tabung Geiger-Müller' Dibuat?", "id": "Tabung Diisi Gas Dengan Elektroda." },
  { "en": "Apa Itu 'Semikonduktor Organik'?", "id": "Semikonduktor Berbasis Molekul Organik." },
  { "en": "Bagaimana 'Transistor Organik (OFET)' Dibuat?", "id": "Deposisi Lapisan Organik Pada Substrat." },
  { "en": "Apa Itu 'Deposisi Vakum Termal'?", "id": "Menguapkan Material Dalam Vakum Tinggi." },
  { "en": "Apa Itu 'Doctor Blading'?", "id": "Teknik Melapisi Cairan Dengan Ketebalan Tepat." },
  { "en": "Bagaimana 'Sel Surya Organik' Dibuat?", "id": "Dengan Melapisi Polimer Donor-Akseptor." },
  { "en": "Apa Itu 'Fullerene'?", "id": "Molekul Karbon Seperti Bola (C60)." },
  { "en": "Apa Peran 'Fullerene' Dalam Sel Surya?", "id": "Sebagai Material Akseptor Elektron." },
  { "en": "Apa Itu 'Perovskite'?", "id": "Material Kristal Dengan Struktur Tertentu." },
  { "en": "Bagaimana 'Sel Surya Perovskite' Dibuat?", "id": "Melalui Pelapisan Larutan (Solution Coating)." },
  { "en": "Apa Itu 'Slot-Die Coating'?", "id": "Teknik Pelapisan Presisi Skala Besar." },
  { "en": "Apa Itu 'Manufaktur Elektronik Cetak'?", "id": "Mencetak Sirkuit Menggunakan Printer Khusus." },
  { "en": "Apa Itu 'Gravure Printing'?", "id": "Teknik Cetak Untuk Volume Sangat Tinggi." },
  { "en": "Apa Itu 'Flexographic Printing'?", "id": "Teknik Cetak Fleksibel Seperti Stempel." },
  { "en": "Apa Itu 'Aerosol Jet Printing'?", "id": "Mencetak Garis Sangat Halus Dengan Aerosol." },
  { "en": "Apa Itu 'Sintering Fotonik'?", "id": "Sintering Tinta Logam Dengan Kilatan Cahaya." },
  { "en": "Bagaimana 'Transduser Piezoelektrik' Dibuat?", "id": "Sintering Keramik Dan Proses Poling." },
  { "en": "Apa Itu 'Poling'?", "id": "Menyelaraskan Domain Dipol Dengan Medan Listrik." },
  { "en": "Apa Itu 'Floorplanning'?", "id": "Tahap Awal Penempatan Blok Desain." },
  { "en": "Apa Itu 'Placement'?", "id": "Menentukan Lokasi Tepat Setiap Sel." },
  { "en": "Apa Itu 'Routing'?", "id": "Proses Menghubungkan Semua Sel Dengan Logam." },
  { "en": "Apa Kepanjangan 'CTS (Clock Tree Synthesis)'?", "id": "Sintesis Pohon Clock." },
  { "en": "Apa Tujuan 'CTS (Clock Tree Synthesis)'?", "id": "Mendistribusikan Sinyal Clock Secara Merata." },
  { "en": "Apa Kepanjangan 'SDF (Standard Delay Format)'?", "id": "Format Penundaan Standar." },
  { "en": "Apa Itu 'Pipelining'?", "id": "Memecah Eksekusi Instruksi Menjadi Tahapan." },
  { "en": "Apa Itu 'Cache L1'?", "id": "Cache Terkecil Dan Tercepat Dalam Inti." },
  { "en": "Apa Itu 'Cache L2'?", "id": "Cache Menengah Antara L1 Dan L3." },
  { "en": "Apa Itu 'Cache L3'?", "id": "Cache Terbesar Yang Dibagi Antar Inti." },
  { "en": "Apa Kepanjangan 'MMU (Memory Management Unit)'?", "id": "Unit Manajemen Memori." },
  { "en": "Apa Fungsi 'MMU (Memory Management Unit)'?", "id": "Menerjemahkan Alamat Virtual Ke Fisik." },
  { "en": "Apa Itu 'Arsitektur Superscalar'?", "id": "Arsitektur Yang Eksekusi Banyak Instruksi." },
  { "en": "Apa Kepanjangan 'DSP (Digital Signal Processor)'?", "id": "Prosesor Sinyal Digital." },
  { "en": "Apa Unit Utama 'DSP (Digital Signal Processor)'?", "id": "Unit MAC (Multiply-Accumulate)." },
  { "en": "Apa Itu 'Filter FIR (Finite Impulse Response)'?", "id": "Filter Digital Tanpa Umpan Balik." },
  { "en": "Apa Itu 'Filter IIR (Infinite Impulse Response)'?", "id": "Filter Digital Dengan Umpan Balik." },
  { "en": "Apa Kepanjangan 'PHY'?", "id": "Physical Layer (Lapisan Fisik)." },
  { "en": "Apa Fungsi 'IC (Integrated Circuit) PHY'?", "id": "Implementasi Lapisan Fisik Jaringan." },
  { "en": "Apa Kepanjangan 'MAC'?", "id": "Media Access Control (Kontrol Akses Media)." },
  { "en": "Apa Itu 'Modulator'?", "id": "Sirkuit Penumpang Sinyal Ke Pembawa." },
  { "en": "Apa Itu 'Demodulator'?", "id": "Sirkuit Pemisah Sinyal Dari Pembawa." },
  { "en": "Apa Itu 'Kapasitor Decoupling'?", "id": "Menyimpan Energi Lokal Untuk IC." },
  { "en": "Apa Kepanjangan 'PDN (Power Distribution Network)'?", "id": "Jaringan Distribusi Daya." },
  { "en": "Apa Itu 'Ground Bounce'?", "id": "Lonjakan Tegangan Pada Jalur Ground." },
  { "en": "Apa Itu 'Celah Pita (Band Gap)'?", "id": "Energi Minimum Eksitasi Elektron." },
  { "en": "Apa Itu 'Mobilitas Elektron'?", "id": "Kemudahan Elektron Bergerak Dalam Material." },
  { "en": "Apa Itu 'Masa Hidup Pembawa (Carrier Lifetime)'?", "id": "Waktu Rata-rata Sebelum Rekombinasi." },
  { "en": "Apa Itu 'Daerah Deplesi (Depletion Region)'?", "id": "Daerah Tanpa Pembawa Muatan Bebas." },
  { "en": "Apa Itu 'Vektor Uji (Test Vector)'?", "id": "Pola Input Untuk Menguji Sirkuit." },
  { "en": "Apa Itu 'Model Kesalahan (Fault Model)'?", "id": "Asumsi Jenis Kerusakan Fisik Sirkuit." },
  { "en": "Apa Itu 'Kesalahan Stuck-at'?", "id": "Asumsi Jalur Terjebak Di 0 Atau 1." },
  { "en": "Apa Itu 'Peta Wafer (Wafer Map)'?", "id": "Peta Visual Hasil Uji Die." },
  { "en": "Apa Itu 'Komputasi Neuromorfik'?", "id": "Komputasi Yang Meniru Otak." },
  { "en": "Apa Itu 'Qubit'?", "id": "Unit Dasar Informasi Kuantum." },
  { "en": "Apa Itu 'Spintronik'?", "id": "Elektronik Berbasis Spin Elektron." },
  { "en": "Apa Itu 'Cermin Arus (Current Mirror)'?", "id": "Sirkuit Untuk Menyalin Arus." },
  { "en": "Apa Itu 'Pasangan Diferensial (Differential Pair)'?", "id": "Inti Dari Banyak Penguat Analog." },
  { "en": "Apa Itu 'Sel Gilbert'?", "id": "Sirkuit Inti Untuk Pencampur (Mixer)." },
  { "en": "Apa Itu 'Referensi Celah Pita (Bandgap Reference)'?", "id": "Sirkuit Penghasil Tegangan Referensi Stabil." },
  { "en": "Apa Itu 'Efek Miller'?", "id": "Peningkatan Kapasitansi Input Akibat Gain." },
  { "en": "Apa Itu 'Kompensasi Frekuensi'?", "id": "Teknik Menstabilkan Penguat." },
  { "en": "Apa Kepanjangan 'CMRR (Common-Mode Rejection Ratio)'?", "id": "Rasio Penolakan Mode Bersama." },
  { "en": "Apa Kepanjangan 'PSRR (Power Supply Rejection Ratio)'?", "id": "Rasio Penolakan Catu Daya." },
  { "en": "Apa Itu 'Tegangan Offset Input'?", "id": "Tegangan DC (Direct Current) Kecil Di Input Op-Amp." },
  { "en": "Apa Itu 'Arus Bias Input'?", "id": "Arus DC (Direct Current) Kecil Mengalir Ke Input." },
  { "en": "Apa Itu 'Slew Rate'?", "id": "Laju Perubahan Output Maksimum." },
  { "en": "Apa Itu 'Produk Gain-Bandwidth'?", "id": "Karakteristik Penguat Operasional." },
  { "en": "Apa Itu 'Derau 1/f'?", "id": "Derau Frekuensi Rendah (Pink Noise)." },
  { "en": "Apa Itu 'Derau Termal'?", "id": "Derau Akibat Agitasi Termal Elektron." },
  { "en": "Apa Itu 'Derau Tembak (Shot Noise)'?", "id": "Derau Akibat Sifat Diskrit Muatan." },
  { "en": "Apa Itu 'Angka Derau (Noise Figure)'?", "id": "Ukuran Degradasi SNR (Signal-to-Noise Ratio)." },
  { "en": "Apa Itu 'Distorsi Harmonik Total (THD)'?", "id": "Ukuran Distorsi Sinyal." },
  { "en": "Apa Itu 'Intermodulasi'?", "id": "Pencampuran Sinyal Yang Menghasilkan Frekuensi Baru." },
  { "en": "Apa Itu 'Titik Kompresi 1-dB'?", "id": "Ukuran Linieritas Penguat." },
  { "en": "Apa Itu 'Titik Potong Orde Ketiga (IP3)'?", "id": "Ukuran Lain Linieritas." },
  { "en": "Apa Itu 'Rentang Dinamis Bebas Spurious (SFDR)'?", "id": "Rentang Antara Sinyal Dan Derau Terkuat." },
  { "en": "Apa Itu 'Konverter Buck-Boost'?", "id": "Bisa Menaikkan Atau Menurunkan Tegangan." },
  { "en": "Apa Itu 'Topologi Cuk'?", "id": "Jenis Konverter DC-DC (Direct Current-DC)." },
  { "en": "Apa Itu 'Topologi SEPIC'?", "id": "Jenis Konverter DC-DC (Direct Current-DC) Lainnya." },
  { "en": "Apa Itu 'Kontrol Mode Arus'?", "id": "Metode Kontrol Untuk Regulator Switching." },
  { "en": "Apa Itu 'Kontrol Mode Tegangan'?", "id": "Metode Kontrol Regulator Switching Lain." },
  { "en": "Apa Itu 'Soft Start'?", "id": "Fitur Menaikkan Tegangan Output Perlahan." },
  { "en": "Apa Itu 'Proteksi Arus Lebih'?", "id": "Fitur Membatasi Arus Output." },
  { "en": "Apa Itu 'Proteksi Suhu Lebih'?", "id": "Fitur Mematikan IC (Integrated Circuit) Saat Panas." },
  { "en": "Apa Itu 'Proteksi Tegangan Kurang (UVLO)'?", "id": "Mematikan IC (Integrated Circuit) Saat Suplai Rendah." },
  { "en": "Apa Itu 'Pengemasan Hermetik'?", "id": "Paket Kedap Udara." },
  { "en": "Mengapa 'Pengemasan Hermetik' Digunakan?", "id": "Untuk Keandalan Tinggi (Militer, Luar Angkasa)." },
  { "en": "Bahan Apa Untuk 'Paket Hermetik'?", "id": "Keramik Atau Logam." },
  { "en": "Apa Itu 'Dielektrik High-k'?", "id": "Isolator Gerbang Dengan Konstanta Dielektrik Tinggi." },
  { "en": "Mengapa 'Dielektrik High-k' Digunakan?", "id": "Mengurangi Arus Bocor Gerbang." },
  { "en": "Apa Itu 'Gerbang Logam (Metal Gate)'?", "id": "Penggunaan Logam Untuk Elektroda Gerbang." },
  { "en": "Apa Itu 'Silikon Terasah (Strained Silicon)'?", "id": "Teknik Peregangan Kisi Silikon." },
  { "en": "Mengapa 'Silikon Terasah' Digunakan?", "id": "Meningkatkan Mobilitas Pembawa Muatan." },
  { "en": "Apa Itu 'SOI (Silicon-on-Insulator)'?", "id": "Silikon Di Atas Isolator." },
  { "en": "Apa Keuntungan 'SOI (Silicon-on-Insulator)'?", "id": "Mengurangi Kapasitansi Parasit." },
  { "en": "Apa Itu 'FinFET'?", "id": "Transistor Dengan Struktur Sirip (Fin)." },
  { "en": "Apa Itu 'Gate-All-Around (GAA)'?", "id": "Struktur Transistor Dengan Gerbang Mengelilingi." },
  { "en": "Apa Itu 'Fotodioda'?", "id": "Dioda Yang Berfungsi Sebagai Sensor Cahaya." },
  { "en": "Apa Itu 'Sel Surya'?", "id": "Dioda Area Besar Untuk Ubah Cahaya." },
  { "en": "Apa Itu 'Dioda Zener'?", "id": "Dioda Untuk Referensi Tegangan." },
  { "en": "Apa Itu 'Dioda Schottky'?", "id": "Dioda Cepat Dengan Jatuh Tegangan Rendah." },
  { "en": "Apa Itu 'Dioda Varactor'?", "id": "Dioda Yang Kapasitansinya Dapat Diubah." },
  { "en": "Apa Itu 'Dioda PIN'?", "id": "Dioda Dengan Lapisan Intrinsik." },
  { "en": "Apa Itu 'Dioda Gunn'?", "id": "Dioda Untuk Menghasilkan Sinyal Microwave." },
  { "en": "Apa Itu 'Dioda IMPATT'?", "id": "Dioda Lain Untuk Aplikasi Microwave." },
  { "en": "Apa Itu 'Sirkuit Tangki LC'?", "id": "Resonator Menggunakan Induktor Dan Kapasitor." },
  { "en": "Apa Itu 'Resonator Kristal'?", "id": "Komponen Piezoelektrik Untuk Frekuensi Stabil." },
  { "en": "Apa Itu 'Resonator Keramik'?", "id": "Resonator Dengan Stabilitas Lebih Rendah." },
  { "en": "Apa Itu 'Ferrite Bead'?", "id": "Komponen Untuk Menekan Derau Frekuensi Tinggi." },
  { "en": "Apa Itu 'Mode Bersama (Common Mode)'?", "id": "Sinyal Yang Sama Pada Dua Jalur." },
  { "en": "Apa Itu 'Mode Diferensial'?", "id": "Sinyal Yang Berlawanan Pada Dua Jalur." },
  { "en": "Apa Itu 'Common-Mode Choke'?", "id": "Induktor Untuk Menekan Derau Mode Bersama." },
  { "en": "Apa Itu 'Kabel Twisted Pair'?", "id": "Sepasang Kabel Yang Dipilin." },
  { "en": "Mengapa Kabel 'Twisted Pair' Digunakan?", "id": "Untuk Mengurangi Gangguan Elektromagnetik." },
  { "en": "Apa Itu 'Pipelining'?", "id": "Memecah Eksekusi Instruksi Menjadi Tahapan." },
  { "en": "Apa Itu 'Pipeline Stall'?", "id": "Penundaan Dalam Aliran Eksekusi Pipeline." },
  { "en": "Apa Itu 'Pipeline Hazard'?", "id": "Kondisi Yang Menyebabkan Pipeline Stall." },
  { "en": "Apa Itu 'Data Hazard'?", "id": "Konflik Ketergantungan Data Antar Instruksi." },
  { "en": "Apa Itu 'Control Hazard'?", "id": "Konflik Akibat Instruksi Percabangan." },
  { "en": "Apa Itu 'Data Forwarding'?", "id": "Meneruskan Hasil Untuk Mengatasi Hazard." },
  { "en": "Apa Itu 'Branch Prediction'?", "id": "Menebak Arah Percabangan Kode." },
  { "en": "Apa Itu 'Arsitektur Superscalar'?", "id": "Arsitektur Yang Dapat Eksekusi Banyak Instruksi." },
  { "en": "Apa Itu 'Out-of-Order Execution'?", "id": "Eksekusi Instruksi Tidak Berdasarkan Urutan." },
  { "en": "Apa Itu 'Register Renaming'?", "id": "Teknik Mengatasi Hazard Dengan Mengganti Nama Register." },
  { "en": "Apa Itu 'Cache Coherency'?", "id": "Menjaga Konsistensi Data Antar Banyak Cache." },
  { "en": "Apa Itu 'Protokol Snooping'?", "id": "Protokol Menjaga Koherensi Cache." },
  { "en": "Apa Itu 'Cache Miss'?", "id": "Data Yang Dicari Tidak Ditemukan Di Cache." },
  { "en": "Apa Itu 'Cache Hit'?", "id": "Data Yang Dicari Ditemukan Di Cache." },
  { "en": "Apa Itu 'Write-Through Cache'?", "id": "Menulis Langsung Ke RAM Dan Cache." },
  { "en": "Apa Itu 'Write-Back Cache'?", "id": "Menulis Ke RAM Hanya Saat Diperlukan." },
  { "en": "Apa Itu 'Memori Virtual'?", "id": "Mekanisme Memori Menggunakan Penyimpanan Sekunder." },
  { "en": "Apa Itu 'Page Table'?", "id": "Tabel Pemetaan Alamat Virtual Ke Fisik." },
  { "en": "Apa Kepanjangan 'TLB (Translation Lookaside Buffer)'?", "id": "Buffer Cache Untuk Page Table." },
  { "en": "Apa Itu 'Page Fault'?", "id": "Akses Ke Halaman Yang Tidak Di RAM." },
  { "en": "Apa Itu 'Jaringan On-Chip (NoC)'?", "id": "Jaringan Komunikasi Di Dalam Chip." },
  { "en": "Apa Itu 'Gerbang Logika Reversibel'?", "id": "Gerbang Yang Inputnya Bisa Direkonstruksi." },
  { "en": "Apa Itu 'Komputasi Adiabatik'?", "id": "Komputasi Yang Meminimalkan Disipasi Panas." },
  { "en": "Apa Itu 'Logika Ternary'?", "id": "Logika Yang Memiliki Tiga Nilai." },
  { "en": "Apa Itu 'Adder-Subtractor'?", "id": "Rangkaian Yang Bisa Menambah Dan Mengurang." },
  { "en": "Apa Itu 'Priority Encoder'?", "id": "Encoder Yang Proses Input Prioritas Tertinggi." },
  { "en": "Apa Itu 'Kode Gray'?", "id": "Kode Biner Yang Hanya Ubah Satu Bit." },
  { "en": "Apa Itu 'Counter Johnson'?", "id": "Shift Register Dengan Umpan Balik Terbalik." },
  { "en": "Apa Kepanjangan 'LFSR (Linear Feedback Shift Register)'?", "id": "Register Geser Umpan Balik Linier." },
  { "en": "Apa Kegunaan 'LFSR (Linear Feedback Shift Register)'?", "id": "Penghasil Angka Acak Semu." },
  { "en": "Apa Itu 'Verifikasi Properti'?", "id": "Memverifikasi Apakah Desain Memenuhi Properti." },
  { "en": "Apa Itu 'Assertion'?", "id": "Pernyataan Properti Yang Harus Benar." },
  { "en": "Apa Itu 'Emulasi Perangkat Keras'?", "id": "Mensimulasikan Desain Di Atas Hardware." },
  { "en": "Apa Itu 'Akselerasi Perangkat Keras'?", "id": "Menggunakan Hardware Khusus Percepat Simulasi." },
  { "en": "Apa Itu 'Desain Asinkron'?", "id": "Desain Sirkuit Tanpa Sinyal Clock Global." },
  { "en": "Apa Itu 'Sinyal Handshake'?", "id": "Sinyal Untuk Sinkronisasi Desain Asinkron." },
  { "en": "Apa Itu 'Arbiter'?", "id": "Sirkuit Pemberi Akses Ke Sumber Daya." },
  { "en": "Apa Itu 'FIFO Asinkron'?", "id": "FIFO (First-In, First-Out) Untuk Lintas Domain Clock." },
  { "en": "Apa Itu 'Logic Array Block (LAB)'?", "id": "Blok Logika Dalam Arsitektur CPLD/FPGA." },
  { "en": "Apa Itu 'Floating Gate'?", "id": "Gerbang Terisolasi Yang Menyimpan Muatan." },
  { "en": "Di Mana 'Floating Gate' Digunakan?", "id": "Di Memori EPROM Dan Flash." },
  { "en": "Apa Itu 'Injeksi Hot-Electron'?", "id": "Mekanisme Pemrograman Untuk Memori Flash." },
  { "en": "Apa Itu 'Fowler-Nordheim Tunneling'?", "id": "Mekanisme Hapus/Tulis Memori Flash." },
  { "en": "Apa Itu 'Wear Leveling'?", "id": "Distribusi Siklus Tulis Di Memori Flash." },
  { "en": "Apa Itu 'Read Disturb'?", "id": "Membaca Satu Sel Mengganggu Sel Tetangga." },
  { "en": "Apa Itu 'Write Amplification'?", "id": "Data Ditulis Lebih Banyak Dari Sebenarnya." },
  { "en": "Apa Itu 'Garbage Collection' Di SSD?", "id": "Proses Mengatur Ulang Blok Data." },
  { "en": "Apa Kepanjangan 'TRIM'?", "id": "Perintah Memberitahu Blok Kosong SSD." },
  { "en": "Apa Kepanjangan 'NVMe (Non-Volatile Memory Express)'?", "id": "Protokol Cepat Untuk Akses SSD." },
  { "en": "Apa Kepanjangan 'SATA (Serial ATA)'?", "id": "Antarmuka Serial Advanced Technology Attachment." },
  { "en": "Apa Itu 'Registered Memory (RDIMM)'?", "id": "Modul RAM (Random-Access Memory) Dengan Register Buffer." },
  { "en": "Di Mana 'RDIMM' Digunakan?", "id": "Di Server Untuk Stabilitas." },
  { "en": "Apa Itu 'Unbuffered Memory (UDIMM)'?", "id": "Modul RAM (Random-Access Memory) Tanpa Buffer." },
  { "en": "Apa Kepanjangan 'ECC (Error-Correcting Code)'?", "id": "Kode Koreksi Kesalahan." },
  { "en": "Apa Itu 'Memory Controller'?", "id": "Sirkuit Pengatur Akses Ke RAM." },
  { "en": "Apa Itu 'Memori Single-Channel'?", "id": "Satu Jalur Data Ke Memori." },
  { "en": "Apa Itu 'Memori Dual-Channel'?", "id": "Dua Jalur Data Paralel Ke Memori." },
  { "en": "Apa Itu 'Latensi CAS (CL)'?", "id": "Penundaan Waktu Dalam Siklus Memori." },
  { "en": "Apa Kepanjangan 'ALU Slice'?", "id": "Satu Bit Dari ALU Lengkap." },
  { "en": "Apa Itu 'Lookahead Carry Generator'?", "id": "Sirkuit Khusus Untuk Mempercepat Carry." },
  { "en": "Apa Itu 'Shift-and-Add'?", "id": "Metode Perkalian Biner Secara Serial." },
  { "en": "Apa Itu 'State Minimization'?", "id": "Proses Mengurangi Jumlah Keadaan FSM." },
  { "en": "Apa Itu 'Unused State'?", "id": "Keadaan Yang Tidak Pernah Tercapai." },
  { "en": "Apa Itu 'Up/Down Counter'?", "id": "Counter Yang Bisa Menghitung Naik/Turun." },
  { "en": "Apa Itu 'Keluaran Totem-Pole'?", "id": "Struktur Output Khas Pada Logika TTL." },
  { "en": "Apa Itu 'Antarmuka Tiga Kabel (Three-Wire)'?", "id": "Nama Umum Untuk Antarmuka SPI." },
  { "en": "Apa Itu 'Antarmuka Dua Kabel (Two-Wire)'?", "id": "Nama Umum Untuk Antarmuka I2C." },
  { "en": "Apa Itu 'Clock Stretching'?", "id": "Mekanisme Slave Memperlambat Master I2C." },
  { "en": "Apa Itu 'Bit Acknowledge (ACK)'?", "id": "Sinyal Konfirmasi Penerimaan Data." },
  { "en": "Apa Itu 'Bit Not Acknowledge (NACK)'?", "id": "Sinyal Penolakan Atau Akhir Transmisi." },
  { "en": "Apa Itu 'Parity Error'?", "id": "Kesalahan Yang Terdeteksi Oleh Bit Paritas." },
  { "en": "Apa Itu 'Framing Error'?", "id": "Kesalahan Dalam Bit Start Atau Stop." },
  { "en": "Apa Itu 'Siklus Desain'?", "id": "Tahapan Dari Spesifikasi Hingga Implementasi." },
  { "en": "Apa Beda 'Verifikasi' Dan 'Pengujian'?", "id": "Verifikasi Di Desain, Pengujian Di Hardware." },
  { "en": "Apa Itu 'Sistem Toleran Kesalahan'?", "id": "Sistem Yang Tahan Terhadap Kegagalan." },
  { "en": "Apa Itu 'Redundansi Perangkat Keras'?", "id": "Menggandakan Komponen Kritis." },
  { "en": "Apa Itu 'Voting Logic'?", "id": "Memilih Output Mayoritas Dari Modul Redundan." },
  { "en": "Apa Itu 'Soft Error'?", "id": "Kesalahan Transien Akibat Partikel Kosmik." },
  { "en": "Apa Kepanjangan 'FPU (Floating Point Unit)'?", "id": "Unit Titik Mengambang." },
  { "en": "Apa Itu 'Set Instruksi'?", "id": "Kumpulan Perintah Yang Dipahami CPU." },
  { "en": "Apa Itu 'Mode Pengalamatan'?", "id": "Cara Instruksi Menentukan Alamat Data." },
  { "en": "Apa Itu 'Endianness'?", "id": "Urutan Penyimpanan Byte Dalam Memori." },
  { "en": "Apa Itu 'Big-Endian'?", "id": "Byte Paling Signifikan Disimpan Terlebih Dahulu." },
  { "en": "Apa Itu 'Little-Endian'?", "id": "Byte Paling Tidak Signifikan Disimpan Dulu." },
  { "en": "Apa Itu 'Checksum Internet'?", "id": "Metode Cek Kesalahan Sederhana." },
  { "en": "Apa Itu 'Device Driver'?", "id": "Perangkat Lunak Pengontrol Perangkat Keras." },
  { "en": "Apa Itu 'Buffer Overflow'?", "id": "Menulis Data Melebihi Batas Buffer." },
  { "en": "Apa Itu 'Stack Overflow'?", "id": "Stack Kehabisan Ruang." },
  { "en": "Apa Itu 'Rekursi'?", "id": "Fungsi Yang Memanggil Dirinya Sendiri." },
  { "en": "Apa Itu 'Struktur Data'?", "id": "Cara Mengorganisir Data Di Memori." },
  { "en": "Apa Itu 'Algoritma'?", "id": "Langkah-langkah Untuk Menyelesaikan Masalah." },
  { "en": "Apa Itu 'Notasi Big-O'?", "id": "Notasi Untuk Kompleksitas Waktu Terburuk." },
  { "en": "Apa Itu 'Hash Table'?", "id": "Struktur Data Untuk Pencarian Cepat." },
  { "en": "Apa Itu 'Pohon Biner'?", "id": "Struktur Data Pohon Dengan Dua Anak." },
  { "en": "Apa Itu 'Sistem Terdistribusi'?", "id": "Sistem Dengan Komponen Di Komputer Berbeda." },
  { "en": "Apa Itu 'Komputasi Paralel'?", "id": "Menjalankan Banyak Komputasi Secara Bersamaan." },
  { "en": "Apa Itu 'Barrel Shifter'?", "id": "Rangkaian Untuk Menggeser Bit Dengan Cepat." },
  { "en": "Apa Itu 'Jalur Kritis (Critical Path)'?", "id": "Jalur Sinyal Dengan Penundaan Terlama." },
  { "en": "Apa Itu 'Simulasi RTL (Register-Transfer Level)'?", "id": "Memverifikasi Fungsionalitas Kode Desain." },
  { "en": "Apa Itu 'Sintesis Logika'?", "id": "Mengubah Kode RTL Menjadi Gerbang Logika." },
  { "en": "Apa Kepanjangan 'ISA (Instruction Set Architecture)'?", "id": "Arsitektur Set Instruksi." },
  { "en": "Apa Beda 'RISC' Dan 'CISC'?", "id": "RISC (Reduced Instruction Set Computer) Instruksi Sederhana." },
  { "en": "Apa Itu 'Penanganan Interrupt'?", "id": "Prosedur Yang Dijalankan Saat Interrupt." },
  { "en": "Apa Itu 'Protokol Bus'?", "id": "Aturan Komunikasi Di Atas Bus." },
  { "en": "Apa Itu 'Cermin Arus Wilson'?", "id": "Sirkuit Cermin Arus Yang Lebih Akurat." },
  { "en": "Apa Kepanjangan 'OTA (Operational Transconductance Amplifier)'?", "id": "Penguat Transkonduktansi Operasional." },
  { "en": "Apa Output Sebuah 'OTA'?", "id": "Sumber Arus Yang Dikontrol Tegangan." },
  { "en": "Apa Penyebab 'Ground Bounce'?", "id": "Induktansi Jalur Ground." },
  { "en": "Di Mana 'Kapasitor Decoupling' Ditempatkan?", "id": "Sedekat Mungkin Dengan Pin Daya IC." },
  { "en": "Apa Itu 'Mobilitas Pembawa'?", "id": "Kecepatan Gerak Pembawa Muatan." },
  { "en": "Apa Itu 'Rekombinasi'?", "id": "Saat Elektron Dan Lubang Bersatu." },
  { "en": "Apa Itu 'Model Kesalahan Stuck-at'?", "id": "Asumsi Jalur Terjebak Di Logika 0/1." },
  { "en": "Apa Itu 'Test Coverage'?", "id": "Persentase Kesalahan Yang Dapat Diuji." },
  { "en": "Apa Kepanjangan 'PMIC (Power Management IC)'?", "id": "IC (Integrated Circuit) Manajemen Daya." },
  { "en": "Apa Kepanjangan 'RFIC (Radio Frequency IC)'?", "id": "IC (Integrated Circuit) Frekuensi Radio." },
  { "en": "Apa Itu 'Bahan Die Attach'?", "id": "Perekat Untuk Melekatkan Die." },
  { "en": "Apa Itu 'Enkapsulasi'?", "id": "Proses Melindungi Chip Dengan Cetakan." },
  { "en": "Bahan Apa Untuk 'Enkapsulasi'?", "id": "Plastik Epoksi." },
  { "en": "Apa Itu 'Manufaktur Semikonduktor'?", "id": "Nama Lain Untuk Fabrikasi IC." },
  { "en": "Apa Itu 'Yield'?", "id": "Persentase Chip Fungsional Dari Wafer." },
  { "en": "Apa Itu 'Lot' Wafer?", "id": "Sekelompok Wafer Yang Diproses Bersama." },
  { "en": "Apa Itu 'Kelas Ruang Bersih'?", "id": "Standar Jumlah Partikel Di Udara." },
  { "en": "Apa Itu 'Fotolitografi'?", "id": "Proses Mencetak Pola Dengan Cahaya." },
  { "en": "Apa Itu 'Photoresist'?", "id": "Cairan Peka Cahaya Untuk Membuat Pola." },
  { "en": "Apa Itu 'Masker Foto (Photomask)'?", "id": "Template Kaca Dengan Pola Sirkuit." },
  { "en": "Apa Itu 'Etsa (Etching)'?", "id": "Proses Menghilangkan Material Secara Selektif." },
  { "en": "Apa Itu 'Deposisi'?", "id": "Proses Menambahkan Lapisan Material Tipis." },
  { "en": "Apa Itu 'Doping'?", "id": "Menambahkan Impuritas Untuk Mengubah Konduktivitas." },
  { "en": "Apa Itu 'Implantasi Ion'?", "id": "Metode Doping Dengan Menembakkan Ion." },
  { "en": "Apa Itu 'Oksidasi'?", "id": "Proses Menumbuhkan Lapisan Silikon Dioksida." },
  { "en": "Apa Itu 'CMP (Chemical Mechanical Planarization)'?", "id": "Proses Meratakan Permukaan Wafer." },
  { "en": "Apa Itu 'Metalisasi'?", "id": "Proses Pembentukan Jalur Logam." },
  { "en": "Apa Itu 'Pengemasan (Packaging)'?", "id": "Proses Melindungi Dan Menghubungkan Die." },
  { "en": "Apa Itu 'Pengujian Wafer (Wafer Sort)'?", "id": "Pengujian Chip Saat Masih Di Wafer." },
  { "en": "Apa Itu 'Pengujian Akhir (Final Test)'?", "id": "Pengujian Chip Setelah Dikemas." },
  { "en": "Apa Itu 'IP (Intellectual Property) Core'?", "id": "Blok Desain Sirkuit Siap Pakai." },
  { "en": "Apa Itu 'Desain RTL (Register-Transfer Level)'?", "id": "Mendeskripsikan Aliran Data Antar Register." },
  { "en": "Apa Itu 'Sintesis'?", "id": "Mengubah Kode RTL Menjadi Gerbang Logika." },
  { "en": "Apa Itu 'Tata Letak (Layout)'?", "id": "Gambar Fisik Geometris Dari Sirkuit." },
  { "en": "Apa Itu 'Verifikasi'?", "id": "Memastikan Desain Berfungsi Sesuai Spesifikasi." },
  { "en": "Apa Itu 'Simulasi'?", "id": "Menjalankan Model Komputer Dari Desain." },
  { "en": "Apa Itu 'Transistor Bipolar'?", "id": "Transistor Yang Gunakan Elektron Dan Lubang." },
  { "en": "Apa Itu 'Transistor Unipolar'?", "id": "Transistor Yang Gunakan Satu Jenis Pembawa." },
  { "en": "Contoh 'Transistor Unipolar'?", "id": "FET (Field-Effect Transistor)." },
  { "en": "Apa Itu 'Daerah Deplesi'?", "id": "Daerah Tanpa Pembawa Muatan Bebas." },
  { "en": "Apa Itu 'Bias Maju'?", "id": "Memberi Tegangan Untuk Menghantarkan Arus." },
  { "en": "Apa Itu 'Bias Mundur'?", "id": "Memberi Tegangan Untuk Menghambat Arus." },
  { "en": "Apa Itu 'Tegangan Tembus (Breakdown)'?", "id": "Tegangan Saat Dioda Mulai Menghantar Mundur." },
  { "en": "Apa Itu 'Efek Zener'?", "id": "Mekanisme Tembus Pada Doping Tinggi." },
  { "en": "Apa Itu 'Efek Avalanche'?", "id": "Mekanisme Tembus Akibat Ionisasi Tumbukan." },
  { "en": "Apa Itu 'Gerbang Logika'?", "id": "Blok Bangunan Dasar Sirkuit Digital." },
  { "en": "Apa Itu 'Keluarga Logika'?", "id": "Grup Gerbang Berbasis Teknologi Sama." },
  { "en": "Apa Itu 'Buffer'?", "id": "Gerbang Yang Menguatkan Sinyal." },
  { "en": "Apa Itu 'Inverter'?", "id": "Gerbang Yang Membalik Sinyal." },
  { "en": "Apa Itu 'Flip-Flop'?", "id": "Elemen Penyimpan Data Satu Bit." },
  { "en": "Apa Itu 'Register'?", "id": "Sekelompok Flip-flop Yang Menyimpan Data." },
  { "en": "Apa Itu 'Counter'?", "id": "Rangkaian Yang Menghitung Pulsa." },
  { "en": "Apa Itu 'Multiplexer'?", "id": "Rangkaian Pemilih Data." },
  { "en": "Apa Itu 'Decoder'?", "id": "Rangkaian Pengubah Kode." },
  { "en": "Apa Itu 'Penjumlah (Adder)'?", "id": "Rangkaian Untuk Operasi Penjumlahan." },
  { "en": "Apa Itu 'Memori'?", "id": "IC (Integrated Circuit) Yang Menyimpan Informasi." },
  { "en": "Apa Itu 'SRAM (Static RAM)'?", "id": "Memori Berbasis Flip-flop." },
  { "en": "Apa Itu 'DRAM (Dynamic RAM)'?", "id": "Memori Berbasis Kapasitor." },
  { "en": "Apa Itu 'Memori Flash'?", "id": "Memori Non-Volatil Yang Dihapus Listrik." },
  { "en": "Apa Itu 'CPU (Central Processing Unit)'?", "id": "Otak Dari Sistem Komputer." },
  { "en": "Apa Itu 'Mikrokontroler'?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu 'Sinyal Analog'?", "id": "Sinyal Dengan Nilai Kontinu." },
  { "en": "Apa Itu 'Sinyal Digital'?", "id": "Sinyal Dengan Nilai Diskrit." },
  { "en": "Apa Itu 'ADC (Analog-to-Digital Converter)'?", "id": "Mengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu 'DAC (Digital-to-Analog Converter)'?", "id": "Mengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu 'Paket DIP (Dual In-line Package)'?", "id": "Paket Dengan Dua Baris Pin." },
  { "en": "Apa Itu 'Paket SMT (Surface-Mount Technology)'?", "id": "Paket Yang Dipasang Di Permukaan." },
  { "en": "Apa Itu 'BGA (Ball Grid Array)'?", "id": "Paket Dengan Bola Solder Di Bawah." },
  { "en": "Apa Itu 'Disipasi Daya'?", "id": "Energi Yang Diubah IC Menjadi Panas." },
  { "en": "Apa Itu 'Tegangan Suplai'?", "id": "Tegangan Yang Dibutuhkan IC Agar Bekerja." },
  { "en": "Apa Itu 'Pin VCC'?", "id": "Pin Catu Daya Positif Bipolar." },
  { "en": "Apa Itu 'Pin VDD'?", "id": "Pin Catu Daya Positif MOS." },
  { "en": "Apa Itu 'Pin GND'?", "id": "Pin Referensi Tegangan Nol (Ground)." },
  { "en": "Apa Itu 'Pin NC'?", "id": "Pin Tanpa Koneksi (No Connection)." },
  { "en": "Apa Itu 'Datasheet'?", "id": "Dokumen Spesifikasi Teknis IC." },
  { "en": "Apa Itu 'Skala Integrasi'?", "id": "Ukuran Kepadatan Transistor Pada IC." },
  { "en": "Apa Itu 'Hukum Moore'?", "id": "Prediksi Peningkatan Kepadatan Transistor." },
  { "en": "Apa Itu 'Inti Prosesor (Core)'?", "id": "Unit Eksekusi Utama Dalam CPU." },
  { "en": "Apa Itu 'Multicore'?", "id": "Prosesor Dengan Beberapa Inti." },
  { "en": "Apa Itu 'Cache'?", "id": "Memori Cepat Di Dekat Prosesor." },
  { "en": "Apa Itu 'Osilator Kristal'?", "id": "Komponen Penghasil Sinyal Clock Stabil." },
  { "en": "Apa Itu 'Regulator LDO (Low-Dropout)'?", "id": "Regulator Tegangan Dengan Beda Rendah." },
  { "en": "Apa Itu 'Op-Amp (Operational Amplifier)'?", "id": "Penguat Diferensial Dengan Gain Tinggi." },
  { "en": "Apa Itu 'Timer 555'?", "id": "IC (Integrated Circuit) Pewaktu Serbaguna." },
  { "en": "Apa Itu 'LED (Light Emitting Diode)'?", "id": "Dioda Yang Memancarkan Cahaya." },
  { "en": "Apa Itu 'Fotodioda'?", "id": "Dioda Yang Peka Terhadap Cahaya." },
  { "en": "Apa Itu 'Optocoupler'?", "id": "Isolator Listrik Menggunakan Cahaya." },
  { "en": "Apa Itu 'Sensor Efek Hall'?", "id": "Sensor Yang Mendeteksi Medan Magnet." },
  { "en": "Apa Itu 'Sintesis Tingkat Tinggi (HLS)'?", "id": "Membuat Desain IC (Integrated Circuit) Dari Bahasa C." },
  { "en": "Apa Itu 'Verifikasi Formal'?", "id": "Membuktikan Kebenaran Desain Secara Matematis." },
  { "en": "Apa Itu 'Clock Tree Synthesis (CTS)'?", "id": "Proses Membangun Jaringan Distribusi Clock." },
  { "en": "Tujuan 'CTS (Clock Tree Synthesis)'?", "id": "Meminimalkan Skew Dan Jitter." },
  { "en": "Apa Itu 'Sinkronisasi'?", "id": "Proses Penyelarasan Dengan Sinyal Clock." },
  { "en": "Apa Itu 'Sistem Digital Multilevel'?", "id": "Sistem Dengan Lebih Dari Dua Level Logika." },
  { "en": "Apa Itu 'Logika Fuzzy'?", "id": "Logika Yang Menangani Nilai Antara 0-1." },
  { "en": "Apa Itu 'Sirkuit Digital Optik'?", "id": "Sirkuit Yang Memproses Sinyal Cahaya." },
  { "en": "Apa Itu 'Clock Recovery'?", "id": "Mengekstrak Sinyal Clock Dari Data." },
  { "en": "Apa Itu 'Data Encoding'?", "id": "Mengubah Data Menjadi Format Transmisi." },
  { "en": "Apa Itu 'Kode Manchester'?", "id": "Encoding Yang Menggabungkan Clock Dan Data." },
  { "en": "Apa Itu 'Transceiver'?", "id": "Perangkat Pengirim (Transmitter) Dan Penerima (Receiver)." },
  { "en": "Apa Itu 'Sistem Toleran Kesalahan'?", "id": "Sistem Yang Tetap Bekerja Meski Ada Gagal." },
  { "en": "Apa Itu 'Redundansi'?", "id": "Menyediakan Komponen Cadangan." },
  { "en": "Apa Itu 'Redundansi Perangkat Keras'?", "id": "Menggunakan Komponen Fisik Cadangan." },
  { "en": "Apa Itu 'Redundansi Informasi'?", "id": "Menambahkan Bit Ekstra (Seperti Paritas)." },
  { "en": "Apa Itu 'Voting Logic'?", "id": "Mengambil Keputusan Berdasarkan Mayoritas." },
  { "en": "Apa Itu 'Sistem Failsafe'?", "id": "Sistem Yang Gagal Dalam Keadaan Aman." },
  { "en": "Apa Itu 'Latch Transparan'?", "id": "Output Mengikuti Input Saat Enable." },
  { "en": "Apa Itu 'Pengalamatan Memori'?", "id": "Metode Untuk Memilih Lokasi Memori." },
  { "en": "Apa Itu 'Peta Memori'?", "id": "Alokasi Alamat Untuk Perangkat Berbeda." },
  { "en": "Apa Itu 'Waktu Siklus (Cycle Time)'?", "id": "Periode Sinyal Clock." },
  { "en": "Apa Itu 'Laju Transfer Data'?", "id": "Jumlah Data Yang Dipindahkan Per Detik." },
  { "en": "Apa Satuan 'Laju Transfer Data'?", "id": "Bit Per Detik (bps)." },
  { "en": "Apa Itu 'Bandwidth'?", "id": "Kapasitas Maksimum Laju Transfer Data." },
  { "en": "Apa Itu 'Latensi'?", "id": "Waktu Tunda Awal Transfer Data." },
  { "en": "Apa Itu 'Multiplexing'?", "id": "Berbagi Satu Saluran Untuk Banyak Sinyal." },
  { "en": "Apa Itu 'Demultiplexing'?", "id": "Memisahkan Sinyal Gabungan Kembali." },
  { "en": "Apa Kepanjangan 'FDM (Frequency Division Multiplexing)'?", "id": "Multiplexing Pembagian Frekuensi." },
  { "en": "Apa Kepanjangan 'TDM (Time Division Multiplexing)'?", "id": "Multiplexing Pembagian Waktu." },
  { "en": "Apa Itu 'Antarmuka Digital'?", "id": "Titik Sambungan Antara Perangkat Digital." },
  { "en": "Contoh 'Antarmuka Digital'?", "id": "USB (Universal Serial Bus), HDMI, SPI, I2C." },
  { "en": "Apa Kepanjangan 'USB (Universal Serial Bus)'?", "id": "Bus Serial Universal." },
  { "en": "Apa Itu 'Sirkuit Digital Kustom'?", "id": "Sirkuit Yang Didesain Untuk Tugas Tertentu." },
  { "en": "Apa Itu 'ASIC (Application-Specific Integrated Circuit)'?", "id": "Contoh Sirkuit Digital Kustom." },
  { "en": "Apa Itu 'Prosesor Sinyal Digital (DSP)'?", "id": "Prosesor Khusus Untuk Pemrosesan Sinyal." },
  { "en": "Apa Itu 'Siklus Tugas 50%'?", "id": "Waktu Sinyal Tinggi Dan Rendah Sama." },
  { "en": "Apa Itu 'Rangkaian Logika Resistor-Transistor (RTL)'?", "id": "Keluarga Logika Awal Menggunakan Resistor-Transistor." },
  { "en": "Apa Itu 'Rangkaian Logika Dioda-Transistor (DTL)'?", "id": "Keluarga Logika Sebelum TTL." },
  { "en": "Apa Itu 'Rangkaian Logika Emitter-Coupled (ECL)'?", "id": "Keluarga Logika Bipolar Berkecepatan Sangat Tinggi." },
  { "en": "Apa Itu 'Active Low'?", "id": "Sinyal Aktif Saat Logika Rendah (0)." },
  { "en": "Apa Itu 'Active High'?", "id": "Sinyal Aktif Saat Logika Tinggi (1)." },
  { "en": "Apa Itu 'Finite Automata'?", "id": "Model Matematika Untuk Mesin Keadaan." },
  { "en": "Apa Beda 'Finite Automata' Dan 'State Machine'?", "id": "Konsep Yang Sama, Beda Terminologi." },
  { "en": "Apa Itu 'System Call'?", "id": "Cara Program Aplikasi Meminta Layanan OS." },
  { "en": "Bagaimana 'Register' Berbeda Dari 'Cache'?", "id": "Register Di Dalam, Cache Di Luar CPU." },
  { "en": "Apa Peran 'Linker' Dalam Kompilasi?", "id": "Menggabungkan File Objek Menjadi Executable." },
  { "en": "Apa Itu 'Siklus Fetch'?", "id": "Tahap Pengambilan Instruksi Dari Memori." },
  { "en": "Apa Itu 'Siklus Execute'?", "id": "Tahap Menjalankan Instruksi Oleh CPU." },
  { "en": "Apa Itu 'Datapath'?", "id": "Jalur Data Dan Komponen Fungsional CPU." },
  { "en": "Apa Itu 'Control Path'?", "id": "Sirkuit Yang Mengontrol Aliran Datapath." },
  { "en": "Apa Itu 'Sinyal Kontrol'?", "id": "Sinyal Yang Dihasilkan Oleh Unit Kontrol." },
  { "en": "Apa Itu 'Micro-operation'?", "id": "Operasi Elementer Yang Dilakukan CPU." },
  { "en": "Apa Itu 'Register Transfer Language (RTL)'?", "id": "Bahasa Simbolik Mendeskripsikan Transfer Register." },
  { "en": "Apa Itu 'Carry Flag'?", "id": "Indikator Terjadinya Carry Out." },
  { "en": "Apa Itu 'Zero Flag'?", "id": "Indikator Hasil Operasi Adalah Nol." },
  { "en": "Apa Itu 'Sign Flag'?", "id": "Indikator Hasil Operasi Adalah Negatif." },
  { "en": "Apa Itu 'Overflow Flag'?", "id": "Indikator Terjadinya Aritmetika Overflow." },
  { "en": "Apa Itu 'Status Register'?", "id": "Register Yang Menyimpan Flag." },
  { "en": "Nama Lain 'Status Register'?", "id": "Flag Register Atau CCR (Condition Code Register)." },
  { "en": "Apa Itu 'Instruksi Percabangan (Branch)'?", "id": "Instruksi Yang Mengubah Alur Program." },
  { "en": "Apa Itu 'Percabangan Bersyarat (Conditional Branch)'?", "id": "Percabangan Yang Bergantung Pada Flag." },
  { "en": "Apa Itu 'Percabangan Tak Bersyarat (Unconditional)'?", "id": "Percabangan Yang Selalu Terjadi." },
  { "en": "Apa Itu 'Subroutine'?", "id": "Blok Kode Yang Bisa Dipanggil." },
  { "en": "Apa Instruksi Untuk Memanggil 'Subroutine'?", "id": "CALL." },
  { "en": "Apa Instruksi Untuk Kembali Dari 'Subroutine'?", "id": "RETURN." },
  { "en": "Bagaimana 'Stack' Digunakan Dalam Panggilan Subroutine?", "id": "Untuk Menyimpan Alamat Kembali." },
  { "en": "Apa Itu 'Interrupt Controller'?", "id": "Mengelola Prioritas Dan Sinyal Interrupt." },
  { "en": "Apa Beda 'Interrupt' Dan 'Subroutine' Call?", "id": "Interrupt Asinkron, Subroutine Sinkron." },
  { "en": "Apa Itu 'Interrupt Masking'?", "id": "Mengabaikan Atau Menonaktifkan Interrupt." },
  { "en": "Apa Itu 'Non-Maskable Interrupt (NMI)'?", "id": "Interrupt Prioritas Tinggi, Tidak Bisa Diabaikan." },
  { "en": "Apa Itu 'Siklus Stealing'?", "id": "DMA (Direct Memory Access) 'Mencuri' Siklus Bus." },
  { "en": "Apa Itu 'Lebar Bus'?", "id": "Jumlah Jalur Paralel Dalam Bus." },
  { "en": "Apa Itu 'Sinkronisasi Bus'?", "id": "Transfer Data Dikendalikan Oleh Clock." },
  { "en": "Apa Itu 'Bus Asinkron'?", "id": "Transfer Data Dikendalikan Sinyal Handshake." },
  { "en": "Apa Itu 'Arbiter Bus'?", "id": "Mengatur Perangkat Mana Yang Gunakan Bus." },
  { "en": "Apa Itu 'I/O Terisolasi'?", "id": "Ruang Alamat Terpisah Untuk I/O." },
  { "en": "Apa Itu 'I/O Dipetakan Memori'?", "id": "Berbagi Ruang Alamat Dengan Memori." },
  { "en": "Apa Itu 'Port Paralel'?", "id": "Antarmuka Untuk Transfer Data Paralel." },
  { "en": "Apa Itu 'Port Serial'?", "id": "Antarmuka Untuk Transfer Data Serial." },
  { "en": "Apa Itu 'Keluarga Logika'?", "id": "Sekelompok Gerbang Dengan Teknologi Sama." },
  { "en": "Apa Itu 'Noise Immunity'?", "id": "Kemampuan Sirkuit Menolak Gangguan Derau." },
  { "en": "Apa Itu 'Active Pull-up'?", "id": "Struktur Output Menggunakan Transistor Aktif." },
  { "en": "Apa Itu 'Passive Pull-up'?", "id": "Struktur Output Menggunakan Resistor." },
  { "en": "Apa Itu 'Gerbang AND-OR-Invert (AOI)'?", "id": "Gerbang Fungsional Kompleks." },
  { "en": "Apa Itu 'Kapasitansi Input'?", "id": "Kapasitansi Yang Dilihat Di Terminal Input." },
  { "en": "Apa Itu 'Kapasitansi Output'?", "id": "Kapasitansi Yang Dilihat Di Terminal Output." },
  { "en": "Apa Itu 'Characteristic Equation'?", "id": "Persamaan Yang Mendeskripsikan Perilaku Flip-Flop." },
  { "en": "Apa Itu 'Sequential Logic Design'?", "id": "Proses Merancang Rangkaian Sekuensial." },
  { "en": "Apa Itu 'Register Geser Universal'?", "id": "Bisa Geser Kiri, Kanan, Muat Paralel." },
  { "en": "Apa Itu 'Array Memori'?", "id": "Susunan Sel Memori Dalam Baris-Kolom." },
  { "en": "Apa Itu 'Sense Amplifier'?", "id": "Mendeteksi Sinyal Lemah Dari Sel Memori." },
  { "en": "Apa Itu 'Programmable Logic Array (PLA)'?", "id": "PLD (Programmable Logic Device) Dengan Plane AND-OR." },
  { "en": "Apa Itu 'Programmable Array Logic (PAL)'?", "id": "PLD (Programmable Logic Device) Dengan Plane AND." },
  { "en": "Apa Itu 'Complex Programmable Logic Device (CPLD)'?", "id": "Perangkat Logika Kompleks Berbasis Makrosel." },
  { "en": "Apa Itu 'Makrosel'?", "id": "Blok Fungsional Dalam CPLD." },
  { "en": "Apa Itu 'Hard Core Processor'?", "id": "CPU (Central Processing Unit) Fisik Di FPGA." },
  { "en": "Apa Itu 'Soft Core Processor'?", "id": "CPU (Central Processing Unit) Diimplementasi Di Logika." },
  { "en": "Apa Beda 'Verifikasi Formal' Dan 'Simulasi'?", "id": "Formal Buktikan, Simulasi Uji." },
  { "en": "Apa Itu 'Batasan Sintesis (Synthesis Constraint)'?", "id": "Aturan Untuk Proses Sintesis." },
  { "en": "Apa Kepanjangan 'CDC (Clock Domain Crossing)'?", "id": "Lintas Domain Clock." },
  { "en": "Apa Jenis 'Instruksi Transfer Data'?", "id": "LOAD, STORE, MOVE." },
  { "en": "Apa Jenis 'Instruksi Aritmetika'?", "id": "ADD, SUB, MUL, DIV." },
  { "en": "Apa Itu 'Mode Pengalamatan Immediate'?", "id": "Data Ada Di Dalam Instruksi." },
  { "en": "Apa Beda 'OTA' Dan 'Op-Amp'?", "id": "OTA (Operational Transconductance Amplifier) Output Arus." },
  { "en": "Apa Itu 'Penguat Umpan Balik Arus'?", "id": "Penguat Dengan Umpan Balik Arus." },
  { "en": "Apa Itu 'Sel Gilbert'?", "id": "Inti Sirkuit Pencampur (Mixer)." },
  { "en": "Bagaimana Cara Mengurangi 'Crosstalk'?", "id": "Jarak Jalur, Jalur Ground." },
  { "en": "Bagaimana 'Lebar Daerah Deplesi' Berubah?", "id": "Tergantung Pada Tegangan Bias." },
  { "en": "Apa Itu 'Simulasi Kesalahan (Fault Simulation)'?", "id": "Mensimulasikan Efek Kerusakan." },
  { "en": "Apa Itu 'Soket Burn-in'?", "id": "Soket Tahan Suhu Tinggi." },
  { "en": "Apa Kepanjangan 'SMC (System Management Controller)'?", "id": "Kontroler Manajemen Sistem." },
  { "en": "Apa Kepanjangan 'TPM (Trusted Platform Module)'?", "id": "Modul Platform Terpercaya." },
  { "en": "Siapa Yang Mencetuskan 'Hukum Moore'?", "id": "Gordon Moore." },
  { "en": "Apa Mikroprosesor Komersial Pertama?", "id": "Intel 4004." },
  { "en": "Apa Itu 'Wafer Sort'?", "id": "Pengujian Dan Pemilahan Die." },
  { "en": "Apa Itu 'Laser Marking'?", "id": "Menandai Paket IC Dengan Laser." },
  { "en": "Apa Itu 'Lead Pitch'?", "id": "Jarak Antar Pusat Pin IC." },
  { "en": "Apa Itu 'Coplanarity'?", "id": "Ketinggian Semua Pin Harus Sama." },
  { "en": "Apa Itu 'Ball Attach'?", "id": "Proses Pemasangan Bola Solder BGA." },
  { "en": "Apa Itu 'Flux'?", "id": "Bahan Kimia Pembersih Untuk Solder." },
  { "en": "Apa Itu 'Solder Bridging'?", "id": "Hubung Singkat Akibat Solder." },
  { "en": "Apa Itu 'Cold Solder Joint'?", "id": "Sambungan Solder Yang Tidak Matang." },
  { "en": "Apa Itu 'Rework Station'?", "id": "Alat Untuk Memperbaiki Kesalahan Solder." },
  { "en": "Apa Itu 'Bake-out'?", "id": "Memanaskan Komponen Untuk Hilangkan Lembab." },
  { "en": "Apa Itu 'Moisture Sensitivity Level (MSL)'?", "id": "Tingkat Sensitivitas IC Terhadap Kelembaban." },
  { "en": "Apa Itu 'Dry Pack'?", "id": "Kemasan Vakum Dengan Desiccant." },
  { "en": "Apa Itu 'Desiccant'?", "id": "Bahan Penyerap Kelembaban." },
  { "en": "Apa Itu 'Humidity Indicator Card (HIC)'?", "id": "Kartu Indikator Tingkat Kelembaban." },
  { "en": "Apa Itu 'Electroplating'?", "id": "Proses Pelapisan Logam Dengan Listrik." },
  { "en": "Apa Itu 'Anodizing'?", "id": "Membentuk Lapisan Oksida Pelindung." },
  { "en": "Apa Itu 'Chemical Etching'?", "id": "Nama Lain Untuk Wet Etching." },
  { "en": "Apa Itu 'Ion Beam Etching'?", "id": "Mengetsa Dengan Menembakkan Berkas Ion." },
  { "en": "Apa Itu 'Passivation'?", "id": "Lapisan Pelindung Terakhir Pada Chip." },
  { "en": "Apa Itu 'Sintering'?", "id": "Memadatkan Serbuk Material Dengan Panas." },
  { "en": "Apa Itu 'Die Preparation'?", "id": "Menyiapkan Wafer Untuk Dipotong." },
  { "en": "Apa Itu 'Wafer Mounting'?", "id": "Melekatkan Wafer Ke Bingkai." },
  { "en": "Apa Itu 'Die Separation'?", "id": "Nama Lain Untuk Proses Dicing." },
  { "en": "Apa Itu 'Blade Dicing'?", "id": "Memotong Wafer Dengan Gergaji Presisi." },
  { "en": "Apa Itu 'Stealth Dicing'?", "id": "Membuat Retakan Internal Dengan Laser." },
  { "en": "Apa Itu 'Eutectic Bonding'?", "id": "Menyambung Menggunakan Paduan Titik Lebur Rendah." },
  { "en": "Apa Itu 'Epoxy Adhesive'?", "id": "Perekat Berbasis Resin Epoksi." },
  { "en": "Apa Itu 'Thermal Conductivity'?", "id": "Kemampuan Bahan Menghantarkan Panas." },
  { "en": "Apa Itu 'Coefficient of Thermal Expansion (CTE)'?", "id": "Tingkat Pemuaian Material Terhadap Panas." },
  { "en": "Mengapa 'CTE (Coefficient of Thermal Expansion)' Penting?", "id": "Perbedaan CTE (Coefficient of Thermal Expansion) Sebabkan Stres." },
  { "en": "Apa Itu 'Stress Buffer'?", "id": "Lapisan Untuk Mengurangi Stres Mekanis." },
  { "en": "Apa Itu 'Wirebond Pull Test'?", "id": "Uji Kekuatan Tarik Kawat Bonding." },
  { "en": "Apa Itu 'Ball Shear Test'?", "id": "Uji Kekuatan Geser Sambungan Bola." },
  { "en": "Apa Itu 'Acoustic Microscopy'?", "id": "Pencitraan Menggunakan Gelombang Suara." },
  { "en": "Apa Fungsi 'Acoustic Microscopy'?", "id": "Mendeteksi Celah Atau Delaminasi." },
  { "en": "Apa Itu 'Delamination'?", "id": "Pemisahan Antar Lapisan Dalam Paket." },
  { "en": "Apa Itu 'Popcorning'?", "id": "Kerusakan Paket Akibat Uap Air." },
  { "en": "Apa Itu 'Tin Whiskers'?", "id": "Pertumbuhan Kristal Timah Mirip Kumis." },
  { "en": "Mengapa 'Tin Whiskers' Berbahaya?", "id": "Dapat Menyebabkan Hubung Singkat." },
  { "en": "Apa Itu 'Automated Optical Inspection (AOI)'?", "id": "Inspeksi Visual Menggunakan Kamera." },
  { "en": "Apa Itu 'Automated X-ray Inspection (AXI)'?", "id": "Inspeksi Menggunakan Sinar-X." },
  { "en": "Apa Itu 'In-Circuit Test (ICT)'?", "id": "Pengujian Komponen Saat Masih Di Papan." },
  { "en": "Apa Itu 'Bed of Nails'?", "id": "Fixture Uji Dengan Banyak Pin." },
  { "en": "Apa Itu 'Flying Probe'?", "id": "Pengujian Tanpa Fixture, Probe Bergerak." },
  { "en": "Apa Itu 'Functional Test'?", "id": "Pengujian Fungsi Akhir Produk." },
  { "en": "Apa Itu 'System-Level Test'?", "id": "Pengujian Sistem Secara Keseluruhan." },
  { "en": "Apa Itu 'Test Fixture'?", "id": "Alat Kustom Untuk Memegang DUT." },
  { "en": "Apa Itu 'Pogo Pin'?", "id": "Pin Kontak Berpegas Untuk Pengujian." },
  { "en": "Apa Itu 'Yield Analysis'?", "id": "Menganalisis Penyebab Rendahnya Yield." },
  { "en": "Apa Itu 'Wafer Map'?", "id": "Peta Visual Die Gagal Di Wafer." },
  { "en": "Apa Itu 'Process Control Monitor (PCM)'?", "id": "Struktur Tes Untuk Memantau Proses." },
  { "en": "Apa Itu 'Design for Yield (DFY)'?", "id": "Desain Untuk Memaksimalkan Yield." },
  { "en": "Apa Itu 'Design for Reliability (DFR)'?", "id": "Desain Untuk Memaksimalkan Keandalan." },
  { "en": "Apa Itu 'Semiconductor Device Modeling'?", "id": "Membuat Model Matematika Perangkat." },
  { "en": "Apa Itu 'SPICE Model'?", "id": "Model Standar Untuk Simulasi Sirkuit." },
  { "en": "Apa Itu 'IBIS Model'?", "id": "Model Perilaku I/O (Input/Output) Untuk Sinyal." },
  { "en": "Apa Itu 'Verilog-A'?", "id": "Bahasa Pemodelan Perilaku Analog." },
  { "en": "Apa Itu 'Verilog-AMS'?", "id": "Verilog Untuk Sinyal Campuran Analog." },
  { "en": "Apa Itu 'SystemVerilog'?", "id": "Ekstensi Verilog Untuk Verifikasi." },
  { "en": "Apa Itu 'Assertion-Based Verification (ABV)'?", "id": "Verifikasi Berbasis Pernyataan Properti." },
  { "en": "Apa Itu 'Universal Verification Methodology (UVM)'?", "id": "Metodologi Standar Untuk Verifikasi." },
  { "en": "Apa Itu 'Standard Cell Library'?", "id": "Kumpulan Gerbang Logika Dasar." },
  { "en": "Apa Itu 'I/O Library'?", "id": "Kumpulan Sel Input/Output." },
  { "en": "Apa Itu 'Memory Compiler'?", "id": "Alat Untuk Menghasilkan Layout Memori." },
  { "en": "Apa Itu 'Logic Synthesis'?", "id": "Mengubah RTL Menjadi Netlist Gerbang." },
  { "en": "Apa Itu 'Physical Synthesis'?", "id": "Optimisasi Netlist Dengan Info Fisik." },
  { "en": "Apa Itu 'Clock Tree Synthesis'?", "id": "Membangun Jaringan Distribusi Sinyal Clock." },
  { "en": "Apa Itu 'Static Timing Analysis (STA)'?", "id": "Menganalisis Waktu Tunda Tanpa Simulasi." },
  { "en": "Apa Itu 'Formal Verification'?", "id": "Membuktikan Kebenaran Desain Secara Matematis." },
  { "en": "Apa Itu 'Equivalence Checking'?", "id": "Membandingkan Dua Versi Desain." },
  { "en": "Apa Itu 'Model Checking'?", "id": "Memverifikasi Apakah Model Memenuhi Spesifikasi." },
  { "en": "Apa Itu 'Tapeout'?", "id": "Tahap Akhir Mengirim Desain Ke Pabrik." },
  { "en": "Apa Itu 'GDSII'?", "id": "Format File Standar Untuk Layout IC." },
  { "en": "Apa Itu 'OASIS'?", "id": "Format File Pengganti GDSII." }




        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
