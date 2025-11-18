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


  { "en": "Apa Satuan Dari Arus Listrik?", "id": "Ampere." },
  { "en": "Apa Satuan Dari Tegangan Listrik?", "id": "Volt." },
  { "en": "Apa Satuan Dari Hambatan Listrik?", "id": "Ohm." },
  { "en": "Apa Satuan Dari Daya Listrik?", "id": "Watt." },
  { "en": "Apa Rumus Hukum Ohm?", "id": "V = I x R." },
  { "en": "Apa Kepanjangan Dari AC? (Alternating Current)", "id": "Arus Bolak-Balik." },
  { "en": "Apa Kepanjangan Dari DC? (Direct Current)", "id": "Arus Searah." },
  { "en": "Apa Fungsi Dari Resistor?", "id": "Menghambat Arus Listrik." },
  { "en": "Apa Fungsi Dari Kapasitor?", "id": "Menyimpan Muatan Listrik Sementara." },
  { "en": "Apa Fungsi Dari Induktor?", "id": "Menyimpan Energi Medan Magnet." },
  { "en": "Apa Fungsi Dari Transformator?", "id": "Menaikkan Atau Menurunkan Tegangan." },
  { "en": "Apa Fungsi Dari Sekring Atau Fuse?", "id": "Mengamankan Rangkaian Dari Arus Lebih." },
  { "en": "Apa Kepanjangan Dari MCB? (Miniature Circuit Breaker)", "id": "Memutus Arus Hubung Singkat." },
  { "en": "Apa Nama Alat Ukur Arus Listrik?", "id": "Amperemeter." },
  { "en": "Apa Nama Alat Ukur Tegangan Listrik?", "id": "Voltmeter." },
  { "en": "Apa Nama Alat Ukur Hambatan Listrik?", "id": "Ohmmeter." },
  { "en": "Apa Nama Alat Ukur Daya Listrik?", "id": "Wattmeter." },
  { "en": "Apa Itu Rangkaian Seri?", "id": "Rangkaian Listrik Tanpa Percabangan Kabel." },
  { "en": "Apa Itu Rangkaian Paralel?", "id": "Rangkaian Listrik Yang Memiliki Percabangan." },
  { "en": "Apa Yang Dimaksud Dengan Konduktor?", "id": "Bahan Mudah Menghantarkan Listrik." },
  { "en": "Apa Contoh Bahan Konduktor?", "id": "Tembaga, Perak, Dan Emas." },
  { "en": "Apa Yang Dimaksud Dengan Isolator?", "id": "Bahan Sulit Menghantarkan Listrik." },
  { "en": "Apa Contoh Bahan Isolator?", "id": "Karet, Plastik, Dan Kaca." },
  { "en": "Apa Yang Dimaksud Dengan Semikonduktor?", "id": "Bahan Antara Konduktor, Isolator." },
  { "en": "Apa Kepanjangan Dari PLN? (Perusahaan Listrik Negara)", "id": "Badan Usaha Milik Negara Kelistrikan." },
  { "en": "Apa Fungsi Dari Generator?", "id": "Mengubah Energi Mekanik Menjadi Listrik." },
  { "en": "Apa Fungsi Dari Motor Listrik?", "id": "Mengubah Energi Listrik Menjadi Mekanik." },
  { "en": "Apa Itu Frekuensi Listrik?", "id": "Jumlah Siklus Arus AC Per Detik." },
  { "en": "Berapa Frekuensi Standar Listrik Di Indonesia?", "id": "50 Hertz." },
  { "en": "Berapa Tegangan Standar Listrik Rumah Tangga Indonesia?", "id": "220 Volt." },
  { "en": "Apa Itu Sistem Tiga Fasa?", "id": "Sistem Listrik Menggunakan Tiga Kawat Fasa." },
  { "en": "Apa Keuntungan Sistem Tiga Fasa?", "id": "Menghantarkan Daya Lebih Besar, Efisien." },
  { "en": "Apa Itu Hubung Singkat Atau Korsleting?", "id": "Hubungan Arus Listrik Tanpa Hambatan." },
  { "en": "Apa Bahaya Dari Hubung Singkat?", "id": "Menyebabkan Kebakaran Dan Kerusakan." },
  { "en": "Apa Fungsi Dari Grounding Atau Pentanahan?", "id": "Mengamankan Sistem Dari Tegangan Berlebih." },
  { "en": "Apa Warna Kabel Fasa Dalam Instalasi?", "id": "Hitam, Cokelat, Atau Abu-Abu." },
  { "en": "Apa Warna Kabel Netral Dalam Instalasi?", "id": "Biru." },
  { "en": "Apa Warna Kabel Ground Dalam Instalasi?", "id": "Hijau-Kuning." },
  { "en": "Apa Itu Gardu Induk?", "id": "Sub-Sistem Penyaluran Listrik Tegangan Tinggi." },
  { "en": "Apa Fungsi Gardu Distribusi?", "id": "Menurunkan Tegangan Ke Pelanggan." },
  { "en": "Apa Itu Transmisi Listrik?", "id": "Proses Penyaluran Listrik Jarak Jauh." },
  { "en": "Mengapa Transmisi Listrik Menggunakan Tegangan Tinggi?", "id": "Mengurangi Rugi-Rugi Daya." },
  { "en": "Apa Kepanjangan Dari KWH? (Kilo Watt Hour)", "id": "Satuan Energi Listrik Yang Digunakan." },
  { "en": "Apa Nama Komponen Pelindung Petir?", "id": "Arrester." },
  { "en": "Apa Itu Saklar?", "id": "Alat Memutus, Menghubungkan Arus Listrik." },
  { "en": "Apa Itu Stop Kontak?", "id": "Titik Sambungan Listrik Pada Dinding." },
  { "en": "Apa Kepanjangan Dari PUIL? (Peraturan Umum Instalasi Listrik)", "id": "Standar Acuan Instalasi Listrik Indonesia." },
  { "en": "Apa Itu Trafo Step-Up?", "id": "Transformator Menaikkan Tegangan Listrik." },
  { "en": "Apa Itu Trafo Step-Down?", "id": "Transformator Menurunkan Tegangan Listrik." },
  { "en": "Apa Yang Dimaksud Dengan Daya Semu?", "id": "Daya Keseluruhan Rangkaian AC." },
  { "en": "Apa Satuan Daya Semu?", "id": "Volt-Ampere (VA)." },
  { "en": "Apa Yang Dimaksud Dengan Daya Reaktif?", "id": "Daya Dibutuhkan Beban Induktif." },
  { "en": "Apa Satuan Daya Reaktif?", "id": "VAR." },
  { "en": "Apa Itu Faktor Daya Atau Power Factor?", "id": "Perbandingan Daya Aktif, Daya Semu." },
  { "en": "Berapa Nilai Ideal Faktor Daya?", "id": "Satu." },
  { "en": "Apa Fungsi Kapasitor Bank?", "id": "Memperbaiki Faktor Daya Listrik." },
  { "en": "Apa Itu Inverter?", "id": "Alat Pengubah Arus DC Menjadi AC." },
  { "en": "Apa Itu Rectifier Atau Penyearah?", "id": "Alat Pengubah Arus AC Menjadi DC." },
  { "en": "Apa Kepanjangan Dari PLC? (Programmable Logic Controller)", "id": "Kontroler Otomatisasi Industri." },
  { "en": "Apa Komponen Utama Panel Listrik?", "id": "MCB, Kontaktor, Busbar, Lainnya." },
  { "en": "Apa Fungsi Dari Busbar?", "id": "Pelat Konduktor Pembagi Arus Listrik." },
  { "en": "Apa Itu Kontaktor Magnetik?", "id": "Saklar Listrik Bekerja Otomatis." },
  { "en": "Apa Fungsi Thermal Overload Relay?", "id": "Melindungi Motor Dari Beban Berlebih." },
  { "en": "Apa Itu Sistem SCADA? (Supervisory Control And Data Acquisition)", "id": "Sistem Pengawasan, Kontrol Jarak Jauh." },
  { "en": "Apa Satuan Dari Induktansi?", "id": "Henry." },
  { "en": "Apa Satuan Dari Kapasitansi?", "id": "Farad." },
  { "en": "Apa Itu Reaktansi Induktif?", "id": "Hambatan Ditimbulkan Oleh Induktor." },
  { "en": "Apa Itu Reaktansi Kapasitif?", "id": "Hambatan Ditimbulkan Oleh Kapasitor." },
  { "en": "Apa Itu Impedansi?", "id": "Total Hambatan Listrik Rangkaian AC." },
  { "en": "Apa Satuan Dari Impedansi?", "id": "Ohm." },
  { "en": "Apa Hukum Kirchhoff Pertama?", "id": "Arus Masuk Sama Dengan Arus Keluar." },
  { "en": "Apa Hukum Kirchhoff Kedua?", "id": "Jumlah Tegangan Loop Tertutup Nol." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Air? (PLTA)", "id": "Pembangkit Listrik Memanfaatkan Tenaga Air." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Uap? (PLTU)", "id": "Pembangkit Listrik Memanfaatkan Tenaga Uap." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Diesel? (PLTD)", "id": "Pembangkit Listrik Menggunakan Mesin Diesel." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Surya? (PLTS)", "id": "Pembangkit Listrik Memanfaatkan Energi Matahari." },
  { "en": "Apa Komponen Utama PLTS?", "id": "Panel Surya." },
  { "en": "Apa Fungsi Panel Surya?", "id": "Mengubah Energi Matahari Menjadi Listrik DC." },
  { "en": "Apa Itu Jaringan Distribusi Primer?", "id": "Jaringan Listrik Tegangan Menengah." },
  { "en": "Apa Itu Jaringan Distribusi Sekunder?", "id": "Jaringan Listrik Tegangan Rendah." },
  { "en": "Berapa Tegangan Jaringan Distribusi Primer Indonesia?", "id": "20 Kilo Volt (kV)." },
  { "en": "Apa Itu Relay Proteksi?", "id": "Alat Mendeteksi Gangguan Sistem Listrik." },
  { "en": "Apa Fungsi Dari Pemutus Tenaga? (PMT)", "id": "Memutus Arus Listrik Saat Gangguan." },
  { "en": "Apa Fungsi Pemisah? (PMS)", "id": "Memisahkan Peralatan Listrik Tanpa Beban." },
  { "en": "Apa Itu Arus Bocor?", "id": "Arus Listrik Mengalir Ke Tanah." },
  { "en": "Apa Kepanjangan Dari ELCB? (Earth Leakage Circuit Breaker)", "id": "Alat Proteksi Terhadap Arus Bocor." },
  { "en": "Apa Bahaya Arus Listrik Bagi Manusia?", "id": "Menyebabkan Cedera Hingga Kematian." },
  { "en": "Apa Itu Tahanan Isolasi?", "id": "Nilai Hambatan Bahan Isolasi Listrik." },
  { "en": "Alat Apa Untuk Mengukur Tahanan Isolasi?", "id": "Insulation Tester (Megger)." },
  { "en": "Apa Itu Efek Kulit? (Skin Effect)", "id": "Arus AC Mengalir Di Permukaan." },
  { "en": "Apa Itu Sistem Interkoneksi Listrik?", "id": "Gabungan Beberapa Sistem Pembangkit Listrik." },
  { "en": "Apa Keuntungan Sistem Interkoneksi?", "id": "Keandalan Pasokan Listrik Lebih Tinggi." },
  { "en": "Apa Itu Beban Puncak?", "id": "Kebutuhan Daya Listrik Tertinggi Sistem." },
  { "en": "Apa Itu Rugi-Rugi Daya? (Power Losses)", "id": "Energi Listrik Hilang Saat Penyaluran." },
  { "en": "Apa Penyebab Utama Rugi-Rugi Daya?", "id": "Hambatan Kawat Penghantar Listrik." },
  { "en": "Apa Itu Motor Induksi?", "id": "Motor AC Paling Umum Digunakan." },
  { "en": "Apa Bagian Motor Yang Diam?", "id": "Stator." },
  { "en": "Apa Bagian Motor Yang Berputar?", "id": "Rotor." },
  { "en": "Apa Itu Starting Bintang-Segitiga? (Star-Delta)", "id": "Metode Mengurangi Arus Start Motor." },
  { "en": "Apa Kepanjangan AVR? (Automatic Voltage Regulator)", "id": "Mengatur Tegangan Output Generator." },
  { "en": "Apa Fungsi Inti Transformator?", "id": "Tempat Mengalirnya Fluks Magnet." },
  { "en": "Apa Itu Lilitan Primer?", "id": "Lilitan Sisi Input Tegangan Trafo." },
  { "en": "Apa Itu Lilitan Sekunder?", "id": "Lilitan Sisi Output Tegangan Trafo." },
  { "en": "Apa Pendingin Trafo Tipe Kering?", "id": "Menggunakan Udara Sebagai Pendingin." },
  { "en": "Apa Pendingin Trafo Tipe Basah?", "id": "Menggunakan Oli Sebagai Pendingin." },
  { "en": "Apa Kepanjangan CT? (Current Transformer)", "id": "Mengukur Arus Besar Tidak Langsung." },
  { "en": "Apa Kepanjangan PT? (Potential Transformer)", "id": "Mengukur Tegangan Tinggi Tidak Langsung." },
  { "en": "Apa Kepanjangan KHA? (Kemampuan Hantar Arus)", "id": "Batas Arus Maksimum Sebuah Kabel." },
  { "en": "Apa Itu Harmonisa?", "id": "Gangguan Gelombang Sinus Murni." },
  { "en": "Apa Satuan Fluks Cahaya?", "id": "Lumen." },
  { "en": "Apa Satuan Kuat Penerangan?", "id": "Lux." },
  { "en": "Apa Itu Motor Sinkron?", "id": "Motor AC Kecepatan Putar Sinkron." },
  { "en": "Apa Itu Slip Pada Motor Induksi?", "id": "Perbedaan Kecepatan Rotor, Medan Stator." },
  { "en": "Apa Itu Torsi Motor?", "id": "Gaya Putar Yang Dihasilkan Motor." },
  { "en": "Apa Itu Eksitasi Generator?", "id": "Proses Pembangkitan Medan Magnet Generator." },
  { "en": "Apa Fungsi Sikat Arang? (Carbon Brush)", "id": "Menghantarkan Listrik Ke Bagian Berputar." },
  { "en": "Apa Itu Trafo Distribusi?", "id": "Trafo Penurun Tegangan Ke Pelanggan." },
  { "en": "Apa Itu Trafo Daya?", "id": "Trafo Kapasitas Besar Di Transmisi." },
  { "en": "Apa Itu Rugi Tembaga? (Copper Loss)", "id": "Rugi Daya Pada Lilitan Trafo." },
  { "en": "Apa Itu Rugi Inti? (Core Loss)", "id": "Rugi Daya Pada Inti Besi Trafo." },
  { "en": "Apa Itu Efisiensi Transformator?", "id": "Perbandingan Daya Output Terhadap Input." },
  { "en": "Apa Syarat Paralel Transformator?", "id": "Tegangan, Polaritas, Impedansi Sama." },
  { "en": "Apa Kepanjangan OCR? (Over Current Relay)", "id": "Relay Proteksi Terhadap Arus Lebih." },
  { "en": "Apa Kepanjangan GFR? (Ground Fault Relay)", "id": "Relay Proteksi Terhadap Gangguan Tanah." },
  { "en": "Apa Itu Relay Diferensial?", "id": "Mengamankan Trafo Dari Gangguan Internal." },
  { "en": "Apa Kepanjangan AIS? (Air Insulated Switchgear)", "id": "Switchgear Berisolasi Udara." },
  { "en": "Apa Kepanjangan GIS? (Gas Insulated Switchgear)", "id": "Switchgear Berisolasi Gas SF6." },
  { "en": "Apa Fungsi Gas SF6?", "id": "Isolator Pemadam Busur Api Listrik." },
  { "en": "Apa Itu Kabel NYM?", "id": "Kabel Inti Tembaga, Isolasi PVC." },
  { "en": "Apa Itu Kabel NYY?", "id": "Kabel Tanah, Isolasi PVC Ganda." },
  { "en": "Apa Itu Kabel NYAF?", "id": "Kabel Serabut, Isolasi PVC Fleksibel." },
  { "en": "Apa Itu Tegangan Jatuh? (Voltage Drop)", "id": "Pengurangan Tegangan Sepanjang Saluran." },
  { "en": "Apa Penyebab Tegangan Jatuh?", "id": "Hambatan Kabel, Panjang Saluran." },
  { "en": "Apa Itu Voltage Sag?", "id": "Penurunan Tegangan Listrik Sementara." },
  { "en": "Apa Itu Voltage Swell?", "id": "Kenaikan Tegangan Listrik Sementara." },
  { "en": "Apa Itu Flicker?", "id": "Kedipan Cahaya Lampu Akibat Fluktuasi." },
  { "en": "Apa Itu Tahanan Pentanahan?", "id": "Nilai Hambatan Sistem Grounding." },
  { "en": "Berapa Nilai Ideal Tahanan Pentanahan?", "id": "Idealnya Di Bawah Satu Ohm." },
  { "en": "Apa Itu Elektroda Pentanahan?", "id": "Batang Logam Ditanam Ke Tanah." },
  { "en": "Apa Kepanjangan APD? (Alat Pelindung Diri)", "id": "Peralatan Keselamatan Kerja Listrik." },
  { "en": "Apa Contoh APD Listrik?", "id": "Helm, Sarung Tangan Isolasi, Sepatu." },
  { "en": "Apa Kepanjangan LOTO? (Lock Out Tag Out)", "id": "Prosedur Penguncian Sumber Energi." },
  { "en": "Apa Tujuan LOTO?", "id": "Mencegah Kecelakaan Saat Perbaikan." },
  { "en": "Apa Itu Motor DC?", "id": "Motor Listrik Menggunakan Arus Searah." },
  { "en": "Apa Bagian Utama Motor DC?", "id": "Stator, Rotor, Dan Komutator." },
  { "en": "Apa Fungsi Komutator?", "id": "Menyearahkan Arus Pada Lilitan Rotor." },
  { "en": "Apa Itu Motor DC Shunt?", "id": "Lilitan Medan Paralel Dengan Rotor." },
  { "en": "Apa Itu Motor DC Seri?", "id": "Lilitan Medan Seri Dengan Rotor." },
  { "en": "Apa Itu Motor DC Kompon?", "id": "Gabungan Lilitan Seri Dan Shunt." },
  { "en": "Apa Itu Generator DC?", "id": "Menghasilkan Tegangan Listrik Arus Searah." },
  { "en": "Apa Beda Generator AC Dan DC?", "id": "AC Pakai Slip Ring, DC Komutator." },
  { "en": "Apa Itu Generator Sinkron?", "id": "Generator AC Frekuensi Putaran Sinkron." },
  { "en": "Apa Itu Generator Asinkron?", "id": "Disebut Juga Generator Induksi." },
  { "en": "Apa Itu Panel ATS? (Automatic Transfer Switch)", "id": "Panel Pemindah Suplai Listrik Otomatis." },
  { "en": "Apa Itu Panel AMF? (Automatic Main Failure)", "id": "Panel Menghidupkan Genset Otomatis." },
  { "en": "Apa Fungsi Kapasitor Starting?", "id": "Membantu Motor Satu Fasa Berputar." },
  { "en": "Apa Itu Motor Satu Fasa?", "id": "Motor Listrik Beroperasi Satu Fasa." },
  { "en": "Apa Itu Belitan Bantu? (Auxiliary Winding)", "id": "Belitan Kedua Motor Satu Fasa." },
  { "en": "Apa Itu Belitan Utama? (Main Winding)", "id": "Belitan Utama Motor Satu Fasa." },
  { "en": "Apa Itu Fasa Listrik?", "id": "Kawat Penghantar Listrik Bertegangan." },
  { "en": "Apa Itu Netral Listrik?", "id": "Kawat Penghantar Listrik Tidak Bertegangan." },
  { "en": "Apa Itu Tegangan Fasa Ke Netral?", "id": "Tegangan Antara Kawat Fasa, Netral." },
  { "en": "Apa Itu Tegangan Fasa Ke Fasa?", "id": "Tegangan Antara Dua Kawat Fasa." },
  { "en": "Berapa Tegangan Fasa Ke Fasa Di Indonesia?", "id": "380 Volt." },
  { "en": "Apa Itu Sambungan Bintang? (Star)", "id": "Titik Netral Tiga Fasa Terhubung." },
  { "en": "Apa Itu Sambungan Segitiga? (Delta)", "id": "Ujung Fasa Terhubung Ke Awal Fasa." },
  { "en": "Apa Keuntungan Sambungan Bintang?", "id": "Mendapatkan Tegangan Fasa-Netral 220V." },
  { "en": "Apa Keuntungan Sambungan Delta?", "id": "Tidak Memerlukan Kawat Netral." },
  { "en": "Apa Itu Sistem TT? (Terra Terra)", "id": "Sistem Pentanahan Netral, Bodi Terpisah." },
  { "en": "Apa Itu Sistem TN? (Terra Neutral)", "id": "Sistem Pentanahan Netral, Bodi Tergabung." },
  { "en": "Apa Itu Sistem IT? (Isolate Terra)", "id": "Sistem Pentanahan Netral Terisolasi." },
  { "en": "Apa Itu Isolasi Listrik?", "id": "Bahan Penyekat Aliran Arus Listrik." },
  { "en": "Apa Itu Tegangan Tembus? (Breakdown Voltage)", "id": "Batas Tegangan Maksimum Isolasi." },
  { "en": "Apa Itu Switching?", "id": "Proses Membuka, Menutup Rangkaian Listrik." },
  { "en": "Apa Itu Busur Api? (Arc Flash)", "id": "Ledakan Listrik Akibat Hubung Singkat." },
  { "en": "Apa Bahaya Busur Api?", "id": "Panas Tinggi, Cahaya Silau, Ledakan." },
  { "en": "Apa Itu Konduktivitas?", "id": "Kemampuan Bahan Menghantarkan Arus." },
  { "en": "Apa Kebalikan Dari Konduktivitas?", "id": "Resistivitas Atau Tahanan Jenis." },
  { "en": "Apa Itu Resistivitas?", "id": "Kemampuan Bahan Menghambat Arus." },
  { "en": "Apa Satuan Resistivitas?", "id": "Ohm-Meter." },
  { "en": "Apa Itu Motor Universal?", "id": "Motor Dapat Bekerja AC Maupun DC." },
  { "en": "Di Mana Motor Universal Digunakan?", "id": "Bor Listrik, Blender, Vacuum Cleaner." },
  { "en": "Apa Itu VFD? (Variable Frequency Drive)", "id": "Mengatur Kecepatan Putaran Motor AC." },
  { "en": "Bagaimana VFD Mengatur Kecepatan Motor?", "id": "Mengubah Frekuensi Tegangan Input." },
  { "en": "Apa Itu Soft Starter?", "id": "Mengurangi Lonjakan Arus Start Motor." },
  { "en": "Apa Itu Trafo Auto? (Autotransformer)", "id": "Trafo Hanya Dengan Satu Lilitan." },
  { "en": "Apa Itu Saluran Udara Tegangan Tinggi? (SUTT)", "id": "Transmisi Listrik Lewat Kabel Udara." },
  { "en": "Apa Itu Saluran Kabel Laut Tegangan Tinggi? (SKLT)", "id": "Transmisi Listrik Lewat Kabel Laut." },
  { "en": "Apa Fungsi Isolator Jaringan?", "id": "Menyekat Kawat Fasa Dari Tiang." },
  { "en": "Apa Jenis Isolator Jaringan?", "id": "Isolator Piring, Isolator Tarik." },
  { "en": "Apa Itu Saluran Udara Tegangan Ekstra Tinggi? (SUTET)", "id": "Transmisi Listrik 500 Kilo Volt (kV)." },
  { "en": "Apa Fungsi Utama SUTET?", "id": "Menyalurkan Listrik Daya Besar, Jarak Jauh." },
  { "en": "Apa Itu Konduktor Berkas? (Bundle Conductor)", "id": "Beberapa Kawat Konduktor Per Fasa." },
  { "en": "Apa Tujuan Konduktor Berkas?", "id": "Mengurangi Efek Korona, Reaktansi Induktif." },
  { "en": "Apa Itu Efek Korona?", "id": "Pelepasan Listrik Di Sekitar Konduktor." },
  { "en": "Apa Tanda-Tanda Efek Korona?", "id": "Suara Mendesis, Cahaya Ungu Malam Hari." },
  { "en": "Apa Itu Kawat Tanah? (Ground Wire)", "id": "Kawat Pelindung Petir Di Atas Fasa." },
  { "en": "Apa Fungsi Lain Kawat Tanah?", "id": "Sebagai Media Komunikasi (OPGW)." },
  { "en": "Apa Kepanjangan OPGW? (Optical Ground Wire)", "id": "Kawat Tanah Dengan Serat Optik." },
  { "en": "Apa Itu Tower Transmisi?", "id": "Struktur Penyangga Konduktor Saluran Udara." },
  { "en": "Apa Jenis Tower Transmisi?", "id": "Tower Lattice, Tower Monopole." },
  { "en": "Apa Itu Andongan Kabel? (Sag)", "id": "Lengkungan Kabel Di Antara Dua Tiang." },
  { "en": "Mengapa Andongan Kabel Penting?", "id": "Mengatur Tegangan Tarik Mekanis Kabel." },
  { "en": "Apa Itu Jarak Bebas? (Clearance)", "id": "Jarak Aman Konduktor Dari Benda Lain." },
  { "en": "Apa Itu Switchyard?", "id": "Area Peralatan Tegangan Tinggi Gardu Induk." },
  { "en": "Apa Itu Bay Trafo?", "id": "Bagian Switchyard Khusus Untuk Transformator." },
  { "en": "Apa Itu Bay Penghantar?", "id": "Bagian Switchyard Menuju Saluran Transmisi." },
  { "en": "Apa Fungsi Minyak Trafo?", "id": "Sebagai Pendingin Dan Isolasi." },
  { "en": "Apa Itu Buchholz Relay?", "id": "Relay Pendeteksi Gangguan Internal Trafo." },
  { "en": "Bagaimana Prinsip Kerja Buchholz Relay?", "id": "Mendeteksi Adanya Gelembung Gas Minyak." },
  { "en": "Apa Itu Tap Changer?", "id": "Alat Pengatur Tegangan Output Trafo." },
  { "en": "Apa Jenis Tap Changer?", "id": "On-Load (OLTC), Off-Load (De-Energized)." },
  { "en": "Apa Itu Bushing Transformator?", "id": "Isolator Penghubung Lilitan Ke Jaringan Luar." },
  { "en": "Apa Itu Konservator Trafo?", "id": "Tangki Cadangan Minyak Transformator." },
  { "en": "Apa Fungsi Silika Gel Trafo?", "id": "Menyerap Kelembaban Udara Pernapasan Trafo." },
  { "en": "Apa Tanda Silika Gel Jenuh?", "id": "Berubah Warna (Biru Menjadi Merah Muda)." },
  { "en": "Apa Itu Tes Tahanan Isolasi?", "id": "Mengukur Kualitas Isolasi Peralatan Listrik." },
  { "en": "Apa Itu Tes Rasio Belitan Trafo?", "id": "Memastikan Perbandingan Lilitan Sesuai." },
  { "en": "Apa Itu Tes Tahanan Lilitan?", "id": "Mengukur Nilai Hambatan DC Lilitan Trafo." },
  { "en": "Apa Itu Sistem Proteksi Listrik?", "id": "Sistem Pengaman Peralatan Dari Gangguan." },
  { "en": "Apa Sifat Sistem Proteksi?", "id": "Harus Selektif, Cepat, Andal, Sensitif." },
  { "en": "Apa Itu Selektivitas Proteksi?", "id": "Hanya Memutus Bagian Yang Terganggu." },
  { "en": "Apa Itu Keandalan Proteksi?", "id": "Pasti Bekerja Saat Dibutuhkan." },
  { "en": "Apa Itu Gangguan Listrik?", "id": "Kondisi Abnormal Sistem Tenaga Listrik." },
  { "en": "Apa Jenis Gangguan Listrik?", "id": "Hubung Singkat, Beban Lebih, Gangguan Tanah." },
  { "en": "Apa Itu Gangguan Fasa Ke Tanah?", "id": "Hubung Singkat Satu Fasa Ke Tanah." },
  { "en": "Apa Itu Gangguan Fasa Ke Fasa?", "id": "Hubung Singkat Antara Dua Fasa." },
  { "en": "Apa Itu Gangguan Tiga Fasa?", "id": "Hubung Singkat Melibatkan Ketiga Fasa." },
  { "en": "Apa Itu Beban Lebih? (Overload)", "id": "Arus Melebihi Batas Normal Terus Menerus." },
  { "en": "Apa Itu Arus Surja? (Inrush Current)", "id": "Lonjakan Arus Sesaat Saat Start." },
  { "en": "Apa Itu Surja Petir?", "id": "Tegangan Lebih Akibat Sambaran Petir." },
  { "en": "Apa Itu Surja Hubung? (Switching Surge)", "id": "Tegangan Lebih Akibat Operasi PMT." },
  { "en": "Apa Fungsi Baterai Di Gardu Induk?", "id": "Sumber Listrik DC Untuk Kontrol, Proteksi." },
  { "en": "Apa Fungsi Charger Baterai?", "id": "Mengisi Ulang Baterai Gardu Induk." },
  { "en": "Apa Itu Panel DC?", "id": "Panel Distribusi Listrik DC Gardu Induk." },
  { "en": "Apa Itu Panel AC?", "id": "Panel Distribusi Listrik AC Gardu Induk." },
  { "en": "Apa Itu Motor Starter?", "id": "Rangkaian Pengendali Operasi Awal Motor." },
  { "en": "Apa Itu DOL Starter? (Direct On Line)", "id": "Menghubungkan Motor Langsung Ke Jaringan." },
  { "en": "Apa Kelemahan DOL Starter?", "id": "Arus Start Sangat Tinggi." },
  { "en": "Kapan DOL Starter Digunakan?", "id": "Motor Berdaya Kecil." },
  { "en": "Apa Itu Rangkaian Kontrol?", "id": "Rangkaian Logika Pengendali Rangkaian Daya." },
  { "en": "Apa Itu Rangkaian Daya?", "id": "Rangkaian Utama Penyalur Listrik Ke Beban." },
  { "en": "Apa Itu Tombol Push Button?", "id": "Saklar Sesaat (Momentary) Untuk Kontrol." },
  { "en": "Apa Itu Lampu Indikator?", "id": "Memberi Tanda Visual Status Operasi." },
  { "en": "Apa Itu Timer Relay?", "id": "Relay Penunda Waktu (Time Delay Relay)." },
  { "en": "Apa Itu Kontrol Interlock?", "id": "Sistem Pengunci Agar Dua Operasi Berbeda." },
  { "en": "Apa Itu Emergency Stop?", "id": "Tombol Mematikan Sistem Saat Darurat." },
  { "en": "Apa Itu Motor Tiga Fasa?", "id": "Motor Listrik Beroperasi Tiga Fasa." },
  { "en": "Apa Itu Nameplate Motor?", "id": "Label Spesifikasi Teknis Motor Listrik." },
  { "en": "Informasi Apa Di Nameplate Motor?", "id": "Daya, Tegangan, Arus, Putaran (RPM)." },
  { "en": "Apa Kepanjangan RPM? (Rotations Per Minute)", "id": "Satuan Kecepatan Putaran Motor." },
  { "en": "Apa Itu Kelas Isolasi Motor?", "id": "Batas Suhu Operasi Maksimum Lilitan." },
  { "en": "Apa Contoh Kelas Isolasi Motor?", "id": "Kelas F, Kelas H, Kelas B." },
  { "en": "Apa Itu IP Rating? (Ingress Protection)", "id": "Standar Proteksi Selubung Peralatan." },
  { "en": "Apa Arti IP65?", "id": "Tahan Debu Total, Tahan Semprotan Air." },
  { "en":"Apa Itu Pembangkit Listrik Tenaga Panas Bumi? (PLTP)", "id": "Pembangkit Listrik Tenaga Panas Bumi." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Gas? (PLTG)", "id": "Pembangkit Listrik Menggunakan Turbin Gas." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Gas Uap? (PLTGU)", "id": "Gabungan PLTG Dan PLTU (Siklus Kombinasi)." },
  { "en": "Apa Keuntungan PLTGU?", "id": "Efisiensi Pembangkitan Energi Sangat Tinggi." },
  { "en": "Apa Itu Energi Terbarukan?", "id": "Energi Dari Sumber Alam Berkelanjutan." },
  { "en": "Apa Contoh Energi Terbarukan?", "id": "Surya, Angin, Air, Panas Bumi." },
  { "en": "Apa Itu Grid Listrik? (Power Grid)", "id": "Jaringan Interkoneksi Listrik Nasional." },
  { "en": "Apa Itu Blackout?", "id": "Padam Listrik Total Di Area Luas." },
  { "en": "Apa Itu Load Shedding?", "id": "Pemadaman Listrik Bergilir Terencana." },
  { "en": "Mengapa Load Shedding Dilakukan?", "id": "Kekurangan Pasokan Daya Listrik." },
  { "en": "Apa Itu Studi Kelayakan? (Feasibility Study)", "id": "Analisis Awal Proyek Ketenagalistrikan." },
  { "en": "Apa Itu Commissioning Test?", "id": "Pengujian Peralatan Baru Sebelum Operasi." },
  { "en": "Apa Itu Preventive Maintenance?", "id": "Perawatan Pencegahan Kerusakan Peralatan." },
  { "en": "Apa Itu Corrective Maintenance?", "id": "Perbaikan Peralatan Setelah Terjadi Kerusakan." },
  { "en": "Apa Itu Predictive Maintenance?", "id": "Perawatan Berdasarkan Prediksi Kondisi Alat." },
  { "en": "Apa Itu Tang Ampere? (Clamp Meter)", "id": "Alat Ukur Arus Tanpa Memutus Rangkaian." },
  { "en": "Apa Itu Multimeter?", "id": "Alat Ukur Listrik Berbagai Besaran." },
  { "en": "Apa Yang Diukur Multimeter?", "id": "Tegangan (Volt), Arus (Ampere), Hambatan (Ohm)." },
  { "en": "Apa Itu Osiloskop?", "id": "Alat Ukur Bentuk Gelombang Listrik." },
  { "en": "Apa Itu Phase Sequence Meter?", "id": "Alat Pengecek Urutan Fasa Listrik." },
  { "en": "Mengapa Urutan Fasa Penting?", "id": "Menentukan Arah Putaran Motor Tiga Fasa." },
  { "en": "Apa Itu Safety Instrumented System? (SIS)", "id": "Sistem Pengaman Kritis Proses Industri." },
  { "en": "Apa Itu Uninterruptible Power Supply? (UPS)", "id": "Catu Daya Listrik Darurat Tanpa Jeda." },
  { "en": "Apa Fungsi Utama UPS?", "id": "Memberi Listrik Saat PLN Padam Sesaat." },
  { "en": "Apa Komponen Utama UPS?", "id": "Rectifier, Baterai, Inverter." },
  { "en": "Apa Itu Genset?", "id": "Generator Set, Pembangkit Listrik Cadangan." },
  { "en": "Apa Bahan Bakar Genset?", "id": "Umumnya Menggunakan Solar (Diesel)." },
  { "en": "Apa Itu Sinkronisasi Genset?", "id": "Proses Paralel Genset Dengan Jaringan." },
  { "en": "Apa Syarat Sinkronisasi?", "id": "Tegangan, Frekuensi, Urutan Fasa Sama." },
  { "en": "Apa Itu Kabel Ladder?", "id": "Rak Penyangga Jalur Instalasi Kabel." },
  { "en": "Apa Itu Kabel Tray?", "id": "Baki Penyangga Jalur Instalasi Kabel." },
  { "en": "Apa Itu Conduit Kabel?", "id": "Pipa Pelindung Instalasi Kabel Listrik." },
  { "en": "Apa Itu Terminasi Kabel?", "id": "Penyambungan Ujung Kabel Ke Peralatan." },
  { "en": "Apa Itu Jointing Kabel?", "id": "Proses Penyambungan Dua Kabel Listrik." },
  { "en": "Apa Itu Kabel Skun? (Cable Lug)", "id": "Konektor Ujung Kabel Ke Terminal." },
  { "en": "Apa Itu Heatshrink Tube?", "id": "Selongsong Isolasi Kabel Yang Menyusut." },
  { "en": "Apa Itu Tes Hi-Pot? (High Potential)", "id": "Tes Uji Tegangan Tinggi Isolasi." },
  { "en": "Apa Tujuan Tes Hi-Pot?", "id": "Memastikan Kekuatan Isolasi Kabel." },
  { "en": "Apa Itu Fasa Meter Digital?", "id": "Alat Ukur Digital Besaran Listrik." },
  { "en": "Apa Itu Power Quality Analyzer?", "id": "Alat Analisis Kualitas Daya Listrik." },
  { "en": "Apa Yang Dianalisis Power Quality?", "id": "Harmonisa, Sag, Swell, Frekuensi." },
  { "en": "Apa Itu Distorsi Harmonik Total? (THD)", "id": "Ukuran Polusi Gelombang Harmonisa." },
  { "en": "Apa Batas Ideal THD Tegangan?", "id": "Di Bawah Lima Persen." },
  { "en": "Apa Penyebab Harmonisa?", "id": "Beban Non-Linier Seperti VFD, UPS." },
  { "en": "Apa Dampak Buruk Harmonisa?", "id": "Pemanasan Kabel Netral, Kerusakan Alat." },
  { "en": "Apa Itu Filter Harmonisa?", "id": "Peralatan Untuk Mengurangi Distorsi Harmonisa." },
  { "en": "Apa Jenis Filter Harmonisa?", "id": "Filter Pasif Dan Filter Aktif." },
  { "en": "Apa Itu Sistem Tenaga Listrik?", "id": "Proses Pembangkitan, Transmisi, Distribusi." },
  { "en": "Apa Itu Pembangkitan Listrik?", "id": "Proses Produksi Energi Listrik." },
  { "en": "Apa Itu Sistem Distribusi Listrik?", "id": "Penyaluran Listrik Ke Pelanggan Akhir." },
  { "en": "Apa Itu Jaringan Tegangan Menengah? (JTM)", "id": "Jaringan Distribusi Primer 20 KV." },
  { "en": "Apa Itu Jaringan Tegangan Rendah? (JTR)", "id": "Jaringan Distribusi Sekunder 220/380 V." },
  { "en": "Apa Itu Tiang Listrik?", "id": "Penyangga Jaringan Saluran Udara." },
  { "en": "Apa Jenis Tiang Listrik?", "id": "Tiang Besi, Tiang Beton." },
  { "en": "Apa Itu Trafo Tiang? (Pole Mounted Transformer)", "id": "Trafo Distribusi Dipasang Di Tiang." },
  { "en": "Apa Itu Kubikel? (Cubicle)", "id": "Panel Listrik Tegangan Menengah Modular." },
  { "en": "Apa Fungsi Kubikel?", "id": "Pemutus, Penghubung, Proteksi Jaringan 20 KV." },
  { "en": "Apa Itu Load Break Switch? (LBS)", "id": "Saklar Pemutus Beban Pada Kubikel." },
  { "en": "Apa Itu Recloser?", "id": "Pemutus Tenaga Otomatis Jaringan Distribusi." },
  { "en": "Apa Fungsi Recloser?", "id": "Memutus, Menyambung Otomatis Gangguan Temporer." },
  { "en": "Apa Itu Sectionalizer?", "id": "Pemisah Jaringan Otomatis Bekerja Dengan Recloser." },
  { "en": "Apa Itu Fused Cut Out? (FCO)", "id": "Pengaman Lebur Jaringan Distribusi." },
  { "en": "Apa Itu Lightning Arrester Distribusi?", "id": "Pelindung Jaringan Distribusi Dari Petir." },
  { "en": "Apa Itu Motor Stepper?", "id": "Motor DC Bergerak Per Langkah (Step)." },
  { "en": "Apa Itu Motor Servo?", "id": "Motor Kontrol Presisi Dengan Umpan Balik." },
  { "en": "Apa Perbedaan Servo Dan Stepper?", "id": "Motor Servo Memiliki Sistem Closed Loop." },
  { "en": "Apa Itu Sistem Open Loop?", "id": "Sistem Kontrol Tanpa Umpan Balik." },
  { "en": "Apa Itu Sistem Closed Loop?", "id": "Sistem Kontrol Dengan Umpan Balik." },
  { "en": "Apa Itu Encoder?", "id": "Sensor Umpan Balik Posisi Putaran Motor." },
  { "en": "Apa Itu Variable Speed Drive? (VSD)", "id": "Nama Lain Dari VFD Atau Inverter." },
  { "en": "Apa Itu Pengereman Dinamik? (Dynamic Braking)", "id": "Metode Penghentian Motor Menggunakan Resistor." },
  { "en": "Apa Itu Pengereman Regeneratif?", "id": "Mengubah Energi Putar Motor Menjadi Listrik." },
  { "en": "Apa Itu Tahanan Pentanahan Netral? (NGR)", "id": "Resistor Pentanahan Netral Generator, Trafo." },
  { "en": "Apa Fungsi NGR? (Neutral Grounding Resistor)", "id": "Membatasi Arus Gangguan Fasa Ke Tanah." },
  { "en": "Apa Itu Sistem Floating Ground?", "id": "Sistem Tidak Ditera (Ungrounded System)." },
  { "en": "Apa Itu Panel SDP? (Sub Distribution Panel)", "id": "Panel Pembagi Listrik Tingkat Lanjut." },
  { "en": "Apa Itu Panel MDP? (Main Distribution Panel)", "id": "Panel Distribusi Utama Gedung." },
  { "en": "Apa Itu MCC? (Motor Control Center)", "id": "Panel Pusat Pengendali Motor-Motor Listrik." },
  { "en": "Apa Komponen Utama MCC?", "id": "Kontaktor, Thermal Overload, MCB, VFD." },
  { "en": "Apa Itu Diagram Satu Garis? (Single Line Diagram)", "id": "Representasi Sederhana Sistem Listrik." },
  { "en": "Apa Itu Diagram Kontrol? (Wiring Diagram)", "id": "Gambar Rinci Rangkaian Kontrol Listrik." },
  { "en": "Apa Itu Standar IEC? (International Electrotechnical Commission)", "id": "Standar Internasional Bidang Elektroteknik." },
  { "en": "Apa Itu Standar NEMA? (National Electrical Manufacturers Association)", "id": "Standar Peralatan Listrik Amerika Utara." },
  { "en": "Apa Itu Standar ANSI? (American National Standards Institute)", "id": "Badan Standarisasi Nasional Amerika." },
  { "en": "Apa Itu Standar SNI? (Standar Nasional Indonesia)", "id": "Standar Yang Berlaku Di Indonesia." },
  { "en": "Apa Itu Kalibrasi Alat Ukur?", "id": "Proses Penyetelan Keakuratan Alat Ukur." },
  { "en": "Mengapa Kalibrasi Penting?", "id": "Memastikan Hasil Pengukuran Akurat, Terpercaya." },
  { "en": "Apa Itu True RMS Multimeter?", "id": "Multimeter Mengukur Akurat Gelombang Non-Sinusoidal." },
  { "en": "Apa Itu Insulation Piercing Connector? (IPC)", "id": "Konektor Sambungan Kabel Berisolasi." },
  { "en": "Apa Itu Kawat Email?", "id": "Kawat Tembaga Berisolasi Lapisan Email Tipis." },
  { "en": "Di Mana Kawat Email Digunakan?", "id": "Lilitan Motor, Trafo, Induktor." },
  { "en": "Apa Itu Kumparan? (Coil)", "id": "Lilitan Kawat Yang Membentuk Induktor." },
  { "en": "Apa Itu Medan Magnet?", "id": "Area Pengaruh Gaya Magnetik." },
  { "en": "Apa Itu Fluks Magnet?", "id": "Ukuran Total Medan Magnet." },
  { "en": "Apa Satuan Fluks Magnet?", "id": "Weber (Wb)." },
  { "en": "Apa Itu Kerapatan Fluks Magnet?", "id": "Fluks Magnet Per Satuan Luas." },
  { "en": "Apa Satuan Kerapatan Fluks Magnet?", "id": "Tesla (T)." },
  { "en": "Apa Itu Hukum Faraday?", "id": "Perubahan Fluks Magnet Hasilkan Tegangan." },
  { "en": "Apa Itu Hukum Lenz?", "id": "Arah Arus Induksi Melawan Perubahan." },
  { "en": "Apa Itu GGL Induksi? (Gaya Gerak Listrik)", "id": "Tegangan Yang Dibangkitkan Induksi Elektromagnetik." },
  { "en": "Apa Itu Medan Listrik?", "id": "Area Pengaruh Gaya Listrik." },
  { "en": "Apa Satuan Medan Listrik?", "id": "Volt Per Meter (V/m)." },
  { "en": "Apa Itu Dielektrik?", "id": "Bahan Isolator Dalam Kapasitor." },
  { "en": "Apa Fungsi Bahan Dielektrik?", "id": "Meningkatkan Nilai Kapasitansi Kapasitor." },
  { "en": "Apa Itu Konstanta Dielektrik?", "id": "Ukuran Kemampuan Bahan Menyimpan Energi Listrik." },
  { "en": "Apa Itu Trafo Arus Saturasi?", "id": "Kondisi CT Tidak Akurat Arus Tinggi." },
  { "en": "Apa Itu Burden CT?", "id": "Beban Total Rangkaian Sekunder CT." },
  { "en": "Apa Itu Kelas Akurasi CT?", "id": "Batas Kesalahan Pengukuran Current Transformer." },
  { "en": "Apa Itu CT Proteksi?", "id": "CT Untuk Kebutuhan Sistem Proteksi." },
  { "en": "Apa Itu CT Pengukuran?", "id": "CT Untuk Kebutuhan Alat Ukur." },
  { "en": "Apa Perbedaan CT Proteksi, Pengukuran?", "id": "CT Proteksi Tahan Saturasi Arus Besar." },
  { "en": "Apa Itu Polaritas CT?", "id": "Arah Lilitan Primer Terhadap Sekunder." },
  { "en": "Mengapa Polaritas CT Penting?", "id": "Penting Untuk Sistem Proteksi, Pengukuran Daya." },
  { "en": "Apa Akibat Polaritas CT Terbalik?", "id": "Kesalahan Pembacaan Alat Ukur, Proteksi Gagal." },
  { "en": "Apa Itu Tegangan Lutut? (Knee Point)", "id": "Batas Linearitas Kurva Saturasi CT." },
  { "en": "Apa Itu Tes Polaritas Trafo?", "id": "Memastikan Arah Polaritas Lilitan Benar." },
  { "en": "Apa Itu Tes Tahanan Kontak? (Contact Resistance)", "id": "Mengukur Tahanan Sambungan PMT, PMS." },
  { "en": "Mengapa Tahanan Kontak Diukur?", "id": "Tahanan Tinggi Sebabkan Panas, Rugi Daya." },
  { "en": "Apa Itu Hot Spot?", "id": "Titik Panas Berlebih Pada Sambungan Listrik." },
  { "en": "Alat Apa Mendeteksi Hot Spot?", "id": "Kamera Termografi (Thermal Imager)." },
  { "en": "Apa Itu Termografi Inframerah?", "id": "Teknik Pengukuran Suhu Permukaan Jarak Jauh." },
  { "en": "Apa Itu Panel Sinkronisasi?", "id": "Panel Mengatur Operasi Paralel Generator." },
  { "en": "Apa Itu Load Sharing?", "id": "Pembagian Beban Antar Generator Paralel." },
  { "en": "Apa Itu Droop Control?", "id": "Metode Kontrol Load Sharing Generator." },
  { "en": "Apa Itu Mode Isokron?", "id": "Metode Menjaga Frekuensi Konstan Generator." },
  { "en": "Apa Itu Governor?", "id": "Alat Pengatur Kecepatan Putaran Mesin Diesel." },
  { "en": "Apa Hubungan Governor Dan Frekuensi?", "id": "Governor Mengatur Frekuensi Listrik Generator." },
  { "en": "Apa Itu Luminansi?", "id": "Ukuran Kecerahan Permukaan Yang Terlihat." },
  { "en": "Apa Satuan Dari Luminansi?", "id": "Candela Per Meter Persegi (Cd/M²)." },
  { "en": "Apa Itu Iluminasi?", "id": "Jumlah Fluks Cahaya Per Satuan Luas." },
  { "en": "Apa Satuan Dari Iluminasi?", "id": "Lux." },
  { "en": "Apa Itu Efisiensi Luminer?", "id": "Perbandingan Lumen Lampu, Lumen Luminer." },
  { "en": "Apa Itu Lampu Pijar? (Incandescent)", "id": "Lampu Menghasilkan Cahaya Dari Pemanasan Filamen." },
  { "en": "Apa Itu Lampu Neon? (Fluorescent)", "id": "Lampu Menggunakan Pendaran Gas Merkuri." },
  { "en": "Apa Fungsi Ballast Pada Lampu Neon?", "id": "Membatasi Arus, Menstabilkan Tegangan." },
  { "en": "Apa Fungsi Starter Pada Lampu Neon?", "id": "Memanaskan Elektroda Awal Penyalaan." },
  { "en": "Apa Itu Lampu LED? (Light Emitting Diode)", "id": "Lampu Berbasis Semikonduktor Dioda." },
  { "en": "Apa Keuntungan Utama Lampu LED?", "id": "Sangat Hemat Energi, Tahan Lama." },
  { "en": "Apa Itu Lampu Sodium? (HPS)", "id": "Lampu Gas Bertekanan Tinggi (High Pressure Sodium)." },
  { "en": "Di Mana Lampu Sodium Sering Digunakan?", "id": "Penerangan Jalan Raya (Warna Kuning)." },
  { "en": "Apa Itu Lampu Merkuri?", "id": "Lampu Gas Menggunakan Uap Merkuri." },
  { "en": "Apa Itu Indeks Renderasi Warna? (CRI)", "id": "Kemampuan Lampu Menampilkan Warna Asli." },
  { "en": "Berapa Nilai CRI Ideal Matahari?", "id": "Nilai Indeksnya Adalah 100." },
  { "en": "Apa Itu Suhu Warna? (Color Temperature)", "id": "Ukuran Warna Cahaya Lampu." },
  { "en": "Apa Satuan Suhu Warna?", "id": "Kelvin (K)." },
  { "en": "Apa Nuansa Warna Warm White?", "id": "Cahaya Kuning Hangat (Di Bawah 3300K)." },
  { "en": "Apa Nuansa Warna Cool White?", "id": "Cahaya Putih Kebiruan (Di Atas 5300K)." },
  { "en": "Apa Itu Lux Meter?", "id": "Alat Ukur Intensitas Cahaya (Iluminasi)." },
  { "en": "Apa Fungsi Lux Meter?", "id": "Mengukur Tingkat Penerangan Suatu Ruangan." },
  { "en": "Apa Itu Motor Servo AC?", "id": "Motor Servo Yang Beroperasi Dengan Arus AC." },
  { "en": "Apa Itu Motor Servo DC?", "id": "Motor Servo Yang Beroperasi Dengan Arus DC." },
  { "en": "Apa Itu Holding Torque Motor Stepper?", "id": "Torsi Menahan Posisi Saat Diam." },
  { "en": "Apa Itu Detent Torque Motor Stepper?", "id": "Torsi Menahan Tanpa Listrik." },
  { "en": "Apa Itu Microstepping?", "id": "Teknik Menghaluskan Gerakan Motor Stepper." },
  { "en": "Apa Itu Driver Motor?", "id": "Rangkaian Elektronik Pengendali Motor Listrik." },
  { "en": "Apa Fungsi Utama Driver Motor?", "id": "Mengatur Arah, Kecepatan Putaran Motor." },
  { "en": "Apa Itu Relay Elektromekanis?", "id": "Saklar Listrik Menggunakan Prinsip Elektromagnet." },
  { "en": "Apa Bagian Utama Relay Elektromekanis?", "id": "Kumparan (Coil) Dan Kontak (Contact)." },
  { "en": "Apa Itu Relay Solid State? (SSR)", "id": "Saklar Listrik Elektronik Tanpa Bagian Bergerak." },
  { "en": "Apa Keuntungan SSR?", "id": "Lebih Cepat, Tahan Lama, Tidak Berisik." },
  { "en": "Apa Itu Relay Arus Lebih Seketika? (Instantaneous)", "id": "Relay Bekerja Instan Saat Arus Lebih." },
  { "en": "Apa Itu Relay Arus Lebih Waktu Tentu? (Definite Time)", "id": "Relay Bekerja Setelah Waktu Tunda Tetap." },
  { "en": "Apa Itu Relay Waktu Terbalik? (Inverse Time)", "id": "Waktu Tunda Berbanding Terbalik Dengan Arus." },
  { "en": "Apa Karakteristik Relay Inverse Time?", "id": "Semakin Besar Arus, Semakin Cepat Trip." },
  { "en": "Apa Itu Kurva IDMT? (Inverse Definite Minimum Time)", "id": "Kurva Karakteristik Relay Waktu Terbalik." },
  { "en": "Apa Itu Plug Setting Multiplier? (PSM)", "id": "Rasio Arus Gangguan Terhadap Arus Setting." },
  { "en": "Apa Itu Time Multiplier Setting? (TMS)", "id": "Pengaturan Pengali Waktu Tunda Relay." },
  { "en": "Apa Itu Relay Gangguan Tanah? (Earth Fault Relay)", "id": "Relay Mendeteksi Arus Bocor Ke Tanah." },
  { "en": "Apa Itu Restricted Earth Fault? (REF)", "id": "Proteksi Gangguan Tanah Terbatas Zona Trafo." },
  { "en": "Apa Fungsi Proteksi REF?", "id": "Melindungi Belitan Trafo Dari Gangguan Tanah." },
  { "en": "Apa Itu Unrestricted Earth Fault? (URF)", "id": "Proteksi Gangguan Tanah Tidak Terbatas." },
  { "en": "Apa Itu Relay Tegangan Kurang? (UVR)", "id": "Relay Bekerja Jika Tegangan Turun." },
  { "en": "Apa Fungsi UVR? (Under Voltage Relay)", "id": "Melindungi Alat Dari Tegangan Terlalu Rendah." },
  { "en": "Apa Itu Relay Tegangan Lebih? (OVR)", "id": "Relay Bekerja Jika Tegangan Naik." },
  { "en": "Apa Fungsi OVR? (Over Voltage Relay)", "id": "Melindungi Alat Dari Tegangan Terlalu Tinggi." },
  { "en": "Apa Itu Relay Frekuensi Kurang? (UFR)", "id": "Relay Bekerja Jika Frekuensi Sistem Turun." },
  { "en": "Apa Itu Relay Frekuensi Lebih? (OFR)", "id": "Relay Bekerja Jika Frekuensi Sistem Naik." },
  { "en": "Apa Itu Auto Reclose Relay?", "id": "Relay Menyambung Kembali Jaringan Otomatis." },
  { "en": "Apa Itu Relay Arah Daya? (Directional Power)", "id": "Relay Bekerja Berdasar Arah Aliran Daya." },
  { "en": "Apa Itu Proteksi Jarak? (Distance Relay)", "id": "Relay Proteksi Berdasarkan Pengukuran Impedansi." },
  { "en": "Di Mana Distance Relay Digunakan?", "id": "Proteksi Utama Saluran Transmisi Listrik." },
  { "en": "Apa Itu Zona Proteksi?", "id": "Area Yang Dilindungi Oleh Sistem Proteksi." },
  { "en": "Apa Itu Kualitas Daya Listrik?", "id": "Ukuran Kualitas Tegangan, Arus Listrik." },
  { "en": "Apa Saja Parameter Kualitas Daya?", "id": "Harmonisa, Sag, Swell, Frekuensi, Transient." },
  { "en": "Apa Itu Transient?", "id": "Lonjakan Tegangan Atau Arus Sangat Cepat." },
  { "en": "Apa Penyebab Transient Tegangan?", "id": "Sambaran Petir, Proses Switching." },
  { "en": "Apa Itu Notching?", "id": "Gangguan Periodik Pada Gelombang Tegangan." },
  { "en": "Apa Itu Sag Compensation?", "id": "Tindakan Kompensasi Penurunan Tegangan Sesaat." },
  { "en": "Alat Apa Untuk Kompensasi Sag?", "id": "Dynamic Voltage Restorer (DVR)." },
  { "en": "Apa Itu DVR? (Dynamic Voltage Restorer)", "id": "Alat Memperbaiki Kualitas Tegangan Cepat." },
  { "en": "Apa Rumus Daya Aktif? (P)", "id": "P = V x I x Cos(Φ)." },
  { "en": "Apa Rumus Daya Reaktif? (Q)", "id": "Q = V x I x Sin(Φ)." },
  { "en": "Apa Rumus Daya Semu? (S)", "id": "S = V x I." },
  { "en": "Apa Itu Segitiga Daya? (Power Triangle)", "id": "Hubungan Grafis P, Q, Dan S." },
  { "en": "Apa Sisi Miring Segitiga Daya?", "id": "Daya Semu (S)." },
  { "en": "Apa Sisi Samping Segitiga Daya?", "id": "Daya Aktif (P)." },
  { "en": "Apa Sisi Depan Segitiga Daya?", "id": "Daya Reaktif (Q)." },
  { "en": "Apa Rumus Faktor Daya?", "id": "Rasio Daya Aktif (P) Dibagi Daya Semu (S)." },
  { "en": "Apa Akibat Faktor Daya Buruk?", "id": "Arus Tinggi, Rugi Daya, Denda PLN." },
  { "en": "Bagaimana Cara Memperbaiki Faktor Daya?", "id": "Memasang Kapasitor Bank." },
  { "en": "Apa Itu Beban Resistif Murni?", "id": "Beban Menyerap Daya Aktif Saja." },
  { "en": "Apa Contoh Beban Resistif?", "id": "Lampu Pijar, Elemen Pemanas." },
  { "en": "Apa Itu Beban Induktif?", "id": "Beban Membutuhkan Daya Reaktif." },
  { "en": "Apa Contoh Beban Induktif?", "id": "Motor Listrik, Transformator, Lampu Neon." },
  { "en": "Apa Itu Beban Kapasitif?", "id": "Beban Menghasilkan Daya Reaktif." },
  { "en": "Apa Contoh Beban Kapasitif?", "id": "Kapasitor Bank." },
  { "en": "Apa Faktor Daya Beban Resistif?", "id": "Satu (Unity Power Factor)." },
  { "en": "Apa Sifat Faktor Daya Induktif?", "id": "Tertinggal (Lagging Power Factor)." },
  { "en": "Apa Sifat Faktor Daya Kapasitif?", "id": "Mendahului (Leading Power Factor)." },
  { "en": "Apa Itu Kapasitor Shunt?", "id": "Kapasitor Dipasang Paralel Jaringan." },
  { "en": "Apa Itu Reaktor Shunt?", "id": "Induktor Dipasang Paralel Jaringan." },
  { "en": "Apa Fungsi Reaktor Shunt?", "id": "Menyerap Daya Reaktif Berlebih (Efek Ferranti)." },
  { "en": "Apa Itu Efek Ferranti?", "id": "Tegangan Ujung Saluran Lebih Tinggi." },
  { "en": "Kapan Efek Ferranti Terjadi?", "id": "Transmisi Panjang, Beban Ringan, Kapasitansi Saluran." },
  { "en": "Bagaimana Mengatasi Efek Ferranti?", "id": "Memasang Reaktor Shunt." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Angin? (PLTB)", "id": "Pembangkit Listrik Memanfaatkan Energi Angin." },
  { "en": "Apa Komponen Utama PLTB?", "id": "Turbin Angin, Generator, Tower." },
  { "en": "Apa Fungsi Turbin Angin?", "id": "Mengubah Energi Kinetik Angin Jadi Mekanik." },
  { "en": "Apa Itu Pitch Control Turbin Angin?", "id": "Kontrol Sudut Bilah Turbin." },
  { "en": "Apa Itu Yaw Control Turbin Angin?", "id": "Kontrol Arah Turbin Menghadap Angin." },
  { "en": "Apa Itu Anemometer?", "id": "Alat Pengukur Kecepatan Angin." },
  { "en": "Apa Fungsi Anemometer Pada PLTB?", "id": "Mengukur Kecepatan Angin Untuk Operasi." },
  { "en": "Apa Itu Transduser?", "id": "Alat Mengubah Satu Bentuk Energi Lainnya." },
  { "en": "Apa Contoh Transduser?", "id": "Mikrofon, Loudspeaker, Sensor Suhu." },
  { "en": "Apa Itu Sensor?", "id": "Alat Mendeteksi Perubahan Fisik, Kimia." },
  { "en": "Apa Beda Sensor Dan Transduser?", "id": "Sensor Mendeteksi, Transduser Mengkonversi." },
  { "en": "Apa Itu Aktuator?", "id": "Komponen Penggerak Mekanis (Motor, Valve)." },
  { "en": "Apa Itu Dioda Daya?", "id": "Komponen Penyearah Arus Listrik." },
  { "en": "Apa Fungsi Utama Dioda?", "id": "Mengalirkan Arus Satu Arah Saja." },
  { "en": "Apa Itu Thyristor?", "id": "Komponen Semikonduktor Berfungsi Saklar Cepat." },
  { "en": "Apa Kepanjangan SCR? (Silicon Controlled Rectifier)", "id": "Jenis Thyristor Berfungsi Penyearah Terkendali." },
  { "en": "Bagaimana Cara Mengaktifkan SCR?", "id": "Memberi Tegangan Picu (Trigger) Ke Gate." },
  { "en": "Apa Itu TRIAC? (Triode for Alternating Current)", "id": "Saklar AC Dua Arah Terkendali." },
  { "en": "Apa Fungsi TRIAC?", "id": "Mengendalikan Daya Listrik AC (Dimmer)." },
  { "en": "Apa Itu DIAC? (Diode for Alternating Current)", "id": "Komponen Pemicu (Trigger) Untuk TRIAC." },
  { "en": "Apa Kepanjangan MOSFET? (Metal Oxide Semiconductor Field Effect Transistor)", "id": "Transistor Efek Medan Berkecepatan Tinggi." },
  { "en": "Apa Kepanjangan IGBT? (Insulated Gate Bipolar Transistor)", "id": "Gabungan Keunggulan MOSFET Dan Bipolar." },
  { "en": "Apa Keuntungan IGBT?", "id": "Switching Cepat, Kemampuan Arus Besar." },
  { "en": "Apa Itu Elektronika Daya?", "id": "Aplikasi Elektronika Pengolahan Daya Listrik." },
  { "en": "Apa Itu Konverter AC-DC?", "id": "Rangkaian Penyearah (Rectifier)." },
  { "en": "Apa Itu Konverter DC-DC?", "id": "Rangkaian Pengubah Level Tegangan DC." },
  { "en": "Apa Contoh Konverter DC-DC?", "id": "Buck Converter, Boost Converter." },
  { "en": "Apa Fungsi Buck Converter?", "id": "Menurunkan Tegangan DC." },
  { "en": "Apa Fungsi Boost Converter?", "id": "Menaikkan Tegangan DC." },
  { "en": "Apa Itu Konverter DC-AC?", "id": "Rangkaian Pembalik (Inverter)." },
  { "en": "Apa Itu Konverter AC-AC?", "id": "Rangkaian Pengubah Frekuensi, Tegangan AC." },
  { "en": "Apa Itu Cycloconverter?", "id": "Konverter AC-AC Langsung Tanpa Perantara." },
  { "en": "Apa Itu PWM? (Pulse Width Modulation)", "id": "Teknik Modulasi Lebar Pulsa." },
  { "en": "Dimana PWM Digunakan?", "id": "Inverter, VFD, Konverter DC-DC." },
  { "en": "Apa Fungsi PWM Pada VFD?", "id": "Membentuk Gelombang Sinus Output Inverter." },
  { "en": "Apa Itu Jembatan Dioda? (Diode Bridge)", "id": "Rangkaian Penyearah Gelombang Penuh." },
  { "en": "Apa Itu Penyearah Setengah Gelombang?", "id": "Hanya Menyearahkan Setengah Siklus AC." },
  { "en": "Apa Itu Penyearah Gelombang Penuh?", "id": "Menyearahkan Kedua Siklus AC." },
  { "en": "Apa Itu Ripple?", "id": "Sisa Tegangan AC Pada Output DC." },
  { "en": "Bagaimana Cara Mengurangi Ripple?", "id": "Menggunakan Kapasitor Filter." },
  { "en": "Apa Itu Heat Sink?", "id": "Komponen Pendingin Semikonduktor Daya." },
  { "en": "Mengapa Heat Sink Diperlukan?", "id": "Membuang Panas Berlebih Komponen Aktif." },
  { "en": "Apa Itu Snubber Circuit?", "id": "Rangkaian Pelindung Lonjakan Tegangan Switching." },
  { "en": "Apa Itu GTO? (Gate Turn-Off Thyristor)", "id": "Thyristor Yang Bisa Dimatikan Lewat Gate." },
  { "en": "Apa Itu Sistem Tenaga Listrik Tiga Fasa?", "id": "Sistem Pembangkitan, Transmisi Tiga Fasa." },
  { "en": "Apa Itu Beban Seimbang?", "id": "Beban Ketiga Fasa Sama Besar." },
  { "en": "Apa Itu Beban Tidak Seimbang?", "id": "Beban Ketiga Fasa Tidak Sama." },
  { "en": "Apa Akibat Beban Tidak Seimbang?", "id": "Timbul Arus Di Kawat Netral." },
  { "en": "Apa Itu Urutan Fasa?", "id": "Urutan Puncak Tegangan Fasa (R-S-T)." },
  { "en": "Apa Itu Urutan Fasa Positif?", "id": "Urutan Fasa Normal (R-S-T)." },
  { "en": "Apa Itu Urutan Fasa Negatif?", "id": "Urutan Fasa Terbalik (R-T-S)." },
  { "en": "Apa Akibat Urutan Fasa Terbalik?", "id": "Membalik Arah Putaran Motor Tiga Fasa." },
  { "en": "Apa Itu Komponen Simetris?", "id": "Metode Analisis Sistem Tidak Seimbang." },
  { "en": "Apa Saja Komponen Simetris?", "id": "Urutan Positif, Negatif, Dan Nol." },
  { "en": "Apa Itu Komponen Urutan Nol?", "id": "Komponen Arus Gangguan Fasa Ke Tanah." },
  { "en": "Apa Itu Stabilitas Sistem Tenaga?", "id": "Kemampuan Sistem Kembali Normal Setelah Gangguan." },
  { "en": "Apa Itu Stabilitas Transient?", "id": "Stabilitas Sistem Beberapa Detik Awal Gangguan." },
  { "en": "Apa Itu Stabilitas Dinamik?", "id": "Stabilitas Sistem Jangka Waktu Menengah." },
  { "en": "Apa Itu Stabilitas Steady State?", "id": "Stabilitas Sistem Dalam Kondisi Tunak." },
  { "en": "Apa Itu Swing Generator?", "id": "Perubahan Kecepatan Generator Saat Gangguan." },
  { "en": "Apa Itu Sudut Daya?", "id": "Sudut Antara Tegangan Rotor, Stator Generator." },
  { "en": "Apa Itu FACTS? (Flexible AC Transmission Systems)", "id": "Sistem Peningkat Fleksibilitas Transmisi AC." },
  { "en": "Apa Contoh Peralatan FACTS?", "id": "STATCOM, SVC, UPFC." },
  { "en": "Apa Itu SVC? (Static VAR Compensator)", "id": "Pengatur Daya Reaktif Statis Cepat." },
  { "en": "Apa Fungsi SVC?", "id": "Memperbaiki Stabilitas Tegangan Jaringan." },
  { "en": "Apa Itu STATCOM? (Static Synchronous Compensator)", "id": "Kompensator Reaktif Berbasis Konverter." },
  { "en": "Apa Itu UPFC? (Unified Power Flow Controller)", "id": "Pengatur Aliran Daya Listrik Terpadu." },
  { "en": "Apa Itu HVDC? (High Voltage Direct Current)", "id": "Sistem Transmisi Listrik Tegangan Tinggi DC." },
  { "en": "Apa Keuntungan Transmisi HVDC?", "id": "Rugi Daya Rendah, Jarak Jauh." },
  { "en": "Kapan HVDC Digunakan?", "id": "Transmisi Bawah Laut, Interkoneksi Beda Frekuensi." },
  { "en": "Apa Itu Konverter Station HVDC?", "id": "Stasiun Pengubah AC Ke DC, Sebaliknya." },
  { "en": "Apa Itu Smart Grid?", "id": "Jaringan Listrik Cerdas Berbasis Digital." },
  { "en": "Apa Karakteristik Smart Grid?", "id": "Komunikasi Dua Arah, Self-Healing." },
  { "en": "Apa Itu Self-Healing Grid?", "id": "Jaringan Listrik Memulihkan Diri Otomatis." },
  { "en": "Apa Itu AMR? (Automatic Meter Reading)", "id": "Sistem Pembacaan Meter Listrik Otomatis." },
  { "en": "Apa Itu AMI? (Advanced Metering Infrastructure)", "id": "Infrastruktur Pengukuran Listrik Canggih." },
  { "en": "Apa Beda AMR Dan AMI?", "id": "AMI Memiliki Komunikasi Dua Arah." },
  { "en": "Apa Itu Demand Response?", "id": "Program Pengurangan Beban Listrik Pelanggan." },
  { "en": "Apa Itu Distributed Generation? (DG)", "id": "Pembangkit Listrik Skala Kecil Terdistribusi." },
  { "en": "Apa Contoh Distributed Generation?", "id": "PLTS Atap, Generator Kecil." },
  { "en": "Apa Itu Microgrid?", "id": "Jaringan Listrik Lokal Mandiri Skala Kecil." },
  { "en": "Apa Keuntungan Microgrid?", "id": "Keandalan Tinggi, Bisa Beroperasi Terpisah." },
  { "en": "Apa Itu Island Mode?", "id": "Mode Operasi Microgrid Terputus Jaringan Utama." },
  { "en": "Apa Itu Proteksi Cadangan? (Backup Protection)", "id": "Proteksi Lapis Kedua Jika Utama Gagal." },
  { "en": "Apa Itu Koordinasi Proteksi?", "id": "Pengaturan Relay Agar Bekerja Selektif." },
  { "en": "Apa Itu Kurva TCC? (Time Current Curve)", "id": "Grafik Koordinasi Waktu Arus Proteksi." },
  { "en": "Apa Itu Grading Margin?", "id": "Batas Waktu Aman Antar Relay Proteksi." },
  { "en": "Apa Itu Switchgear?", "id": "Peralatan Pemutus, Penghubung Jaringan Listrik." },
  { "en": "Apa Itu Metal-Clad Switchgear?", "id": "Switchgear Dengan Kompartemen Logam Terpisah." },
  { "en": "Apa Itu Metal-Enclosed Switchgear?", "id": "Switchgear Dalam Selungkup Logam." },
  { "en": "Apa Itu Arc Resistant Switchgear?", "id": "Switchgear Tahan Ledakan Busur Api Internal." },
  { "en": "Apa Itu Pemeliharaan Berbasis Kondisi? (CBM)", "id": "Pemeliharaan Berdasarkan Kondisi Real-Time Alat." },
  { "en": "Apa Itu Partial Discharge? (PD)", "id": "Pelepasan Sebagian Muatan Isolasi Listrik." },
  { "en": "Mengapa Tes Partial Discharge Penting?", "id": "Mendeteksi Degradasi Dini Isolasi." },
  { "en": "Apa Itu Tes DGA? (Dissolved Gas Analysis)", "id": "Analisis Gas Terlarut Minyak Trafo." },
  { "en": "Apa Fungsi Tes DGA?", "id": "Mendeteksi Gangguan Internal Transformator." },
  { "en": "Apa Itu Gas Kunci DGA?", "id": "Hidrogen, Metana, Asetilena, Etilena." },
  { "en": "Gas Apa Menandakan Suhu Sangat Tinggi?", "id": "Asetilena (C2H2)." },
  { "en": "Apa Itu Tes SFRA? (Sweep Frequency Response Analysis)", "id": "Tes Mendeteksi Pergeseran Lilitan Trafo." },
  { "en": "Apa Itu Tes Faktor Daya Isolasi?", "id": "Mengukur Kualitas Isolasi (Tan Delta)." },
  { "en": "Apa Itu Sistem Baterai VRLA? (Valve Regulated Lead Acid)", "id": "Baterai Kering Asam Timbal Terkontrol." },
  { "en": "Apa Keuntungan Baterai VRLA?", "id": "Bebas Perawatan (Maintenance Free)." },
  { "en": "Apa Itu Baterai Flooded Type?", "id": "Baterai Basah (Memerlukan Penambahan Air)." },
  { "en": "Apa Itu Tegangan Float Baterai?", "id": "Tegangan Pengisian Baterai Mode Siaga." },
  { "en": "Apa Itu Tegangan Boost Baterai?", "id": "Tegangan Pengisian Cepat Baterai." },
  { "en": "Apa Itu Sistem Pentanahan Solid?", "id": "Netral Trafo Dihubungkan Langsung Tanah." },
  { "en": "Apa Itu Sistem Pentanahan Tahanan?", "id": "Netral Dihubungkan Tanah Lewat Resistor." },
  { "en": "Apa Itu Sistem Pentanahan Reaktansi?", "id": "Netral Dihubungkan Tanah Lewat Reaktor." },
  { "en": "Apa Itu Petersen Coil?", "id": "Reaktor Pentanahan Resonansi (Arc Suppression)." },
  { "en": "Apa Fungsi Petersen Coil?", "id": "Mengurangi Arus Gangguan Tanah Temporer." },
  { "en": "Apa Itu Prosedur Keselamatan Listrik?", "id": "Langkah Kerja Aman Kelistrikan." },
  { "en": "Kapan Prosedur LOTO Digunakan?", "id": "Setiap Pekerjaan Perbaikan, Pemeliharaan." },
  { "en": "Apa Itu Izin Kerja? (Permit to Work)", "id": "Izin Tertulis Melakukan Pekerjaan Berbahaya." },
  { "en": "Apa Itu JSA? (Job Safety Analysis)", "id": "Analisis Keselamatan Pekerjaan." },
  { "en": "Apa Itu Tes Tegangan Tahan? (Withstand Voltage Test)", "id": "Tes Menguji Kekuatan Isolasi." },
  { "en": "Apa Itu Tes Indeks Polarisasi? (PI)", "id": "Tes Menilai Kualitas Isolasi Motor/Generator." },
  { "en": "Berapa Nilai PI Yang Dianggap Baik?", "id": "Nilai Di Atas 2.0." },
  { "en": "Apa Itu Tes Penyerapan Dielektrik? (DAR)", "id": "Tes Kualitas Isolasi Sejenis PI." },
  { "en": "Apa Itu Kertas Isolasi?", "id": "Bahan Isolator Listrik Berbasis Selulosa." },
  { "en": "Apa Itu Papan Pres? (Pressboard)", "id": "Bahan Isolasi Kaku Trafo Minyak." },
  { "en": "Apa Itu Mika?", "id": "Bahan Isolasi Mineral Tahan Panas Tinggi." },
  { "en": "Apa Itu Porselen?", "id": "Bahan Keramik Isolator Jaringan Listrik." },
  { "en": "Apa Itu Kaca?", "id": "Bahan Isolator Transparan (Isolator Piring)." },
  { "en": "Apa Itu Karet Silikon?", "id": "Bahan Isolasi Fleksibel Tahan Cuaca." },
  { "en": "Apa Itu Resin Epoksi?", "id": "Bahan Isolasi Keras Trafo Kering." },
  { "en": "Apa Itu XLPE? (Cross-Linked Polyethylene)", "id": "Isolasi Kabel Tegangan Menengah Tinggi." },
  { "en": "Apa Keuntungan Kabel XLPE?", "id": "Tahan Suhu Tinggi, Kekuatan Isolasi Baik." },
  { "en": "Apa Itu PVC? (Polyvinyl Chloride)", "id": "Bahan Isolasi Kabel Tegangan Rendah." },
  { "en": "Apa Itu Standar IEEE? (Institute of Electrical and Electronics Engineers)", "id": "Organisasi Standar Profesional Teknik Global." },
  { "en": "Apa Itu Standar IEC 60364?", "id": "Standar Instalasi Listrik Tegangan Rendah." },
  { "en": "Apa Itu Standar IEC 60228?", "id": "Standar Ukuran Konduktor Kabel Listrik." },
  { "en": "Apa Itu AWG? (American Wire Gauge)", "id": "Standar Ukuran Diameter Kawat Amerika." },
  { "en": "Apa Hubungan AWG Dan Ukuran?", "id": "Semakin Kecil AWG, Semakin Besar Kawatnya." },
  { "en": "Apa Itu Ukuran Kabel MM2?", "id": "Satuan Luas Penampang Kabel Milimeter Persegi." },
  { "en": "Apa Itu Kode Huruf Kabel?", "id": "Menunjukkan Jenis Bahan Isolasi, Konduktor." },
  { "en": "Apa Arti Huruf N Pada Kabel?", "id": "Kabel Standar Tembaga (Normenleitung)." },
  { "en": "Apa Arti Huruf Y Pada Kabel?", "id": "Isolasi Berbahan Dasar PVC." },
  { "en": "Apa Arti Huruf A Pada Kabel?", "id": "Konduktor Berbahan Aluminium." },
  { "en": "Apa Arti Huruf F Pada Kabel?", "id": "Kabel Serabut Fleksibel." },
  { "en": "Apa Itu Kabel NYMHY?", "id": "Kabel Serabut Fleksibel Isolasi PVC." },
  { "en": "Apa Itu Diagram Lokasi?", "id": "Gambar Tata Letak Peralatan Listrik." },
  { "en": "Apa Itu Diagram Pengawatan?", "id": "Gambar Detail Koneksi Kabel Aktual." },
  { "en": "Apa Simbol Untuk Resistor?", "id": "Garis Zig-Zag Atau Persegi Panjang." },
  { "en": "Apa Simbol Untuk Kapasitor?", "id": "Dua Garis Paralel Sejajar." },
  { "en": "Apa Simbol Untuk Induktor?", "id": "Garis Melingkar Seperti Pegas." },
  { "en": "Apa Simbol Untuk Ground?", "id": "Tiga Garis Horizontal Semakin Pendek." },
  { "en": "Apa Simbol Untuk MCB?", "id": "Kontak Saklar Dengan Tanda Termal, Magnetik." },
  { "en": "Apa Simbol Untuk Kontaktor?", "id": "Kontak Utama, Kumparan (Coil)." },
  { "en": "Apa Simbol Untuk Motor Tiga Fasa?", "id": "Lingkaran Dengan Huruf M, Tiga Terminal." },
  { "en": "Apa Simbol Untuk Transformator?", "id": "Dua Lingkaran Induktor Dipisah Garis." },
  { "en": "Apa Itu Arus Nominal?", "id": "Arus Operasi Normal Peralatan Listrik." },
  { "en": "Apa Itu Tegangan Nominal?", "id": "Tegangan Operasi Normal Peralatan Listrik." },
  { "en": "Apa Itu Beban Penuh? (Full Load)", "id": "Kondisi Operasi Peralatan Daya Maksimum." },
  { "en": "Apa Itu Arus Beban Penuh? (FLA)", "id": "Arus Saat Motor Berbeban Penuh." },
  { "en": "Apa Itu Arus Tanpa Beban? (No Load Current)", "id": "Arus Motor Saat Berputar Tanpa Beban." },
  { "en": "Apa Itu Starting Motor Bintang? (Star)", "id": "Arus Awal Rendah, Torsi Awal Rendah." },
  { "en": "Apa Itu Starting Motor Segitiga? (Delta)", "id": "Arus Awal Tinggi, Torsi Awal Tinggi." },
  { "en": "Apa Itu Rotor Sangkar Tupai? (Squirrel Cage)", "id": "Jenis Rotor Paling Umum Motor Induksi." },
  { "en": "Apa Itu Rotor Belitan? (Wound Rotor)", "id": "Rotor Dengan Lilitan Terhubung Slip Ring." },
  { "en": "Apa Keuntungan Motor Wound Rotor?", "id": "Torsi Awal Tinggi, Arus Start Terkendali." },
  { "en": "Apa Itu Tahanan Start Rotor?", "id": "Resistor Eksternal Motor Wound Rotor." },
  { "en": "Apa Itu Motor Sinkron Reluktansi?", "id": "Motor Sinkron Tanpa Belitan Medan Rotor." },
  { "en": "Apa Itu Generator Penguat? (Exciter)", "id": "Generator Kecil Pemasok Eksitasi Utama." },
  { "en": "Apa Itu Sistem Eksitasi Sikat? (Brushed)", "id": "Eksitasi Disalurkan Melalui Sikat Arang." },
  { "en": "Apa Itu Sistem Eksitasi Tanpa Sikat? (Brushless)", "id": "Eksitasi Induksi Tanpa Kontak Fisik." },
  { "en": "Apa Keuntungan Brushless Exciter?", "id": "Perawatan Lebih Mudah, Tidak Ada Sikat." },
  { "en": "Apa Itu Tes Saturasi Generator?", "id": "Tes Menentukan Karakteristik Medan Magnet." },
  { "en": "Apa Itu Kurva V Generator?", "id": "Grafik Hubungan Arus Jangkar, Arus Medan." },
  { "en": "Apa Itu Generator Over-Excited?", "id": "Generator Menyuplai Daya Reaktif (Kapasitif)." },
  { "en": "Apa Itu Generator Under-Excited?", "id": "Generator Menyerap Daya Reaktif (Induktif)." },
  { "en": "Apa Itu Kondensor Sinkron?", "id": "Motor Sinkron Tanpa Beban, Mengatur Daya Reaktif." },
  { "en": "Apa Itu Reluktansi Magnetik?", "id": "Hambatan Rangkaian Magnetik Terhadap Fluks." },
  { "en": "Apa Itu Permeabilitas Magnetik?", "id": "Kemampuan Bahan Dilewati Fluks Magnet." },
  { "en": "Apa Bahan Feromagnetik?", "id": "Bahan Mudah Ditarik Kuat Magnet." },
  { "en": "Apa Contoh Bahan Feromagnetik?", "id": "Besi, Baja, Nikel, Kobalt." },
  { "en": "Apa Itu Histeresis Magnetik?", "id": "Ketinggalan Fluks Magnet Saat Magnetisasi." },
  { "en": "Apa Itu Rugi Histeresis?", "id": "Energi Hilang Akibat Histeresis Inti Trafo." },
  { "en": "Apa Itu Arus Eddy? (Eddy Current)", "id": "Arus Pusar Terinduksi Inti Konduktif." },
  { "en": "Apa Itu Rugi Arus Eddy?", "id": "Energi Hilang Akibat Arus Eddy." },
  { "en": "Bagaimana Mengurangi Rugi Arus Eddy?", "id": "Menggunakan Inti Besi Berlapis (Laminasi)." },
  { "en": "Apa Itu Inti Laminasi?", "id": "Inti Besi Terdiri Lapisan Tipis Terisolasi." },
  { "en": "Apa Itu Baja Silikon?", "id": "Bahan Inti Trafo, Motor (Rugi Rendah)." },
  { "en": "Apa Itu Motor Kutub Bayangan? (Shaded Pole)", "id": "Motor AC Satu Fasa Daya Sangat Kecil." },
  { "en": "Di Mana Motor Shaded Pole Digunakan?", "id": "Kipas Angin Kecil, Pompa Akuarium." },
  { "en": "Apa Itu Motor Kapasitor Terpisah? (Split Phase)", "id": "Motor Satu Fasa Dengan Belitan Bantu." },
  { "en": "Apa Itu Saklar Sentrifugal Motor?", "id": "Memutus Belitan Bantu Saat Kecepatan Tercapai." },
  { "en": "Apa Itu Motor Kapasitor Start?", "id": "Motor Split Phase Dengan Kapasitor Start." },
  { "en": "Apa Itu Motor Kapasitor Run?", "id": "Motor Dengan Kapasitor Terhubung Permanen." },
  { "en": "Apa Itu Motor Kapasitor Start-Run?", "id": "Motor Dengan Dua Kapasitor (Start, Run)." },
  { "en": "Apa Itu Elektromagnet?", "id": "Magnet Dihasilkan Aliran Arus Listrik." },
  { "en": "Apa Itu Solenoida?", "id": "Kumparan Kawat Berbentuk Tabung." },
  { "en": "Apa Itu Jangka Sorong?", "id": "Alat Ukur Presisi Diameter, Ketebalan." },
  { "en": "Apa Itu Mikrometer Sekrup?", "id": "Alat Ukur Sangat Presisi (Diameter Kawat)." },
  { "en": "Apa Itu Kawat Tahan Panas?", "id": "Kabel Berisolasi Tahan Suhu Operasi Tinggi." },
  { "en": "Apa Itu Kabel Instrumentasi?", "id": "Kabel Transmisi Sinyal Kontrol, Pengukuran." },
  { "en": "Apa Itu Kabel Shielded?", "id": "Kabel Dengan Lapisan Pelindung Interferensi." },
  { "en": "Apa Fungsi Shield Kabel?", "id": "Melindungi Sinyal Dari Gangguan Elektromagnetik." },
  { "en": "Apa Itu EMI? (Electromagnetic Interference)", "id": "Gangguan Elektromagnetik Merusak Sinyal." },
  { "en": "Apa Itu RFI? (Radio Frequency Interference)", "id": "Gangguan Akibat Frekuensi Radio." },
  { "en": "Apa Itu Filter EMI?", "id": "Rangkaian Meredam Gangguan EMI." },
  { "en": "Apa Itu Ferrite Bead?", "id": "Komponen Pasif Meredam EMI Frekuensi Tinggi." },
  { "en": "Apa Itu Analisis Sirkuit Listrik?", "id": "Metode Menghitung Arus, Tegangan Rangkaian." },
  { "en": "Apa Itu Analisis Mesh?", "id": "Metode Analisis Sirkuit Menggunakan Loop Arus." },
  { "en": "Apa Itu Analisis Node?", "id": "Metode Analisis Sirkuit Menggunakan Tegangan Titik." },
  { "en": "Apa Itu Teorema Superposisi?", "id": "Analisis Rangkaian Linier Satu Sumber Aktif." },
  { "en": "Apa Itu Teorema Thevenin?", "id": "Menyederhanakan Rangkaian Menjadi Sumber Tegangan, Resistor." },
  { "en": "Apa Itu Teorema Norton?", "id": "Menyederhanakan Rangkaian Menjadi Sumber Arus, Resistor." },
  { "en": "Apa Itu Resistor Ekuivalen Thevenin?", "id": "Hambatan Pengganti Dilihat Dari Terminal." },
  { "en": "Apa Itu Sumber Tegangan Ideal?", "id": "Sumber Tegangan Hambatan Dalam Nol." },
  { "en": "Apa Itu Sumber Arus Ideal?", "id": "umber Arus Hambatan Dalam Tak Terhingga." },
  { "en": "Apa Itu Resonansi Rangkaian RLC?", "id": "Kondisi Reaktansi Induktif, Kapasitif Sama." },
  { "en": "Apa Itu Frekuensi Resonansi?", "id": "Frekuensi Terjadinya Kondisi Resonansi." },
  { "en": "Apa Sifat Rangkaian RLC Seri Resonansi?", "id": "Impedansi Minimum (Bersifat Resistif Murni)." },
  { "en": "Apa Sifat Rangkaian RLC Paralel Resonansi?", "id": "Impedansi Maksimum (Bersifat Resistif Murni)." },
  { "en": "Apa Itu Faktor Kualitas? (Q Factor)", "id": "Ukuran Selektivitas Rangkaian Resonansi." },
  { "en": "Apa Itu Bandwidth?", "id": "Lebar Pita Frekuensi Rangkaian Resonansi." },
  { "en": "Apa Itu VCB? (Vacuum Circuit Breaker)", "id": "PMT Menggunakan Ruang Hampa Udara." },
  { "en": "Apa Keuntungan VCB?", "id": "Pemadaman Busur Api Cepat, Bebas Perawatan." },
  { "en": "Apa Itu OCB? (Oil Circuit Breaker)", "id": "PMT Menggunakan Minyak Sebagai Pemadam." },
  { "en": "Apa Itu ACB? (Air Circuit Breaker)", "id": "PMT Menggunakan Udara Tekanan Rendah." },
  { "en": "Di Mana ACB Biasa Digunakan?", "id": "Panel Distribusi Utama Tegangan Rendah." },
  { "en": "Apa Itu GCB? (Gas Circuit Breaker)", "id": "PMT Menggunakan Gas SF6." },
  { "en": "Apa Itu Rating Pemutusan? (Breaking Capacity)", "id": "Kemampuan Maksimum PMT Memutus Arus." },
  { "en": "Apa Satuan Breaking Capacity?", "id": "Kilo Ampere (kA)." },
  { "en": "Apa Itu Rating Pembuatan? (Making Capacity)", "id": "Kemampuan PMT Menutup Arus Hubung Singkat." },
  { "en": "Apa Itu Operasi Buka-Tutup? (O-C-O)", "id": "Siklus Operasi Standar PMT, Recloser." },
  { "en": "Apa Itu Energi Listrik?", "id": "Daya Listrik Yang Digunakan Waktu Tertentu." },
  { "en": "Apa Satuan Energi Listrik?", "id": "Watt-Hour (Wh) Atau Kilo Watt-Hour (KWH)." },
  { "en": "Apa Itu Meter KWH?", "id": "Alat Pengukur Konsumsi Energi Listrik." },
  { "en": "Apa Itu Meter KWH Analog?", "id": "Meter Menggunakan Piringan Berputar." },
  { "en": "Apa Itu Meter KWH Digital?", "id": "Meter Elektronik Dengan Tampilan Digital." },
  { "en": "Apa Itu Meter Prabayar?", "id": "Meter Menggunakan Sistem Token Pulsa Listrik." },
  { "en": "Apa Itu Meter Pascabayar?", "id": "Meter Tagihan Listrik Bulanan." },
  { "en": "Apa Itu Konstanta Meter KWH?", "id": "Jumlah Putaran Atau Kedipan Per KWH." },
  { "en": "Apa Itu Tarif Listrik?", "id": "Harga Energi Listrik Per KWH." },
  { "en": "Apa Itu Tarif Blok?", "id": "Tarif Berbeda Berdasarkan Jumlah Pemakaian." },
  { "en": "Apa Itu Biaya Beban?", "id": "Biaya Tetap Langganan Listrik Bulanan." },
  { "en": "Apa Itu WBP? (Waktu Beban Puncak)", "id": "Waktu Pemakaian Listrik Tertinggi (Malam Hari)." },
  { "en": "Apa Itu LWBP? (Luar Waktu Beban Puncak)", "id": "Waktu Pemakaian Listrik Selain WBP." },
  { "en": "Mengapa Tarif WBP Lebih Mahal?", "id": "Mendorong Hemat Energi Saat Beban Puncak." },
  { "en": "Apa Itu Pencurian Listrik?", "id": "Penggunaan Listrik Ilegal Tidak Tercatat." },
  { "en": "Apa Itu P2TL? (Penertiban Pemakaian Tenaga Listrik)", "id": "Tim Penertiban Pencurian Listrik PLN." },
  { "en": "Apa Itu Sambungan Rumah? (SR)", "id": "Kabel Penghubung JTR Ke Meter Pelanggan." },
  { "en": "Apa Itu APP? (Alat Pengukur, Pembatas)", "id": "Meter KWH Dan MCB Pelanggan." },
  { "en": "Apa Itu Box APP?", "id": "Kotak Pelindung Meter KWH, MCB." },
  { "en": "Apa Itu Instalasi Listrik Rumah?", "id": "Jaringan Kabel Di Dalam Bangunan Pelanggan." },
  { "en": "Apa Itu SLO? (Sertifikat Laik Operasi)", "id": "Sertifikat Keamanan Instalasi Listrik." },
  { "en": "Siapa Yang Menerbitkan SLO?", "id": "Lembaga Inspeksi Teknik Terakreditasi." },
  { "en": "Mengapa SLO Diperlukan?", "id": "Syarat Sambungan Baru, Tambah Daya." },
  { "en": "Apa Itu Lembaga KONSUIL?", "id": "Komite Nasional Keselamatan Instalasi Listrik." },
  { "en": "Apa Itu Kelistrikan Industri?", "id": "Sistem Kelistrikan Skala Besar Pabrik." },
  { "en": "Apa Beda Listrik Industri, Rumah?", "id": "Industri Umumnya Tiga Fasa Daya Besar." },
  { "en": "Apa Itu Kabel Ladder?", "id": "Rak Kabel Terbuka Berbentuk Tangga." },
  { "en": "Apa Itu Kabel Tray?", "id": "Rak Kabel Tertutup Atau Berlubang." },
  { "en": "Apa Fungsi Kabel Ladder/Tray?", "id": "Menopang, Melindungi Jalur Kabel Listrik." },
  { "en": "Apa Itu Sistem Busduct?", "id": "Sistem Distribusi Listrik Menggunakan Batang Konduktor." },
  { "en": "Apa Keuntungan Busduct?", "id": "Instalasi Rapi, KHA Besar, Fleksibel." },
  { "en": "Apa Itu Rising Main?", "id": "Jalur Busduct Atau Kabel Vertikal Gedung." },
  { "en": "Apa Itu Sistem Penerangan Darurat? (Emergency Lighting)", "id": "Lampu Menyala Otomatis Saat Listrik Padam." },
  { "en": "Apa Sumber Listrik Lampu Darurat?", "id": "Baterai Internal Atau UPS Sentral." },
  { "en": "Apa Itu Sistem Peringatan Kebakaran? (Fire Alarm)", "id": "Sistem Pendeteksi Dini Kebakaran." },
  { "en": "Apa Itu Detektor Asap? (Smoke Detector)", "id": "Sensor Mendeteksi Adanya Asap." },
  { "en": "Apa Itu Detektor Panas? (Heat Detector)", "id": "Sensor Mendeteksi Kenaikan Suhu Drastis." },
  { "en": "Apa Itu MCP? (Manual Call Point)", "id": "Tombol Manual Aktivasi Alarm Kebakaran." },
  { "en": "Apa Itu Annunciator Panel?", "id": "Panel Menampilkan Lokasi Titik Alarm." },
  { "en": "Apa Itu Sistem Tata Suara? (Sound System)", "id": "Sistem Pengeras Suara Informasi, Evakuasi." },
  { "en": "Apa Itu Sistem Proteksi Petir?", "id": "Instalasi Pengaman Bangunan Dari Petir." },
  { "en": "Apa Itu Proteksi Petir Eksternal?", "id": "Melindungi Fisik Bangunan Dari Sambaran Langsung." },
  { "en": "Apa Itu Proteksi Petir Internal?", "id": "Melindungi Peralatan Elektronik Dari Surja." },
  { "en": "Apa Itu Penangkal Petir Konvensional?", "id": "Sistem Franklin Rod (Splitzen)." },
  { "en": "Apa Itu Penangkal Petir Elektrostatis?", "id": "Sistem Radius Proteksi Lebih Luas." },
  { "en": "Apa Itu Konduktor Penyalur? (Down Conductor)", "id": "Kabel Menyalurkan Arus Petir Ke Tanah." },
  { "en": "Apa Itu Elektroda Pentanahan Petir?", "id": "Sistem Pembuangan Arus Petir Tanah." },
  { "en": "Apa Itu SPD? (Surge Protection Device)", "id": "Alat Proteksi Surja Petir Internal." },
  { "en": "Dimana SPD Dipasang?", "id": "Panel Listrik Utama, Sub-Panel." },
  { "en": "Apa Itu Ikatan Ekuipotensial? (Equipotential Bonding)", "id": "Menyamakan Potensial Benda Logam, Ground." },
  { "en": "Apa Tujuan Ikatan Ekuipotensial?", "id": "Mencegah Beda Tegangan Berbahaya." },
  { "en": "Apa Itu Sistem Otomasi Gedung? (BAS)", "id": "Sistem Kontrol Terpusat Fasilitas Gedung." },
  { "en": "Apa Kepanjangan BAS? (Building Automation System)", "id": "Sistem Kontrol Otomatis Gedung Cerdas." },
  { "en": "Apa Yang Diatur BAS?", "id": "AC (HVAC), Lampu, Listrik, Keamanan." },
  { "en": "Apa Kepanjangan HVAC? (Heating, Ventilation, Air Conditioning)", "id": "Sistem Tata Udara Gedung." },
  { "en": "Apa Itu Chiller?", "id": "Mesin Pendingin Air Skala Besar HVAC." },
  { "en": "Apa Itu AHU? (Air Handling Unit)", "id": "Unit Pengolah Udara Distribusi HVAC." },
  { "en": "Apa Itu Ducting?", "id": "Saluran Distribusi Udara HVAC." },
  { "en": "Apa Itu DDC? (Direct Digital Control)", "id": "Kontroler Digital Sistem BAS." },
  { "en": "Apa Itu Substation Automation System? (SAS)", "id": "Sistem Otomasi Kontrol Gardu Induk." },
  { "en": "Apa Itu IED? (Intelligent Electronic Device)", "id": "Perangkat Pintar (Relay Digital) SAS." },
  { "en": "Apa Protokol Komunikasi SAS?", "id": "IEC 61850." },
  { "en": "Apa Itu HMI? (Human Machine Interface)", "id": "Tampilan Grafis Operator SAS, SCADA." },
  { "en": "Apa Itu RTU? (Remote Terminal Unit)", "id": "Terminal Pengumpul Data Lapangan SCADA." },
  { "en": "Apa Fungsi RTU?", "id": "Mengirim Data Telemetri Ke Pusat Kontrol." },
  { "en": "Apa Itu Telemetri?", "id": "Pengukuran Data Jarak Jauh." },
  { "en": "Apa Itu Telekontrol?", "id": "Pengendalian Peralatan Jarak Jauh." },
  { "en": "Apa Itu Pusat Kontrol Listrik? (Control Center)", "id": "Pusat Pemantauan Operasi Sistem Listrik." },
  { "en": "Apa Itu Dispatcher?", "id": "Operator Pusat Kontrol Listrik." },
  { "en": "Apa Itu Studi Aliran Daya? (Load Flow Study)", "id": "Analisis Aliran Daya Kondisi Steady State." },
  { "en": "Apa Tujuan Studi Aliran Daya?", "id": "Mengetahui Tegangan, Arus, Daya Sistem." },
  { "en": "Apa Itu Studi Hubung Singkat?", "id": "Analisis Arus Maksimum Saat Gangguan." },
  { "en": "Apa Tujuan Studi Hubung Singkat?", "id": "Menentukan Rating PMT, Setting Proteksi." },
  { "en": "Apa Itu Studi Stabilitas?", "id": "Analisis Kemampuan Sistem Pulih Gangguan." },
  { "en": "Apa Itu Tes Rasio CT?", "id": "Menguji Perbandingan Arus Primer, Sekunder CT." },
  { "en": "Apa Itu Tes Injeksi Primer?", "id": "Pengujian Proteksi Dengan Arus Aktual Primer." },
  { "en": "Apa Itu Tes Injeksi Sekunder?", "id": "Pengujian Relay Dengan Arus, Tegangan Sekunder." },
  { "en": "Apa Itu Megger?", "id": "Alat Ukur Tahanan Isolasi (Insulation Tester)." },
  { "en": "Apa Itu Earth Tester?", "id": "Alat Ukur Tahanan Pentanahan." },
  { "en": "Apa Metode Tes Tahanan Tanah?", "id": "Metode Tiga Titik (Fall of Potential)." },
  { "en": "Apa Itu Loop Tester?", "id": "Mengukur Impedansi Loop Gangguan Tanah." },
  { "en": "Apa Itu RCD Tester?", "id": "Alat Tes Fungsi ELCB Atau RCD." },
  { "en": "Apa Kepanjangan RCD? (Residual Current Device)", "id": "Nama Lain ELCB, Proteksi Arus Bocor." },
  { "en": "Apa Itu Tingkat Sensitivitas RCD?", "id": "Arus Bocor Minimum Pemicu Trip." },
  { "en": "Berapa Sensitivitas RCD Proteksi Manusia?", "id": "30 Mili Ampere (mA) Atau Kurang." },
  { "en": "Apa Itu Nuisance Trip?", "id": "Kondisi Trip Peralatan Tanpa Gangguan Nyata." },
  { "en": "Apa Itu Motor DC Brushless? (BLDC)", "id": "Motor DC Tanpa Sikat Arang." },
  { "en": "Apa Keuntungan Motor BLDC?", "id": "Efisiensi Tinggi, Perawatan Mudah." },
  { "en": "Apa Itu Hall Effect Sensor?", "id": "Sensor Posisi Rotor Motor BLDC." },
  { "en": "Apa Itu Komutasi Elektronik?", "id": "Proses Komutasi Motor BLDC Secara Elektronik." },
  { "en": "Apa Itu Generator Induksi?", "id": "Motor Induksi Berfungsi Sebagai Generator." },
  { "en": "Kapan Generator Induksi Dipakai?", "id": "Pembangkit Listrik Tenaga Angin (PLTB)." },
  { "en": "Apa Itu Slip Ring?", "id": "Cincin Konduktif Transfer Daya Rotor." },
  { "en": "Dimana Slip Ring Digunakan?", "id": "Motor Rotor Belitan, Generator Sinkron." },
  { "en": "Apa Itu Tes Beban Nol?", "id": "Tes Trafo, Motor Tanpa Beban." },
  { "en": "Apa Tujuan Tes Beban Nol Trafo?", "id": "Menentukan Rugi Inti Besi." },
  { "en": "Apa Itu Tes Hubung Singkat?", "id": "Tes Trafo, Motor Kondisi Terminal Singkat." },
  { "en": "Apa Tujuan Tes Hubung Singkat Trafo?", "id": "Menentukan Rugi Tembaga, Impedansi." },
  { "en": "Apa Itu Impedansi Trafo?", "id": "Hambatan Internal Trafo Saat Beban Penuh." },
  { "en": "Mengapa Impedansi Trafo Penting?", "id": "Penting Untuk Operasi Paralel, Arus Gangguan." },
  { "en": "Apa Itu Grup Vektor Trafo?", "id": "Menunjukkan Pergeseran Fasa Lilitan Trafo." },
  { "en": "Apa Contoh Grup Vektor?", "id": "Dyn11, Yy0, Yd1." },
  { "en": "Mengapa Grup Vektor Penting?", "id": "Syarat Utama Paralel Transformator." },
  { "en": "Apa Itu Pergeseran Fasa?", "id": "Perbedaan Sudut Waktu Gelombang AC." },
  { "en": "Apa Itu Hubungan Zig-Zag Trafo?", "id": "Hubungan Khusus Meredam Harmonisa Ketiga." },
  { "en": "Apa Itu Harmonisa Ketiga?", "id": "Komponen Frekuensi Tiga Kali Fundamental." },
  { "en": "Apa Itu Arus Magnetisasi?", "id": "Arus Dibutuhkan Membangkitkan Fluks Inti." },
  { "en": "Apa Sifat Arus Magnetisasi?", "id": "Non-Linier, Mengandung Banyak Harmonisa." },
  { "en": "Apa Itu Sirkit Magnetik?", "id": "Jalur Tertutup Fluks Magnetik." },
  { "en": "Apa Itu Gaya Gerak Magnet? (GGM)", "id": "Penyebab Timbulnya Fluks Magnetik." },
  { "en": "Apa Rumus GGM?", "id": "GGM = N x I (Lilitan Kali Arus)." },
  { "en": "Apa Satuan GGM?", "id": "Ampere-Turns (AT)." },
  { "en": "Apa Itu Intensitas Medan Magnet? (H)", "id": "GGM Per Satuan Panjang Sirkit." },
  { "en": "Apa Satuan Intensitas Medan Magnet?", "id": "Ampere-Turns Per Meter (AT/m)." },
  { "en": "Apa Itu Kurva B-H?", "id": "Grafik Hubungan Kerapatan Fluks (B), Intensitas (H)." },
  { "en": "Apa Itu Saturasi Magnetik?", "id": "Kondisi Inti Besi Jenuh Fluks Magnet." },
  { "en": "Apa Akibat Saturasi?", "id": "Arus Magnetisasi Naik Tajam." },
  { "en": "Apa Itu Tes Loop Dielektrik?", "id": "Tes Mengukur Kerugian Dielektrik Isolasi." },
  { "en": "Apa Itu Tan Delta?", "id": "Nama Lain Faktor Daya Isolasi." },
  { "en": "Apa Arti Nilai Tan Delta Tinggi?", "id": "Kualitas Isolasi Buruk, Lembab, Kotor." },
  { "en": "Apa Itu Motor Linier?", "id": "Motor Menghasilkan Gerakan Lurus (Linier)." },
  { "en": "Apa Prinsip Kerja Motor Linier?", "id": "Sama Seperti Motor Induksi (Stator Dibuka)." },
  { "en": "Dimana Motor Linier Digunakan?", "id": "Kereta Maglev, Mesin Industri Presisi." },
  { "en": "Apa Itu Kereta Maglev?", "id": "Kereta Melayang Menggunakan Levitasi Magnetik." },
  { "en": "Apa Itu Superkonduktor?", "id": "Bahan Hambatan Listrik Nol Suhu Rendah." },
  { "en": "Apa Keuntungan Superkonduktor?", "id": "Transfer Energi Tanpa Rugi Daya." },
  { "en": "Apa Itu SMES? (Superconducting Magnetic Energy Storage)", "id": "Penyimpan Energi Magnetik Superkonduktor." },
  { "en": "Apa Itu Pendinginan Kriogenik?", "id": "Pendinginan Suhu Sangat Rendah (Superkonduktor)." },
  { "en": "Apa Itu Sel Bahan Bakar? (Fuel Cell)", "id": "Mengubah Energi Kimia (Hidrogen) Jadi Listrik." },
  { "en": "Apa Emisi Sel Bahan Bakar?", "id": "Hanya Air (H2O), Ramah Lingkungan." },
  { "en": "Apa Itu Elektrolisis?", "id": "Proses Mengurai Air Menjadi Hidrogen, Oksigen." },
  { "en": "Apa Itu Baterai?", "id": "Alat Penyimpan Energi Kimia Jadi Listrik." },
  { "en": "Apa Itu Baterai Primer?", "id": "Baterai Sekali Pakai (Tidak Bisa Diisi)." },
  { "en": "Apa Contoh Baterai Primer?", "id": "Alkaline, Lithium (Koin)." },
  { "en": "Apa Itu Baterai Sekunder?", "id": "Baterai Dapat Diisi Ulang." },
  { "en": "Apa Contoh Baterai Sekunder?", "id": "Aki Mobil, Lithium-Ion (Ponsel)." },
  { "en": "Apa Itu Kapasitas Baterai?", "id": "Jumlah Muatan Tersimpan Dalam Baterai." },
  { "en": "Apa Satuan Kapasitas Baterai?", "id": "Ampere-Hour (Ah)." },
  { "en": "Apa Itu Siklus Hidup Baterai?", "id": "Jumlah Siklus Pengisian Ulang Baterai." },
  { "en": "Apa Itu Deep Cycle Battery?", "id": "Baterai Dirancang Tahan Pengosongan Dalam." },
  { "en": "Dimana Deep Cycle Battery Digunakan?", "id": "Sistem PLTS, Golf Cart, Kapal." },
  { "en": "Apa Itu BESS? (Battery Energy Storage System)", "id": "Sistem Penyimpanan Energi Skala Besar." },
  { "en": "Apa Fungsi BESS?", "id": "Menstabilkan Jaringan, Menyimpan Energi Terbarukan." },
  { "en": "Apa Itu Peak Shaving?", "id": "Menggunakan BESS Mengurangi Beban Puncak." },
  { "en": "Apa Itu Load Shifting?", "id": "Memindah Waktu Penggunaan Listrik (BESS)." },
  { "en": "Apa Itu Manajemen Energi?", "id": "Upaya Penggunaan Energi Efisien, Efektif." },
  { "en": "Apa Itu Audit Energi?", "id": "Evaluasi Penggunaan Energi Suatu Fasilitas." },
  { "en": "Apa Tujuan Audit Energi?", "id": "Mengidentifikasi Peluang Penghematan Energi." },
  { "en": "Apa Itu Konservasi Energi?", "id": "Tindakan Mengurangi Pemakaian Energi." },
  { "en": "Apa Itu Efisiensi Energi?", "id": "Menggunakan Energi Lebih Sedikit Pekerjaan Sama." },
  { "en": "Apa Itu Label Tanda Hemat Energi?", "id": "Label Menunjukkan Tingkat Efisiensi Alat." },
  { "en": "Apa Itu Standar IEC 60076?", "id": "Standar Internasional Untuk Transformator Daya." },
  { "en": "Apa Itu Standar IEC 60034?", "id": "Standar Internasional Untuk Mesin Listrik Berputar." },
  { "en": "Apa Itu Kelas Proteksi Ledakan? (Ex)", "id": "Klasifikasi Peralatan Area Berbahaya." },
  { "en": "Apa Itu Area Berbahaya? (Hazardous Area)", "id": "Area Berpotensi Terdapat Gas, Debu Mudah Terbakar." },
  { "en": "Apa Itu Peralatan Explosion Proof? (Ex d)", "id": "Selungkup Tahan Ledakan Internal." },
  { "en": "Apa Itu Peralatan Intrinsic Safety? (Ex i)", "id": "Energi Sirkuit Dibatasi (Tidak Mampu Memicu)." },
  { "en": "Apa Itu NEC? (National Electrical Code)", "id": "Standar Instalasi Listrik Amerika Serikat." },
  { "en": "Apa Itu NFPA 70E?", "id": "Standar Keselamatan Listrik Tempat Kerja." },
  { "en": "Apa Itu Batas Busur Api? (Arc Flash Boundary)", "id": "Jarak Aman Minimal Bahaya Busur Api." },
  { "en": "Apa Itu APD Busur Api?", "id": "Pakaian Pelindung Khusus Tahan Panas." },
  { "en": "Apa Itu Analisis Bahaya Busur Api?", "id": "Studi Menentukan Energi Insiden Busur Api." },
  { "en": "Apa Itu Energi Insiden?", "id": "Energi Panas Diterima Pada Jarak Tertentu." },
  { "en": "Apa Satuan Energi Insiden?", "id": "Kalori Per Sentimeter Persegi (Cal/cm²)." },
  { "en": "Apa Itu Kategori APD Busur Api?", "id": "Tingkatan Proteksi (CAT 1 Hingga 4)." },
  { "en": "Apa Itu Tes TTR? (Transformer Turns Ratio)", "id": "Tes Mengukur Rasio Lilitan Trafo." },
  { "en": "Apa Itu Power Factor Tester?", "id": "Alat Ukur Faktor Daya Isolasi (Tan Delta)." },
  { "en": "Apa Itu Ductor Test?", "id": "Nama Lain Tes Tahanan Kontak Rendah." },
  { "en": "Apa Itu Motor Hibrida?", "id": "Menggabungkan Fitur Dua Tipe Motor (Stepper)." },
  { "en": "Apa Itu Motor Reluktansi Variabel? (VR)", "id": "Motor Stepper Menggunakan Prinsip Reluktansi." },
  { "en": "Apa Itu Motor Magnet Permanen? (PM)", "id": "Motor Menggunakan Magnet Permanen Di Rotor." },
  { "en": "Apa Itu Generator Magnet Permanen? (PMG)", "id": "Generator Menggunakan Magnet Permanen." },
  { "en": "Dimana PMG Digunakan?", "id": "Turbin Angin Kecil, Sepeda Listrik." },
  { "en": "Apa Itu Sistem Tenaga DC?", "id": "Sistem Kelistrikan Menggunakan Arus Searah." },
  { "en": "Dimana Sistem Tenaga DC Digunakan?", "id": "Telekomunikasi, Data Center, Gardu Induk." },
  { "en": "Apa Tegangan Sistem DC Telekomunikasi?", "id": "Umumnya -48 Volt DC." },
  { "en": "Mengapa Menggunakan -48 Volt DC?", "id": "Mengurangi Korosi Kabel Positif Ditanahkan." },
  { "en": "Apa Itu PDB? (Power Distribution Board)", "id": "Panel Distribusi Daya Listrik DC." },
  { "en": "Apa Itu Bank Baterai?", "id": "Kumpulan Baterai Disusun Seri, Paralel." },
  { "en": "Apa Itu Tegangan Nominal Baterai?", "id": "Tegangan Referensi Satu Sel Baterai." },
  { "en": "Berapa Tegangan Sel Aki Timbal-Asam?", "id": "Sekitar 2 Volt Per Sel." },
  { "en": "Berapa Sel Dalam Aki 12 Volt?", "id": "Enam Sel Disusun Seri." },
  { "en": "Apa Itu BMS? (Battery Management System)", "id": "Sistem Elektronik Pengelola Baterai Isi Ulang." },
  { "en": "Apa Fungsi BMS?", "id": "Melindungi Baterai, Menyeimbangkan Sel, Monitoring." },
  { "en": "Apa Itu Penyeimbangan Sel? (Cell Balancing)", "id": "Menyamakan Level Tegangan Sel Baterai." },
  { "en": "Mengapa Penyeimbangan Sel Penting?", "id": "Memaksimalkan Kapasitas, Umur Pak Baterai." },
  { "en": "Apa Itu Kendaraan Listrik? (EV)", "id": "Kendaraan Menggunakan Motor Listrik." },
  { "en": "Apa Itu BEV? (Battery Electric Vehicle)", "id": "Kendaraan Listrik Murni Menggunakan Baterai." },
  { "en": "Apa Itu HEV? (Hybrid Electric Vehicle)", "id": "Kendaraan Hibrida (Motor Listrik, Mesin Bensin)." },
  { "en": "Apa Itu PHEV? (Plug-in Hybrid Electric Vehicle)", "id": "Kendaraan Hibrida Baterai Bisa Diisi Eksternal." },
  { "en": "Apa Itu Stasiun Pengisian EV? (Charging Station)", "id": "Tempat Pengisian Ulang Baterai Kendaraan List." },
  { "en": "Apa Itu Pengisian Cepat DC?", "id": "Mengisi Baterai EV Arus DC Besar." },
  { "en": "Apa Itu Pengisian AC?", "id": "Mengisi Baterai EV Arus AC (Lewat Charger Internal)." },
  { "en": "Apa Itu On-Board Charger? (OBC)", "id": "Charger Internal Mobil Listrik (AC Ke DC)." },
  { "en": "Apa Itu Vehicle-to-Grid? (V2G)", "id": "Mobil Listrik Menyuplai Listrik Ke Jaringan." },
  { "en": "Apa Itu Smart Charging EV?", "id": "Pengisian Cerdas Mengatur Waktu, Daya." },
  { "en": "Apa Itu Konektor Tipe 2? (Type 2)", "id": "Standar Konektor Pengisian AC Eropa, Indonesia." },
  { "en": "Apa Itu Konektor CCS? (Combined Charging System)", "id": "Standar Konektor Pengisian Cepat DC." },
  { "en": "Apa Itu Konektor CHAdeMO?", "id": "Standar Konektor Pengisian Cepat DC Jepang." },
  { "en": "Apa Itu Wallbox Charger?", "id": "Charger EV Dipasang Di Dinding Rumah." },
  { "en": "Apa Itu Range Kendaraan Listrik?", "id": "Jarak Tempuh Maksimum Satu Kali Pengisian." },
  { "en": "Apa Itu Regenerative Braking?", "id": "Pengereman Mengisi Ulang Baterai Mobil Listrik." },
  { "en": "Apa Itu Motor Hub?", "id": "Motor Listrik Terpasang Langsung Roda." },
  { "en": "Apa Itu Skid Steer?", "id": "Kendaraan Roda Berputar Beda Arah." },
  { "en": "Apa Itu Electrical Resistivity Tomography? (ERT)", "id": "Metode Geofisika Mengukur Tahanan Tanah." },
  { "en": "Apa Itu Zero Sequence Current?", "id": "Komponen Arus Urutan Nol." },
  { "en": "Kapan Arus Urutan Nol Muncul?", "id": "Saat Terjadi Gangguan Fasa Ke Tanah." },
  { "en": "Apa Itu Zero Sequence Impedance?", "id": "Impedansi Urutan Nol Sistem Tenaga." },
  { "en": "Apa Itu Positive Sequence Impedance?", "id": "Impedansi Urutan Positif Sistem." },
  { "en": "Apa Itu Negative Sequence Impedance?", "id": "Impedansi Urutan Negatif Sistem." },
  { "en": "Apa Itu Negative Sequence Relay?", "id": "Relay Mendeteksi Beban Tidak Seimbang." },
  { "en": "Apa Fungsi Negative Sequence Relay?", "id": "Melindungi Generator Dari Pemanasan Rotor." },
  { "en": "Apa Itu Generator Berpendingin Hidrogen?", "id": "Generator Besar Menggunakan Pendingin Hidrogen." },
  { "en": "Apa Keuntungan Pendingin Hidrogen?", "id": "Pendinginan Jauh Lebih Efisien Daripada Udara." },
  { "en": "Apa Itu Generator Berpendingin Air?", "id": "Lilitan Stator Didinginkan Langsung Air." },
  { "en": "Apa Itu Stator?", "id": "Bagian Diam Mesin Listrik Berputar." },
  { "en": "Apa Itu Rotor?", "id": "Bagian Berputar Mesin Listrik." },
  { "en": "Apa Itu Celah Udara? (Air Gap)", "id": "Celah Antara Stator Dan Rotor." },
  { "en": "Mengapa Celah Udara Penting?", "id": "Memungkinkan Rotor Berputar Bebas." },
  { "en": "Apa Itu Lilitan Jangkar? (Armature Winding)", "id": "Lilitan Utama Tempat Induksi Tegangan." },
  { "en": "Apa Itu Lilitan Medan? (Field Winding)", "id": "Lilitan Pembangkit Medan Magnet Utama." },
  { "en": "Di Mana Lilitan Medan Generator Sinkron?", "id": "Lilitan Medan Terletak Di Rotor." },
  { "en": "Di Mana Lilitan Jangkar Generator Sinkron?", "id": "Lilitan Jangkar Terletak Di Stator." },
  { "en": "Apa Itu Mesin Kutub Menonjol? (Salient Pole)", "id": "Rotor Dengan Kutub Magnet Menonjol." },
  { "en": "Dimana Rotor Salient Pole Digunakan?", "id": "Generator PLTA (Putaran Rendah)." },
  { "en": "Apa Itu Mesin Kutub Silinder? (Cylindrical Pole)", "id": "Rotor Berbentuk Silinder Halus." },
  { "en": "Dimana Rotor Cylindrical Digunakan?", "id": "Generator PLTU/PLTG (Putaran Tinggi)." },
  { "en": "Apa Itu Kecepatan Sinkron?", "id": "Kecepatan Putar Medan Magnet Stator." },
  { "en": "Apa Rumus Kecepatan Sinkron? (Ns)", "id": "Ns = (120 x F) / P." },
  { "en": "Apa Itu F Dalam Rumus Sinkron?", "id": "F Adalah Frekuensi Listrik (Hertz)." },
  { "en": "Apa Itu P Dalam Rumus Sinkron?", "id": "P Adalah Jumlah Kutub (Pole) Motor." },
  { "en": "Apa Itu Slip Motor Induksi?", "id": "Perbedaan Persentase Kecepatan Sinkron, Rotor." },
  { "en": "Apa Rumus Slip Motor? (S)", "id": "S = (Ns - Nr) / Ns." },
  { "en": "Apa Itu Nr Dalam Rumus Slip?", "id": "Nr Adalah Kecepatan Putar Rotor Aktual." },
  { "en": "Berapa Nilai Slip Motor Saat Diam?", "id": "Nilai Slip Adalah Satu (S=1)." },
  { "en": "Berapa Nilai Slip Motor Sinkron?", "id": "Nilai Slip Adalah Nol (S=0)." },
  { "en": "Mengapa Motor Induksi Perlu Slip?", "id": "Menghasilkan Induksi Tegangan, Torsi Rotor." },
  { "en": "Apa Itu Torsi Awal? (Starting Torque)", "id": "Torsi Motor Saat Mulai Berputar." },
  { "en": "Apa Itu Torsi Puncak? (Breakdown Torque)", "id": "Torsi Maksimum Yang Dihasilkan Motor." },
  { "en": "Apa Itu Torsi Beban Penuh?", "id": "Torsi Motor Saat Berbeban Penuh." },
  { "en": "Apa Itu Pengasutan Motor? (Motor Starting)", "id": "Proses Awal Motor Mulai Berputar." },
  { "en": "Apa Itu Arus Asut? (Starting Current)", "id": "Arus Sangat Tinggi Saat Motor Start." },
  { "en": "Berapa Arus Asut Motor Induksi?", "id": "Enam Sampai Delapan Kali Arus Nominal." },
  { "en": "Apa Itu Pengasutan Autotransformator?", "id": "Mengurangi Tegangan Start Motor Pakai Autotrafo." },
  { "en": "Apa Itu Pengasutan Tahanan Primer?", "id": "Menambah Resistor Seri Stator Motor." },
  { "en": "Apa Itu Pengasutan Tahanan Rotor?", "id": "Menambah Resistor Eksternal Motor Wound Rotor." },
  { "en": "Apa Itu Pengereman Motor Listrik?", "id": "Metode Menghentikan Putaran Motor." },
  { "en": "Apa Itu Pengereman Mekanis?", "id": "Pengereman Menggunakan Gesekan (Rem Cakram)." },
  { "en": "Apa Itu Pengereman Elektrik?", "id": "Pengereman Menggunakan Prinsip Elektromagnetik." },
  { "en": "Apa Itu Pengereman Plugging?", "id": "Membalik Urutan Fasa Motor Induksi." },
  { "en": "Apa Kerugian Pengereman Plugging?", "id": "Arus Sangat Besar, Berbahaya Jika Gagal." },
  { "en": "Apa Itu Pengereman Injeksi DC?", "id": "Menginjeksikan Arus DC Ke Stator Motor AC." },
  { "en": "Apa Itu Perawatan Rutin?", "id": "Pemeliharaan Terjadwal Secara Periodik." },
  { "en": "Apa Itu Shutdown Maintenance?", "id": "Pemeliharaan Saat Mesin Berhenti Total." },
  { "en": "Apa Itu Online Maintenance?", "id": "Pemeliharaan Saat Peralatan Beroperasi." },
  { "en": "Apa Contoh Online Maintenance?", "id": "Pengujian Termografi, Analisis Getaran." },
  { "en": "Apa Itu Analisis Getaran? (Vibration Analysis)", "id": "Mendeteksi Kerusakan Mekanis Mesin Berputar." },
  { "en": "Apa Itu Misalignment?", "id": "Kondisi Poros Motor, Beban Tidak Lurus." },
  { "en": "Apa Akibat Misalignment?", "id": "Getaran Tinggi, Kerusakan Bearing Cepat." },
  { "en": "Apa Itu Unbalance?", "id": "Distribusi Massa Rotor Tidak Seimbang." },
  { "en": "Apa Itu Balancing Rotor?", "id": "Proses Menyeimbangkan Distribusi Massa Rotor." },
  { "en": "Apa Itu Bearing?", "id": "Bantalan Poros Berputar." },
  { "en": "Apa Fungsi Pelumasan Bearing?", "id": "Mengurangi Gesekan, Panas, Keausan." },
  { "en": "Apa Itu Grease?", "id": "Pelumas Kental Untuk Bearing." },
  { "en": "Apa Itu Tes Tahanan Kontak?", "id": "Mengukur Tahanan Sambungan Busbar, PMT." },
  { "en": "Mengapa Tahanan Kontak Harus Rendah?", "id": "Mencegah Panas Berlebih (Hotspot)." },
  { "en": "Apa Itu Tes Kecepatan PMT?", "id": "Mengukur Waktu Buka, Tutup Kontak PMT." },
  { "en": "Apa Itu Motor Tahan Api? (Flameproof)", "id": "Motor Khusus Area Berbahaya (Explosion Proof)." },
  { "en": "Apa Itu Breather Trafo?", "id": "Saluran Pernapasan Minyak Trafo." },
  { "en": "Apa Itu Tes Kadar Air Minyak?", "id": "Mengukur Kandungan Air Dalam Minyak Trafo." },
  { "en": "Apa Akibat Air Dalam Minyak Trafo?", "id": "Menurunkan Kekuatan Isolasi Dielektrik." },
  { "en": "Apa Itu Tes Tegangan Tembus Minyak?", "id": "Mengukur Kemampuan Dielektrik Minyak Trafo." },
  { "en": "Apa Satuan Tegangan Tembus Minyak?", "id": "Kilo Volt Per Mili Meter (kV/mm)." },
  { "en": "Apa Itu Purifikasi Minyak Trafo?", "id": "Proses Pemurnian Minyak Trafo (Filterisasi)." },
  { "en": "Apa Itu Reclaiming Minyak Trafo?", "id": "Proses Regenerasi Kualitas Minyak Trafo." },
  { "en": "Apa Itu Pressure Relief Device?", "id": "Katup Pelepas Tekanan Berlebih Trafo." },
  { "en": "Apa Itu Indikator Level Minyak?", "id": "Alat Pemantau Ketinggian Minyak Trafo." },
  { "en": "Apa Itu Sistem Proteksi Kebakaran Trafo?", "id": "Sistem Nitrogen, Semprotan Air (Water Spray)." },
  { "en": "Apa Itu Dinding Api? (Firewall)", "id": "Dinding Pemisah Tahan Api Antar Trafo." },
  { "en": "Apa Itu Oil Containment Pit?", "id": "Bak Penampung Tumpahan Minyak Trafo." },
  { "en": "Apa Itu Sistem Pentanahan TN-C? (Terra Neutral Combined)", "id": "Netral, Proteksi (PE) Digabung (PEN)." },
  { "en": "Apa Itu Sistem Pentanahan TN-S? (Terra Neutral Separated)", "id": "Netral, Proteksi (PE) Terpisah Total." },
  { "en": "Apa Itu Sistem Pentanahan TN-C-S? (Combined Separated)", "id": "Gabungan TN-C (Jalan), TN-S (Gedung)." },
  { "en": "Apa Itu Kawat PEN? (Protective Earth Neutral)", "id": "Konduktor Gabungan Netral, Grounding." },
  { "en": "Apa Itu Kawat PE? (Protective Earth)", "id": "Konduktor Khusus Proteksi Pentanahan." },
  { "en": "Apa Itu Sistem IT? (Isolated Terra)", "id": "Netral Sistem Terisolasi Dari Tanah." },
  { "en": "Di Mana Sistem IT Digunakan?", "id": "Rumah Sakit, Industri (Keandalan Tinggi)." },
  { "en": "Apa Itu IMD? (Insulation Monitoring Device)", "id": "Memantau Isolasi Sistem IT." },
  { "en": "Apa Itu Tegangan Sentuh? (Touch Voltage)", "id": "Tegangan Antara Bodi Alat, Tanah." },
  { "en": "Apa Itu Tegangan Langkah? (Step Voltage)", "id": "Beda Tegangan Antara Dua Kaki (Tanah)." },
  { "en": "Apa Bahaya Tegangan Sentuh, Langkah?", "id": "Menyebabkan Aliran Arus Kejut Listrik." },
  { "en": "Apa Rumus Daya Listrik Satu Fasa?", "id": "P = V x I x Cos(Φ)." },
  { "en": "Apa Rumus Daya Listrik Tiga Fasa?", "id": "P = √3 x VL x IL x Cos(Φ)." },
  { "en": "Apa Itu VL? (Voltage Line)", "id": "Tegangan Antar Fasa (Line-to-Line)." },
  { "en": "Apa Itu IL? (Current Line)", "id": "Arus Pada Saluran Fasa." },
  { "en": "Apa Hubungan Tegangan Line, Fasa (Bintang)?", "id": "VL = √3 x Vph." },
  { "en": "Apa Hubungan Arus Line, Fasa (Bintang)?", "id": "IL = Iph." },
  { "en": "Apa Hubungan Tegangan Line, Fasa (Delta)?", "id": "VL = Vph." },
  { "en": "Apa Hubungan Arus Line, Fasa (Delta)?", "id": "IL = √3 x Iph." },
  { "en": "Apa Rumus Rugi Daya Kabel?", "id": "PLoss = I² x R." },
  { "en": "Apa Rumus Impedansi Total RLC Seri?", "id": "Z = √(R² + (XL - XC)²)." },
  { "en": "Apa Rumus Reaktansi Induktif? (XL)", "id": "XL = 2 x π x f x L." },
  { "en": "Apa Rumus Reaktansi Kapasitif? (XC)", "id": "XC = 1 / (2 x π x f x C)." },
  { "en": "Apa Itu Trafo Zig-Zag?", "id": "Trafo Pentanahan (Grounding Transformer)." },
  { "en": "Apa Fungsi Trafo Zig-Zag?", "id": "Menyediakan Titik Netral Buatan." },
  { "en": "Apa Itu Trafo Scott-T?", "id": "Trafo Pengubah Tiga Fasa Ke Dua Fasa." },
  { "en": "Apa Itu Tes Doble?", "id": "Nama Merek Tes Faktor Daya Isolasi." },
  { "en": "Apa Itu Pengujian Getaran? (Vibration Test)", "id": "Menganalisis Kondisi Mekanis Mesin Berputar." },
  { "en": "Apa Itu Thermovision?", "id": "Nama Lain Inspeksi Termografi Inframerah." },
  { "en": "Apa Itu Analisis Tanda Tangan Motor? (MCSA)", "id": "Menganalisis Motor Lewat Arus, Tegangan." },
  { "en": "Apa Kepanjangan MCSA? (Motor Current Signature Analysis)", "id": "Analisis Tanda Tangan Arus Motor." },
  { "en": "Apa Itu Broken Rotor Bar?", "id": "Kerusakan Batang Rotor Motor Induksi." },
  { "en": "Bagaimana MCSA Mendeteksi Broken Bar?", "id": "Melalui Frekuensi Slip Sisi Arus." },
  { "en": "Apa Itu Tes Surge (Gelombang Kejut)?", "id": "Tes Mendeteksi Kelemahan Isolasi Lilitan." },
  { "en": "Kapan Tes Surge Dilakukan?", "id": "Setelah Motor Selesai Digulung Ulang." },
  { "en": "Apa Itu Proses VPI? (Vaccum Pressure Impregnation)", "id": "Proses Pelapisan Ulang Lilitan Motor." },
  { "en": "Apa Tujuan VPI?", "id": "Meningkatkan Kekuatan Isolasi, Mekanis Lilitan." },
  { "en": "Apa Itu Bearing Berisolasi?", "id": "Bantalan (Bearing) Berisolasi Listrik." },
  { "en": "Mengapa Bearing Diisolasi?", "id": "Mencegah Arus Poros (Shaft Current)." },
  { "en": "Apa Penyebab Arus Poros?", "id": "Asimetri Fluks, Penggunaan VFD." },
  { "en": "Apa Akibat Arus Poros?", "id": "Kerusakan Dini Bearing (Fluting)." },
  { "en": "Apa Itu Cincin Pentanahan Poros? (Shaft Grounding Ring)", "id": "Mengalirkan Arus Poros Ke Ground." },
  { "en": "Apa Itu Kode ANSI Relay 50?", "id": "Relay Arus Lebih Seketika (Instantaneous)." },
  { "en": "Apa Itu Kode ANSI Relay 51?", "id": "Relay Arus Lebih Waktu Tunda (IDMT)." },
  { "en": "Apa Itu Kode ANSI Relay 87?", "id": "Relay Proteksi Diferensial." },
  { "en": "Apa Itu Kode ANSI Relay 27?", "id": "Relay Tegangan Kurang (Under Voltage)." },
  { "en": "Apa Itu Kode ANSI Relay 59?", "id": "Relay Tegangan Lebih (Over Voltage)." },
  { "en": "Apa Itu Kode ANSI Relay 32?", "id": "Relay Daya Balik (Reverse Power)." },
  { "en": "Apa Itu Kode ANSI Relay 49?", "id": "Relay Termal (Beban Lebih)." },
  { "en": "Apa Itu Kode ANSI Relay 67?", "id": "Relay Arus Lebih Berarah." },
  { "en": "Apa Itu Kode ANSI Relay 21?", "id": "Relay Jarak (Distance Relay)." },
  { "en": "Apa Itu Kode ANSI Relay 86?", "id": "Relay Pengunci (Lockout Relay)." },
  { "en": "Apa Fungsi Relay Lockout (86)?", "id": "Mengunci Operasi PMT Setelah Trip Berat." },
  { "en": "Apa Itu Kode ANSI Relay 46?", "id": "Relay Arus Urutan Negatif." },
  { "en": "Apa Itu Kode ANSI Relay 64?", "id": "Relay Gangguan Tanah (Earth Fault)." },
  { "en": "Apa Itu Kode ANSI Relay 79?", "id": "Relay Penutup Balik Otomatis (Recloser)." },
  { "en": "Apa Itu Kode ANSI Relay 25?", "id": "Relay Pengecekan Sinkronisasi (Synchro-Check)." },
  { "en": "Apa Fungsi Relay 25?", "id": "Memastikan Syarat Paralel Generator Terpenuhi." },
  { "en": "Apa Itu Kode ANSI Relay 81?", "id": "Relay Frekuensi (Lebih/Kurang)." },
  { "en": "Apa Itu Proteksi Busbar?", "id": "Proteksi Diferensial Mengamankan Busbar." },
  { "en": "Apa Itu K-Factor Transformator?", "id": "Kemampuan Trafo Menahan Harmonisa Beban." },
  { "en": "Mengapa Trafo K-Factor Diperlukan?", "id": "Beban Non-Linier (Komputer, VFD)." },
  { "en": "Apa Itu Detuned Reactor?", "id": "Reaktor Mencegah Resonansi Kapasitor Bank." },
  { "en": "Apa Itu Filter Pasif Harmonisa?", "id": "Rangkaian L-C Ditala Frekuensi Harmonisa." },
  { "en": "Apa Itu Filter Aktif Harmonisa? (AHF)", "id": "Menginjeksikan Arus Kompensasi Harmonisa." },
  { "en": "Apa Itu Sag?", "id": "Penurunan Tegangan Sesaat (Kualitas Daya)." },
  { "en": "Apa Itu Swell?", "id": "Kenaikan Tegangan Sesaat (Kualitas Daya)." },
  { "en": "Apa Itu Interupsi?", "id": "Hilang Tegangan Total Sesaat." },
  { "en": "Apa Itu CBEMA Curve?", "id": "Kurva Toleransi Peralatan Komputer (Daya)." },
  { "en": "Apa Itu ITIC Curve?", "id": "Penerus Kurva CBEMA (Toleransi Peralatan IT)." },
  { "en": "Apa Itu Pemanas Induksi? (Induction Heating)", "id": "Pemanasan Logam Menggunakan Arus Eddy." },
  { "en": "Apa Itu Tungku Busur Listrik? (Electric Arc Furnace)", "id": "Melebur Logam Menggunakan Busur Api Listrik." },
  { "en": "Apa Itu Elektroplating?", "id": "Proses Pelapisan Logam Menggunakan Listrik DC." },
  { "en": "Apa Itu Proteksi Katodik?", "id": "Metode Pencegahan Korosi Logam (Pipa)." },
  { "en": "Apa Itu Arus Anoda Korban? (Sacrificial Anode)", "id": "Logam Lebih Reaktif Melindungi Logam Lain." },
  { "en": "Apa Itu Arus Impresi? (Impressed Current)", "id": "Proteksi Katodik Menggunakan Sumber Listrik DC." },
  { "en": "Apa Itu Motor Traksi?", "id": "Motor Listrik Penggerak (Kereta, EV)." },
  { "en": "Apa Itu Catenary?", "id": "Kabel Listrik Atas Kereta Listrik (LAA)." },
  { "en": "Apa Itu Pantograf?", "id": "Alat Pengambil Listrik Catenary Kereta." },
  { "en": "Apa Itu Rel Ketiga? (Third Rail)", "id": "Suplai Listrik Kereta Lewat Rel Samping." },
  { "en": "Di Mana Rel Ketiga Digunakan?", "id": "Sistem Kereta Bawah Tanah (MRT)." },
  { "en": "Apa Itu Relai Tekanan Mendadak? (Sudden Pressure)", "id": "Mendeteksi Kenaikan Tekanan Cepat Trafo." },
  { "en": "Apa Itu Tes Torsi Motor?", "id": "Mengukur Karakteristik Torsi, Kecepatan Motor." },
  { "en": "Apa Itu Dynamometer?", "id": "Alat Pengukur Torsi, Daya Motor." },
  { "en": "Apa Itu Faktor Servis Motor? (Service Factor)", "id": "Kemampuan Beban Lebih Motor Berkelanjutan." },
  { "en": "Apa Arti Service Factor 1.15?", "id": "Motor Boleh Dibebani 115 Persen." },
  { "en": "Apa Itu Desain NEMA Motor?", "id": "Klasifikasi Karakteristik Torsi Motor (A,B,C,D)." },
  { "en": "Apa Karakteristik NEMA Desain B?", "id": "Torsi Normal, Arus Start Normal (Umum)." },
  { "en": "Apa Karakteristik NEMA Desain C?", "id": "Torsi Start Tinggi, Arus Start Normal." },
  { "en": "Apa Karakteristik NEMA Desain D?", "id": "Torsi Start Sangat Tinggi, Slip Tinggi." },
  { "en": "Apa Itu Tes Tahanan Belitan DC?", "id": "Mengukur Tahanan Lilitan Trafo, Motor." },
  { "en": "Apa Itu Tes Jembatan Wheatstone?", "id": "Metode Klasik Mengukur Tahanan Presisi." },
  { "en": "Apa Itu Tes Jembatan Kelvin?", "id": "Modifikasi Jembatan Wheatstone Tahanan Rendah." },
  { "en": "Apa Itu Koefisien Suhu Tahanan?", "id": "Perubahan Nilai Tahanan Akibat Suhu." },
  { "en": "Apa Itu Koreksi Suhu Tahanan?", "id": "Menyesuaikan Hasil Ukur Tahanan Ke Suhu Standar." },
  { "en": "Apa Itu Diverter Switch OLTC?", "id": "Saklar Pemindah Arus Saat Tap Changing." },
  { "en": "Apa Itu Tes Faktor Daya Ujung-Ke-Ujung? (Overall)", "id": "Tes Isolasi Trafo Termasuk Bushing." },
  { "en": "Apa Itu Tes Faktor Daya GST? (Grounded Specimen Test)", "id": "Tes Isolasi Lilitan Ke Ground." },
  { "en": "Apa Itu Tes Faktor Daya UST? (Ungrounded Specimen Test)", "id": "Tes Isolasi Antar Lilitan Trafo." },
  { "en": "Apa Itu Hot Collar Test?", "id": "Tes Faktor Daya Khusus Bushing Trafo." },
  { "en": "Apa Itu Excitation Current Test?", "id": "Tes Arus Eksitasi Trafo (Beban Nol)." },
  { "en": "Apa Itu Tes Frekuensi Rendah? (LF)", "id": "Tes Tahanan Isolasi Frekuensi Rendah." },
  { "en": "Apa Itu Tes Recovery Voltage Method? (RVM)", "id": "Tes Mengukur Degradasi Isolasi (Kelembaban)." },
  { "en": "Apa Itu Pembumian Titik Bintang Generator?", "id": "Pentanahan Titik Netral Stator Generator." },
  { "en": "Apa Proteksi Netral Generator?", "id": "Relay Gangguan Tanah, Relay Tegangan Lebih." },
  { "en": "Apa Itu Proteksi Arus Urutan Negatif?", "id": "Melindungi Rotor Generator Dari Pemanasan." },
  { "en": "Apa Itu Proteksi Kehilangan Medan? (Loss of Field)", "id": "Relay 40, Mendeteksi Hilang Eksitasi." },
  { "en": "Apa Akibat Hilang Eksitasi Generator?", "id": "Generator Menyerap Daya Reaktif, Overheat." },
  { "en": "Apa Itu Proteksi Daya Balik? (Reverse Power)", "id": "Mendeteksi Generator Berubah Menjadi Motor." },
  { "en": "Apa Itu Motoring Generator?", "id": "Kondisi Generator Berfungsi Sebagai Motor." },
  { "en": "Apa Itu Proteksi Over-Fluxing? (V/Hz)", "id": "Melindungi Trafo, Generator Dari Fluks Berlebih." },
  { "en": "Apa Penyebab Over-Fluxing?", "id": "Tegangan Terlalu Tinggi, Frekuensi Terlalu Rendah." },
  { "en": "Apa Itu Proteksi Out-of-Step?", "id": "Mendeteksi Generator Kehilangan Sinkronisasi." },
  { "en": "Apa Itu Hunting Generator?", "id": "Osilasi Daya, Kecepatan Generator Sekitar Sinkron." },
  { "en": "Apa Itu Peredam Amper? (Amortisseur Winding)", "id": "Lilitan Meredam Osilasi (Hunting) Generator." },
  { "en": "Dimana Lilitan Amortisseur Dipasang?", "id": "Pada Permukaan Kutub Rotor Salient Pole." },
  { "en": "Apa Itu Diagram Lingkaran Motor Induksi?", "id": "Grafik Menganalisis Performa Motor Induksi." },
  { "en": "Data Apa Dibutuhkan Diagram Lingkaran?", "id": "Tes Beban Nol, Tes Hubung Singkat." },
  { "en": "Apa Itu Tes Tahanan DC Stator?", "id": "Mengukur Tahanan Murni Lilitan Stator." },
  { "en": "Apa Itu Tes Pengeringan? (Dry Out)", "id": "Proses Mengeringkan Lilitan Motor Lembab." },
  { "en": "Bagaimana Metode Pengeringan Motor?", "id": "Pemanasan Eksternal, Injeksi Arus DC Rendah." },
  { "en": "Apa Itu Tes Impedansi Urutan Nol?", "id": "Mengukur Impedansi Gangguan Tanah Sistem." },
  { "en": "Apa Itu Saluran Transmisi?", "id": "Kawat Penghantar Penyalur Listrik Jarak Jauh." },
  { "en": "Apa Jenis Konduktor Transmisi?", "id": "ACSR, AAAC, AAC." },
  { "en": "Apa Kepanjangan ACSR? (Aluminium Conductor Steel Reinforced)", "id": "Konduktor Aluminium Diperkuat Inti Baja." },
  { "en": "Mengapa ACSR Banyak Digunakan?", "id": "Ringan, Kuat, Konduktivitas Cukup Baik." },
  { "en": "Apa Kepanjangan AAAC? (All Aluminium Alloy Conductor)", "id": "Konduktor Seluruhnya Paduan Aluminium." },
  { "en": "Apa Itu Efek Kulit? (Skin Effect)", "id": "Arus AC Mengalir Di Permukaan Konduktor." },
  { "en": "Apa Itu Efek Kedekatan? (Proximity Effect)", "id": "Distribusi Arus Akibat Konduktor Berdekatan." },
  { "en": "Apa Itu Parameter Saluran Transmisi?", "id": "Resistansi (R), Induktansi (L), Kapasitansi (C)." },
  { "en": "Apa Itu Transposisi Saluran Transmisi?", "id": "Pertukaran Posisi Kawat Fasa." },
  { "en": "Apa Tujuan Transposisi?", "id": "Menyeimbangkan Induktansi, Kapasitansi Ketiga Fasa." },
  { "en": "Apa Itu Saluran Transmisi Pendek?", "id": "Saluran Kurang 80 KM, Kapasitansi Diabaikan." },
  { "en": "Apa Itu Saluran Transmisi Menengah?", "id": "Saluran 80 KM Hingga 250 KM." },
  { "en": "Apa Itu Saluran Transmisi Panjang?", "id": "Saluran Lebih Dari 250 KM." },
  { "en": "Apa Model Saluran Menengah?", "id": "Model Nominal Pi (π), Model T." },
  { "en": "Apa Model Saluran Panjang?", "id": "Model Parameter Terdistribusi." },
  { "en": "Apa Itu Konstanta ABCD?", "id": "Parameter Matriks Saluran Transmisi." },
  { "en": "Apa Itu Tegangan Regulasi?", "id": "Perubahan Tegangan Saluran (Tanpa Beban, Beban Penuh)." },
  { "en": "Apa Itu Daya Karakteristik Saluran? (SIL)", "id": "Beban Ideal Saluran Transmisi." },
  { "en": "Apa Kepanjangan SIL? (Surge Impedance Loading)", "id": "Beban Impedansi Surja." },
  { "en": "Apa Itu Impedansi Surja?", "id": "Impedansi Karakteristik Saluran Transmisi." },
  { "en": "Apa Itu Gelombang Berjalan? (Traveling Wave)", "id": "Gelombang Surja Tegangan Akibat Petir." },
  { "en": "Apa Itu Matriks Admitansi Bus? (Y-Bus)", "id": "Matriks Representasi Jaringan Analisis Aliran Daya." },
  { "en": "Apa Itu Matriks Impedansi Bus? (Z-Bus)", "id": "Matriks Analisis Gangguan Hubung Singkat." },
  { "en": "Apa Itu Metode Newton-Raphson?", "id": "Metode Iteratif Menyelesaikan Aliran Daya." },
  { "en": "Apa Itu Metode Gauss-Seidel?", "id": "Metode Iteratif Lain Studi Aliran Daya." },
  { "en": "Apa Itu Slack Bus? (Swing Bus)", "id": "Bus Referensi Dalam Studi Aliran Daya." },
  { "en": "Apa Fungsi Slack Bus?", "id": "Menyeimbangkan Daya Sistem (Generator Referensi)." },
  { "en": "Apa Itu PV Bus? (Generator Bus)", "id": "Bus Diketahui Daya Aktif (P), Tegangan (V)." },
  { "en": "Apa Itu PQ Bus? (Load Bus)", "id": "Bus Diketahui Daya Aktif (P), Reaktif (Q)." },
  { "en": "Apa Itu Kontrol Frekuensi Beban? (LFC)", "id": "Mengatur Frekuensi Sistem Tetap Stabil." },
  { "en": "Apa Itu Kontrol Pembangkitan Otomatis? (AGC)", "id": "Kontrol Otomatis Output Generator (AGC)." },
  { "en": "Apa Kepanjangan AGC? (Automatic Generation Control)", "id": "Pengatur Pembangkitan Otomatis." },
  { "en": "Apa Itu ACE? (Area Control Error)", "id": "Kesalahan Kontrol Area (Frekuensi, Aliran Daya)." },
  { "en": "Apa Itu Cadangan Berputar? (Spinning Reserve)", "id": "Kapasitas Generator Sinkron Siap Naik Cepat." },
  { "en": "Apa Itu Cadangan Dingin? (Cold Reserve)", "id": "Kapasitas Pembangkit Siap Operasi (Beberapa Jam)." },
  { "en": "Apa Itu Black Start Capability?", "id": "Kemampuan Pembangkit Hidup Tanpa Listrik Eksternal." },
  { "en": "Pembangkit Apa Punya Kemampuan Black Start?", "id": "PLTA, PLTD, PLTG." },
  { "en": "Apa Itu Aliran Daya Optimal? (OPF)", "id": "Studi Aliran Daya Paling Ekonomis." },
  { "en": "Apa Itu Unit Commitment?", "id": "Penjadwalan Unit Pembangkit Beroperasi." },
  { "en": "Apa Itu Dispas Ekonomi? (Economic Dispatch)", "id": "Pembagian Beban Pembangkit Paling Ekonomis." },
  { "en": "Apa Itu Biaya Incremental? (Incremental Cost)", "id": "Biaya Tambahan Menghasilkan Satu MW Berikutnya." },
  { "en": "Apa Itu Tes Kinerja Motor?", "id": "Mengukur Efisiensi, Torsi, Arus Motor." },
  { "en": "Apa Itu Efisiensi Motor?", "id": "Perbandingan Daya Output Mekanis, Input Listrik." },
  { "en": "Apa Itu Kelas Efisiensi Motor? (IE)", "id": "Standar Efisiensi Motor (IE1, IE2, IE3, IE4)." },
  { "en": "Apa Itu Motor IE3? (Premium Efficiency)", "id": "Motor Dengan Tingkat Efisiensi Premium." },
  { "en": "Mengapa Motor Efisiensi Tinggi Penting?", "id": "Menghemat Biaya Energi Listrik Jangka Panjang." },
  { "en": "Apa Itu Rugi-Rugi Motor?", "id": "Rugi Tembaga, Rugi Inti, Rugi Gesekan." },
  { "en": "Apa Itu Rugi Tambahan? (Stray Load Loss)", "id": "Rugi-Rugi Lain Akibat Beban Motor." },
  { "en": "Apa Itu Tes Cincin Flux? (Ring Flux Test)", "id": "Tes Mendeteksi Kerusakan Inti Stator." },
  { "en": "Apa Itu EL-CID Test? (Electromagnetic Core Imperfection Detection)", "id": "Tes Mendeteksi Kerusakan Inti Stator." },
  { "en": "Apa Itu Sistem Kontrol Terdistribusi? (DCS)", "id": "Sistem Kontrol Proses Industri Terdistribusi." },
  { "en": "Apa Kepanjangan DCS? (Distributed Control System)", "id": "Sistem Kontrol Terdistribusi." },
  { "en": "Apa Beda PLC Dan DCS?", "id": "PLC Fokus Logika Cepat, DCS Kontrol Proses." },
  { "en": "Apa Itu Loop Kontrol?", "id": "Sistem Umpan Balik (Sensor, Kontroler, Aktuator)." },
  { "en": "Apa Itu Sensor Proximity?", "id": "Sensor Pendeteksi Objek Tanpa Sentuhan." },
  { "en": "Apa Jenis Sensor Proximity?", "id": "Induktif, Kapasitif, Ultrasonik, Optik." },
  { "en": "Apa Yang Dideteksi Proximity Induktif?", "id": "Mendeteksi Objek Logam." },
  { "en": "Apa Yang Dideteksi Proximity Kapasitif?", "id": "Mendeteksi Benda Padat, Cair." },
  { "en": "Apa Itu Sensor Fotolistrik?", "id": "Sensor Optik Mendeteksi Cahaya." },
  { "en": "Apa Jenis Sensor Fotolistrik?", "id": "Through-Beam, Retro-Reflective, Diffuse." },
  { "en": "Apa Itu Through-Beam Sensor?", "id": "Sensor Pemancar, Penerima Terpisah." },
  { "en": "Apa Itu Retro-Reflective Sensor?", "id": "Sensor Menggunakan Reflektor (Mata Kucing)." },
  { "en": "Apa Itu Diffuse Sensor?", "id": "Sensor Memantulkan Cahaya Dari Objek." },
  { "en": "Apa Itu Limit Switch?", "id": "Saklar Mekanis Pembatas Gerakan Fisik." },
  { "en": "Apa Itu Kontrol PID? (Proportional Integral Derivative)", "id": "Algoritma Kontrol Umpan Balik Loop Tertutup." },
  { "en": "Apa Fungsi Kontrol Proporsional? (P)", "id": "Mengurangi Error Proporsional Nilai Error." },
  { "en": "Apa Fungsi Kontrol Integral? (I)", "id": "Menghilangkan Error Kondisi Steady State." },
  { "en": "Apa Fungsi Kontrol Derivatif? (D)", "id": "Mempercepat Respon, Meredam Osilasi." },
  { "en": "Apa Itu Setpoint?", "id": "Nilai Target Yang Diinginkan Sistem Kontrol." },
  { "en": "Apa Itu Process Variable? (PV)", "id": "Nilai Aktual Yang Diukur Sensor." },
  { "en": "Apa Itu Error?", "id": "Perbedaan Antara Setpoint Dan Process Variable." },
  { "en": "Apa Itu Tuning PID?", "id": "Proses Penyesuaian Parameter Kp, Ki, Kd." },
  { "en": "Apa Itu Metode Tuning Ziegler-Nichols?", "id": "Metode Klasik Penyetelan Parameter PID." },
  { "en": "Apa Itu Kontrol On-Off?", "id": "Sistem Kontrol Paling Sederhana (Hidup/Mati)." },
  { "en": "Apa Itu Histeresis Kontrol On-Off?", "id": "Rentang Jarak Antara Titik On, Off." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Kontinu Mewakili Besaran Fisik." },
  { "en": "Apa Standar Sinyal Arus Analog?", "id": "4 Sampai 20 Mili Ampere (mA)." },
  { "en": "Apa Keuntungan Sinyal 4-20 MA?", "id": "Tahan Noise, Bisa Mendeteksi Kabel Putus." },
  { "en": "Apa Standar Sinyal Tegangan Analog?", "id": "0 Sampai 10 Volt DC." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Diskrit (Hanya Logika 1 Atau 0)." },
  { "en": "Apa Itu ADC? (Analog to Digital Converter)", "id": "Mengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC? (Digital to Analog Converter)", "id": "Mengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Resolusi ADC?", "id": "Jumlah Bit Hasil Konversi Digital." },
  { "en": "Apa Itu Ladder Logic?", "id": "Bahasa Pemrograman Grafis Untuk PLC." },
  { "en": "Apa Itu Rung Pada Ladder Logic?", "id": "Garis Horizontal Program Logika PLC." },
  { "en": "Apa Itu Kontak Normally Open? (NO)", "id": "Kontak Terbuka Saat Tidak Aktif." },
  { "en": "Apa Itu Kontak Normally Closed? (NC)", "id": "Kontak Tertutup Saat Tidak Aktif." },
  { "en": "Apa Itu Fungsi Latching (Pengunci)?", "id": "Menahan Output Tetap Aktif (Self-Holding)." },
  { "en": "Apa Itu Timer On-Delay?", "id": "Menunda Output Aktif Setelah Input Aktif." },
  { "en": "Apa Itu Timer Off-Delay?", "id": "Menahan Output Aktif Setelah Input Mati." },
  { "en": "Apa Itu Counter PLC?", "id": "Pencacah (Menghitung) Jumlah Input Pulsa." },
  { "en": "Apa Itu Counter Up? (CTU)", "id": "Menghitung Maju (0, 1, 2, 3)." },
  { "en": "Apa Itu Counter Down? (CTD)", "id": "Menghitung Mundur (3, 2, 1, 0)." },
  { "en": "Apa Itu Scan Cycle PLC?", "id": "Siklus PLC (Input, Program, Output)." },
  { "en": "Apa Itu Watchdog Timer?", "id": "Timer Memantau Waktu Scan Cycle PLC." },
  { "en": "Apa Itu Modul Input Digital?", "id": "Membaca Sinyal Digital (Saklar, Sensor)." },
  { "en": "Apa Itu Modul Output Digital?", "id": "Mengaktifkan Output Digital (Relay, Lampu)." },
  { "en": "Apa Itu Modul Input Analog?", "id": "Membaca Sinyal Analog (4-20 MA)." },
  { "en": "Apa Itu Modul Output Analog?", "id": "Mengirim Sinyal Analog (Ke VFD)." },
  { "en": "Apa Itu Fieldbus?", "id": "Jaringan Komunikasi Digital Industri." },
  { "en": "Apa Contoh Fieldbus?", "id": "Profibus, Modbus, DeviceNet." },
  { "en": "Apa Itu Modbus?", "id": "Protokol Komunikasi Serial Industri (Umum)." },
  { "en": "Apa Itu Modbus RTU?", "id": "Modbus Menggunakan Jaringan Serial (RS485)." },
  { "en": "Apa Itu Modbus TCP/IP?", "id": "Modbus Menggunakan Jaringan Ethernet." },
  { "en": "Apa Itu RS-232?", "id": "Standar Komunikasi Serial Jarak Pendek." },
  { "en": "Apa Itu RS-485?", "id": "Standar Komunikasi Serial Jarak Jauh (Multi-Drop)." },
  { "en": "Apa Itu Ethernet Industri?", "id": "Ethernet Didesain Lingkungan Industri." },
  { "en": "Apa Itu Profinet?", "id": "Standar Ethernet Industri (Siemens)." },
  { "en": "Apa Itu OPC? (OLE for Process Control)", "id": "Standar Interoperabilitas Perangkat Lunak Industri." },
  { "en": "Apa Itu Penggerak Pneumatik?", "id": "Aktuator Menggunakan Tenaga Udara Bertekanan." },
  { "en": "Apa Itu Silinder Pneumatik?", "id": "Aktuator Pneumatik Gerakan Lurus." },
  { "en": "Apa Itu Katup Solenoida? (Solenoid Valve)", "id": "Katup Kontrol Aliran Udara/Cairan Listrik." },
  { "en": "Apa Itu Penggerak Hidrolik?", "id": "Aktuator Menggunakan Tenaga Cairan (Oli)." },
  { "en": "Apa Keuntungan Hidrolik?", "id": "Menghasilkan Gaya Sangat Besar." },
  { "en": "Apa Itu Kompresor Udara?", "id": "Mesin Penghasil Udara Bertekanan." },
  { "en": "Apa Itu Air Service Unit?", "id": "Unit (Filter, Regulator) Sistem Pneumatik." },
  { "en": "Apa Itu Sistem Tenaga Fluida?", "id": "Sistem Tenaga Pneumatik, Hidrolik." },
  { "en": "Apa Itu Diagram P&ID? (Piping and Instrumentation Diagram)", "id": "Diagram Alur Proses, Instrumentasi Pabrik." },
  { "en": "Apa Itu Transmiter?", "id": "Mengubah Sinyal Sensor Jadi Sinyal Standar." },
  { "en": "Apa Itu Transmiter Tekanan?", "id": "Mengukur Tekanan, Mengirim Sinyal 4-20 MA." },
  { "en": "Apa Itu Transmiter Suhu?", "id": "Mengukur Suhu, Mengirim Sinyal 4-20 MA." },
  { "en": "Apa Itu Sensor RTD? (Resistance Temperature Detector)", "id": "Sensor Suhu Berbasis Perubahan Tahanan." },
  { "en": "Apa Bahan RTD Paling Umum?", "id": "Platinum (Pt100)." },
  { "en": "Apa Arti Pt100?", "id": "Sensor Platinum Tahanan 100 Ohm (0°C)." },
  { "en": "Apa Itu Termokopel? (Thermocouple)", "id": "Sensor Suhu Berbasis Efek Seebeck." },
  { "en": "Apa Prinsip Efek Seebeck?", "id": "Dua Logam Beda Hasilkan Tegangan (Suhu)." },
  { "en": "Apa Jenis Termokopel Umum?", "id": "Tipe K, Tipe J, Tipe S." },
  { "en": "Apa Itu Sensor Level?", "id": "Sensor Mengukur Ketinggian Cairan, Padatan." },
  { "en": "Apa Jenis Sensor Level?", "id": "Pelampung, Ultrasonik, Radar, Tekanan." },
  { "en": "Apa Itu Sensor Aliran? (Flow Meter)", "id": "Sensor Mengukur Laju Aliran Fluida." },
  { "en": "Apa Jenis Flow Meter?", "id": "Turbin, Magnetik, Ultrasonik, Orifice." },
  { "en": "Apa Itu Flow Meter Magnetik?", "id": "Mengukur Aliran Fluida Konduktif (Air)." },
  { "en": "Apa Itu Load Cell?", "id": "Sensor Mengukur Berat Atau Gaya (Timbangan)." },
  { "en": "Apa Prinsip Load Cell?", "id": "Menggunakan Strain Gauge." },
  { "en": "Apa Itu Strain Gauge?", "id": "Sensor Mengukur Regangan Mekanis." },
  { "en": "Apa Itu Katup Kontrol? (Control Valve)", "id": "Katup Mengatur Laju Aliran (Aktuator)." },
  { "en": "Apa Itu Positioner Katup?", "id": "Mengatur Posisi Bukaan Katup Kontrol." },
  { "en": "Apa Itu Sinyal I/P? (Current to Pressure)", "id": "Konverter Arus (4-20 MA) Ke Tekanan (Pneumatik)." },
  { "en": "Apa Itu Robot Industri?", "id": "Lengan Mekanis Otomatis Terprogram." },
  { "en": "Apa Itu Derajat Kebebasan? (DOF)", "id": "Jumlah Sumbu Gerak Robot Industri." },
  { "en": "Apa Itu End Effector?", "id": "Perkakas Di Ujung Lengan Robot (Gripper)." },
  { "en": "Apa Itu Teach Pendant?", "id": "Perangkat Genggam Memprogram Robot." },
  { "en": "Apa Itu Sistem Visi? (Machine Vision)", "id": "Sistem Kamera Inspeksi Otomatis." },
  { "en": "Apa Itu Fire and Gas System? (FGS)", "id": "Sistem Deteksi Api, Kebocoran Gas." },
  { "en": "Apa Itu Detektor Gas Mudah Terbakar?", "id": "Sensor Mendeteksi Gas Hidrokarbon." },
  { "en": "Apa Itu Detektor Gas Beracun?", "id": "Sensor Mendeteksi Gas H2S, CO." },
  { "en": "Apa Itu Detektor Api? (Flame Detector)", "id": "Sensor Mendeteksi Radiasi Api (UV/IR)." },
  { "en": "Apa Itu Sistem ESD? (Emergency Shutdown)", "id": "Sistem Mematikan Pabrik Saat Darurat." },
  { "en": "Apa Itu Katup ESD?", "id": "Katup Menutup Otomatis Saat ESD." },
  { "en": "Apa Itu HIPPS? (High Integrity Pressure Protection System)", "id": "Sistem Proteksi Tekanan Berlebih Kritis." },
  { "en": "Apa Itu SIL? (Safety Integrity Level)", "id": "Tingkat Integritas Keselamatan Sistem." },
  { "en": "Apa Tingkat SIL Tertinggi?", "id": "SIL 4 (Paling Aman, Paling Kritis)." },
  { "en": "Apa Itu Bukti Tes? (Proof Test)", "id": "Tes Periodik Memvalidasi Fungsi Keselamatan." },
  { "en": "Apa Itu Probabilitas Kegagalan? (PFD)", "id": "Peluang Sistem Keselamatan Gagal Berfungsi." },
  { "en": "Apa Itu Redundansi?", "id": "Penggunaan Komponen Cadangan (Backup)." },
  { "en": "Apa Itu Redundansi 1oo2? (One out of Two)", "id": "Satu Dari Dua Komponen Cukup." },
  { "en": "Apa Itu Redundansi 2oo3? (Two out of Three)", "id": "Dua Dari Tiga Komponen Harus Bekerja." },
  { "en": "Apa Itu Common Cause Failure?", "id": "Kegagalan Beberapa Komponen Akibat Sebab Sama." },
  { "en": "Apa Itu Analisis LOPA? (Layer of Protection Analysis)", "id": "Metode Analisis Lapisan Proteksi Risiko." },
  { "en": "Apa Itu Lapisan Proteksi Independen? (IPL)", "id": "Lapisan Pengaman Independen (Alarm, SIS, PSV)." },
  { "en": "Apa Itu PSV? (Pressure Safety Valve)", "id": "Katup Pelepas Tekanan Berlebih Mekanis." },
  { "en": "Apa Itu Sensor pH?", "id": "Mengukur Tingkat Keasaman, Kebasaan Cairan." },
  { "en": "Apa Itu Sensor Konduktivitas?", "id": "Mengukur Kemampuan Cairan Menghantarkan Listrik." },
  { "en": "Apa Itu Panel Kontrol Lokal? (LCP)", "id": "Panel Kontrol Di Lapangan (Dekat Mesin)." },
  { "en": "Apa Itu Junction Box?", "id": "Kotak Sambungan Terminal Kabel." },
  { "en": "Apa Itu Gland Kabel? (Cable Gland)", "id": "Penyekat Kabel Masuk Ke Panel." },
  { "en": "Apa Fungsi Gland Kabel?", "id": "Melindungi Panel Dari Debu, Air." },
  { "en": "Apa Itu Gland Kabel Ex-Proof?", "id": "Gland Kabel Khusus Area Berbahaya." },
  { "en": "Apa Itu Sertifikasi ATEX?", "id": "Standar Eropa Peralatan Area Berbahaya." },
  { "en": "Apa Itu Sertifikasi IECEx?", "id": "Standar Internasional Peralatan Area Berbahaya." },
  { "en": "Apa Itu Zona 0? (Hazardous Area)", "id": "Area Gas Mudah Terbakar Selalu Ada." },
  { "en": "Apa Itu Zona 1? (Hazardous Area)", "id": "Area Gas Mudah Terbakar Kadang Muncul." },
  { "en": "Apa Itu Zona 2? (Hazardous Area)", "id": "Area Gas Mudah Terbakar Jarang Muncul." },
  { "en": "Apa Itu Zona 20, 21, 22?", "id": "Klasifikasi Zona Area Debu Mudah Terbakar." },
  { "en": "Apa Itu Purging Panel?", "id": "Memberi Tekanan Positif Panel (Area Ex)." },
  { "en": "Apa Tujuan Purging Panel?", "id": "Mencegah Gas Berbahaya Masuk Panel." },
  { "en": "Apa Itu Gas Blanket?", "id": "Selimut Gas Inert (Nitrogen) Tangki." },
  { "en": "Apa Itu Arus Searah? (DC)", "id": "Arus Listrik Mengalir Satu Arah." },
  { "en": "Apa Itu Arus Bolak-Balik? (AC)", "id": "Arus Listrik Arahnya Berubah Periodik." },
  { "en": "Siapa Penemu Arus DC (Komersial)?", "id": "Thomas Alva Edison." },
  { "en": "Siapa Pengembang Arus AC (Komersial)?", "id": "Nikola Tesla, George Westinghouse." },
  { "en": "Apa Itu Perang Arus? (War of Currents)", "id": "Persaingan Sistem AC Lawan DC." },
  { "en": "Mengapa AC Memenangkan Perang Arus?", "id": "Mudah Ditransmisikan Jarak Jauh (Trafo)." },
  { "en": "Apa Itu Generator Van de Graaff?", "id": "Generator Menghasilkan Tegangan DC Statis Tinggi." },
  { "en": "Apa Itu Tes Kontinuitas?", "id": "Memeriksa Kabel Terhubung Sempurna (Tidak Putus)." },
  { "en": "Alat Apa Untuk Tes Kontinuitas?", "id": "Multimeter (Mode Ohm Atau Buzzer)." },
  { "en": "Apa Itu Tes Resistansi Rendah?", "id": "Mengukur Tahanan Sambungan (Micro-Ohmmeter)." },
  { "en": "Apa Itu Tes Winding?", "id": "Tes Tahanan Lilitan Motor, Trafo." },
  { "en": "Apa Itu Tes Hubungan Singkat Lilitan?", "id": "Mencari Korsleting Antar Lilitan Motor." },
  { "en": "Apa Itu Growler?", "id": "Alat Tes Hubung Singkat Rotor, Stator." },
  { "en": "Apa Itu Osilator?", "id": "Rangkaian Elektronik Pembangkit Sinyal Periodik." },
  { "en": "Apa Itu Filter Elektronik?", "id": "Rangkaian Meloloskan Frekuensi Tertentu." },
  { "en": "Apa Itu Filter Lolos Bawah? (LPF)", "id": "Meloloskan Frekuensi Rendah." },
  { "en": "Apa Itu Filter Lolos Atas? (HPF)", "id": "Meloloskan Frekuensi Tinggi." },
  { "en": "Apa Itu Filter Lolos Pita? (BPF)", "id": "Meloloskan Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Filter Stop Pita? (BSF)", "id": "Menolak Rentang Frekuensi Tertentu (Notch)." },
  { "en": "Apa Itu Rangkaian Resonator?", "id": "Rangkaian RLC Pembangkit Frekuensi Tertentu." },
  { "en": "Apa Itu Amplifier? (Penguat)", "id": "Rangkaian Menaikkan Amplitudo Sinyal." },
  { "en": "Apa Itu Gain Amplifier?", "id": "Faktor Penguatan Sinyal (Output / Input)." },
  { "en": "Apa Itu Satuan Gain?", "id": "Tanpa Satuan, Atau Decibel (dB)." },
  { "en": "Apa Itu Op-Amp? (Operational Amplifier)", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Itu Rangkaian Inverting Amplifier?", "id": "Penguat Membalik Fasa Sinyal Output." },
  { "en": "Apa Itu Rangkaian Non-Inverting Amplifier?", "id": "Penguat Tidak Membalik Fasa Sinyal." },
  { "en": "Apa Itu Rangkaian Comparator?", "id": "Op-Amp Pembanding Dua Sinyal Tegangan." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Nuklir? (PLTN)", "id": "Pembangkit Listrik Energi Reaksi Nuklir." },
  { "en": "Apa Itu Reaksi Fisi?", "id": "Reaksi Pembelahan Inti Atom (PLTN)." },
  { "en": "Apa Itu Reaksi Fusi?", "id": "Reaksi Penggabungan Inti Atom (Matahari)." },
  { "en": "Apa Bahan Bakar PLTN?", "id": "Uranium Atau Plutonium." },
  { "en": "Apa Itu Reaktor Nuklir?", "id": "Tempat Reaksi Fisi Terkendali." },
  { "en": "Apa Fungsi Batang Kendali? (Control Rods)", "id": "Menyerap Neutron, Mengendalikan Reaksi." },
  { "en": "Apa Itu Moderator Reaktor?", "id": "Memperlambat Neutron (Air, Grafit)." },
  { "en": "Apa Itu Pendingin Reaktor?", "id": "Mengambil Panas Reaktor (Air, Gas)." },
  { "en": "Apa Itu PWR? (Pressurized Water Reactor)", "id": "Reaktor Air Tekan (Jenis PLTN)." },
  { "en": "Apa Itu BWR? (Boiling Water Reactor)", "id": "Reaktor Air Didih (Jenis PLTN)." },
  { "en": "Apa Itu Limbah Nuklir?", "id": "Bahan Radioaktif Sisa Operasi PLTN." },
  { "en": "Apa Itu Radiasi?", "id": "Pancaran Energi Partikel (Alpha, Beta, Gamma)." },
  { "en": "Apa Satuan Dosis Radiasi?", "id": "Sievert (Sv)." },
  { "en": "Apa Itu Kontaminasi Radioaktif?", "id": "Paparan Bahan Radioaktif Tidak Diinginkan." },
  { "en": "Apa Itu Dekontaminasi?", "id": "Proses Menghilangkan Kontaminasi Radioaktif." },
  { "en": "Apa Itu Motor DC Seri?", "id": "Lilitan Medan, Jangkar Terhubung Seri." },
  { "en": "Apa Karakteristik Motor DC Seri?", "id": "Torsi Awal Sangat Tinggi, Kecepatan Bervariasi." },
  { "en": "Dimana Motor DC Seri Digunakan?", "id": "Starter Mobil, Kereta Listrik (Traksi)." },
  { "en": "Apa Itu Motor DC Shunt?", "id": "Lilitan Medan, Jangkar Terhubung Paralel." },
  { "en": "Apa Karakteristik Motor DC Shunt?", "id": "Kecepatan Cenderung Konstan." },
  { "en": "Dimana Motor DC Shunt Digunakan?", "id": "Mesin Bubut, Kipas Angin DC." },
  { "en": "Apa Itu Motor DC Kompon?", "id": "Gabungan Lilitan Seri, Shunt." },
  { "en": "Apa Itu Motor Kompon Kumulatif?", "id": "Fluks Medan Seri, Shunt Saling Menguatkan." },
  { "en": "Apa Itu Motor Kompon Diferensial?", "id": "Fluks Medan Seri, Shunt Saling Melemahkan." },
  { "en": "Apa Itu Kontrol Kecepatan Motor DC?", "id": "Mengatur Tegangan Jangkar, Arus Medan." },
  { "en": "Apa Itu Ward Leonard System?", "id": "Metode Kontrol Kecepatan Motor DC." },
  { "en": "Apa Itu Kontrol Thyristor Motor DC?", "id": "Mengatur Tegangan DC Menggunakan SCR." },
  { "en": "Apa Itu Chopper DC?", "id": "Konverter DC-DC (Pengatur Kecepatan Motor DC)." },
  { "en": "Apa Itu Reaksi Jangkar?", "id": "Pengaruh Medan Magnet Jangkar (Rotor)." },
  { "en": "Apa Akibat Reaksi Jangkar?", "id": "Melemahkan Fluks Utama, Menggeser Netral." },
  { "en": "Apa Itu Kutub Bantu? (Interpole)", "id": "Kutub Kecil Mengurangi Efek Reaksi Jangkar." },
  { "en": "Apa Fungsi Lilitan Kompensasi?", "id": "Menetralkan Reaksi Jangkar Di Bawah Kutub." },
  { "en": "Apa Itu Percikan Api Sikat? (Sparking)", "id": "Bunga Api Komutator Akibat Komutasi Buruk." },
  { "en": "Apa Itu Bidang Netral Geometris? (GNA)", "id": "Garis Tengah Antara Kutub Utama." },
  { "en": "Apa Itu Bidang Netral Magnetis? (MNA)", "id": "Titik Tanpa Induksi Tegangan Rotor." },
  { "en": "Apa Itu Komutasi?", "id": "Proses Pembalikan Arah Arus Lilitan Jangkar." },
  { "en": "Apa Itu Mesin Listrik Fraksional?", "id": "Motor Listrik Daya Rendah (Dibawah 1 HP)." },
  { "en": "Apa Itu Konversi Energi?", "id": "Perubahan Satu Bentuk Energi Bentuk Lain." },
  { "en": "Apa Itu Efek Termoelektrik?", "id": "Fenomena Listrik Akibat Perbedaan Suhu." },
  { "en": "Apa Itu Efek Seebeck?", "id": "Perbedaan Suhu Hasilkan Tegangan (Termokopel)." },
  { "en": "Apa Itu Efek Peltier?", "id": "Aliran Arus Hasilkan Perbedaan Suhu (Pendingin)." },
  { "en": "Apa Itu Efek Thomson?", "id": "Arus, Gradien Suhu Hasilkan Panas/Dingin." },
  { "en": "Apa Itu Sel Peltier?", "id": "Komponen Pendingin Elektronik Efek Peltier." },
  { "en": "Apa Itu Efek Piezoelektrik?", "id": "Bahan Hasilkan Listrik Diberi Tekanan Mekanis." },
  { "en": "Dimana Efek Piezoelektrik Digunakan?", "id": "Sensor Tekanan, Pemantik Api Gas." },
  { "en": "Apa Itu Efek Fotovoltaik? (Photovoltaic)", "id": "Mengubah Cahaya Matahari Menjadi Listrik." },
  { "en": "Apa Bahan Dasar Panel Surya?", "id": "Umumnya Silikon (Monokristalin, Polikristalin)." },
  { "en": "Apa Itu Sel Surya? (Solar Cell)", "id": "Unit Dasar Pembangkit Listrik Panel Surya." },
  { "en": "Apa Itu Modul Surya?", "id": "Gabungan Sel Surya (Panel Surya)." },
  { "en": "Apa Itu Array Surya?", "id": "Gabungan Beberapa Modul Surya." },
  { "en": "Apa Itu Sistem PLTS On-Grid?", "id": "Sistem Terhubung Langsung Jaringan PLN." },
  { "en": "Apa Itu Sistem PLTS Off-Grid?", "id": "Sistem Mandiri (Menggunakan Baterai)." },
  { "en": "Apa Itu Sistem PLTS Hibrida?", "id": "Gabungan On-Grid, Off-Grid (PLN, Baterai)." },
  { "en": "Apa Fungsi Inverter PLTS?", "id": "Mengubah Arus DC Panel Menjadi AC." },
  { "en": "Apa Itu Inverter Grid-Tie?", "id": "Inverter Khusus Sistem PLTS On-Grid." },
  { "en": "Apa Itu Inverter Off-Grid?", "id": "Inverter Mandiri (Membutuhkan Baterai)." },
  { "en": "Apa Itu Inverter Hibrida?", "id": "Inverter Mengatur PLN, Baterai, Panel Surya." },
  { "en": "Apa Itu MPPT? (Maximum Power Point Tracking)", "id": "Teknologi Optimasi Daya Panel Surya." },
  { "en": "Apa Fungsi MPPT?", "id": "Mencari Titik Daya Maksimum Panel Surya." },
  { "en": "Apa Itu Solar Charge Controller? (SCC)", "id": "Mengatur Pengisian Baterai Dari Panel Surya." },
  { "en": "Apa Jenis SCC?", "id": "PWM (Pulse Width Modulation), MPPT." },
  { "en": "Apa Kelebihan SCC Tipe MPPT?", "id": "Lebih Efisien Daripada Tipe PWM." },
  { "en": "Apa Itu Net-Metering? (PLTS On-Grid)", "id": "Sistem Ekspor-Impor Listrik Dengan PLN." },
  { "en": "Apa Itu Standar Radiasi Matahari?", "id": "1000 Watt Per Meter Persegi (W/m²)." },
  { "en": "Apa Itu Peak Sun Hour? (PSH)", "id": "Jam Ekivalen Radiasi Puncak Matahari." },
  { "en": "Apa Itu Shading Panel Surya?", "id": "Bayangan Menutupi Sebagian Panel Surya." },
  { "en": "Apa Akibat Shading Panel Surya?", "id": "Menurunkan Produksi Listrik Secara Drastis." },
  { "en": "Apa Itu Dioda Bypass?", "id": "Dioda Mengatasi Efek Shading Panel Surya." },
  { "en": "Apa Itu Hotspot Panel Surya?", "id": "Titik Panas Berlebih Panel Akibat Shading." },
  { "en": "Apa Itu Degradasi Panel Surya?", "id": "Penurunan Kinerja Panel Surya Seiring Waktu." },
  { "en": "Berapa Rata-Rata Umur Panel Surya?", "id": "Sekitar 25 Hingga 30 Tahun." },
  { "en": "Apa Itu Efisiensi Panel Surya?", "id": "Persentase Energi Cahaya Menjadi Listrik." },
  { "en": "Apa Itu Tegangan Rangkaian Terbuka? (Voc)", "id": "Tegangan Maksimum Panel Surya (Tanpa Beban)." },
  { "en": "Apa Itu Arus Hubung Singkat? (Isc)", "id": "Arus Maksimum Panel Surya (Hubung Singkat)." },
  { "en": "Apa Itu Tegangan Daya Maksimum? (Vmp)", "id": "Tegangan Saat Panel Hasilkan Daya Maksimum." },
  { "en": "Apa Itu Arus Daya Maksimum? (Imp)", "id": "Arus Saat Panel Hasilkan Daya Maksimum." },
  { "en": "Apa Itu Kurva I-V Panel Surya?", "id": "Grafik Karakteristik Arus, Tegangan Panel." },
  { "en": "Bagaimana Pengaruh Suhu Panel Surya?", "id": "Suhu Naik, Tegangan Turun, Efisiensi Turun." },
  { "en": "Apa Itu Koefisien Suhu Panel?", "id": "Ukuran Penurunan Daya Akibat Kenaikan Suhu." },
  { "en": "Apa Itu Sistem Pemasangan Panel Surya?", "id": "Struktur Penyangga Panel (Rooftop, Ground)." },
  { "en": "Apa Itu Kabel Solar? (PV Cable)", "id": "Kabel Khusus Instalasi PLTS (Tahan UV)." },
  { "en": "Apa Itu Konektor MC4?", "id": "Konektor Standar Sambungan Antar Panel Surya." },
  { "en": "Apa Itu Combiner Box DC?", "id": "Panel Penggabung Jalur String Panel Surya." },
  { "en": "Apa Itu String Panel Surya?", "id": "Beberapa Panel Surya Dihubungkan Seri." },
  { "en": "Apa Itu Disconnect Switch DC?", "id": "Saklar Pemutus Arus DC Panel Surya." },
  { "en": "Apa Itu Proteksi Surge DC?", "id": "Arrester Pelindung Surja Petir Sisi DC." },
  { "en": "Apa Itu Grounding PLTS?", "id": "Pentanahan Rangka Panel, Sistem Listrik." },
  { "en": "Apa Itu Monitoring System PLTS?", "id": "Sistem Pemantauan Produksi Listrik PLTS." },
  { "en": "Apa Itu Datalogger?", "id": "Perangkat Perekam Data Produksi PLTS." },
  { "en": "Apa Itu Irradiance Meter?", "id": "Sensor Pengukur Intensitas Radiasi Matahari." },
  { "en": "Apa Itu Sel Monokristalin? (Monocrystalline)", "id": "Efisiensi Tinggi, Warna Hitam Seragam." },
  { "en": "Apa Itu Sel Polikristalin? (Polycrystalline)", "id": "Efisiensi Sedang, Warna Biru Bercak." },
  { "en": "Apa Itu Sel Thin Film?", "id": "Panel Surya Lapisan Tipis, Fleksibel." },
  { "en": "Apa Itu Sistem Pembersihan Panel?", "id": "Membersihkan Debu, Kotoran Permukaan Panel." },
  { "en": "Seberapa Sering Panel Dibersihkan?", "id": "Tergantung Kondisi Lingkungan (Debu, Hujan)." },
  { "en": "Apa Itu Anti-Islanding Protection?", "id": "Proteksi Inverter On-Grid (Mati Saat PLN Padam)." },
  { "en": "Mengapa Anti-Islanding Penting?", "id": "Keselamatan Pekerja Perbaikan Jaringan PLN." },
  { "en": "Apa Itu Microinverter?", "id": "Inverter Kecil Dipasang Per Satu Panel." },
  { "en": "Apa Keuntungan Microinverter?", "id": "Optimasi Per Panel, Tahan Shading." },
  { "en": "Apa Itu Power Optimizer?", "id": "Optimasi DC-DC Per Panel (Gabung Inverter String)." },
  { "en": "Apa Itu Kapasitas PLTS (WP)?", "id": "Satuan Daya Panel Surya (Watt Peak)." },
  { "en": "Apa Itu Rasio Kinerja? (Performance Ratio)", "id": "Ukuran Kinerja Aktual Sistem PLTS." },
  { "en": "Apa Itu Studi Kelayakan PLTS?", "id": "Analisis Potensi, Desain, Ekonomi PLTS." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Bayu? (PLTB)", "id": "Pembangkit Listrik Menggunakan Tenaga Angin." },
  { "en": "Apa Itu Turbin Angin?", "id": "Kincir Angin Pembangkit Listrik." },
  { "en": "Apa Jenis Turbin Angin?", "id": "Sumbu Horizontal (HAWT), Sumbu Vertikal (VAWT)." },
  { "en": "Apa Itu HAWT? (Horizontal Axis Wind Turbine)", "id": "Turbin Sumbu Horizontal (Paling Umum)." },
  { "en": "Apa Itu VAWT? (Vertical Axis Wind Turbine)", "id": "Turbin Sumbu Vertikal." },
  { "en": "Apa Komponen Utama HAWT?", "id": "Bilah (Blade), Nacelle, Tower." },
  { "en": "Apa Itu Nacelle?", "id": "Rumah Generator, Gearbox Di Atas Tower." },
  { "en": "Apa Itu Gearbox Turbin Angin?", "id": "Menaikkan Putaran Rendah Bilah Ke Generator." },
  { "en": "Apa Itu Turbin Angin Direct Drive?", "id": "Generator Putaran Rendah (Tanpa Gearbox)." },
  { "en": "Apa Itu Kontrol Pitch?", "id": "Pengaturan Sudut Bilah Turbin Angin." },
  { "en": "Apa Tujuan Kontrol Pitch?", "id": "Mengatur Kecepatan Putar, Daya Output." },
  { "en": "Apa Itu Kontrol Yaw?", "id": "Pengaturan Arah Nacelle Menghadap Angin." },
  { "en": "Apa Itu Wind Vane?", "id": "Alat Pengukur Arah Angin." },
  { "en": "Apa Itu Cut-In Speed?", "id": "Kecepatan Angin Minimum Turbin Mulai Berputar." },
  { "en": "Apa Itu Rated Speed?", "id": "Kecepatan Angin Turbin Hasilkan Daya Maksimum." },
  { "en": "Apa Itu Cut-Out Speed?", "id": "Kecepatan Angin Maksimum (Turbin Berhenti)." },
  { "en": "Mengapa Ada Cut-Out Speed?", "id": "Melindungi Turbin Dari Kerusakan Badai." },
  { "en": "Apa Itu Generator Turbin Angin?", "id": "Umumnya Generator Induksi, Generator Sinkron (PMG)." },
  { "en": "Apa Itu DFIG? (Doubly Fed Induction Generator)", "id": "Generator Induksi Umpan Ganda (Umum PLTB)." },
  { "en": "Apa Keuntungan DFIG?", "id": "Kontrol Daya Reaktif, Kecepatan Variabel." },
  { "en": "Apa Itu Wind Farm? (Kebun Angin)", "id": "Area Kumpulan Banyak Turbin Angin." },
  { "en": "Apa Itu Offshore Wind Farm?", "id": "Kebun Angin Berlokasi Di Lepas Pantai." },
  { "en": "Apa Keuntungan Offshore Wind Farm?", "id": "Angin Lebih Kuat, Stabil, Lahan Luas." },
  { "en": "Apa Tantangan Offshore Wind Farm?", "id": "Biaya Instalasi Mahal, Korosi Air Laut." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Air? (PLTA)", "id": "Pembangkit Listrik Menggunakan Aliran Air." },
  { "en": "Apa Itu Bendungan? (Dam)", "id": "Struktur Penahan Air Waduk." },
  { "en": "Apa Itu Waduk? (Reservoir)", "id": "Danau Buatan Penyimpan Air PLTA." },
  { "en": "Apa Itu Pipa Pesat? (Penstock)", "id": "Pipa Penyalur Air Tekanan Tinggi Turbin." },
  { "en": "Apa Itu Turbin Air?", "id": "Mengubah Energi Potensial Air Jadi Mekanis." },
  { "en": "Apa Jenis Turbin Air Utama?", "id": "Pelton, Francis, Kaplan." },
  { "en": "Kapan Turbin Pelton Digunakan?", "id": "Head (Tinggi Jatuh) Sangat Tinggi, Aliran Rendah." },
  { "en": "Kapan Turbin Francis Digunakan?", "id": "Head (Tinggi Jatuh) Menengah." },
  { "en": "Kapan Turbin Kaplan Digunakan?", "id": "Head (Tinggi Jatuh) Rendah, Aliran Sangat Tinggi." },
  { "en": "Apa Itu Head PLTA?", "id": "Beda Tinggi Vertikal Waduk, Turbin." },
  { "en": "Apa Itu Pembangkit Listrik Mikrohidro? (PLTMH)", "id": "PLTA Skala Kecil (Di Bawah 1 MW)." },
  { "en": "Apa Keuntungan PLTMH?", "id": "Cocok Daerah Terpencil, Ramah Lingkungan." },
  { "en": "Apa Itu Run-of-River PLTA?", "id": "PLTA Mengandalkan Aliran Sungai Langsung." },
  { "en": "Apa Itu Pumped Storage PLTA?", "id": "PLTA Menyimpan Energi (Memompa Air Kembali)." },
  { "en": "Kapan Pumped Storage Memompa Air?", "id": "Saat Beban Listrik Rendah (Malam Hari)." },
  { "en": "Kapan Pumped Storage Menghasilkan Listrik?", "id": "Saat Beban Listrik Puncak (Siang Hari)." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Panas Bumi? (PLTP)", "id": "Pembangkit Listrik Menggunakan Uap Panas Bumi." },
  { "en": "Apa Sumber Panas PLTP?", "id": "Energi Geotermal Dari Dalam Bumi." },
  { "en": "Apa Itu Sumur Produksi PLTP?", "id": "Sumur Pengeboran Penghasil Uap Panas Bumi." },
  { "en": "Apa Itu Sumur Injeksi PLTP?", "id": "Sumur Mengembalikan Air Ke Dalam Bumi." },
  { "en": "Apa Itu Separator PLTP?", "id": "Pemisah Uap Kering, Air Panas (Brine)." },
  { "en": "Apa Jenis Turbin PLTP?", "id": "Turbin Uap Tekanan Rendah Khusus." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Uap? (PLTU)", "id": "Pembangkit Listrik Menggunakan Uap Batubara." },
  { "en": "Apa Komponen Utama PLTU?", "id": "Boiler, Turbin Uap, Generator, Kondensor." },
  { "en": "Apa Fungsi Boiler PLTU?", "id": "Memanaskan Air Menjadi Uap Bertekanan Tinggi." },
  { "en": "Apa Bahan Bakar Utama PLTU?", "id": "Umumnya Batubara Kualitas Rendah." },
  { "en": "Apa Fungsi Turbin Uap?", "id": "Mengubah Energi Uap Menjadi Energi Mekanis." },
  { "en": "Apa Fungsi Kondensor PLTU?", "id": "Mengubah Uap Sisa Turbin Menjadi Air." },
  { "en": "Mengapa Kondensor Diperlukan?", "id": "Menciptakan Vakum, Meningkatkan Efisiensi Siklus." },
  { "en": "Apa Itu Menara Pendingin? (Cooling Tower)", "id": "Mendinginkan Air Pendingin Kondensor." },
  { "en": "Apa Itu Siklus Rankine?", "id": "Siklus Termodinamika Dasar PLTU." },
  { "en": "Apa Itu Superheater?", "id": "Memanaskan Uap Jenuh Menjadi Uap Kering." },
  { "en": "Apa Itu Reheater?", "id": "Memanaskan Kembali Uap Antar Tingkat Turbin." },
  { "en": "Apa Itu Economizer?", "id": "Memanaskan Air Umpan Boiler Gas Buang." },
  { "en": "Apa Itu Air Preheater?", "id": "Memanaskan Udara Pembakaran Gas Buang." },
  { "en": "Apa Itu Deaerator?", "id": "Menghilangkan Oksigen Terlarut Air Umpan." },
  { "en": "Apa Itu Pompa Air Umpan? (Boiler Feed Pump)", "id": "Memompa Air Ke Boiler Tekanan Tinggi." },
  { "en": "Apa Itu Electrostatic Precipitator? (ESP)", "id": "Penangkap Abu Terbang Gas Buang." },
  { "en": "Apa Fungsi ESP?", "id": "Mengurangi Polusi Udara (Debu Batubara)." },
  { "en": "Apa Itu Flue Gas Desulfurization? (FGD)", "id": "Menghilangkan Sulfur Dioksida (SO2) Gas Buang." },
  { "en": "Apa Itu Bottom Ash?", "id": "Abu Sisa Pembakaran Di Dasar Boiler." },
  { "en": "Apa Itu Fly Ash?", "id": "Abu Ringan Ikut Gas Buang." },
  { "en": "Apa Siklus Dasar PLTG?", "id": "Siklus Brayton (Siklus Terbuka)." },
  { "en": "Apa Komponen Utama PLTG?", "id": "Kompresor, Ruang Bakar, Turbin Gas." },
  { "en": "Apa Fungsi Kompresor PLTG?", "id": "Menekan Udara Sekitar Ke Ruang Bakar." },
  { "en": "Apa Fungsi Ruang Bakar PLTG?", "id": "Mencampur Udara, Bahan Bakar, Pembakaran." },
  { "en": "Apa Bahan Bakar PLTG?", "id": "Gas Alam (Paling Umum), Minyak Diesel." },
  { "en": "Apa Fungsi Turbin Gas?", "id": "Mengubah Energi Gas Panas Menjadi Mekanis." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Gas Uap? (PLTGU)", "id": "Gabungan Siklus PLTG Dan PLTU." },
  { "en": "Apa Itu Siklus Kombinasi? (Combined Cycle)", "id": "Siklus Termodinamika PLTGU." },
  { "en": "Apa Keuntungan Utama PLTGU?", "id": "Efisiensi Termal Sangat Tinggi." },
  { "en": "Apa Itu HRSG? (Heat Recovery Steam Generator)", "id": "Boiler Pemanfaat Gas Buang Turbin Gas." },
  { "en": "Apa Fungsi HRSG?", "id": "Menghasilkan Uap Untuk Turbin Uap PLTU." },
  { "en": "Apa Itu Pembangkit Listtirk Tenaga Mesin Gas? (PLTMG)", "id": "Pembangkit Listrik Menggunakan Mesin Gas (Internal Combustion)." },
  { "en": "Apa Beda PLTG Dan PLTMG?", "id": "PLTG Pakai Turbin, PLTMG Pakai Mesin Piston." },
  { "en": "Apa Bahan Bakar PLTD?", "id": "Solar (High Speed Diesel/HSD)." },
  { "en": "Kapan PLTD Digunakan?", "id": "Beban Puncak, Cadangan Darurat, Daerah Terpencil." },
  { "en": "Apa Itu Kogenerasi? (Cogeneration)", "id": "Produksi Listrik, Panas Bersamaan." },
  { "en": "Apa Itu CHP? (Combined Heat and Power)", "id": "Nama Lain Dari Kogenerasi." },
  { "en": "Apa Keuntungan Kogenerasi?", "id": "Meningkatkan Efisiensi Energi Total." },
  { "en": "Apa Itu Trigenerasi?", "id": "Produksi Listrik, Panas, Pendingin Bersamaan." },
  { "en": "Apa Itu Kapasitas Terpasang?", "id": "Total Kemampuan Pembangkit Listrik (MW)." },
  { "en": "Apa Itu Kapasitas Mampu?", "id": "Kapasitas Terpasang Dikurangi Pemakaian Sendiri." },
  { "en": "Apa Itu Pemakaian Sendiri? (Auxiliary)", "id": "Listrik Digunakan Operasi Pembangkit Sendiri." },
  { "en": "Apa Itu Faktor Ketersediaan? (Availability Factor)", "id": "Persentase Waktu Pembangkit Siap Operasi." },
  { "en": "Apa Itu Faktor Kapasitas? (Capacity Factor)", "id": "Rasio Produksi Aktual, Produksi Maksimum." },
  { "en": "Apa Itu Forced Outage Rate? (FOR)", "id": "Persentase Waktu Pembangkit Gagal (Paksa)." },
  { "en": "Apa Itu Planned Outage?", "id": "Pemadaman Terencana (Pemeliharaan)." },
  { "en": "Apa Itu Heat Rate?", "id": "Jumlah Energi Panas (Btu) Per KWH." },
  { "en": "Apa Hubungan Heat Rate, Efisiensi?", "id": "Semakin Rendah Heat Rate, Semakin Efisien." },
  { "en": "Apa Itu Pembangkit Base Load?", "id": "Pembangkit Beroperasi Terus Menerus." },
  { "en": "Apa Contoh Pembangkit Base Load?", "id": "PLTU Batubara, PLTN, PLTA Waduk." },
  { "en": "Apa Itu Pembangkit Peaker?", "id": "Pembangkit Beroperasi Saat Beban Puncak." },
  { "en": "Apa Contoh Pembangkit Peaker?", "id": "PLTG, PLTD." },
  { "en": "Apa Itu Pembangkit Load Follower?", "id": "Pembangkit Mengikuti Fluktuasi Beban Harian." },
  { "en": "Apa Itu Kurva Beban Harian?", "id": "Grafik Kebutuhan Listrik Selama 24 Jam." },
  { "en": "Apa Itu Pasar Listrik? (Electricity Market)", "id": "Sistem Jual Beli Tenaga Listrik." },
  { "en": "Apa Itu Deregulasi Listrik?", "id": "Pemisahan Bisnis Pembangkitan, Transmisi, Distribusi." },
  { "en": "Apa Itu ISO? (Independent System Operator)", "id": "Operator Sistem Independen (Mengatur Jaringan)." },
  { "en": "Apa Itu Power Purchase Agreement? (PPA)", "id": "Kontrak Perjanjian Jual Beli Listrik." },
  { "en": "Apa Itu IPP? (Independent Power Producer)", "id": "Produsen Listrik Swasta Independen." },
  { "en": "Apa Itu Tarif Feed-In?", "id": "Tarif Khusus Pembelian Listrik Energi Terbarukan." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Ombak?", "id": "Pembangkit Energi Gelombang Laut." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Pasang Surut?", "id": "Pembangkit Energi Gerakan Pasang Surut." },
  { "en": "Apa Itu OTEC? (Ocean Thermal Energy Conversion)", "id": "Pembangkit Energi Perbedaan Suhu Laut." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Biomassa?", "id": "Pembangkit Energi Bahan Organik (Limbah)." },
  { "en": "Apa Contoh Bahan Bakar Biomassa?", "id": "Cangkang Sawit, Sekam Padi, Sampah." },
  { "en": "Apa Itu Biogas?", "id": "Gas Metana Hasil Dekomposisi Anaerobik." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Sampah? (PLTSa)", "id": "Pembangkit Listrik Menggunakan Sampah Kota." },
  { "en": "Apa Itu Teknologi Waste-to-Energy? (WTE)", "id": "Mengubah Sampah Menjadi Energi (Listrik/Panas)." },
  { "en": "Apa Itu Insinerator?", "id": "Tungku Pembakaran Sampah Suhu Tinggi." },
  { "en": "Apa Itu Landfill Gas?", "id": "Gas Metana Dari Tumpukan Sampah." },
  { "en": "Apa Itu Pembangkit Listrik Terapung?", "id": "Pembangkit Listrik Dibangun Diatas Kapal/Ponton." },
  { "en": "Apa Contoh Pembangkit Listrik Terapung?", "id": "Powership (PLTD/PLTMG Terapung)." },
  { "en": "Apa Itu Kabel Bawah Laut?", "id": "Kabel Transmisi Listrik Di Dasar Laut." },
  { "en": "Apa Tantangan Kabel Bawah Laut?", "id": "Instalasi Sulit, Perbaikan Mahal, Korosi." },
  { "en": "Apa Itu Kabel Tanah?", "id": "Kabel Transmisi Listrik Ditanam Bawah Tanah." },
  { "en": "Apa Keuntungan Kabel Tanah?", "id": "Estetika Kota, Aman Cuaca Buruk." },
  { "en": "Apa Kerugian Kabel Tanah?", "id": "Biaya Mahal, Pendinginan Sulit, Perbaikan Sulit." },
  { "en": "Apa Itu Saluran Udara? (Overhead Line)", "id": "Kabel Transmisi Listrik Di Tiang/Tower." },
  { "en": "Apa Itu ROW? (Right of Way)", "id": "Jalur Bebas Area Bawah Saluran Transmisi." },
  { "en": "Mengapa ROW Diperlukan?", "id": "Alasan Keamanan, Jarak Bebas (Clearance)." },
  { "en": "Apa Itu Induksi Elektromagnetik?", "id": "Tegangan Timbul Akibat Perubahan Medan Magnet." },
  { "en": "Apa Itu Isolator Gantung?", "id": "Isolator Piring Dirangkai (Tegangan Tinggi)." },
  { "en": "Apa Itu Isolator Batang Panjang? (Long Rod)", "id": "Isolator Porselen Atau Polimer Utuh." },
  { "en": "Apa Itu Isolator Pos? (Post Insulator)", "id": "Isolator Penyangga Kaku (Gardu Induk)." },
  { "en": "Apa Itu Jarak Rambat? (Creepage Distance)", "id": "Jarak Permukaan Isolator Mencegah Flashover." },
  { "en": "Apa Itu Flashover?", "id": "Loncatan Api Listrik Permukaan Isolator." },
  { "en": "Apa Itu Spacer Kabel?", "id": "Pemisah Antar Konduktor Fasa (Transmisi)." },
  { "en": "Apa Itu Peredam Getaran? (Vibration Damper)", "id": "Meredam Getaran Konduktor Akibat Angin." },
  { "en": "Apa Itu Kawat Pelindung? (Shield Wire)", "id": "Kawat Tanah (Ground Wire) Diatas Fasa." },
  { "en": "Apa Fungsi Utama Kawat Pelindung?", "id": "Melindungi Kawat Fasa Sambaran Petir." },
  { "en": "Apa Itu Pondasi Tower Transmisi?", "id": "Struktur Penopang Tower Di Tanah." },
  { "en": "Apa Jenis Pondasi Tower?", "id": "Pondasi Tiang Pancang, Pondasi Tapak." },
  { "en": "Apa Itu Oksidasi Minyak Trafo?", "id": "Penurunan Kualitas Minyak Akibat Oksigen." },
  { "en": "Apa Itu Inhibitor Oksidasi?", "id": "Zat Aditif Pencegah Oksidasi Minyak." },
  { "en": "Apa Itu PCB? (Polychlorinated Biphenyls)", "id": "Cairan Isolasi Trafo Lama (Beracun)." },
  { "en": "Mengapa PCB Dilarang?", "id": "Berbahaya Bagi Lingkungan, Kesehatan." },
  { "en": "Apa Itu Retrofilling Trafo?", "id": "Penggantian Minyak PCB Minyak Baru." },
  { "en": "Apa Itu Minyak Trafo Mineral?", "id": "Minyak Isolasi Berbasis Minyak Bumi." },
  { "en": "Apa Itu Minyak Trafo Nabati?", "id": "Minyak Isolasi Berbasis Tumbuhan (Ester)." },
  { "en": "Apa Keuntungan Minyak Nabati?", "id": "Mudah Terurai, Titik Nyala Tinggi." },
  { "en": "Apa Itu Titik Nyala? (Flash Point)", "id": "Suhu Terendah Minyak Menguap, Terbakar." },
  { "en": "Apa Itu Titik Tuang? (Pour Point)", "id": "Suhu Terendah Minyak Bisa Mengalir." },
  { "en": "Apa Itu Viskositas?", "id": "Ukuran Kekentalan Fluida (Minyak)." },
  { "en": "Apa Itu Tes Furan?", "id": "Tes Minyak Mendeteksi Degradasi Kertas Isolasi." },
  { "en": "Apa Senyawa Furan?", "id": "Senyawa Kimia Indikator Penuaan Kertas." },
  { "en": "Apa Itu Penuaan Isolasi?", "id": "Penurunan Kualitas Isolasi Seiring Waktu." },
  { "en": "Apa Faktor Penuaan Isolasi?", "id": "Panas, Oksigen, Kelembaban." },
  { "en": "Apa Itu Umur Trafo?", "id": "Perkiraan Waktu Operasi Trafo (Tahun)." },
  { "en": "Apa Itu Manajemen Aset Listrik?", "id": "Pengelolaan Siklus Hidup Aset Ketenagalistrikan." },
  { "en": "Apa Itu Studi Penuaan Aset?", "id": "Analisis Kondisi, Sisa Umur Peralatan." },
  { "en": "Apa Itu Penggantian Aset? (Replacement)", "id": "Mengganti Peralatan Lama Dengan Baru." },
  { "en": "Apa Itu Retrofit Peralatan?", "id": "Modifikasi Peralatan Lama (Misal: Relay)." },
  { "en": "Apa Itu Gardu Induk Konvensional?", "id": "Gardu Induk Tipe Terbuka (AIS)." },
  { "en": "Apa Itu Gardu Induk Pasangan Dalam? (GIS)", "id": "Gardu Induk Tipe Kubikel Gas SF6." },
  { "en": "Apa Keuntungan GIS?", "id": "Ukuran Jauh Lebih Kecil (Kompak)." },
  { "en": "Apa Itu Gardu Induk Bergerak? (Mobile Substation)", "id": "Gardu Induk Diatas Trailer (Darurat)." },
  { "en": "Apa Itu Gardu Induk Digital?", "id": "Gardu Induk Menggunakan Jaringan Digital (IEC 61850)." },
  { "en": "Apa Itu Merging Unit?", "id": "Mengubah Sinyal Analog CT/PT Digital." },
  { "en": "Apa Itu Sampled Values? (SV)", "id": "Data Sinyal Digital CT/PT." },
  { "en": "Apa Itu GOOSE Message? (Generic Object Oriented Substation Event)", "id": "Pesan Digital Komunikasi Antar IED." },
  { "en": "Apa Itu SCADA? (Supervisory Control And Data Acquisition)", "id": "Sistem Kontrol, Akuisisi Data Terpusat." },
  { "en": "Apa Fungsi SCADA?", "id": "Memantau, Mengendalikan Jaringan Listrik Jarak Jauh." },
  { "en": "Apa Itu Jaringan Komunikasi SCADA?", "id": "Radio, Serat Optik, PLC (Power Line Carrier)." },
  { "en": "Apa Itu PLC? (Power Line Carrier)", "id": "Komunikasi Data Lewat Jaringan Listrik." },
  { "en": "Apa Itu Protokol DNP3?", "id": "Protokol Komunikasi Umum SCADA, RTU." },
  { "en": "Apa Itu Protokol IEC 60870-5-101?", "id": "Protokol Komunikasi Serial SCADA." },
  { "en": "Apa Itu Protokol IEC 60870-5-104?", "id": "Protokol Komunikasi SCADA Berbasis TCP/IP." },
  { "en": "Apa Itu Master Station?", "id": "Pusat Kontrol (Server) Sistem SCADA." },
  { "en": "Apa Itu Outstation?", "id": "Unit Terminal Jarak Jauh (RTU)." },
  { "en": "Apa Itu EMS? (Energy Management System)", "id": "Perangkat Lunak Manajemen Sistem Tenaga (SCADA)." },
  { "en": "Apa Fungsi EMS?", "id": "Analisis Aliran Daya, AGC, Dispas Ekonomi." },
  { "en": "Apa Itu DMS? (Distribution Management System)", "id": "EMS Untuk Jaringan Distribusi Listrik." },
  { "en": "Apa Itu OMS? (Outage Management System)", "id": "Sistem Manajemen Pemadaman Listrik." },
  { "en": "Apa Itu GIS? (Geographic Information System)", "id": "Sistem Pemetaan Aset Jaringan Listrik." },
  { "en": "Apa Itu Keamanan Siber SCADA? (Cyber Security)", "id": "Perlindungan Sistem SCADA Dari Serangan Hacker." },
  { "en": "Mengapa Keamanan SCADA Penting?", "id": "Melindungi Infrastruktur Kritis Nasional." },
  { "en": "Apa Itu Firewall?", "id": "Perangkat Keamanan Jaringan Komputer." },
  { "en": "Apa Itu VPN? (Virtual Private Network)", "id": "Jaringan Pribadi Virtual (Akses Aman)." },
  { "en": "Apa Itu Data Historian?", "id": "Database Penyimpan Data Operasi SCADA." },
  { "en": "Apa Itu Alarm Flooding?", "id": "Kondisi Terlalu Banyak Alarm (Operator Bingung)." },
  { "en": "Apa Itu Manajemen Alarm?", "id": "Pengelolaan Prioritas, Tampilan Alarm." },
  { "en": "Apa Itu Pemeliharaan Jarak Jauh? (Remote Maintenance)", "id": "Mengakses Peralatan Jarak Jauh (VPN)." },
  { "en": "Apa Itu Uji Pabrik? (FAT)", "id": "Pengujian Peralatan Di Pabrik Produsen." },
  { "en": "Apa Kepanjangan FAT? (Factory Acceptance Test)", "id": "Tes Penerimaan Pabrik." },
  { "en": "Apa Itu Uji Lapangan? (SAT)", "id": "Pengujian Peralatan Di Lokasi Instalasi." },
  { "en": "Apa Kepanjangan SAT? (Site Acceptance Test)", "id": "Tes Penerimaan Lapangan." },
  { "en": "Apa Itu Commissioning?", "id": "Proses Pengujian, Peralatan Siap Operasi." },
  { "en": "Apa Itu Pra-Commissioning?", "id": "Pengecekan Fisik, Tes Individual (Sebelum Listrik)." },
  { "en": "Apa Itu Commissioning Fungsional?", "id": "Pengujian Fungsi Sistem (Dengan Listrik)." },
  { "en": "Apa Itu Energize?", "id": "Pemberian Tegangan Pertama Kali." },
  { "en": "Apa Itu Uji Beban? (Load Test)", "id": "Pengujian Peralatan Menggunakan Beban Aktual." },
  { "en": "Apa Itu Dokumen As-Built Drawing?", "id": "Gambar Aktual Terpasang Di Lapangan." },
  { "en": "Apa Itu Manual O&M? (Operation & Maintenance)", "id": "Buku Panduan Operasi, Pemeliharaan." },
  { "en": "Apa Itu Pelatihan Operator?", "id": "Pelatihan Staf Mengoperasikan Sistem Baru." },
  { "en": "Apa Itu Masa Garansi?", "id": "Periode Jaminan Kualitas Peralatan." },
  { "en": "Apa Itu Cacat Laten? (Latent Defect)", "id": "Kerusakan Tersembunyi (Material, Desain)." },
  { "en": "Apa Itu Daftar Simak? (Checklist)", "id": "Daftar Pengecekan Prosedur Tes." },
  { "en": "Apa Itu Prosedur Tes?", "id": "Dokumen Langkah-Langkah Pengujian." },
  { "en": "Apa Itu Laporan Tes?", "id": "Dokumen Hasil Pengujian Peralatan." },
  { "en": "Apa Itu Pengujian Tipe? (Type Test)", "id": "Tes Merusak (Sampel) Verifikasi Desain." },
  { "en": "Apa Itu Pengujian Rutin? (Routine Test)", "id": "Tes Non-Merusak (Setiap Unit Produksi)." },
  { "en": "Apa Itu Tes Impuls Petir? (Lightning Impulse)", "id": "Tes Ketahanan Isolasi Surja Petir." },
  { "en": "Apa Itu Tes Impuls Hubung? (Switching Impulse)", "id": "Tes Ketahanan Isolasi Surja Hubung." },
  { "en": "Apa Itu Tes Kenaikan Suhu? (Temperature Rise)", "id": "Tes Mengukur Pemanasan Alat Beban Penuh." },
  { "en": "Apa Itu Tes Hubung Singkat Dinamis?", "id": "Tes Kekuatan Mekanis Trafo (Arus Singkat)." },
  { "en": "Apa Itu Tes Kebisingan?", "id": "Mengukur Tingkat Suara (Bising) Trafo." },
  { "en": "Apa Itu Gelombang Sinusoidal?", "id": "Bentuk Gelombang Ideal Arus AC." },
  { "en": "Apa Itu Nilai Puncak? (Peak Value)", "id": "Nilai Amplitudo Maksimum Gelombang AC." },
  { "en": "Apa Itu Nilai RMS? (Root Mean Square)", "id": "Nilai Efektif Gelombang AC." },
  { "en": "Apa Hubungan Puncak, RMS (Sinus)?", "id": "VPuncak = VRMS x √2." },
  { "en": "Berapa Nilai RMS Tegangan 220V?", "id": "Nilai Efektifnya Adalah 220 Volt." },
  { "en": "Berapa Nilai Puncak Tegangan 220V?", "id": "Sekitar 311 Volt." },
  { "en": "Apa Itu Nilai Rata-Rata? (Average Value)", "id": "Nilai Rata-Rata Gelombang (Setengah Siklus)." },
  { "en": "Apa Itu Faktor Bentuk? (Form Factor)", "id": "Rasio Nilai RMS, Nilai Rata-Rata." },
  { "en": "Apa Itu Faktor Puncak? (Crest Factor)", "id": "Rasio Nilai Puncak, Nilai RMS." },
  { "en": "Berapa Crest Factor Sinus Murni?", "id": "Akar Dua (Sekitar 1.414)." },
  { "en": "Apa Itu Analisis Fourier?", "id": "Mengurai Gelombang Non-Sinus (Harmonisa)." },
  { "en": "Apa Itu Rangkaian Tiga Fasa?", "id": "Tiga Sumber Gelombang AC Beda Fasa." },
  { "en": "Berapa Beda Fasa Sistem Tiga Fasa?", "id": "120 Derajat Listrik." },
  { "en": "Apa Keuntungan Sistem Tiga Fasa?", "id": "Daya Konstan, Efisien, Hemat Konduktor." },
  { "en": "Apa Itu Konduktor?", "id": "Bahan Mudah Mengalirkan Arus Listrik." },
  { "en": "Apa Itu Resistansi?", "id": "Hambatan Bahan Terhadap Aliran Arus." },
  { "en": "Apa Itu Hukum Ohm?", "id": "V = I x R." },
  { "en": "Apa Itu Hukum Joule?", "id": "Energi Panas = I² x R x t." },
  { "en": "Apa Itu Hukum Kirchhoff Arus? (KCL)", "id": "Arus Masuk Titik Sama Dengan Arus Keluar." },
  { "en": "Apa Itu Hukum Kirchhoff Tegangan? (KVL)", "id": "Jumlah Tegangan Dalam Loop Tertutup Nol." },
  { "en": "Apa Itu Kapasitansi?", "id": "Kemampuan Menyimpan Muatan Listrik." },
  { "en": "Apa Itu Induktansi?", "id": "Kemampuan Hasilkan GGL Induksi Magnet." },
  { "en": "Apa Itu Rangkaian RC?", "id": "Rangkaian Terdiri Resistor, Kapasitor." },
  { "en": "Apa Itu Rangkaian RL?", "id": "Rangkaian Terdiri Resistor, Induktor." },
  { "en": "Apa Itu Rangkaian RLC?", "id": "Rangkaian Resistor, Induktor, Kapasitor." },
  { "en": "Apa Itu Respon Transien?", "id": "Respon Rangkaian Sesaat Setelah Perubahan." },
  { "en": "Apa Itu Respon Steady State?", "id": "Respon Rangkaian Setelah Kondisi Stabil." },
  { "en": "Apa Itu Konstanta Waktu RC?", "id": "Tau = R x C." },
  { "en": "Apa Itu Konstanta Waktu RL?", "id": "Tau = L / R." },
  { "en": "Apa Itu Rangkaian Orde Satu?", "id": "Rangkaian Hanya Punya Satu Elemen Penyimpan." },
  { "en": "Apa Itu Rangkaian Orde Dua?", "id": "Rangkaian Punya Dua Elemen Penyimpan (LC)." },
  { "en": "Apa Itu Respon Overdamped?", "id": "Respon Lambat Tanpa Osilasi." },
  { "en": "Apa Itu Respon Underdamped?", "id": "Respon Cepat Dengan Osilasi Teredam." },
  { "en": "Apa Itu Respon Critically Damped?", "id": "Respon Tercepat Tanpa Osilasi." },
  { "en": "Apa Itu Faktor Redaman? (Damping Factor)", "id": "Ukuran Redaman Osilasi Rangkaian." },
  { "en": "Apa Itu Transformasi Laplace?", "id": "Metode Analisis Rangkaian Domain Frekuensi." },
  { "en": "Apa Itu Domain Waktu? (Time Domain)", "id": "Analisis Sinyal Terhadap Waktu." },
  { "en": "Apa Itu Domain Frekuensi? (Frequency Domain)", "id": "Analisis Sinyal Terhadap Frekuensi." },
  { "en": "Apa Itu Fungsi Transfer?", "id": "Rasio Output Terhadap Input Domain Frekuensi." },
  { "en": "Apa Itu Pole Fungsi Transfer?", "id": "Akar Penyebut Fungsi Transfer." },
  { "en": "Apa Itu Zero Fungsi Transfer?", "id": "Akar Pembilang Fungsi Transfer." },
  { "en": "Apa Itu Diagram Bode?", "id": "Grafik Respon Frekuensi (Gain, Fasa)." },
  { "en": "Apa Itu Gain Margin?", "id": "Ukuran Kestabilan Sistem Kontrol." },
  { "en": "Apa Itu Phase Margin?", "id": "Ukuran Kestabilan Sistem Kontrol." },
  { "en": "Apa Itu Root Locus?", "id": "Grafik Lokasi Pole Loop Tertutup." },
  { "en": "Apa Itu Kriteria Kestabilan Nyquist?", "id": "Metode Menentukan Kestabilan Sistem Loop Tertutup." },
  { "en": "Apa Itu Sistem Kontrol Umpan Maju? (Feedforward)", "id": "Kontrol Tanpa Mengukur Output." },
  { "en": "Apa Itu Sistem Kontrol Umpan Balik? (Feedback)", "id": "Kontrol Menggunakan Pengukuran Output (Error)." },
  { "en": "Apa Itu Kestabilan Sistem?", "id": "Kemampuan Sistem Kembali Ke Keseimbangan." },
  { "en": "Apa Itu Kestabilan BIBO? (Bounded Input Bounded Output)", "id": "Input Terbatas Hasilkan Output Terbatas." },
  { "en": "Apa Itu State Space Analysis?", "id": "Metode Analisis Sistem Persamaan Diferensial." },
  { "en": "Apa Itu Variabel Keadaan? (State Variables)", "id": "Variabel Utama Analisis State Space." },
  { "en": "Apa Itu Matriks Keadaan? (State Matrix)", "id": "Matriks (A, B, C, D) State Space." },
  { "en": "Apa Itu Kontrolabilitas?", "id": "Kemampuan Input Mengendalikan State." },
  { "en": "Apa Itu Observabilitas?", "id": "Kemampuan Output Mengukur State." },
  { "en": "Apa Itu Kontrol Optimal?", "id": "Desain Kontroler Kinerja Paling Optimal." },
  { "en": "Apa Itu Kontrol Adaptif?", "id": "Kontroler Menyesuaikan Diri Perubahan Sistem." },
  { "en": "Apa Itu Kontrol Robust?", "id": "Kontroler Tahan Ketidakpastian Model." },
  { "en": "Apa Itu Logika Fuzzy? (Fuzzy Logic)", "id": "Logika Berbasis Derajat Keanggotaan (Bukan 0/1)." },
  { "en": "Apa Itu Jaringan Saraf Tiruan? (ANN)", "id": "Model Komputasi Meniru Otak Manusia." },
  { "en": "Apa Itu Algoritma Genetika?", "id": "Algoritma Optimasi Berbasis Evolusi." },
  { "en": "Apa Itu Sistem Cerdas?", "id": "Sistem Menggunakan Kecerdasan Buatan (AI)." },
  { "en": "Apa Itu Sensor Suhu?", "id": "Alat Ukur Besaran Suhu." },
  { "en": "Apa Itu Sensor Tekanan?", "id": "Alat Ukur Besaran Tekanan." },
  { "en": "Apa Itu Sensor Posisi?", "id": "Alat Ukur Posisi, Perpindahan." },
  { "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel (Sensor Posisi Sudut)." },
  { "en": "Apa Itu LVDT? (Linear Variable Differential Transformer)", "id": "Sensor Posisi Linier Sangat Akurat." },
  { "en": "Apa Itu Sensor Kecepatan?", "id": "Alat Ukur Kecepatan Putaran." },
  { "en": "Apa Itu Tachometer?", "id": "Alat Ukur Kecepatan Putaran (RPM)." },
  { "en": "Apa Itu Tachogenerator?", "id": "Generator Kecil Hasilkan Tegangan Sebanding Kecepatan." },
  { "en": "Apa Itu Sensor Akselerasi? (Accelerometer)", "id": "Sensor Mengukur Percepatan, Getaran." },
  { "en": "Apa Itu Sensor Ultrasonik?", "id": "Sensor Menggunakan Gelombang Suara Ultrasonik." },
  { "en": "Apa Fungsi Sensor Ultrasonik?", "id": "Mengukur Jarak, Mendeteksi Objek." },
  { "en": "Apa Itu Sensor Inframerah? (IR)", "id": "Sensor Mendeteksi Radiasi Inframerah (Panas)." },
  { "en": "Apa Itu Sensor PIR? (Passive Infrared)", "id": "Sensor Gerakan Mendeteksi Panas Tubuh." },
  { "en": "Apa Itu Sensor Visi? (Vision Sensor)", "id": "Kamera Cerdas Untuk Inspeksi." },
  { "en": "Apa Itu Pemrosesan Sinyal Digital? (DSP)", "id": "Mengolah Sinyal Menggunakan Prosesor Digital." },
  { "en": "Apa Itu Aliasing?", "id": "Kesalahan Sampling Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu Teorema Sampling Nyquist?", "id": "Frekuensi Sampling Harus Dua Kali Sinyal." },
  { "en": "Apa Itu Filter Digital?", "id": "Filter Diterapkan Secara Perangkat Lunak (DSP)." },
  { "en": "Apa Itu Filter FIR? (Finite Impulse Response)", "id": "Jenis Filter Digital (Selalu Stabil)." },
  { "en": "Apa Itu Filter IIR? (Infinite Impulse Response)", "id": "Jenis Filter Digital (Menggunakan Umpan Balik)." },
  { "en": "Apa Itu Transformasi Fourier Cepat? (FFT)", "id": "Algoritma Efisien Analisis Spektrum Frekuensi." },
  { "en": "Apa Itu Spektrum Frekuensi?", "id": "Komposisi Frekuensi Dalam Sinyal." },
  { "en": "Apa Itu Noise?", "id": "Sinyal Acak Pengganggu Sinyal Informasi." },
  { "en": "Apa Itu Signal-to-Noise Ratio? (SNR)", "id": "Rasio Kekuatan Sinyal Terhadap Noise." },
  { "en": "Apa Itu Ground Loop?", "id": "Gangguan Akibat Beda Potensial Titik Ground." },
  { "en": "Bagaimana Mengatasi Ground Loop?", "id": "Isolasi Sinyal (Optocoupler), Grounding Satu Titik." },
  { "en": "Apa Itu Isolator Optik? (Optocoupler)", "id": "Isolasi Sinyal Listrik Menggunakan Cahaya." },
  { "en": "Apa Itu Twisted Pair Cable?", "id": "Kabel Pasangan Terpilin Mengurangi Noise." },
  { "en": "Apa Itu Kabel Koaksial? (Coaxial)", "id": "Kabel Frekuensi Tinggi (Lapisan Pelindung)." },
  { "en": "Apa Itu Serat Optik? (Fiber Optic)", "id": "Media Transmisi Data Menggunakan Cahaya." },
  { "en": "Apa Keuntungan Serat Optik?", "id": "Bandwidth Besar, Tahan Noise Elektromagnetik." },
  { "en": "Apa Itu Single Mode Fiber?", "id": "Serat Optik Jarak Sangat Jauh." },
  { "en": "Apa Itu Multi Mode Fiber?", "id": "Serat Optik Jarak Pendek (Gedung)." },
  { "en": "Apa Itu OTDR? (Optical Time Domain Reflectometer)", "id": "Alat Tes Lokasi Putus Serat Optik." },
  { "en": "Apa Itu Fusion Splicing?", "id": "Metode Penyambungan Serat Optik (Las)." },
  { "en": "Apa Itu Atenuasi Sinyal?", "id": "Pelemahan Kekuatan Sinyal." },
  { "en": "Apa Itu Repeater?", "id": "Perangkat Penguat Sinyal Kembali." },
  { "en": "Apa Itu Jaringan Komputer?", "id": "Interkoneksi Antar Komputer, Perangkat." },
  { "en": "Apa Itu LAN? (Local Area Network)", "id": "Jaringan Area Lokal (Gedung, Kampus)." },
  { "en": "Apa Itu WAN? (Wide Area Network)", "id": "Jaringan Area Luas (Internet)." },
  { "en": "Apa Itu Topologi Jaringan?", "id": "Struktur Fisik, Logis Jaringan." },
  { "en": "Apa Itu Topologi Star?", "id": "Semua Perangkat Terhubung Ke Hub/Switch Pusat." },
  { "en": "Apa Itu Topologi Bus?", "id": "Semua Perangkat Terhubung Kabel Utama." },
  { "en": "Apa Itu Topologi Ring?", "id": "Semua Perangkat Terhubung Membentuk Cincin." },
  { "en": "Apa Itu Topologi Mesh?", "id": "Setiap Perangkat Terhubung Banyak Perangkat Lain." },
  { "en": "Apa Itu Switch Jaringan?", "id": "Menghubungkan Perangkat Jaringan (Lebih Cerdas)." },
  { "en": "Apa Itu Router?", "id": "Menghubungkan Antar Jaringan Berbeda (Internet)." },
  { "en": "Apa Itu IP Address?", "id": "Alamat Unik Perangkat Jaringan." },
  { "en": "Apa Itu Subnet Mask?", "id": "Menentukan Bagian Network, Host IP Address." },
  { "en": "Apa Itu Gateway?", "id": "Gerbang Keluar Jaringan Lokal (Router)." },
  { "en": "Apa Itu DNS? (Domain Name System)", "id": "Mengubah Nama Domain Menjadi IP Address." },
  { "en": "Apa Itu DHCP? (Dynamic Host Configuration Protocol)", "id": "Memberi IP Address Otomatis." },
  { "en": "Apa Itu TCP? (Transmission Control Protocol)", "id": "Protokol Transportasi Andal (Ada Koneksi)." },
  { "en": "Apa Itu UDP? (User Datagram Protocol)", "id": "Protokol Transportasi Cepat (Tanpa Koneksi)." },
  { "en": "Apa Itu HTTP? (Hypertext Transfer Protocol)", "id": "Protokol Transfer Halaman Web." },
  { "en": "Apa Itu HTTPS?", "id": "Versi Aman HTTP (Menggunakan Enkripsi)." },
  { "en": "Apa Itu Enkripsi?", "id": "Proses Mengacak Data Agar Aman." },
  { "en": "Apa Itu FTP? (File Transfer Protocol)", "id": "Protokol Transfer File Jaringan." },
  { "en": "Apa Itu IoT? (Internet of Things)", "id": "Jaringan Benda Fisik Tertanam Sensor." },
  { "en": "Apa Itu Industri 4.0?", "id": "Revolusi Industri Keempat (Otomasi, IoT)." },
  { "en": "Apa Itu Cloud Computing?", "id": "Layanan Komputasi Melalui Internet." },
  { "en": "Apa Itu Big Data?", "id": "Kumpulan Data Sangat Besar, Kompleks." },
  { "en": "Apa Itu Machine Learning?", "id": "Sistem Belajar Dari Data (AI)." },
  { "en": "Apa Itu Kecerdasan Buatan? (AI)", "id": "Kecerdasan Ditunjukkan Oleh Mesin." },
  { "en": "Apa Itu Deep Learning?", "id": "Sub-Bidang Machine Learning (ANN Kompleks)." },
  { "en": "Apa Itu Digital Twin?", "id": "Model Digital Aset Fisik Aktual." },
  { "en": "Apa Fungsi Digital Twin?", "id": "Simulasi, Prediksi, Optimasi Aset." },
  { "en": "Apa Itu Blockchain?", "id": "Teknologi Buku Besar Digital Terdistribusi." },
  { "en": "Apa Aplikasi Blockchain Energi?", "id": "Perdagangan Energi Peer-to-Peer (P2P)." },
  { "en": "Apa Itu 5G Dalam Industri?", "id": "Konektivitas Nirkabel Cepat, Latensi Rendah." },
  { "en": "Apa Itu Edge Computing?", "id": "Pemrosesan Data Dekat Sumber Data." },
  { "en": "Apa Beda Edge, Cloud Computing?", "id": "Edge Lebih Cepat (Latensi Rendah)." },
  { "en": "Apa Itu Arus RMS?", "id": "Nilai Efektif Arus Bolak-Balik." },
  { "en": "Apa Itu Arus Rata-Rata?", "id": "Nilai Rata-Rata Gelombang (Satu Siklus)." },
  { "en": "Apa Nilai Rata-Rata Sinus Murni?", "id": "Nol." },
  { "en": "Apa Nilai Rata-Rata Penyearah Setengah Gelombang?", "id": "Vpuncak Dibagi Pi (π)." },
  { "en": "Apa Nilai Rata-Rata Penyearah Gelombang Penuh?", "id": "Dua Vpuncak Dibagi Pi (π)." },
  { "en": "Apa Itu Thyristor Semikonduktor?", "id": "Komponen Empat Lapis (PNPN)." },
  { "en": "Apa Itu Arus Latching?", "id": "Arus Minimum Menahan SCR Tetap Aktif." },
  { "en": "Apa Itu Arus Holding?", "id": "Arus Minimum Menjaga SCR Tetap Aktif." },
  { "en": "Apa Beda Latching, Holding Current?", "id": "Latching (Saat ON), Holding (Tetap ON)." },
  { "en": "Bagaimana Cara Mematikan SCR?", "id": "Arus Turun Dibawah Arus Holding." },
  { "en": "Apa Itu Komutasi SCR?", "id": "Proses Mematikan Thyristor (SCR)." },
  { "en": "Apa Itu Komutasi Natural?", "id": "SCR Mati Alami (Tegangan AC Nol)." },
  { "en": "Apa Itu Komutasi Paksa? (Forced)", "id": "Memaksa Arus SCR Nol (Rangkaian DC)." },
  { "en": "Apa Itu Rangkaian Snubber?", "id": "Melindungi Thyristor Dari Laju DV/DT." },
  { "en": "Apa Itu DV/DT?", "id": "Laju Kenaikan Tegangan Terhadap Waktu." },
  { "en": "Apa Itu DI/DT?", "id": "Laju Kenaikan Arus Terhadap Waktu." },
  { "en": "Apa Proteksi Terhadap DI/DT?", "id": "Induktor Kecil Dihubungkan Seri SCR." },
  { "en": "Apa Itu Sudut Penyalaan? (Firing Angle)", "id": "Sudut Picu Gate SCR (Kontrol Daya)." },
  { "en": "Apa Itu Sudut Konduksi?", "id": "Lama Sudut SCR Menghantar Arus." },
  { "en": "Apa Itu Controlled Rectifier?", "id": "Penyearah Tegangan Output Bisa Diatur (SCR)." },
  { "en": "Apa Itu Inverter Jembatan Penuh? (Full-Bridge)", "id": "Inverter Menggunakan Empat Saklar." },
  { "en": "Apa Itu Inverter Setengah Jembatan? (Half-Bridge)", "id": "Inverter Menggunakan Dua Saklar." },
  { "en": "Apa Itu Multilevel Inverter?", "id": "Inverter Hasilkan Tegangan Output Bertingkat." },
  { "en": "Apa Keuntungan Multilevel Inverter?", "id": "Harmonisa Rendah, Tegangan Tinggi." },
  { "en": "Apa Itu Space Vector Modulation? (SVM)", "id": "Teknik PWM Canggih Inverter Tiga Fasa." },
  { "en": "Apa Itu Elektrolit Kapasitor?", "id": "Bahan Dielektrik Cair, Pasta (Kapasitor Elco)." },
  { "en": "Apa Itu Kapasitor Non-Polar?", "id": "Kapasitor Tanpa Polaritas Positif, Negatif." },
  { "en": "Apa Contoh Kapasitor Non-Polar?", "id": "Keramik, Mika, Film." },
  { "en": "Apa Itu Kapasitor Polar?", "id": "Kapasitor Memiliki Polaritas (Elco, Tantalum)." },
  { "en": "Apa Itu ESR Kapasitor? (Equivalent Series Resistance)", "id": "Resistansi Seri Internal Kapasitor." },
  { "en": "Apa Akibat ESR Tinggi?", "id": "Kapasitor Panas, Efisiensi Turun." },
  { "en": "Apa Itu ESL Kapasitor? (Equivalent Series Inductance)", "id": "Induktansi Seri Internal Kapasitor." },
  { "en": "Apa Itu Frekuensi Resonansi Kapasitor?", "id": "Frekuensi Saat Reaktansi ESL, XC Sama." },
  { "en": "Apa Itu Ferrite Core?", "id": "Inti Magnetik Ferit (Frekuensi Tinggi)." },
  { "en": "Mengapa Ferrite Core Frekuensi Tinggi?", "id": "Resistivitas Tinggi, Rugi Eddy Rendah." },
  { "en": "Apa Itu SMPS? (Switch Mode Power Supply)", "id": "Catu Daya Efisiensi Tinggi (Switching)." },
  { "en": "Apa Prinsip SMPS?", "id": "Switching Frekuensi Tinggi, Trafo Kecil." },
  { "en": "Apa Beda Trafo SMPS, Konvensional?", "id": "Trafo SMPS Pakai Inti Ferit (Kecil)." },
  { "en": "Apa Itu Konverter Flyback?", "id": "Jenis Topologi SMPS Sederhana, Terisolasi." },
  { "en": "Apa Itu Konverter Forward?", "id": "Jenis Topologi SMPS Terisolasi." },
  { "en": "Apa Itu Konverter Push-Pull?", "id": "Topologi SMPS Menggunakan Dua Transistor." },
  { "en": "Apa Itu DILO?", "id": "Perusahaan Alat Servis Gas SF6." },
  { "en": "Apa Itu Alat Analisis Gas SF6?", "id": "Mengukur Kemurnian, Kelembaban, Dekomposisi SF6." },
  { "en": "Apa Produk Dekomposisi SF6?", "id": "Gas Beracun Akibat Busur Api." },
  { "en": "Mengapa Gas SF6 Harus Ditangani Khusus?", "id": "Gas Rumah Kaca Sangat Kuat." },
  { "en": "Apa Itu Global Warming Potential? (GWP)", "id": "Potensi Pemanasan Global Gas." },
  { "en": "Berapa GWP Gas SF6?", "id": "Sangat Tinggi (Puluhan Ribu Kali CO2)." },
  { "en": "Apa Alternatif Gas SF6?", "id": "Campuran Gas Ramah Lingkungan (g3)." },
  { "en": "Apa Itu Bushing Tipe Kering?", "id": "Bushing Menggunakan Isolasi Padat (Resin)." },
  { "en": "Apa Itu Bushing Tipe Minyak? (OIP)", "id": "Bushing Menggunakan Isolasi Kertas, Minyak." },
  { "en": "Apa Itu Bushing Tipe RIP? (Resin Impregnated Paper)", "id": "Bushing Isolasi Kertas Diresapi Resin." },
  { "en": "Apa Itu Bushing Tipe RIS? (Resin Impregnated Synthetics)", "id": "Bushing Isolasi Sintetis Diresapi Resin." },
  { "en": "Apa Itu Bushing Kapasitif?", "id": "Bushing Dengan Lapisan Konduktif (Kontrol Medan)." },
  { "en": "Apa Itu Tes Tap Kapasitansi Bushing?", "id": "Mengukur Faktor Daya Isolasi Bushing." },
  { "en": "Apa Itu Arrester Tipe Celah? (Gapped)", "id": "Arrester Menggunakan Celah Percik Api." },
  { "en": "Apa Itu Arrester Tipe MOV? (Metal Oxide Varistor)", "id": "Arrester Tanpa Celah (Paling Umum)." },
  { "en": "Apa Bahan Dasar MOV?", "id": "Zinc Oxide (ZnO)." },
  { "en": "Apa Karakteristik MOV?", "id": "Resistansi Tinggi (Tegangan Normal), Rendah (Surja)." },
  { "en": "Apa Itu Tegangan Sisa Arrester?", "id": "Tegangan Terukur Di Arrester Saat Surja." },
  { "en": "Apa Itu Arus Pelepasan Nominal?", "id": "Arus Surja Desain Arrester." },
  { "en": "Apa Itu Kelas Arrester?", "id": "Distribusi, Gardu Induk, Transmisi." },
  { "en": "Apa Itu Surge Counter?", "id": "Penghitung Jumlah Surja Arrester." },
  { "en": "Apa Itu Monitor Arus Bocor Arrester?", "id": "Mengukur Arus Bocor Arrester MOV." },
  { "en": "Apa Itu Pemisah? (Disconnector)", "id": "Saklar Memisahkan Peralatan Tanpa Beban." },
  { "en": "Apa Itu Saklar Pentanahan? (Earth Switch)", "id": "Saklar Menghubungkan Jaringan Ke Tanah." },
  { "en": "Kapan Earth Switch Ditutup?", "id": "Saat Perbaikan (Alasan Keselamatan)." },
  { "en": "Apa Interlock Earth Switch, Disconnector?", "id": "Earth Switch Tutup Jika Disconnector Buka." },
  { "en": "Apa Itu Disconnector Tipe Pantograf?", "id": "Pemisah Model Lengan Lipat (Pantograf)." },
  { "en": "Apa Itu Disconnector Tipe Lengan Ganda?", "id": "Pemisah Dua Lengan Berputar." },
  { "en": "Apa Itu Disconnector Tipe Lengan Putar?", "id": "Pemisah Satu Lengan Berputar." },
  { "en": "Apa Itu Elektroda Batang? (Rod Electrode)", "id": "Batang Logam Ditanam Vertikal (Grounding)." },
  { "en": "Apa Itu Elektroda Pita? (Strip Electrode)", "id": "Konduktor Pita Ditanam Horizontal." },
  { "en": "Apa Itu Elektroda Pelat? (Plate Electrode)", "id": "Pelat Logam Ditanam (Jarang Dipakai)." },
  { "en": "Apa Itu Mesh Pentanahan?", "id": "Jaringan Konduktor Tertanam (Area Gardu Induk)." },
  { "en": "Apa Fungsi Mesh Pentanahan?", "id": "Mengontrol Tegangan Sentuh, Langkah." },
  { "en": "Apa Itu Bentonit?", "id": "Bahan Aditif Memperbaiki Tahanan Tanah." },
  { "en": "Kapan Bentonit Digunakan?", "id": "Area Tanah Kering, Berbatu." },
  { "en": "Apa Itu Overhaul?", "id": "Pemeliharaan Besar (Bongkar Total)." },
  { "en": "Apa Itu Reconditioning?", "id": "Perbaikan Mengembalikan Kondisi Awal." },
  { "en": "Apa Itu Remanufaktur?", "id": "Membangun Ulang Produk Sesuai Spesifikasi Asli." },
  { "en": "Apa Itu Duty Cycle Motor?", "id": "Pola Operasi Beban Motor (S1-S9)." },
  { "en": "Apa Itu Duty S1? (Continuous)", "id": "Motor Beroperasi Terus Menerus Beban Konstan." },
  { "en": "Apa Itu Duty S2? (Short Time)", "id": "Motor Beroperasi Waktu Singkat, Lalu Dingin." },
  { "en": "Apa Itu Duty S3? (Intermittent)", "id": "Motor Beroperasi Intermiten (Beban, Jeda)." },
  { "en": "Apa Itu NEMA Enclosure?", "id": "Standar Tipe Selungkup Motor (Proteksi)." },
  { "en": "Apa Itu NEMA Tipe 1?", "id": "Proteksi Dalam Ruangan (Debu Ringan)." },
  { "en": "Apa Itu NEMA Tipe 3R?", "id": "Proteksi Luar Ruangan (Hujan, Salju)." },
  { "en": "Apa Itu NEMA Tipe 4X?", "id": "Tahan Air, Tahan Korosi." },
  { "en": "Apa Itu NEMA Tipe 12?", "id": "Industri Dalam Ruangan (Debu, Minyak)." },
  { "en": "Apa Itu Cincin Korona? (Corona Ring)", "id": "Cincin Logam Isolator Tegangan Tinggi." },
  { "en": "Apa Fungsi Cincin Korona?", "id": "Membagi Medan Listrik, Mencegah Korona." },
  { "en": "Apa Itu Tanduk Api? (Arcing Horn)", "id": "Pelindung Isolator Dari Loncatan Api." },
  { "en": "Dimana Arcing Horn Dipasang?", "id": "Di Ujung Isolator Saluran Transmisi." },
  { "en": "Apa Itu Level Isolasi Dasar? (BIL)", "id": "Ketahanan Isolasi Terhadap Tegangan Impuls." },
  { "en": "Apa Kepanjangan BIL? (Basic Insulation Level)", "id": "Tingkat Isolasi Dasar Peralatan." },
  { "en": "Apa Itu Koordinasi Isolasi?", "id": "Menyelaraskan Kekuatan Isolasi, Proteksi Surja." },
  { "en": "Apa Tujuan Koordinasi Isolasi?", "id": "Mencegah Kerusakan Isolasi Peralatan Mahal." },
  { "en": "Peralatan Apa Paling Lemah (BIL Rendah)?", "id": "Umumnya Transformator Daya." },
  { "en": "Apa Itu Pelindung Surja? (Arrester)", "id": "Peralatan Proteksi Tegangan Lebih." },
  { "en": "Apa Itu Tegangan Tembus Udara?", "id": "Tegangan Menyebabkan Udara Menjadi Konduktif." },
  { "en": "Apa Itu Jarak Bebas Fasa-Tanah?", "id": "Jarak Aman Minimum Konduktor Ke Ground." },
  { "en": "Apa Itu Jarak Bebas Fasa-Fasa?", "id": "Jarak Aman Minimum Antar Konduktor Fasa." },
  { "en": "Apa Itu Ruang Bebas Gardu Induk?", "id": "Area Aman Operasi, Pemeliharaan." },
  { "en": "Apa Itu Resistivitas Tanah?", "id": "Kemampuan Tanah Menghambat Arus Listrik." },
  { "en": "Kapan Resistivitas Tanah Diukur?", "id": "Sebelum Merancang Sistem Pentanahan." },
  { "en": "Apa Metode Ukur Resistivitas Tanah?", "id": "Metode Wenner (Empat Titik)." },
  { "en": "Apa Pengaruh Resistivitas Tanah Tinggi?", "id": "Semakin Sulit Mendapatkan Tahanan Rendah." },
  { "en": "Apa Itu Tes Tahanan Jenis Tanah?", "id": "Nama Lain Pengukuran Resistivitas Tanah." },
  { "en": "Apa Itu Kontrol Frekuensi Primer?", "id": "Respon Cepat Governor Generator (Droop)." },
  { "en": "Apa Itu Kontrol Frekuensi Sekunder?", "id": "Respon Otomatis (AGC) Mengembalikan Frekuensi." },
  { "en": "Apa Itu Kontrol Frekuensi Tersier?", "id": "Respon Manual (Dispas Ekonomi) Mengatur Cadangan." },
  { "en": "Apa Itu Penurunan Frekuensi? (Under Frequency)", "id": "Kondisi Frekuensi Sistem Dibawah Normal." },
  { "en": "Apa Penyebab Frekuensi Turun?", "id": "Daya Pembangkit Lebih Kecil Dari Beban." },
  { "en": "Apa Itu Pelepasan Beban? (Load Shedding)", "id": "Memutus Sebagian Beban (Frekuensi Turun)." },
  { "en": "Apa Itu Relay UFR? (Under Frequency Relay)", "id": "Relay Pemicu Pelepasan Beban Otomatis." },
  { "en": "Apa Itu Kenaikan Frekuensi? (Over Frequency)", "id": "Kondisi Frekuensi Sistem Diatas Normal." },
  { "en": "Apa Penyebab Frekuensi Naik?", "id": "Daya Pembangkit Lebih Besar Dari Beban." },
  { "en": "Apa Itu House Load Operation?", "id": "Pembangkit Hanya Menyuplai Beban Internal." },
  { "en": "Apa Itu Islanding?", "id": "Sebagian Jaringan Terpisah Sistem Utama." },
  { "en": "Apa Itu Sinkronoskop?", "id": "Alat Bantu Visual Proses Sinkronisasi." },
  { "en": "Apa Itu Lampu Sinkron?", "id": "Metode Manual Sinkronisasi Menggunakan Lampu." },
  { "en": "Apa Itu Urutan Fasa R-S-T?", "id": "Urutan Standar Fasa Listrik Tiga Fasa." },
  { "en": "Apa Itu Catu Daya? (Power Supply)", "id": "Peralatan Penyedia Tenaga Listrik." },
  { "en": "Apa Itu Catu Daya Linier?", "id": "Catu Daya Menggunakan Trafo Konvensional." },
  { "en": "Apa Kerugian Catu Daya Linier?", "id": "Ukuran Besar, Berat, Efisiensi Rendah." },
  { "en": "Apa Keuntungan Catu Daya Linier?", "id": "Noise Output Sangat Rendah." },
  { "en": "Apa Itu Catu Daya Switching? (SMPS)", "id": "Catu Daya Efisiensi Tinggi (Frekuensi Tinggi)." },
  { "en": "Apa Itu Regulator Tegangan?", "id": "Rangkaian Menstabilkan Tegangan Output." },
  { "en": "Apa Itu IC Regulator 7805?", "id": "Regulator Tegangan Output Tetap 5 Volt." },
  { "en": "Apa Itu IC Regulator 7905?", "id": "Regulator Tegangan Output Tetap -5 Volt." },
  { "en": "Apa Itu Regulator LM317?", "id": "Regulator Tegangan Output Variabel (Positif)." },
  { "en": "Apa Itu Zener Dioda?", "id": "Dioda Bekerja Tegangan Tembus Mundur." },
  { "en": "Apa Fungsi Zener Dioda?", "id": "Sebagai Referensi, Regulator Tegangan Sederhana." },
  { "en": "Apa Itu Tegangan Zener?", "id": "Tegangan Tembus Mundur (Breakdown) Zener." },
  { "en": "Apa Itu Transistor Bipolar? (BJT)", "id": "Transistor Dikendalikan Arus Basis." },
  { "en": "Apa Jenis Transistor BJT?", "id": "NPN Dan PNP." },
  { "en": "Apa Kaki Transistor BJT?", "id": "Basis (Base), Kolektor (Collector), Emitor (Emitter)." },
  { "en": "Apa Fungsi Transistor BJT?", "id": "Saklar Elektronik, Penguat Arus." },
  { "en": "Apa Itu Daerah Aktif Transistor?", "id": "Mode Operasi Transistor Sebagai Penguat." },
  { "en": "Apa Itu Daerah Saturasi Transistor?", "id": "Mode Operasi Transistor Saklar Tertutup (On)." },
  { "en": "Apa Itu Daerah Cut-Off Transistor?", "id": "Mode Operasi Transistor Saklar Terbuka (Off)." },
  { "en": "Apa Itu Penguatan Arus DC? (Hfe/Beta)", "id": "Rasio Arus Kolektor Terhadap Arus Basis." },
  { "en": "Apa Itu FET? (Field Effect Transistor)", "id": "Transistor Dikendalikan Tegangan Gate." },
  { "en": "Apa Jenis FET?", "id": "JFET Dan MOSFET." },
  { "en": "Apa Kaki MOSFET?", "id": "Gate, Drain, Source." },
  { "en": "Apa Keuntungan MOSFET Dibanding BJT?", "id": "Impedansi Input Tinggi, Switching Cepat." },
  { "en": "Apa Itu MOSFET Tipe Enhancement?", "id": "MOSFET Perlu Tegangan Gate (Positif) ON." },
  { "en": "Apa Itu MOSFET Tipe Depletion?", "id": "MOSFET Kondisi ON Saat Tegangan Gate Nol." },
  { "en": "Apa Itu CMOS? (Complementary Metal Oxide Semiconductor)", "id": "Teknologi Menggunakan Pasangan NMOS, PMOS." },
  { "en": "Apa Keuntungan CMOS?", "id": "Konsumsi Daya Statis Sangat Rendah." }



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
