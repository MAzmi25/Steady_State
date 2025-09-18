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


{ "en": "Bentuk gelombang tegangan/arus bolak-balik?", "id": "Bentuk gelombang sinusoidal." },
{ "en": "Besaran vektor berputar representasi sinusoida?", "id": "Fasor." },
{ "en": "Domain analisis rangkaian AC menggunakan fasor?", "id": "Domain frekuensi." },
{ "en": "Satuan frekuensi dalam rangkaian AC?", "id": "Hertz (Hz)." },
{ "en": "Hubungan frekuensi sudut dan frekuensi linier?", "id": "Omega sama dengan dua pi f." },
{ "en": "Waktu untuk satu siklus gelombang?", "id": "Periode (T)." },
{ "en": "Nilai puncak gelombang sinusoidal?", "id": "Amplitudo." },
{ "en": "Nilai DC ekuivalen dari gelombang AC?", "id": "Nilai efektif (RMS)." },
{ "en": "Pergeseran fasa antara dua gelombang?", "id": "Sudut fasa (phase angle)." },
{ "en": "Representasi domain waktu dari fasor V?", "id": "V cosinus omega t plus fasa." },
{ "en": "Operator j dalam bilangan kompleks?", "id": "Akar kuadrat dari minus satu." },
{ "en": "Bentuk bilangan kompleks a + jb?", "id": "Bentuk rektangular." },
{ "en": "Bentuk bilangan kompleks r sudut fasa?", "id": "Bentuk polar." },
{ "en": "Apa itu 'r' dalam bentuk polar?", "id": "Magnitudo atau modulus." },
{ "en": "Apa itu 'fasa' dalam bentuk polar?", "id": "Argumen." },
{ "en": "Operasi fasor yang mudah dalam bentuk rektangular?", "id": "Penjumlahan dan pengurangan." },
{ "en": "Operasi fasor yang mudah dalam bentuk polar?", "id": "Perkalian dan pembagian." },
{ "en": "Total hambatan terhadap arus AC?", "id": "Impedansi (Z)." },
{ "en": "Satuan dari impedansi?", "id": "Ohm." },
{ "en": "Kebalikan dari impedansi?", "id": "Admitansi (Y)." },
{ "en": "Satuan dari admitansi?", "id": "Siemens (S)." },
{ "en": "Bagian riil dari impedansi?", "id": "Resistansi (R)." },
{ "en": "Bagian imajiner dari impedansi?", "id": "Reaktansi (X)." },
{ "en": "Bagian riil dari admitansi?", "id": "Konduktansi (G)." },
{ "en": "Bagian imajiner dari admitansi?", "id": "Suseptansi (B)." },
{ "en": "Reaktansi bernilai positif milik komponen apa?", "id": "Induktor (Reaktansi Induktif)." },
{ "en": "Reaktansi bernilai negatif milik komponen apa?", "id": "Kapasitor (Reaktansi Kapasitif)." },
{ "en": "Hukum yang menghubungkan tegangan, arus, impedansi?", "id": "Hukum Ohm untuk rangkaian AC." },
{ "en": "Jika tegangan mendahului arus, beban bersifat?", "id": "Induktif." },
{ "en": "Jika arus mendahului tegangan, beban bersifat?", "id": "Kapasitif." },
{ "en": "Jika tegangan dan arus sefasa?", "id": "Resistif murni." },
{ "en": "Nilai RMS dari Vm cos(ωt)?", "id": "Vm dibagi akar dua." },
{ "en": "Berapa sudut fasa operator j?", "id": "Sembilan puluh derajat." },
{ "en": "Konversi rektangular ke polar menggunakan teorema?", "id": "Teorema Pythagoras." },
{ "en": "Alat ukur nilai RMS tegangan AC?", "id": "Voltmeter AC." },
{ "en": "Berapa nilai rata-rata gelombang sinus penuh?", "id": "Nol." },
{ "en": "Hubungan tegangan dan arus pada resistor?", "id": "Sefasa." },
{ "en": "Hubungan tegangan dan arus pada induktor?", "id": "Tegangan mendahului arus 90 derajat." },
{ "en": "Hubungan tegangan dan arus pada kapasitor?", "id": "Arus mendahului tegangan 90 derajat." },
{ "en": "Impedansi resistor murni?", "id": "R (hanya bagian riil)." },
{ "en": "Impedansi induktor murni?", "id": "j omega L." },
{ "en": "Impedansi kapasitor murni?", "id": "Satu per j omega C." },
{ "en": "Admitansi resistor murni?", "id": "G atau satu per R." },
{ "en": "Admitansi induktor murni?", "id": "Satu per j omega L." },
{ "en": "Admitansi kapasitor murni?", "id": "j omega C." },
{ "en": "Apakah impedansi dan admitansi besaran fasor?", "id": "Bukan, besaran kompleks." },
{ "en": "Frekuensi jaringan listrik di Indonesia?", "id": "Lima puluh Hertz." },
{ "en": "Apa itu keadaan tunak (steady state)?", "id": "Kondisi setelah transien hilang." },
{ "en": "Analisis keadaan tunak AC disebut juga?", "id": "Analisis sinusoidal keadaan tunak." },
{ "en": "Besaran yang sama untuk semua komponen seri?", "id": "Arus." },
{ "en": "Besaran yang sama untuk semua komponen paralel?", "id": "Tegangan." },
{ "en": "Frekuensi sudut nol disebut kondisi?", "id": "DC (Direct Current)." },
{ "en": "Impedansi kapasitor pada kondisi DC?", "id": "Tak hingga (rangkaian terbuka)." },
{ "en": "Impedansi induktor pada kondisi DC?", "id": "Nol (rangkaian pendek)." },
{ "en": "Kombinasi resistansi dan reaktansi disebut?", "id": "Impedansi." },
{ "en": "Magnitudo impedansi Z = R + jX?", "id": "Akar dari R kuadrat plus X kuadrat." },
{ "en": "Sudut impedansi Z = R + jX?", "id": "Arctan dari X per R." },
{ "en": "Apakah nilai RMS selalu positif?", "id": "Ya, selalu positif." },
{ "en": "Apakah frekuensi berubah dalam rangkaian linier?", "id": "Tidak, frekuensi tetap sama." },
{ "en": "Metode analisis rangkaian AC didasarkan pada?", "id": "Aljabar kompleks." },
{ "en": "Konjugat kompleks dari a + jb?", "id": "a minus jb." },
{ "en": "Hasil perkalian bilangan kompleks dengan konjugatnya?", "id": "Kuadrat dari magnitudonya." },
{ "en": "Nilai puncak-ke-puncak (peak-to-peak) gelombang sinus?", "id": "Dua kali amplitudo." },
{ "en": "Laju perubahan fasa gelombang sinusoidal?", "id": "Frekuensi sudut (omega)." },
{ "en": "Apakah fasor menunjukkan frekuensi?", "id": "Tidak, frekuensi diasumsikan konstan." },
{ "en": "Grafik tegangan atau arus vs waktu?", "id": "Bentuk gelombang." },
{ "en": "Diagram yang menunjukkan hubungan fasa?", "id": "Diagram fasor." },
{ "en": "Panjang panah dalam diagram fasor menunjukkan?", "id": "Magnitudo (nilai RMS atau amplitudo)." },
{ "en": "Sudut antar panah dalam diagram fasor?", "id": "Perbedaan fasa." },
{ "en": "Sumbu referensi pada diagram fasor?", "id": "Sumbu riil positif." },
{ "en": "Rotasi fasor berlawanan arah jarum jam?", "id": "Sudut fasa positif." },
{ "en": "Reaktansi induktif sebanding dengan?", "id": "Frekuensi." },
{ "en": "Reaktansi kapasitif berbanding terbalik dengan?", "id": "Frekuensi." },
{ "en": "Suseptansi kapasitif sebanding dengan?", "id": "Frekuensi." },
{ "en": "Suseptansi induktif berbanding terbalik dengan?", "id": "Frekuensi." },
{ "en": "Komponen yang menyimpan energi medan magnet?", "id": "Induktor." },
{ "en": "Komponen yang menyimpan energi medan listrik?", "id": "Kapasitor." },
{ "en": "Komponen yang mendisipasikan energi?", "id": "Resistor." },
{ "en": "Persamaan tegangan sinusoidal umum?", "id": "Vm cosinus omega t plus fasa." },
{ "en": "Bentuk eksponensial dari fasor?", "id": "r dikali e pangkat j fasa." },
{ "en": "Hubungan eksponensial kompleks dan trigonometri?", "id": "Rumus Euler." },
{ "en": "Apa singkatan RMS?", "id": "Root Mean Square (Akar Rata-rata Kuadrat)." },
{ "en": "Kapan dua sinusoida disebut sefasa?", "id": "Saat sudut fasanya sama." },
{ "en": "Kapan dua sinusoida berlawanan fasa?", "id": "Saat beda fasanya 180 derajat." },
{ "en": "Beda fasa antara sinus dan cosinus?", "id": "Sembilan puluh derajat." },
{ "en": "Istilah lain untuk nilai efektif?", "id": "Nilai RMS." },
{ "en": "Apakah analisis AC hanya untuk sinusoida?", "id": "Utamanya, tapi bisa diperluas dengan Fourier." },
{ "en": "Impedansi ekuivalen untuk impedansi seri?", "id": "Jumlah dari semua impedansi." },
{ "en": "Admitansi ekuivalen untuk admitansi paralel?", "id": "Jumlah dari semua admitansi." },
{ "en": "Apakah KVL berlaku dalam domain frekuensi?", "id": "Ya, dengan tegangan fasor." },
{ "en": "Apakah KCL berlaku dalam domain frekuensi?", "id": "Ya, dengan arus fasor." },
{ "en": "Perangkat yang mengubah AC ke DC?", "id": "Penyearah (rectifier)." },
{ "en": "Perangkat yang mengubah DC ke AC?", "id": "Inverter." },
{ "en": "Impedansi total rangkaian R dan L seri?", "id": "R ditambah j omega L." },
{ "en": "Impedansi total rangkaian R dan C seri?", "id": "R ditambah satu per j omega C." },
{ "en": "Impedansi total rangkaian R, L, C seri?", "id": "R ditambah j (XL minus XC)." },
{ "en": "Admitansi total rangkaian R dan C paralel?", "id": "G ditambah j omega C." },
{ "en": "Admitansi total rangkaian R dan L paralel?", "id": "G ditambah satu per j omega L." },
{ "en": "Admitansi total rangkaian R, L, C paralel?", "id": "G ditambah j (BC minus BL)." },
{ "en": "Hukum Ohm dalam bentuk fasor?", "id": "V sama dengan I kali Z." },
{ "en": "Tegangan pada resistor dalam bentuk fasor?", "id": "Arus I dikali resistansi R." },
{ "en": "Tegangan pada induktor dalam bentuk fasor?", "id": "Arus I dikali j omega L." },
{ "en": "Tegangan pada kapasitor dalam bentuk fasor?", "id": "Arus I dibagi j omega C." },
{ "en": "Arus pada resistor dalam bentuk fasor?", "id": "Tegangan V dibagi resistansi R." },
{ "en": "Arus pada induktor dalam bentuk fasor?", "id": "Tegangan V dibagi j omega L." },
{ "en": "Arus pada kapasitor dalam bentuk fasor?", "id": "Tegangan V dikali j omega C." },
{ "en": "Kondisi reaktansi total sama dengan nol?", "id": "Resonansi." },
{ "en": "Dalam rangkaian seri, arus digunakan sebagai?", "id": "Fasor referensi." },
{ "en": "Dalam rangkaian paralel, tegangan digunakan sebagai?", "id": "Fasor referensi." },
{ "en": "Jumlah aljabar fasor tegangan dalam loop?", "id": "Sama dengan nol (KVL)." },
{ "en": "Jumlah aljabar fasor arus pada simpul?", "id": "Sama dengan nol (KCL)." },
{ "en": "Analisis dengan tegangan simpul sebagai variabel?", "id": "Analisis Simpul (Node Analysis)." },
{ "en": "Analisis dengan arus loop sebagai variabel?", "id": "Analisis Jala (Mesh Analysis)." },
{ "en": "Rangkaian ekuivalen sumber tegangan dan impedansi seri?", "id": "Ekuivalen Thevenin." },
{ "en": "Rangkaian ekuivalen sumber arus dan impedansi paralel?", "id": "Ekuivalen Norton." },
{ "en": "Tegangan pada terminal beban terbuka?", "id": "Tegangan Thevenin (Vth)." },
{ "en": "Arus melalui terminal beban terhubung singkat?", "id": "Arus Norton (In)." },
{ "en": "Impedansi dilihat dari terminal beban?", "id": "Impedansi Thevenin (Zth)." },
{ "en": "Hubungan Zth dan Z norton?", "id": "Keduanya sama (Zth = Zn)." },
{ "en": "Hubungan Vth, In, dan Zth?", "id": "Vth sama dengan In dikali Zth." },
{ "en": "Mengubah sumber tegangan seri ke sumber arus?", "id": "Transformasi Sumber." },
{ "en": "Respon total adalah jumlah respon individu?", "id": "Teorema Superposisi." },
{ "en": "Kapan superposisi digunakan?", "id": "Rangkaian dengan beberapa sumber independen." },
{ "en": "Saat menggunakan superposisi, sumber tegangan lain?", "id": "Dihubung singkat (short circuit)." },
{ "en": "Saat menggunakan superposisi, sumber arus lain?", "id": "Dibuka (open circuit)." },
{ "en": "Apakah superposisi berlaku untuk daya?", "id": "Tidak, karena daya tidak linier." },
{ "en": "Impedansi input dari sebuah rangkaian?", "id": "Rasio tegangan input dan arus input." },
{ "en": "Kombinasi dua impedansi paralel?", "id": "Hasil kali dibagi hasil jumlah." },
{ "en": "Rangkaian dengan hubungan bintang (Y)?", "id": "Tiga impedansi terhubung ke satu titik." },
{ "en": "Rangkaian dengan hubungan delta (Δ)?", "id": "Tiga impedansi terhubung membentuk loop." },
{ "en": "Transformasi dari delta ke bintang?", "id": "Transformasi Delta-Wye." },
{ "en": "Transformasi dari bintang ke delta?", "id": "Transformasi Wye-Delta." },
{ "en": "Jembatan untuk mengukur impedansi?", "id": "Jembatan AC (contoh: Jembatan Maxwell)." },
{ "en": "Kondisi setimbang pada jembatan AC?", "id": "Tegangan atau arus detektor nol." },
{ "en": "Rangkaian dengan penguat operasional (op-amp)?", "id": "Rangkaian Op-Amp AC." },
{ "en": "Penguat pembalik (inverting amplifier) AC?", "id": "Gain sama dengan minus Zf per Zi." },
{ "en": "Penguat tak-pembalik (non-inverting) AC?", "id": "Gain 1 plus Zf per Zi." },
{ "en": "Impedansi input op-amp ideal?", "id": "Tak hingga." },
{ "en": "Impedansi output op-amp ideal?", "id": "Nol." },
{ "en": "Arus yang masuk ke terminal input op-amp?", "id": "Nol." },
{ "en": "Rangkaian kapasitor di umpan balik op-amp?", "id": "Integrator." },
{ "en": "Rangkaian induktor di umpan balik op-amp?", "id": "Differentiator." },
{ "en": "Tujuan analisis AC?", "id": "Menemukan respon keadaan tunak." },
{ "en": "Langkah pertama dalam analisis AC?", "id": "Mengubah rangkaian ke domain frekuensi." },
{ "en": "Variabel dalam analisis simpul?", "id": "Fasor tegangan simpul." },
{ "en": "Variabel dalam analisis jala?", "id": "Fasor arus jala." },
{ "en": "Apakah teorema rangkaian DC berlaku di AC?", "id": "Ya, dengan impedansi dan fasor." },
{ "en": "Impedansi Thevenin untuk sumber dependen?", "id": "Menggunakan sumber uji tegangan atau arus." },
{ "en": "Bagaimana impedansi seri dijumlahkan secara grafis?", "id": "Penjumlahan vektor fasor." },
{ "en": "Jika reaktansi induktif lebih besar dari kapasitif?", "id": "Rangkaian bersifat induktif." },
{ "en": "Jika reaktansi kapasitif lebih besar dari induktif?", "id": "Rangkaian bersifat kapasitif." },
{ "en": "Jika reaktansi induktif sama dengan kapasitif?", "id": "Rangkaian bersifat resistif (resonansi)." },
{ "en": "Frekuensi saat XL sama dengan XC?", "id": "Frekuensi resonansi." },
{ "en": "Apa yang terjadi pada impedansi RLC seri saat resonansi?", "id": "Mencapai nilai minimum (sama dengan R)." },
{ "en": "Apa yang terjadi pada arus RLC seri saat resonansi?", "id": "Mencapai nilai maksimum." },
{ "en": "Apa yang terjadi pada admitansi RLC paralel saat resonansi?", "id": "Mencapai nilai minimum (sama dengan G)." },
{ "en": "Apa yang terjadi pada impedansi RLC paralel saat resonansi?", "id": "Mencapai nilai maksimum." },
{ "en": "Apakah frekuensi resonansi seri dan paralel sama?", "id": "Ya, untuk rangkaian RLC ideal." },
{ "en": "Rangkaian yang meloloskan frekuensi rendah?", "id": "Filter lolos bawah (Low-pass filter)." },
{ "en": "Rangkaian yang meloloskan frekuensi tinggi?", "id": "Filter lolos atas (High-pass filter)." },
{ "en": "Filter RC seri sederhana (output di C)?", "id": "Filter lolos bawah." },
{ "en": "Filter RC seri sederhana (output di R)?", "id": "Filter lolos atas." },
{ "en": "Filter RL seri sederhana (output di R)?", "id": "Filter lolos bawah." },
{ "en": "Filter RL seri sederhana (output di L)?", "id": "Filter lolos atas." },
{ "en": "Rangkaian yang meloloskan pita frekuensi tertentu?", "id": "Filter lolos pita (Band-pass filter)." },
{ "en": "Rangkaian RLC seri (output di R) adalah?", "id": "Filter lolos pita." },
{ "en": "Rangkaian yang menolak pita frekuensi tertentu?", "id": "Filter tolak pita (Band-stop filter)." },
{ "en": "Rangkaian RLC paralel (output di LC) adalah?", "id": "Filter tolak pita." },
{ "en": "Transformator ideal mengubah apa?", "id": "Tegangan dan arus." },
{ "en": "Apa yang konstan pada transformator ideal?", "id": "Daya." },
{ "en": "Rasio lilitan transformator?", "id": "Rasio tegangan." },
{ "en": "Transformator menaikkan tegangan disebut?", "id": "Transformator step-up." },
{ "en": "Transformator menurunkan tegangan disebut?", "id": "Transformator step-down." },
{ "en": "Fungsi transformator dalam analisis rangkaian?", "id": "Pencocokan impedansi (impedance matching)." },
{ "en": "Impedansi yang direfleksikan ke sisi primer?", "id": "Impedansi sekunder dikali kuadrat rasio lilitan." },
{ "en": "Rangkaian dengan induktor yang terkopel magnetis?", "id": "Induktor berpasangan (coupled inductors)." },
{ "en": "Ukuran kopling magnetis?", "id": "Induktansi timbal balik (Mutual Inductance)." },
{ "en": "Faktor kopling (k) bernilai antara?", "id": "Nol dan satu." },
{ "en": "Konvensi titik pada induktor berpasangan?", "id": "Menentukan polaritas tegangan induksi." },
{ "en": "Arus memasuki titik, tegangan di kumparan lain?", "id": "Positif di titik kumparan lain." },
{ "en": "Arus meninggalkan titik, tegangan di kumparan lain?", "id": "Negatif di titik kumparan lain." },
{ "en": "Rangkaian ekuivalen untuk induktor berpasangan?", "id": "Ekuivalen T (atau Pi)." },
{ "en": "Analisis rangkaian magnetik menggunakan konsep?", "id": "Reluktansi." },
{ "en": "Sistem AC dengan tiga sumber tegangan?", "id": "Sistem tiga fasa." },
{ "en": "Beda fasa antar tegangan dalam sistem tiga fasa?", "id": "Seratus dua puluh derajat." },
{ "en": "Urutan fasa ABC atau ACB disebut?", "id": "Urutan fasa (phase sequence)." },
{ "en": "Sistem tiga fasa dengan beban sama?", "id": "Sistem seimbang." },
{ "en": "Keuntungan sistem tiga fasa?", "id": "Daya konstan dan efisien." },
{ "en": "Hubungan sumber/beban tiga fasa?", "id": "Bintang (Y) atau Delta (Δ)." },
{ "en": "Titik netral ada pada hubungan?", "id": "Bintang (Y)." },
{ "en": "Tegangan antara satu fasa dan netral?", "id": "Tegangan fasa." },
{ "en": "Tegangan antara dua fasa?", "id": "Tegangan line (antar-fasa)." },
{ "en": "Arus yang mengalir pada satu fasa?", "id": "Arus fasa." },
{ "en": "Arus yang mengalir di saluran transmisi?", "id": "Arus line." },
{ "en": "Pada hubungan Y, arus line dan fasa?", "id": "Sama (IL = Iph)." },
{ "en": "Pada hubungan Δ, tegangan line dan fasa?", "id": "Sama (VL = Vph)." },
{ "en": "Hubungan VL dan Vph pada hubungan Y?", "id": "VL sama dengan Vph akar tiga." },
{ "en": "Hubungan IL dan Iph pada hubungan Δ?", "id": "IL sama dengan Iph akar tiga." },
{ "en": "Apa itu rangkaian RLC seri?", "id": "R, L, dan C terhubung seri." },
{ "en": "Impedansi total Z dalam RLC seri?", "id": "Akar (R^2 + (XL-XC)^2)." },
{ "en": "Sudut fasa dalam RLC seri?", "id": "Arctan ((XL-XC)/R)." },
{ "en": "Arus I dalam RLC seri?", "id": "V dibagi Z total." },
{ "en": "Tegangan pada R dalam RLC seri?", "id": "I dikali R." },
{ "en": "Tegangan pada L dalam RLC seri?", "id": "I dikali jXL." },
{ "en": "Tegangan pada C dalam RLC seri?", "id": "I dikali (-jXC)." },
{ "en": "Jumlah fasor tegangan VR, VL, VC?", "id": "Sama dengan tegangan sumber (VS)." },
{ "en": "Segitiga yang menghubungkan R, X, dan Z?", "id": "Segitiga Impedansi." },
{ "en": "Sisi miring segitiga impedansi?", "id": "Impedansi (Z)." },
{ "en": "Sisi datar segitiga impedansi?", "id": "Resistansi (R)." },
{ "en": "Sisi tegak segitiga impedansi?", "id": "Reaktansi (X)." },
{ "en": "Apa itu rangkaian RLC paralel?", "id": "R, L, dan C terhubung paralel." },
{ "en": "Admitansi total Y dalam RLC paralel?", "id": "Akar (G^2 + (BC-BL)^2)." },
{ "en": "Sudut fasa dalam RLC paralel?", "id": "Arctan ((BC-BL)/G)." },
{ "en": "Tegangan V dalam RLC paralel?", "id": "I total dibagi Y total." },
{ "en": "Arus pada R dalam RLC paralel?", "id": "V dikali G." },
{ "en": "Arus pada L dalam RLC paralel?", "id": "V dikali (-jBL)." },
{ "en": "Arus pada C dalam RLC paralel?", "id": "V dikali jBC." },
{ "en": "Jumlah fasor arus IR, IL, IC?", "id": "Sama dengan arus sumber (IS)." },
{ "en": "Segitiga yang menghubungkan G, B, dan Y?", "id": "Segitiga Admitansi." },
{ "en": "Aturan pembagi tegangan untuk impedansi?", "id": "Tegangan Zx = Vs * (Zx / Ztotal)." },
{ "en": "Kapan aturan pembagi tegangan digunakan?", "id": "Pada rangkaian seri." },
{ "en": "Aturan pembagi arus untuk admitansi?", "id": "Arus Yx = Is * (Yx / Ytotal)." },
{ "en": "Aturan pembagi arus untuk impedansi paralel?", "id": "Arus Zx = Is * (Zlain / (Zx+Zlain))." },
{ "en": "Kapan aturan pembagi arus digunakan?", "id": "Pada rangkaian paralel." },
{ "en": "Tegangan di Z2 pada rangkaian Z1-Z2 seri?", "id": "Vs * Z2 / (Z1+Z2)." },
{ "en": "Arus di Z2 pada rangkaian Z1-Z2 paralel?", "id": "Is * Z1 / (Z1+Z2)." },
{ "en": "Kapasitor ekuivalen untuk C1 dan C2 seri?", "id": "C1*C2 / (C1+C2)." },
{ "en": "Kapasitor ekuivalen untuk C1 dan C2 paralel?", "id": "C1 + C2." },
{ "en": "Induktor ekuivalen untuk L1 dan L2 seri?", "id": "L1 + L2." },
{ "en": "Induktor ekuivalen untuk L1 dan L2 paralel?", "id": "L1*L2 / (L1+L2)." },
{ "en": "Mengapa impedansi tidak bisa dijumlahkan seperti resistor?", "id": "Karena impedansi adalah besaran kompleks." },
{ "en": "Tegangan pada induktor mendahului arus sebesar?", "id": "90 derajat." },
{ "en": "Arus pada kapasitor mendahului tegangan sebesar?", "id": "90 derajat." },
{ "en": "Tegangan dan arus pada resistor?", "id": "Sefasa." },
{ "en": "Analisis mesh/jala menggunakan hukum apa?", "id": "Hukum Tegangan Kirchhoff (KVL)." },
{ "en": "Analisis node/simpul menggunakan hukum apa?", "id": "Hukum Arus Kirchhoff (KCL)." },
{ "en": "Jumlah impedansi dalam satu loop disebut?", "id": "Impedansi loop." },
{ "en": "Impedansi bersama antara dua loop?", "id": "Impedansi mutual." },
{ "en": "Berapa banyak persamaan simpul yang dibutuhkan?", "id": "Jumlah simpul dikurangi satu." },
{ "en": "Simpul yang tegangannya diasumsikan nol?", "id": "Simpul referensi atau ground." },
{ "en": "Berapa banyak persamaan jala yang dibutuhkan?", "id": "Jumlah jala (mesh) independen." },
{ "en": "Arah arus jala biasanya diasumsikan?", "id": "Searah jarum jam." },
{ "en": "Apa itu supermesh?", "id": "Loop KVL yang melewati sumber arus." },
{ "en": "Apa itu supernode?", "id": "Permukaan tertutup yang melingkupi sumber tegangan." },
{ "en": "Tujuan utama analisis rangkaian?", "id": "Menentukan arus dan tegangan." },
{ "en": "Langkah setelah mendapatkan persamaan simpul/jala?", "id": "Menyelesaikan sistem persamaan linier." },
{ "en": "Metode penyelesaian sistem persamaan?", "id": "Substitusi, eliminasi, atau aturan Cramer." },
{ "en": "Impedansi input rangkaian seri?", "id": "Jumlah semua impedansi." },
{ "en": "Admitansi input rangkaian paralel?", "id": "Jumlah semua admitansi." },
{ "en": "Rangkaian yang perilakunya tergantung frekuensi?", "id": "Rangkaian selektif frekuensi." },
{ "en": "Contoh rangkaian selektif frekuensi?", "id": "Filter atau rangkaian resonansi." },
{ "en": "Rasio output terhadap input sebagai fungsi frekuensi?", "id": "Fungsi transfer." },
{ "en": "Diagram fasor untuk RLC seri?", "id": "Menunjukkan hubungan fasa V, VR, VL, VC." },
{ "en": "Diagram fasor untuk RLC paralel?", "id": "Menunjukkan hubungan fasa I, IR, IL, IC." },
{ "en": "Dalam RLC seri, jika XL > XC, fasa V?", "id": "Mendahului I." },
{ "en": "Dalam RLC seri, jika XC > XL, fasa V?", "id": "Tertinggal dari I." },
{ "en": "Dalam RLC paralel, jika BC > BL, fasa I?", "id": "Mendahului V." },
{ "en": "Dalam RLC paralel, jika BL > BC, fasa I?", "id": "Tertinggal dari V." },
{ "en": "Rangkaian jembatan setimbang jika?", "id": "Impedansi lengan silang hasil kalinya sama." },
{ "en": "Transformasi Y-Δ digunakan untuk?", "id": "Menyederhanakan rangkaian jembatan tidak setimbang." },
{ "en": "Impedansi Zy pada transformasi Δ-Y?", "id": "Hasil kali dua impedansi delta terdekat." },
{ "en": "Impedansi ZΔ pada transformasi Y-Δ?", "id": "Jumlah perkalian pasangan impedansi Y." },
{ "en": "Analisis tunak berarti?", "id": "Semua tegangan dan arus adalah sinusoida." },
{ "en": "Frekuensi sudut (ω) memiliki satuan?", "id": "Radian per detik." },
{ "en": "Nilai RMS dari sinyal DC?", "id": "Sama dengan nilai DC itu sendiri." },
{ "en": "Bagaimana mengubah cosinus ke sinus?", "id": "cos(x) = sin(x + 90°)." },
{ "en": "Bagaimana mengubah -sinus ke cosinus?", "id": "-sin(x) = cos(x + 90°)." },
{ "en": "Resistansi R bergantung pada frekuensi?", "id": "Tidak, resistansi ideal tidak." },
{ "en": "Induktansi L bergantung pada frekuensi?", "id": "Tidak, induktansi ideal tidak." },
{ "en": "Kapasitansi C bergantung pada frekuensi?", "id": "Tidak, kapasitansi ideal tidak." },
{ "en": "Reaktansi X bergantung pada frekuensi?", "id": "Ya, reaktansi sangat bergantung frekuensi." },
{ "en": "Apakah analisis fasor bisa untuk sinyal non-sinusoidal?", "id": "Tidak secara langsung." },
{ "en": "Metode untuk sinyal non-sinusoidal periodik?", "id": "Analisis Deret Fourier." },
{ "en": "Apakah fasor adalah fungsi waktu?", "id": "Bukan, fasor adalah bilangan kompleks statis." },
{ "en": "Fasor I merepresentasikan gelombang?", "id": "i(t) = |I| cos(ωt + sudut I)." },
{ "en": "Magnitudo impedansi disebut juga?", "id": "Besar impedansi." },
{ "en": "Suseptansi bernilai positif milik komponen apa?", "id": "Kapasitor (Suseptansi Kapasitif)." },
{ "en": "Suseptansi bernilai negatif milik komponen apa?", "id": "Induktor (Suseptansi Induktif)." },
{ "en": "Kombinasi konduktansi dan suseptansi?", "id": "Admitansi." },
{ "en": "Langkah pertama transformasi sumber?", "id": "Hitung tegangan/arus ekuivalennya." },
{ "en": "Impedansi seri pada transformasi sumber?", "id": "Menjadi impedansi paralel." },
{ "en": "Impedansi paralel pada transformasi sumber?", "id": "Menjadi impedansi seri." },
{ "en": "Rangkaian dua terminal disebut juga?", "id": "Jaringan satu-port." },
{ "en": "Impedansi ekuivalen dari jaringan satu-port?", "id": "Rasio V/I di terminal." },
{ "en": "Kapan analisis jala lebih mudah?", "id": "Saat jumlah jala lebih sedikit." },
{ "en": "Kapan analisis simpul lebih mudah?", "id": "Saat jumlah simpul lebih sedikit." },
{ "en": "Analisis simpul menghasilkan matriks?", "id": "Matriks admitansi simpul." },
{ "en": "Analisis jala menghasilkan matriks?", "id": "Matriks impedansi jala." },
{ "en": "Elemen diagonal matriks admitansi simpul?", "id": "Jumlah admitansi terhubung ke simpul." },
{ "en": "Elemen non-diagonal matriks admitansi simpul?", "id": "Negatif admitansi antara dua simpul." },
{ "en": "Elemen diagonal matriks impedansi jala?", "id": "Jumlah impedansi dalam satu jala." },
{ "en": "Elemen non-diagonal matriks impedansi jala?", "id": "Negatif impedansi bersama dua jala." },
{ "en": "Solusi rangkaian AC memberikan informasi apa?", "id": "Magnitudo dan fasa." },
{ "en": "Apa yang terjadi jika frekuensi sumber berubah?", "id": "Nilai reaktansi dan impedansi berubah." },
{ "en": "Apakah KVL/KCL berlaku untuk nilai sesaat?", "id": "Ya, selalu berlaku." },
{ "en": "Apakah hukum Ohm berlaku untuk nilai sesaat?", "id": "Ya, untuk resistor." },
{ "en": "Hubungan V-I sesaat untuk induktor?", "id": "v(t) = L di(t)/dt." },
{ "en": "Hubungan V-I sesaat untuk kapasitor?", "id": "i(t) = C dv(t)/dt." },
{ "en": "Mengapa fasor menyederhanakan analisis?", "id": "Mengubah persamaan diferensial menjadi aljabar." },
{ "en": "Rangkaian yang mengandung sumber dependen?", "id": "Dianalisis dengan simpul atau jala." },
{ "en": "Impedansi ekuivalen untuk n impedansi seri?", "id": "Jumlah dari n impedansi." },
{ "en": "Admitansi ekuivalen untuk n admitansi paralel?", "id": "Jumlah dari n admitansi." },
{ "en": "Apa itu diagram impedansi?", "id": "Plot Z pada bidang kompleks." },
{ "en": "Apa itu diagram admitansi?", "id": "Plot Y pada bidang kompleks." },
{ "en": "Sumbu horizontal bidang kompleks?", "id": "Sumbu riil." },
{ "en": "Sumbu vertikal bidang kompleks?", "id": "Sumbu imajiner." },
{ "en": "Impedansi induktif berada di kuadran?", "id": "Kuadran pertama (atas)." },
{ "en": "Impedansi kapasitif berada di kuadran?", "id": "Kuadran keempat (bawah)." },
{ "en": "Reaktansi murni berada di sumbu?", "id": "Sumbu imajiner." },
{ "en": "Resistansi murni berada di sumbu?", "id": "Sumbu riil." },
{ "en": "Tujuan Teorema Thevenin?", "id": "Menyederhanakan rangkaian menjadi sumber tegangan." },
{ "en": "Komponen ekuivalen Thevenin?", "id": "Vth seri dengan Zth." },
{ "en": "Bagaimana cara mencari Tegangan Thevenin (Vth)?", "id": "Ukur tegangan terminal beban terbuka." },
{ "en": "Bagaimana cara mencari Impedansi Thevenin (Zth)?", "id": "Matikan sumber independen, hitung Zeq." },
{ "en": "Cara mematikan sumber tegangan independen?", "id": "Hubung singkat (short circuit)." },
{ "en": "Cara mematikan sumber arus independen?", "id": "Buka rangkaian (open circuit)." },
{ "en": "Bagaimana dengan sumber dependen saat mencari Zth?", "id": "Biarkan aktif, gunakan sumber uji." },
{ "en": "Tujuan Teorema Norton?", "id": "Menyederhanakan rangkaian menjadi sumber arus." },
{ "en": "Komponen ekuivalen Norton?", "id": "In paralel dengan Zn." },
{ "en": "Bagaimana cara mencari Arus Norton (In)?", "id": "Ukur arus terminal beban terhubung singkat." },
{ "en": "Impedansi Norton (Zn) sama dengan?", "id": "Impedansi Thevenin (Zth)." },
{ "en": "Teorema Thevenin dan Norton adalah?", "id": "Saling dual." },
{ "en": "Setelah dapat rangkaian Thevenin, arus beban?", "id": "Vth dibagi (Zth + ZL)." },
{ "en": "Setelah dapat rangkaian Norton, tegangan beban?", "id": "In dikali impedansi paralel total." },
{ "en": "Teorema untuk rangkaian linier dengan banyak sumber?", "id": "Teorema Superposisi." },
{ "en": "Prinsip dasar Teorema Superposisi?", "id": "Analisis satu sumber pada satu waktu." },
{ "en": "Respon akhir dalam superposisi?", "id": "Jumlah aljabar semua respon parsial." },
{ "en": "Apakah superposisi berlaku untuk sumber frekuensi berbeda?", "id": "Ya, analisis satu frekuensi per waktu." },
{ "en": "Apakah superposisi bisa menghitung daya?", "id": "Tidak secara langsung, harus dari V/I total." },
{ "en": "Kondisi agar transfer daya ke beban maksimal?", "id": "Impedansi beban konjugat impedansi sumber." },
{ "en": "Ini adalah pernyataan dari teorema?", "id": "Teorema Transfer Daya Maksimum." },
{ "en": "Jika Zs = Rs + jXs, ZL optimal?", "id": "ZL = Rs - jXs." },
{ "en": "Kondisi ini disebut?", "id": "Pencocokan konjugat (conjugate matching)." },
{ "en": "Apa yang terjadi jika ZL = Zs?", "id": "Bukan transfer daya maksimum." },
{ "en": "Berapa daya maksimum yang ditransfer?", "id": "Kuadrat Vth dibagi 4 kali Rs." },
{ "en": "Jika beban hanya bisa resistif, RL optimal?", "id": "RL sama dengan magnitudo Zs." },
{ "en": "Efisiensi saat transfer daya maksimum?", "id": "Lima puluh persen." },
{ "en": "Di mana aplikasi transfer daya maksimum penting?", "id": "Sistem komunikasi dan frekuensi tinggi." },
{ "en": "Di mana efisiensi lebih penting dari daya maks?", "id": "Sistem transmisi daya listrik." },
{ "en": "Teorema Millman digunakan untuk?", "id": "Mencari tegangan pada cabang paralel." },
{ "en": "Bentuk umum Teorema Millman?", "id": "V = (Σ I) / (Σ Y)." },
{ "en": "Teorema Resiprositas menyatakan?", "id": "Rasio eksitasi dan respon tidak berubah." },
{ "en": "Kondisi Teorema Resiprositas?", "id": "Jaringan linier, pasif, dan bilateral." },
{ "en": "Apa itu jaringan bilateral?", "id": "Sifatnya sama untuk kedua arah arus." },
{ "en": "Contoh komponen non-bilateral?", "id": "Dioda atau sumber dependen." },
{ "en": "Teorema Substitusi menyatakan?", "id": "Cabang dapat diganti sumber independen." },
{ "en": "Nilai sumber substitusi?", "id": "Sama dengan tegangan/arus cabang asli." },
{ "en": "Teorema Kompensasi berhubungan dengan?", "id": "Efek perubahan kecil pada impedansi." },
{ "en": "Perubahan impedansi dapat diganti dengan?", "id": "Sumber tegangan kompensasi." },
{ "en": "Teorema Thevenin menyederhanakan sisa rangkaian menjadi?", "id": "Satu port aktif." },
{ "en": "Teorema apa yang paling fundamental?", "id": "Superposisi, karena membuktikan kelinieran." },
{ "en": "Apa yang terjadi pada sumber dependen saat superposisi?", "id": "Selalu dibiarkan aktif." },
{ "en": "Dapatkah Thevenin/Norton digunakan pada sumber dependen?", "id": "Ya, tapi Zth dicari dengan sumber uji." },
{ "en": "Tegangan sumber uji (Vt) dibagi arus uji (It)?", "id": "Impedansi Thevenin (Zth)." },
{ "en": "Mengapa menggunakan sumber uji 1V atau 1A?", "id": "Untuk menyederhanakan perhitungan." },
{ "en": "Jika Zth negatif, artinya apa?", "id": "Rangkaian memberikan daya (aktif)." },
{ "en": "Pencocokan impedansi untuk apa?", "id": "Transfer daya maksimum atau mengurangi pantulan." },
{ "en": "Jika ZL = R, transfer daya maks saat?", "id": "RL = |Zth|." },
{ "en": "Jika hanya reaktansi beban bisa diubah?", "id": "Set XL = -Xs." },
{ "en": "Jika hanya resistansi beban bisa diubah?", "id": "Set RL = |Rs + j(Xs+XL)|." },
{ "en": "Teorema Thevenin pada domain waktu?", "id": "Tidak umum, biasanya di domain frekuensi." },
{ "en": "Apakah teorema ini berlaku untuk rangkaian nonlinier?", "id": "Tidak." },
{ "en": "Teorema yang paling berguna untuk analisis sensitivitas?", "id": "Teorema Kompensasi." },
{ "en": "Transformasi sumber adalah aplikasi dari teorema?", "id": "Thevenin dan Norton." },
{ "en": "Menentukan Vth untuk rangkaian jembatan?", "id": "Selisih tegangan antara dua titik tengah." },
{ "en": "Menentukan Zth untuk rangkaian jembatan?", "id": "Paralelkan seri, lalu serikan." },
{ "en": "Superposisi sangat membantu jika sumber punya?", "id": "Frekuensi yang berbeda-beda." },
{ "en": "Respon karena sumber DC pada rangkaian AC?", "id": "Analisis DC (L=short, C=open)." },
{ "en": "Respon karena sumber AC pada rangkaian AC?", "id": "Analisis fasor." },
{ "en": "Respon total jika ada sumber AC dan DC?", "id": "Jumlah respon DC dan AC." },
{ "en": "Apakah Vth dan In selalu ada?", "id": "Ya, untuk rangkaian linier." },
{ "en": "Nilai Zth bisa murni resistif?", "id": "Ya." },
{ "en": "Nilai Zth bisa murni reaktif?", "id": "Ya." },
{ "en": "Nilai Zth bisa kompleks?", "id": "Ya, paling umum." },
{ "en": "Nilai Vth adalah fasor atau skalar?", "id": "Fasor (bilangan kompleks)." },
{ "en": "Nilai In adalah fasor atau skalar?", "id": "Fasor (bilangan kompleks)." },
{ "en": "Superposisi pada op-amp?", "id": "Ya, sangat berguna." },
{ "en": "Transfer daya maksimum pada transformator?", "id": "Saat impedansi beban terefleksi cocok." },
{ "en": "Teorema manakah yang paling sering digunakan?", "id": "Thevenin." },
{ "en": "Bagaimana menemukan In dari Thevenin?", "id": "In = Vth / Zth." },
{ "en": "Bagaimana menemukan Vth dari Norton?", "id": "Vth = In * Zn." },
{ "en": "Apakah Zth tergantung pada beban?", "id": "Tidak, hanya tergantung pada sumber rangkaian." },
{ "en": "Kapan Zth sama dengan nol?", "id": "Untuk sumber tegangan ideal." },
{ "en": "Kapan Zth tak hingga?", "id": "Untuk sumber arus ideal." },
{ "en": "Dapatkah kita menggunakan superposisi pada sumber dependen?", "id": "Tidak, sumber dependen selalu aktif." },
{ "en": "Teorema Thevenin menyederhanakan perhitungan untuk?", "id": "Beban yang bervariasi." },
{ "en": "Berapa nilai daya reaktif saat transfer daya maks?", "id": "Nol, karena reaktansi saling meniadakan." },
{ "en": "Teorema berlaku untuk semua rangkaian linier?", "id": "Ya, selama linier." },
{ "en": "Penerapan transformasi Y-Δ?", "id": "Menyederhanakan sebelum menerapkan teorema lain." },
{ "en": "Kapan rangkaian dikatakan resiprokal?", "id": "Jika memenuhi Teorema Resiprositas." },
{ "en": "Apakah rangkaian dengan op-amp resiprokal?", "id": "Tidak, karena op-amp adalah komponen aktif." },
{ "en": "Kondisi transfer daya maksimum adalah untuk daya?", "id": "Daya rata-rata." },
{ "en": "Semua teorema ini adalah alat untuk?", "id": "Menyederhanakan analisis rangkaian." },
{ "en": "Ekuivalen Thevenin adalah model rangkaian?", "id": "Model sumber tegangan praktis." },
{ "en": "Ekuivalen Norton adalah model rangkaian?", "id": "Model sumber arus praktis." },
{ "en": "Teorema apa yang berhubungan dengan dualitas?", "id": "Thevenin dan Norton." },
{ "en": "Dualitas antara tegangan dan arus?", "id": "Tegangan dual dengan arus." },
{ "en": "Dualitas antara impedansi dan admitansi?", "id": "Impedansi dual dengan admitansi." },
{ "en": "Dualitas antara seri dan paralel?", "id": "Seri dual dengan paralel." },
{ "en": "Dualitas antara KVL dan KCL?", "id": "KVL dual dengan KCL." },
{ "en": "Dualitas antara loop dan simpul?", "id": "Loop dual dengan simpul." },
{ "en": "Sumber tegangan DC pada analisis AC?", "id": "Dianggap frekuensi nol." },
{ "en": "Langkah akhir setelah superposisi frekuensi berbeda?", "id": "Jumlahkan respon domain waktu." },
{ "en": "Teorema mana yang mengurangi jumlah jala/simpul?", "id": "Transformasi Sumber." },
{ "en": "Teorema mana yang paling baik untuk satu output?", "id": "Thevenin atau Norton." },
{ "en": "Jika Zth = 0, Vth?", "id": "Tegangan sumber ideal." },
{ "en": "Jika Zn tak hingga, In?", "id": "Arus sumber ideal." },
{ "en": "Superposisi adalah manifestasi dari sifat?", "id": "Sifat linieritas." },
{ "en": "Rangkaian disebut linier jika memenuhi?", "id": "Homogenitas dan aditivitas." },
{ "en": "Apakah Zth bisa diukur secara praktis?", "id": "Ya, dengan meteran impedansi." },
{ "en": "Apakah Vth bisa diukur secara praktis?", "id": "Ya, dengan voltmeter AC." },
{ "en": "Apakah In bisa diukur secara praktis?", "id": "Ya, dengan ammeter AC (hati-hati)." },
{ "en": "Mengapa pengukuran In harus hati-hati?", "id": "Karena dapat menyebabkan arus sangat besar." },
{ "en": "Teorema Thevenin berlaku untuk port mana pun?", "id": "Ya, di antara dua terminal mana pun." },
{ "en": "Daya pada suatu waktu t?", "id": "Daya sesaat, p(t)." },
{ "en": "Rumus daya sesaat?", "id": "p(t) = v(t) dikali i(t)." },
{ "en": "Satuan daya sesaat?", "id": "Watt (W)." },
{ "en": "Daya rata-rata selama satu periode?", "id": "Daya rata-rata atau daya aktif (P)." },
{ "en": "Daya rata-rata juga disebut?", "id": "Daya riil atau nyata." },
{ "en": "Satuan daya rata-rata?", "id": "Watt (W)." },
{ "en": "Komponen yang menyerap daya rata-rata?", "id": "Hanya resistor." },
{ "en": "Daya rata-rata pada induktor murni?", "id": "Nol." },
{ "en": "Daya rata-rata pada kapasitor murni?", "id": "Nol." },
{ "en": "Rumus daya rata-rata (P)?", "id": "Vrms Irms cosinus theta." },
{ "en": "Rumus lain untuk daya rata-rata P?", "id": "Kuadrat Irms dikali R." },
{ "en": "Cosinus dari selisih sudut fasa V dan I?", "id": "Faktor Daya (Power Factor, PF)." },
{ "en": "Nilai faktor daya berkisar antara?", "id": "Nol dan satu." },
{ "en": "Faktor daya untuk beban resistif murni?", "id": "Satu (sefasa)." },
{ "en": "Faktor daya untuk beban reaktif murni?", "id": "Nol (beda fasa 90 derajat)." },
{ "en": "Jika arus tertinggal dari tegangan, PF?", "id": "Lagging (tertinggal)." },
{ "en": "Beban yang menyebabkan PF lagging?", "id": "Beban induktif." },
{ "en": "Jika arus mendahului tegangan, PF?", "id": "Leading (mendahului)." },
{ "en": "Beban yang menyebabkan PF leading?", "id": "Beban kapasitif." },
{ "en": "Daya yang bolak-balik antara sumber dan beban?", "id": "Daya reaktif (Q)." },
{ "en": "Satuan daya reaktif?", "id": "Volt-Ampere Reactive (VAR)." },
{ "en": "Daya reaktif diserap oleh?", "id": "Induktor (Q positif)." },
{ "en": "Daya reaktif disuplai oleh?", "id": "Kapasitor (Q negatif)." },
{ "en": "Rumus daya reaktif (Q)?", "id": "Vrms Irms sinus theta." },
{ "en": "Rumus lain untuk daya reaktif Q?", "id": "Kuadrat Irms dikali X." },
{ "en": "Kombinasi daya aktif dan reaktif?", "id": "Daya kompleks (S)." },
{ "en": "Rumus daya kompleks S?", "id": "S = P + jQ." },
{ "en": "Rumus lain daya kompleks S?", "id": "Fasor V dikali konjugat fasor I." },
{ "en": "Magnitudo dari daya kompleks |S|?", "id": "Daya semu (Apparent Power)." },
{ "en": "Satuan daya semu?", "id": "Volt-Ampere (VA)." },
{ "en": "Rumus daya semu |S|?", "id": "Vrms dikali Irms." },
{ "en": "Hubungan P, |S|, dan PF?", "id": "P = |S| dikali PF." },
{ "en": "Representasi grafis P, Q, dan S?", "id": "Segitiga Daya." },
{ "en": "Sisi datar segitiga daya?", "id": "Daya aktif (P)." },
{ "en": "Sisi tegak segitiga daya?", "id": "Daya reaktif (Q)." },
{ "en": "Sisi miring segitiga daya?", "id": "Daya semu (|S|)." },
{ "en": "Sudut segitiga daya?", "id": "Sudut impedansi (theta)." },
{ "en": "Faktor daya dari segitiga daya?", "id": "Cosinus sudut segitiga daya." },
{ "en": "Total daya kompleks dalam suatu sistem?", "id": "Jumlah aljabar daya kompleks tiap komponen." },
{ "en": "Apakah daya aktif bisa dijumlahkan?", "id": "Ya, Ptotal = Σ P." },
{ "en": "Apakah daya reaktif bisa dijumlahkan?", "id": "Ya, Qtotal = Σ Q." },
{ "en": "Apakah daya semu bisa dijumlahkan?", "id": "Tidak, |S|total bukan Σ |S|." },
{ "en": "Daya yang tertera pada tagihan listrik?", "id": "Energi (daya aktif x waktu)." },
{ "en": "Satuan energi listrik?", "id": "Watt-jam (Wh) atau kilowatt-jam (kWh)." },
{ "en": "Daya sesaat p(t) terdiri dari?", "id": "Komponen konstan dan komponen frekuensi ganda." },
{ "en": "Komponen konstan dari p(t) adalah?", "id": "Daya rata-rata (P)." },
{ "en": "Frekuensi komponen AC dari p(t)?", "id": "Dua kali frekuensi sumber (2ω)." },
{ "en": "Jika p(t) selalu positif, beban?", "id": "Resistif." },
{ "en": "Jika p(t) positif dan negatif, beban?", "id": "Mengandung reaktansi." },
{ "en": "Alat untuk mengukur daya aktif?", "id": "Wattmeter." },
{ "en": "Alat untuk mengukur daya reaktif?", "id": "VARmeter." },
{ "en": "Nilai PF yang diinginkan industri?", "id": "Mendekati satu." },
{ "en": "Mengapa PF rendah tidak diinginkan?", "id": "Menyebabkan arus lebih tinggi untuk daya sama." },
{ "en": "Arus lebih tinggi menyebabkan?", "id": "Kerugian daya (I^2 R) lebih besar." },
{ "en": "Cara memperbaiki faktor daya lagging?", "id": "Menambahkan kapasitor secara paralel." },
{ "en": "Tujuan koreksi faktor daya?", "id": "Mengurangi daya reaktif total." },
{ "en": "Kapasitor yang ditambahkan menyuplai apa?", "id": "Daya reaktif (VAR)." },
{ "en": "Daya reaktif kapasitor (Qc) nilainya?", "id": "Negatif." },
{ "en": "Daya aktif setelah koreksi PF?", "id": "Tetap sama." },
{ "en": "Daya semu setelah koreksi PF?", "id": "Menurun." },
{ "en": "Arus total dari sumber setelah koreksi PF?", "id": "Menurun." },
{ "en": "Daya kompleks konjugat S*?", "id": "P minus jQ." },
{ "en": "Rumus lain daya kompleks S?", "id": "Kuadrat magnitudo V dibagi konjugat Z." },
{ "en": "Rumus lain daya kompleks S (arus)?", "id": "Kuadrat magnitudo I dikali Z." },
{ "en": "Daya maksimum ditransfer saat beban?", "id": "Konjugat impedansi Thevenin." },
{ "en": "Daya rata-rata pada Z = R+jX?", "id": "1/2 * Vm * Im * cos(θv - θi)." },
{ "en": "Daya pada beban resistif murni?", "id": "P = Vrms * Irms." },
{ "en": "Daya pada beban induktif murni?", "id": "Q = Vrms * Irms." },
{ "en": "Daya pada beban kapasitif murni?", "id": "Q = -Vrms * Irms." },
{ "en": "Konservasi daya kompleks berarti?", "id": "Total daya kompleks disuplai sama dengan diserap." },
{ "en": "Daya kompleks yang disuplai oleh sumber?", "id": "S = -V * I*." },
{ "en": "Faktor daya satu disebut juga?", "id": "Faktor daya uniti." },
{ "en": "Jika P=0 tapi Q tidak nol, beban?", "id": "Reaktif murni." },
{ "en": "Jika Q=0 tapi P tidak nol, beban?", "id": "Resistif murni." },
{ "en": "Segitiga daya untuk beban kapasitif?", "id": "Q menunjuk ke bawah." },
{ "en": "Segitiga daya untuk beban induktif?", "id": "Q menunjuk ke atas." },
{ "en": "Jika beban kompleks, daya sesaat?", "id": "Berfluktuasi antara positif dan negatif." },
{ "en": "Bagaimana daya diukur di laboratorium?", "id": "Menggunakan wattmeter dengan kumparan arus dan tegangan." },
{ "en": "Apakah daya reaktif melakukan kerja nyata?", "id": "Tidak, diperlukan untuk medan magnet/listrik." },
{ "en": "Daya yang terkait dengan energi tersimpan?", "id": "Daya reaktif." },
{ "en": "Daya yang terkait dengan energi terdisipasi?", "id": "Daya aktif." },
{ "en": "Total daya semu dari dua beban?", "id": "Jumlah vektor dari daya kompleksnya." },
{ "en": "Faktor daya keseluruhan dari banyak beban?", "id": "Ptotal dibagi |Stotal|." },
{ "en": "Koreksi PF mengubah sudut impedansi menjadi?", "id": "Lebih kecil." },
{ "en": "Daya rata-rata maksimum terjadi saat?", "id": "Sudut impedansi nol (resonansi)." },
{ "en": "Apakah daya sesaat bisa negatif?", "id": "Ya, artinya daya kembali ke sumber." },
{ "en": "Kapan p(t) negatif?", "id": "Saat beban reaktif melepas energi." },
{ "en": "Perusahaan listrik mengenakan denda untuk?", "id": "Faktor daya yang rendah." },
{ "en": "Bank kapasitor digunakan untuk?", "id": "Koreksi faktor daya skala besar." },
{ "en": "Motor induksi adalah beban bersifat?", "id": "Induktif (PF lagging)." },
{ "en": "Kabel panjang memiliki sifat?", "id": "Kapasitif (PF leading)." },
{ "en": "Apa itu daya distorsi?", "id": "Daya akibat komponen harmonisa." },
{ "en": "Faktor daya sebenarnya (true PF) memperhitungkan?", "id": "Harmonisa." },
{ "en": "Daya yang dibutuhkan untuk membentuk medan magnet?", "id": "Daya reaktif induktif." },
{ "en": "Daya yang dibutuhkan untuk membentuk medan listrik?", "id": "Daya reaktif kapasitif." },
{ "en": "Jika S = 800 + j600 VA, P?", "id": "800 Watt." },
{ "en": "Jika S = 800 + j600 VA, Q?", "id": "600 VAR, induktif." },
{ "en": "Jika S = 800 + j600 VA, |S|?", "id": "1000 VA." },
{ "en": "Jika S = 800 + j600 VA, PF?", "id": "0.8 lagging." },
{ "en": "Tujuan utama koreksi faktor daya?", "id": "Meningkatkan efisiensi sistem tenaga." },
{ "en": "Apa yang dikoreksi dalam PF correction?", "id": "Faktor daya (mendekati 1)." },
{ "en": "Komponen yang biasa digunakan untuk koreksi PF?", "id": "Kapasitor." },
{ "en": "Bagaimana kapasitor dikoneksikan untuk koreksi PF?", "id": "Paralel dengan beban." },
{ "en": "Mengapa kapasitor dipasang paralel?", "id": "Agar tidak mengubah tegangan kerja beban." },
{ "en": "Beban apa yang umumnya membutuhkan koreksi PF?", "id": "Beban induktif seperti motor." },
{ "en": "Kapasitor memberikan daya reaktif yang bersifat?", "id": "Kapasitif (negatif)." },
{ "en": "Daya reaktif kapasitor akan melawan daya?", "id": "Reaktif induktif (positif)." },
{ "en": "Apakah koreksi PF mengubah daya aktif (P)?", "id": "Tidak, P beban tetap." },
{ "en": "Apakah koreksi PF mengubah daya reaktif (Q) beban?", "id": "Tidak, hanya Q total dari sumber." },
{ "en": "Q baru setelah koreksi?", "id": "Q lama dikurangi Q kapasitor." },
{ "en": "Daya semu baru setelah koreksi?", "id": "Lebih kecil dari daya semu lama." },
{ "en": "Arus yang ditarik dari sumber setelah koreksi?", "id": "Lebih kecil." },
{ "en": "Segitiga daya setelah koreksi?", "id": "Sisi tegak (Q) menjadi lebih pendek." },
{ "en": "Rumus menghitung Q kapasitor yang dibutuhkan?", "id": "P dikali (tan θ_lama - tan θ_baru)." },
{ "en": "Rumus menghitung kapasitansi C dari Qc?", "id": "C = Qc / (ω * Vrms^2)." },
{ "en": "Apa itu koreksi berlebih (over-correction)?", "id": "Faktor daya menjadi leading." },
{ "en": "Apakah koreksi berlebih baik?", "id": "Tidak, sama-sama tidak efisien." },
{ "en": "Faktor daya target biasanya berapa?", "id": "Sekitar 0.95 lagging." },
{ "en": "Teorema transfer daya maksimum berlaku untuk?", "id": "Daya rata-rata." },
{ "en": "Kondisi transfer daya maksimum dari sumber ke beban?", "id": "ZL = Zth* (konjugat)." },
{ "en": "Apa arti ZL = Zth*?", "id": "RL = Rth dan XL = -Xth." },
{ "en": "Jika Xth induktif, XL harus?", "id": "Kapasitif dengan nilai sama." },
{ "en": "Jika Xth kapasitif, XL harus?", "id": "Induktif dengan nilai sama." },
{ "en": "Apa efek pencocokan reaktansi?", "id": "Menghilangkan daya reaktif, menyebabkan resonansi." },
{ "en": "Daya maksimum (Pmax) yang bisa ditransfer?", "id": "|Vth|^2 / (4 * Rth)." },
{ "en": "Kondisi ini disebut pencocokan?", "id": "Pencocokan konjugat (conjugate matching)." },
{ "en": "Jika magnitudo beban |ZL| bisa diubah?", "id": "Daya maks saat |ZL| = |Zth|." },
{ "en": "Jika hanya resistansi beban RL bisa diubah?", "id": "Daya maks saat RL = |Zth|." },
{ "en": "Efisiensi saat transfer daya maksimum?", "id": "50%." },
{ "en": "Mengapa efisiensi hanya 50%?", "id": "Daya yang sama hilang di Rth." },
{ "en": "Apakah transfer daya maksimum selalu diinginkan?", "id": "Tidak, seringkali efisiensi lebih penting." },
{ "en": "Contoh di mana efisiensi lebih utama?", "id": "Transmisi daya PLN." },
{ "en": "Contoh di mana daya maksimum lebih utama?", "id": "Output penguat ke antena." },
{ "en": "Jaringan pencocokan impedansi (matching network)?", "id": "Rangkaian untuk membuat ZL terlihat konjugat." },
{ "en": "Komponen yang digunakan untuk matching network?", "id": "Induktor dan kapasitor (tanpa rugi-rugi)." },
{ "en": "Beban apa yang ditarik oleh saluran transmisi?", "id": "Daya aktif dan reaktif." },
{ "en": "Dampak arus tinggi pada kabel?", "id": "Pemanasan berlebih dan jatuh tegangan." },
{ "en": "Keuntungan PF tinggi bagi konsumen?", "id": "Tagihan listrik lebih rendah (jika ada denda PF)." },
{ "en": "Keuntungan PF tinggi bagi perusahaan listrik?", "id": "Kapasitas jaringan meningkat." },
{ "en": "Dapatkah motor sinkron memperbaiki PF?", "id": "Ya, dengan eksitasi berlebih (over-excited)." },
{ "en": "Motor sinkron yang dioperasikan demikian disebut?", "id": "Kondensor sinkron." },
{ "en": "Koreksi PF untuk beban tiga fasa?", "id": "Menggunakan bank kapasitor tiga fasa." },
{ "en": "Bagaimana bank kapasitor tiga fasa terhubung?", "id": "Bisa hubungan bintang atau delta." },
{ "en": "Koreksi PF bersifat statis atau dinamis?", "id": "Bisa keduanya (kapasitor tetap atau otomatis)." },
{ "en": "Kapan transfer daya maksimum tidak menghasilkan 50% efisiensi?", "id": "Jika beban hanya bisa resistif murni." },
{ "en": "Apakah daya reaktif ditransfer dari sumber?", "id": "Tidak, hanya berosilasi dalam rangkaian." },
{ "en": "Kapasitor koreksi PF menyediakan jalur lokal untuk?", "id": "Arus reaktif." },
{ "en": "Faktor daya lagging berarti beban menyerap?", "id": "Daya reaktif (Q > 0)." },
{ "en": "Faktor daya leading berarti beban menyuplai?", "id": "Daya reaktif (Q < 0)." },
{ "en": "Jika PF = 1, daya reaktif Q?", "id": "Nol." },
{ "en": "Jika PF = 1, daya semu |S|?", "id": "|S| = P." },
{ "en": "Jika PF = 1, arus dari sumber?", "id": "Minimum untuk daya P yang sama." },
{ "en": "Menambahkan kapasitor secara seri akan?", "id": "Mengubah impedansi total, tidak untuk koreksi PF." },
{ "en": "Teorema transfer daya maksimum mencari nilai?", "id": "Daya rata-rata maksimum." },
{ "en": "Jika Zth murni resistif, ZL optimal?", "id": "Resistif murni dengan nilai sama." },
{ "en": "Jika Zth murni reaktif?", "id": "Transfer daya rata-rata maksimum adalah nol." },
{ "en": "Mengapa Pmax nol jika Zth reaktif?", "id": "Karena Rth nol, Pmax tak hingga teoritis." },
{ "en": "Dalam praktek, Zth selalu memiliki komponen?", "id": "Resistif (Rth > 0)." },
{ "en": "Langkah pertama menghitung Pmax?", "id": "Mencari Vth dan Zth." },
{ "en": "Koreksi PF juga mengurangi?", "id": "Jatuh tegangan pada saluran." },
{ "en": "Apakah faktor daya bisa lebih dari 1?", "id": "Tidak mungkin." },
{ "en": "Apakah daya aktif bisa negatif?", "id": "Ya, jika beban adalah sumber." },
{ "en": "Apakah daya reaktif bisa negatif?", "id": "Ya, untuk beban kapasitif." },
{ "en": "Koreksi PF untuk beban non-linier?", "id": "Lebih rumit, butuh filter harmonisa." },
{ "en": "Menentukan PF dari pembacaan wattmeter, voltmeter, ammeter?", "id": "PF = P / (V * I)." },
{ "en": "Daya reaktif yang dibutuhkan kapasitor adalah?", "id": "Selisih daya reaktif sebelum dan sesudah." },
{ "en": "Jika daya semu S = 10kVA, PF = 0.8 lagging, P?", "id": "8 kW." },
{ "en": "Jika S = 10kVA, PF = 0.8 lagging, Q?", "id": "6 kVAR." },
{ "en": "Tujuan menaikkan PF dari 0.8 ke 0.95?", "id": "Mengurangi Q dari 6kVAR menjadi lebih kecil." },
{ "en": "Pencocokan impedansi penting untuk menghindari?", "id": "Gelombang pantul (reflections)." },
{ "en": "Gelombang pantul terjadi di?", "id": "Saluran transmisi frekuensi tinggi." },
{ "en": "Kondisi transfer daya maksimum adalah untuk sumber?", "id": "Sumber praktis (dengan Zth)." },
{ "en": "Untuk sumber ideal, daya maksimum?", "id": "Tak hingga (secara teoritis)." },
{ "en": "Koreksi PF adalah isu dalam sistem?", "id": "Distribusi dan industri." },
{ "en": "Apakah rumah tangga perlu koreksi PF?", "id": "Umumnya tidak, karena beban kecil." },
{ "en": "Menambahkan terlalu banyak kapasitansi menyebabkan?", "id": "Tegangan naik dan PF leading." },
{ "en": "Peralatan yang mengatur kapasitor secara otomatis?", "id": "Automatic Power Factor Controller (APFC)." },
{ "en": "Daya yang hilang di saluran transmisi?", "id": "PLoss = I^2 * R_saluran." },
{ "en": "Bagaimana koreksi PF mengurangi PLoss?", "id": "Dengan mengurangi arus total (I)." },
{ "en": "Pencocokan konjugat memaksimalkan?", "id": "Penyerapan daya oleh beban." },
{ "en": "Koreksi PF memaksimalkan?", "id": "Efisiensi pengiriman daya." },
{ "en": "Daya reaktif diukur dalam satuan?", "id": "VAR (Volt-Ampere Reactive)." },
{ "en": "Daya semu diukur dalam satuan?", "id": "VA (Volt-Ampere)." },
{ "en": "Daya aktif diukur dalam satuan?", "id": "W (Watt)." },
{ "en": "Jika sudut impedansi positif, beban?", "id": "Induktif." },
{ "en": "Jika sudut impedansi negatif, beban?", "id": "Kapasitif." },
{ "en": "Jika sudut impedansi nol, beban?", "id": "Resistif." },
{ "en": "Koreksi PF mengurangi sudut antara?", "id": "Tegangan dan arus sumber." },
{ "en": "Koreksi PF tidak mengubah sudut?", "id": "Sudut impedansi beban." },
{ "en": "Pencocokan impedansi pada audio?", "id": "Menghubungkan speaker ke amplifier." },
{ "en": "Jika ZL tidak bisa konjugat, P maks?", "id": "Terjadi saat turunan daya terhadap RL nol." },
{ "en": "Apakah rugi-rugi kapasitor diabaikan?", "id": "Ya, dalam perhitungan koreksi PF dasar." },
{ "en": "Apakah transformator bisa mencocokkan impedansi?", "id": "Ya, dengan mengubah rasio lilitan." },
{ "en": "Fenomena saat reaktansi total rangkaian nol?", "id": "Resonansi." },
{ "en": "Pada resonansi, reaktansi induktif XL?", "id": "Sama dengan reaktansi kapasitif XC." },
{ "en": "Frekuensi saat resonansi terjadi?", "id": "Frekuensi resonansi (ωo atau fo)." },
{ "en": "Rumus frekuensi resonansi sudut (ωo)?", "id": "Satu per akar LC." },
{ "en": "Pada resonansi, impedansi rangkaian bersifat?", "id": "Murni resistif." },
{ "en": "Pada resonansi, faktor daya rangkaian?", "id": "Satu (uniti)." },
{ "en": "Apa yang terjadi pada impedansi RLC seri saat resonansi?", "id": "Minimum (Z = R)." },
{ "en": "Apa yang terjadi pada arus RLC seri saat resonansi?", "id": "Maksimum (I = V/R)." },
{ "en": "Apa yang terjadi pada admitansi RLC paralel saat resonansi?", "id": "Minimum (Y = G)." },
{ "en": "Apa yang terjadi pada impedansi RLC paralel saat resonansi?", "id": "Maksimum (Z = R)." },
{ "en": "Apa yang terjadi pada arus RLC paralel saat resonansi?", "id": "Minimum." },
{ "en": "Tegangan pada L dan C di RLC seri saat resonansi?", "id": "Bisa sangat besar, saling meniadakan." },
{ "en": "Arus pada L dan C di RLC paralel saat resonansi?", "id": "Bisa sangat besar, saling meniadakan." },
{ "en": "Rasio reaktansi terhadap resistansi saat resonansi?", "id": "Faktor Kualitas (Q)." },
{ "en": "Rumus faktor kualitas Q untuk RLC seri?", "id": "ωoL / R." },
{ "en": "Rumus lain Q untuk RLC seri?", "id": "1 / (ωoRC)." },
{ "en": "Rumus faktor kualitas Q untuk RLC paralel?", "id": "ωoRC atau R / (ωoL)." },
{ "en": "Arti dari nilai Q yang tinggi?", "id": "Resonansi yang tajam atau selektif." },
{ "en": "Arti dari nilai Q yang rendah?", "id": "Resonansi yang tumpul atau lebar." },
{ "en": "Hubungan tegangan kapasitor/induktor dan sumber (seri)?", "id": "VL atau VC = Q * V_sumber." },
{ "en": "Fenomena ini disebut?", "id": "Magnifikasi tegangan." },
{ "en": "Hubungan arus kapasitor/induktor dan sumber (paralel)?", "id": "IL atau IC = Q * I_sumber." },
{ "en": "Fenomena ini disebut?", "id": "Magnifikasi arus." },
{ "en": "Rentang frekuensi di mana daya > 1/2 daya maks?", "id": "Lebar pita (Bandwidth, BW)." },
{ "en": "Frekuensi di mana daya setengah maksimum?", "id": "Frekuensi cut-off atau setengah daya." },
{ "en": "Ada berapa frekuensi setengah daya?", "id": "Dua (ω1 dan ω2)." },
{ "en": "Hubungan BW, Q, dan ωo?", "id": "BW = ωo / Q." },
{ "en": "Lebar pita berbanding terbalik dengan?", "id": "Faktor kualitas (Q)." },
{ "en": "Semakin tinggi Q, semakin ... bandwidth?", "id": "Sempit." },
{ "en": "Rumus bandwidth untuk RLC seri?", "id": "BW = R / L." },
{ "en": "Rumus bandwidth untuk RLC paralel?", "id": "BW = 1 / (RC)." },
{ "en": "Selektivitas suatu rangkaian resonansi ditentukan oleh?", "id": "Faktor kualitas Q." },
{ "en": "Rangkaian resonansi seri berguna sebagai filter?", "id": "Lolos pita (bandpass)." },
{ "en": "Rangkaian resonansi paralel berguna sebagai filter?", "id": "Tolak pita (bandstop)." },
{ "en": "Plot magnitudo atau fasa vs frekuensi?", "id": "Respons frekuensi." },
{ "en": "Satuan magnitudo dalam plot Bode?", "id": "Desibel (dB)." },
{ "en": "Rangkaian yang meloloskan semua frekuensi?", "id": "Filter all-pass." },
{ "en": "Frekuensi cut-off (fc) juga disebut?", "id": "Frekuensi -3dB." },
{ "en": "Filter pasif terdiri dari komponen?", "id": "R, L, dan C." },
{ "en": "Filter aktif terdiri dari komponen?", "id": "Op-amp, R, dan C." },
{ "en": "Keuntungan filter aktif?", "id": "Bisa memiliki gain, tanpa induktor." },
{ "en": "Skala logaritmik pada sumbu frekuensi disebut?", "id": "Dekade atau oktaf." },
{ "en": "Satu dekade adalah perubahan frekuensi sebesar?", "id": "Faktor sepuluh." },
{ "en": "Satu oktaf adalah perubahan frekuensi sebesar?", "id": "Faktor dua." },
{ "en": "Plot Bode terdiri dari?", "id": "Plot magnitudo dan plot fasa." },
{ "en": "Kemiringan (slope) pada plot Bode magnitudo?", "id": "dB per dekade atau dB per oktaf." },
{ "en": "Filter RC lolos bawah orde satu?", "id": "Slope -20 dB/dekade." },
{ "en": "Filter RL lolos atas orde satu?", "id": "Slope +20 dB/dekade." },
{ "en": "Frekuensi resonansi geometris dari ω1 dan ω2?", "id": "ωo = akar(ω1 * ω2)." },
{ "en": "Rangkaian tangki (tank circuit)?", "id": "Rangkaian LC paralel." },
{ "en": "Apa yang terjadi di rangkaian tangki saat resonansi?", "id": "Energi berosilasi antara L dan C." },
{ "en": "Damping pada rangkaian resonansi disebabkan oleh?", "id": "Resistansi (R)." },
{ "en": "Resonansi seri: impedansi rendah, arus?", "id": "Tinggi." },
{ "en": "Resonansi paralel: impedansi tinggi, arus?", "id": "Rendah." },
{ "en": "Aplikasi rangkaian resonansi?", "id": "Tuning radio, filter, osilator." },
{ "en": "Saat tuning radio, apa yang diubah?", "id": "Kapasitansi atau induktansi." },
{ "en": "Mengubah C atau L akan mengubah?", "id": "Frekuensi resonansi." },
{ "en": "Frekuensi cut-off untuk filter lolos bawah RC?", "id": "fc = 1 / (2πRC)." },
{ "en": "Frekuensi cut-off untuk filter lolos atas RC?", "id": "fc = 1 / (2πRC)." },
{ "en": "Frekuensi cut-off untuk filter RL?", "id": "fc = R / (2πL)." },
{ "en": "Fungsi transfer H(s) dievaluasi pada?", "id": "s = jω untuk respons frekuensi." },
{ "en": "Apa itu 's' dalam H(s)?", "id": "Variabel frekuensi kompleks." },
{ "en": "Passband sebuah filter adalah?", "id": "Rentang frekuensi yang diloloskan." },
{ "en": "Stopband sebuah filter adalah?", "id": "Rentang frekuensi yang ditolak/dilemahkan." },
{ "en": "Transisi antara passband dan stopband?", "id": "Tidak instan, memiliki kemiringan." },
{ "en": "Filter ideal memiliki transisi?", "id": "Sangat tajam (brick wall)." },
{ "en": "Rangkaian RLC seri sebagai filter lolos pita?", "id": "Output diambil pada R." },
{ "en": "Rangkaian RLC seri sebagai filter tolak pita?", "id": "Output diambil pada L dan C." },
{ "en": "Apakah nilai Q bisa kurang dari 1?", "id": "Ya, untuk resonansi yang sangat tumpul." },
{ "en": "Untuk Q > 10, rangkaian dianggap?", "id": "Resonansi Q tinggi." },
{ "en": "Hubungan antara energi tersimpan dan terdisipasi?", "id": "Faktor kualitas Q." },
{ "en": "Q = 2π * (Energi maks tersimpan / ...)?", "id": "Energi hilang per siklus." },
{ "en": "Apakah resonansi selalu diinginkan?", "id": "Tidak, bisa merusak dalam sistem mekanis." },
{ "en": "Respons frekuensi dapat diukur dengan?", "id": "Network analyzer atau signal generator." },
{ "en": "Lokasi pole pada fungsi transfer menentukan?", "id": "Frekuensi cut-off atau resonansi." },
{ "en": "Lokasi zero pada fungsi transfer menentukan?", "id": "Frekuensi di mana output nol." },
{ "en": "Filter lolos bawah memiliki zero di?", "id": "Tak hingga." },
{ "en": "Filter lolos atas memiliki zero di?", "id": "Nol (DC)." },
{ "en": "Filter tolak pita memiliki zero di?", "id": "Frekuensi resonansi." },
{ "en": "Resonansi paralel praktis memperhitungkan?", "id": "Resistansi kumparan induktor." },
{ "en": "Resistansi kumparan menyebabkan frekuensi resonansi?", "id": "Sedikit bergeser." },
{ "en": "Jenis respons filter (Butterworth, Chebyshev)?", "id": "Menentukan kerataan passband dan ketajaman transisi." },
{ "en": "Filter Butterworth memiliki passband yang?", "id": "Sangat datar (maximally flat)." },
{ "en": "Filter Chebyshev memiliki passband yang?", "id": "Memiliki riak (ripple)." },
{ "en": "Keuntungan filter Chebyshev?", "id": "Transisi yang lebih tajam." },
{ "en": "Pada frekuensi sangat tinggi, kapasitor bersifat?", "id": "Hubung singkat (short)." },
{ "en": "Pada frekuensi sangat tinggi, induktor bersifat?", "id": "Rangkaian terbuka (open)." },
{ "en": "Pada frekuensi sangat rendah (DC), kapasitor?", "id": "Rangkaian terbuka." },
{ "en": "Pada frekuensi sangat rendah (DC), induktor?", "id": "Hubung singkat." },
{ "en": "Analisis ini berguna untuk?", "id": "Memverifikasi perilaku filter." },
{ "en": "Desibel adalah satuan yang bersifat?", "id": "Logaritmik." },
{ "en": "Gain 20 dB berarti rasio tegangan?", "id": "Sepuluh." },
{ "en": "Gain -3 dB berarti rasio daya?", "id": "Setengah." },
{ "en": "Gain -3 dB berarti rasio tegangan?", "id": "Satu per akar dua." },
{ "en": "Resonansi dalam sistem kelistrikan bisa menyebabkan?", "id": "Tegangan atau arus berlebih." },
{ "en": "Apa itu sistem tiga fasa?", "id": "Sistem AC dengan tiga sumber sefasa." },
{ "en": "Berapa beda fasa antar sumbernya?", "id": "120 derajat." },
{ "en": "Mengapa sistem tiga fasa digunakan?", "id": "Lebih efisien dan daya konstan." },
{ "en": "Dua konfigurasi koneksi sumber/beban utama?", "id": "Bintang (Y) dan Delta (Δ)." },
{ "en": "Titik hubung bersama pada koneksi Y?", "id": "Titik netral (N)." },
{ "en": "Apakah koneksi Delta punya titik netral?", "id": "Tidak." },
{ "en": "Sistem tiga fasa disebut seimbang jika?", "id": "Sumber seimbang dan beban seimbang." },
{ "en": "Sumber seimbang berarti tegangannya punya?", "id": "Magnitudo sama, beda fasa 120 derajat." },
{ "en": "Beban seimbang berarti impedansi per fasa?", "id": "Sama." },
{ "en": "Urutan fasa mencapai puncak (misal A-B-C)?", "id": "Urutan fasa." },
{ "en": "Urutan fasa positif?", "id": "ABC." },
{ "en": "Urutan fasa negatif?", "id": "ACB." },
{ "en": "Tegangan antara satu saluran dan netral?", "id": "Tegangan fasa (Vp)." },
{ "en": "Tegangan antara dua saluran (misal A dan B)?", "id": "Tegangan line atau antar-fasa (VL)." },
{ "en": "Arus yang mengalir pada satu impedansi beban?", "id": "Arus fasa (Ip)." },
{ "en": "Arus yang mengalir pada saluran transmisi?", "id": "Arus line (IL)." },
{ "en": "Pada koneksi Y, hubungan arus?", "id": "Arus line sama dengan arus fasa." },
{ "en": "Pada koneksi Y, hubungan tegangan?", "id": "VL = Vp * akar(3)." },
{ "en": "Pada koneksi Y, tegangan line mendahului Vp?", "id": "Sebesar 30 derajat." },
{ "en": "Pada koneksi Δ, hubungan tegangan?", "id": "Tegangan line sama dengan tegangan fasa." },
{ "en": "Pada koneksi Δ, hubungan arus?", "id": "IL = Ip * akar(3)." },
{ "en": "Pada koneksi Δ, arus line tertinggal dari Ip?", "id": "Sebesar 30 derajat." },
{ "en": "Analisis sistem tiga fasa seimbang disederhanakan menjadi?", "id": "Analisis per fasa ekuivalen." },
{ "en": "Apa itu rangkaian per fasa ekuivalen?", "id": "Satu fasa sumber, saluran, dan beban." },
{ "en": "Daya total pada sistem tiga fasa seimbang?", "id": "Tiga kali daya per fasa." },
{ "en": "Rumus daya aktif total (PT) tiga fasa?", "id": "PT = akar(3) * VL * IL * cos(θ)." },
{ "en": "Rumus daya reaktif total (QT) tiga fasa?", "id": "QT = akar(3) * VL * IL * sin(θ)." },
{ "en": "Rumus daya semu total (ST) tiga fasa?", "id": "ST = akar(3) * VL * IL." },
{ "en": "Apa itu θ dalam rumus daya tiga fasa?", "id": "Sudut faktor daya (sudut impedansi beban)." },
{ "en": "Total daya sesaat pada sistem seimbang?", "id": "Konstan, tidak berfluktuasi." },
{ "en": "Koneksi sumber Y dan beban Y disebut?", "id": "Koneksi Y-Y." },
{ "en": "Koneksi sumber Y dan beban Δ disebut?", "id": "Koneksi Y-Δ." },
{ "en": "Koneksi sumber Δ dan beban Y disebut?", "id": "Koneksi Δ-Y." },
{ "en": "Koneksi sumber Δ dan beban Δ disebut?", "id": "Koneksi Δ-Δ." },
{ "en": "Untuk analisis, beban Δ sering diubah menjadi?", "id": "Beban Y ekuivalen." },
{ "en": "Rumus konversi impedansi ZΔ ke ZY?", "id": "ZY = ZΔ / 3." },
{ "en": "Kawat keempat pada sistem Y?", "id": "Kawat netral." },
{ "en": "Fungsi kawat netral pada sistem seimbang?", "id": "Tidak mengalirkan arus." },
{ "en": "Fungsi kawat netral pada sistem tak seimbang?", "id": "Mengalirkan arus ketidakseimbangan." },
{ "en": "Dua wattmeter dapat mengukur daya pada sistem?", "id": "Sistem tiga fasa tiga kawat." },
{ "en": "Metode ini disebut?", "id": "Metode dua wattmeter." },
{ "en": "Daya total yang terukur adalah?", "id": "Jumlah pembacaan dua wattmeter (P1 + P2)." },
{ "en": "Daya reaktif total dari metode dua wattmeter?", "id": "Akar(3) * (P1 - P2)." },
{ "en": "Jika P1 = P2, faktor daya?", "id": "Satu (beban resistif)." },
{ "en": "Jika P1 = -P2, faktor daya?", "id": "Nol (beban reaktif murni)." },
{ "en": "Jika satu wattmeter membaca nol, PF?", "id": "0.5." },
{ "en": "Koreksi faktor daya pada sistem tiga fasa?", "id": "Menambahkan bank kapasitor Y atau Δ." },
{ "en": "Contoh beban tiga fasa?", "id": "Motor induksi tiga fasa." },
{ "en": "Tegangan jala-jala di Indonesia (antar-fasa)?", "id": "380 Volt." },
{ "en": "Tegangan fasa-netral di Indonesia?", "id": "220 Volt." },
{ "en": "Hubungan 380V dan 220V?", "id": "380 ≈ 220 * akar(3)." },
{ "en": "Sistem distribusi ke perumahan biasanya?", "id": "Satu fasa dari sistem tiga fasa." },
{ "en": "Keuntungan utama motor tiga fasa?", "id": "Self-starting (dapat berputar sendiri)." },
{ "en": "Urutan fasa penting untuk?", "id": "Arah putaran motor." },
{ "en": "Bagaimana cara membalik urutan fasa?", "id": "Tukar posisi dua dari tiga saluran." },
{ "en": "Apa itu sistem tak seimbang?", "id": "Beban per fasa tidak sama." },
{ "en": "Analisis sistem tak seimbang menggunakan?", "id": "Analisis simpul/jala atau komponen simetris." },
{ "en": "Komponen simetris terdiri dari urutan?", "id": "Positif, negatif, dan nol." },
{ "en": "Apakah rangkaian per fasa berlaku untuk tak seimbang?", "id": "Tidak." },
{ "en": "Daya pada sistem tak seimbang?", "id": "Dihitung per fasa lalu dijumlahkan." },
{ "en": "Daya sesaat pada sistem tak seimbang?", "id": "Berfluktuasi." },
{ "en": "Penulisan fasor tegangan fasa A?", "id": "|Vp| sudut 0°." },
{ "en": "Penulisan fasor tegangan fasa B (urutan ABC)?", "id": "|Vp| sudut -120°." },
{ "en": "Penulisan fasor tegangan fasa C (urutan ABC)?", "id": "|Vp| sudut -240° atau +120°." },
{ "en": "Jumlah fasor tegangan fasa pada sumber Y?", "id": "Nol." },
{ "en": "Jumlah fasor tegangan line pada sumber Δ?", "id": "Nol (memenuhi KVL)." },
{ "en": "Jumlah fasor arus line pada sistem seimbang?", "id": "Nol." },
{ "en": "Impedansi saluran transmisi diperhitungkan dalam?", "id": "Model rangkaian per fasa." },
{ "en": "Beban Δ lebih umum untuk?", "id": "Beban industri daya tinggi." },
{ "en": "Daya yang hilang pada saluran (rugi-rugi)?", "id": "3 * IL^2 * R_saluran." },
{ "en": "PLN mentransmisikan daya pada tegangan?", "id": "Sangat tinggi (untuk mengurangi IL)." },
{ "en": "Transformator tiga fasa digunakan untuk?", "id": "Menaikkan atau menurunkan tegangan tiga fasa." },
{ "en": "Daya kompleks per fasa?", "id": "Sp = Vp * Ip* (konjugat)." },
{ "en": "Daya kompleks total tiga fasa?", "id": "St = 3 * Sp." },
{ "en": "Hubungan daya Δ dan Y untuk impedansi sama?", "id": "Daya Δ tiga kali lebih besar." },
{ "en": "Mengapa daya Δ lebih besar?", "id": "Karena tegangan per fasa lebih tinggi." },
{ "en": "Tegangan fasa pada beban Δ?", "id": "Sama dengan tegangan line." },
{ "en": "Tegangan fasa pada beban Y?", "id": "Tegangan line dibagi akar tiga." },
{ "en": "Arus netral pada sistem Y-Y seimbang?", "id": "Nol." },
{ "en": "Pembangkit listrik menghasilkan listrik dalam bentuk?", "id": "Tiga fasa." },
{ "en": "Pengukuran tegangan fasa memerlukan akses ke?", "id": "Titik netral." },
{ "en": "Apakah selalu ada koneksi fisik ke netral?", "id": "Tidak, bisa jadi netral mengambang." },
{ "en": "Untuk beban Y seimbang tanpa netral, analisis?", "id": "Tetap bisa pakai analisis per fasa." },
{ "en": "Daya rata-rata total pada sistem tiga fasa?", "id": "Tidak tergantung pada urutan fasa." },
{ "en": "Bagian mana dari sistem tenaga yang tiga fasa?", "id": "Pembangkitan, transmisi, dan distribusi utama." },
{ "en": "Sistem tiga fasa empat kawat berarti koneksi?", "id": "Koneksi Y dengan kawat netral." },
{ "en": "Sistem tiga fasa tiga kawat berarti koneksi?", "id": "Bisa Δ atau Y tanpa netral." },
{ "en": "Mengapa beban Δ tidak bisa dianalisis per fasa langsung?", "id": "Karena tidak ada titik netral." },
{ "en": "Langkah pertama analisis beban Δ?", "id": "Ubah ke beban Y ekuivalen." },
{ "en": "Koneksi wattmeter untuk metode dua wattmeter?", "id": "Kumparan arus di dua line, tegangan ke line ketiga." },
{ "en": "Apakah metode dua wattmeter berlaku untuk tak seimbang?", "id": "Ya." },
{ "en": "Pentingnya sistem seimbang?", "id": "Operasi optimal dan analisis sederhana." },
{ "en": "Apa itu tegangan urutan nol?", "id": "Komponen tegangan yang sama di ketiga fasa." },
{ "en": "Arus urutan nol hanya mengalir jika ada?", "id": "Jalur kembali ke netral." },
{ "en": "Daya reaktif dalam sistem tiga fasa?", "id": "Disediakan oleh kapasitor atau generator." },
{ "en": "Faktor daya tiga fasa adalah rasio?", "id": "Daya aktif total dan daya semu total." },
{ "en": "Kode warna kabel fasa di Indonesia?", "id": "Merah, Kuning, Hitam." },
{ "en": "Kode warna kabel netral?", "id": "Biru." },
{ "en": "Kode warna kabel ground/tanah?", "id": "Hijau-kuning." },
{ "en": "Operator 'a' dalam komponen simetris?", "id": "Operator rotasi 120 derajat (1 sudut 120°)." },
{ "en": "Induktor yang saling mempengaruhi secara magnetis?", "id": "Induktor berpasangan (coupled inductors)." },
{ "en": "Ukuran pengaruh satu induktor ke lainnya?", "id": "Induktansi timbal balik (M)." },
{ "en": "Satuan induktansi timbal balik?", "id": "Henry (H)." },
{ "en": "Tingkat keketatan kopling magnetik?", "id": "Koefisien kopling (k)." },
{ "en": "Rentang nilai koefisien kopling k?", "id": "Antara 0 dan 1." },
{ "en": "Jika k = 1, kopling bersifat?", "id": "Sempurna (perfect coupling)." },
{ "en": "Jika k = 0, kopling bersifat?", "id": "Tidak ada kopling." },
{ "en": "Hubungan M, k, L1, dan L2?", "id": "M = k * akar(L1*L2)." },
{ "en": "Tanda pada terminal induktor berpasangan?", "id": "Konvensi titik (dot convention)." },
{ "en": "Arus masuk ke titik, tegangan induksi?", "id": "Positif pada titik lainnya." },
{ "en": "Arus meninggalkan titik, tegangan induksi?", "id": "Negatif pada titik lainnya." },
{ "en": "Tegangan pada L1 karena arusnya sendiri?", "id": "jωL1 * I1." },
{ "en": "Tegangan pada L1 karena arus di L2?", "id": "±jωM * I2." },
{ "en": "Tanda ± pada tegangan induksi M tergantung?", "id": "Arah arus dan konvensi titik." },
{ "en": "Rangkaian ekuivalen untuk induktor berpasangan?", "id": "Ekuivalen T atau Pi." },
{ "en": "Energi yang tersimpan dalam induktor berpasangan?", "id": "Bergantung pada L1, L2, M, I1, I2." },
{ "en": "Perangkat pasif dua-port berbasis kopling magnetik?", "id": "Transformator." },
{ "en": "Transformator ideal memiliki koefisien kopling?", "id": "k = 1." },
{ "en": "Kumparan di sisi sumber pada transformator?", "id": "Kumparan primer." },
{ "en": "Kumparan di sisi beban pada transformator?", "id": "Kumparan sekunder." },
{ "en": "Rasio tegangan pada transformator ideal?", "id": "Sama dengan rasio lilitan (n)." },
{ "en": "Rasio lilitan n?", "id": "N2 / N1." },
{ "en": "Hubungan V2/V1 pada transformator ideal?", "id": "V2/V1 = n." },
{ "en": "Rasio arus pada transformator ideal?", "id": "Berkebalikan dengan rasio lilitan." },
{ "en": "Hubungan I2/I1 pada transformator ideal?", "id": "I2/I1 = 1/n." },
{ "en": "Jika n > 1, transformator jenis?", "id": "Step-up." },
{ "en": "Jika n < 1, transformator jenis?", "id": "Step-down." },
{ "en": "Jika n = 1, transformator jenis?", "id": "Isolasi." },
{ "en": "Daya pada sisi primer dan sekunder trafo ideal?", "id": "Sama (S1 = S2)." },
{ "en": "Fungsi transformator selain mengubah tegangan?", "id": "Isolasi listrik dan pencocokan impedansi." },
{ "en": "Impedansi beban ZL dilihat dari sisi primer?", "id": "Impedansi terefleksi (Z_ref)." },
{ "en": "Rumus impedansi terefleksi?", "id": "Z_ref = ZL / n^2." },
{ "en": "Transformator linier memperhitungkan?", "id": "Induktansi magnetisasi." },
{ "en": "Transformator non-ideal memiliki?", "id": "Kerugian tembaga dan inti." },
{ "en": "Transformator dengan beberapa kumparan sekunder?", "id": "Transformator multi-lilitan." },
{ "en": "Transformator dengan satu lilitan?", "id": "Autotransformator." },
{ "en": "Keuntungan autotransformator?", "id": "Ukuran lebih kecil, efisiensi lebih tinggi." },
{ "en": "Plot magnitudo dan fasa vs frekuensi?", "id": "Plot Bode." },
{ "en": "Fungsi yang menggambarkan perilaku rangkaian vs frekuensi?", "id": "Fungsi transfer H(ω)." },
{ "en": "Sumbu vertikal plot magnitudo Bode?", "id": "Dalam Desibel (dB)." },
{ "en": "Rumus magnitudo dalam dB?", "id": "20 log10 |H(ω)|." },
{ "en": "Sumbu horizontal plot Bode?", "id": "Frekuensi dalam skala logaritmik." },
{ "en": "Sumbu vertikal plot fasa Bode?", "id": "Derajat atau radian." },
{ "en": "Frekuensi di mana gain turun 3 dB?", "id": "Frekuensi cut-off." },
{ "en": "Kemiringan (slope) asimtot pada plot Bode?", "id": "±20n dB/dekade." },
{ "en": "Apa itu 'n' pada kemiringan?", "id": "Orde dari pole atau zero." },
{ "en": "Faktor (s) pada pembilang fungsi transfer?", "id": "Zero di titik asal." },
{ "en": "Efek zero di titik asal pada plot magnitudo?", "id": "Slope +20 dB/dekade." },
{ "en": "Efek zero di titik asal pada plot fasa?", "id": "Fasa konstan +90 derajat." },
{ "en": "Faktor (s) pada penyebut fungsi transfer?", "id": "Pole di titik asal." },
{ "en": "Efek pole di titik asal pada plot magnitudo?", "id": "Slope -20 dB/dekade." },
{ "en": "Efek pole di titik asal pada plot fasa?", "id": "Fasa konstan -90 derajat." },
{ "en": "Pole sederhana pada fungsi transfer?", "id": "Menyebabkan kemiringan -20 dB/dekade." },
{ "en": "Zero sederhana pada fungsi transfer?", "id": "Menyebabkan kemiringan +20 dB/dekade." },
{ "en": "Frekuensi di mana asimtot bertemu?", "id": "Frekuensi pojok (corner frequency)." },
{ "en": "Frekuensi pojok ditentukan oleh?", "id": "Lokasi pole atau zero." },
{ "en": "Kesalahan antara plot Bode asimtotik dan aktual?", "id": "Maksimum pada frekuensi pojok." },
{ "en": "Berapa kesalahan maksimum untuk pole/zero sederhana?", "id": "Sekitar 3 dB." },
{ "en": "Pole kuadratik pada fungsi transfer?", "id": "Mewakili sistem orde kedua." },
{ "en": "Respons pole kuadratik tergantung pada?", "id": "Rasio redaman (damping ratio)." },
{ "en": "Jika rasio redaman kecil, respons?", "id": "Memiliki puncak (peak) resonansi." },
{ "en": "Plot Bode sangat berguna untuk?", "id": "Analisis filter dan stabilitas sistem kontrol." },
{ "en": "Rangkaian yang hanya mengubah fasa, bukan magnitudo?", "id": "Filter all-pass atau phase shifter." },
{ "en": "Gain DC sebuah sistem?", "id": "Nilai |H(ω)| saat ω mendekati nol." },
{ "en": "Gain frekuensi tinggi sebuah sistem?", "id": "Nilai |H(ω)| saat ω tak hingga." },
{ "en": "Filter lolos bawah punya gain DC?", "id": "Konstan (misal: 0 dB)." },
{ "en": "Filter lolos atas punya gain DC?", "id": "Sangat kecil (minus tak hingga dB)." },
{ "en": "Plot polar dari fungsi transfer?", "id": "Plot Nyquist." },
{ "en": "Plot magnitudo vs frekuensi pada skala linier?", "id": "Spektrum magnitudo." },
{ "en": "Plot fasa vs frekuensi pada skala linier?", "id": "Spektrum fasa." },
{ "en": "Kopling seri-membantu (series-aiding)?", "id": "Fluks magnetik saling menguatkan." },
{ "en": "Induktansi total seri-membantu?", "id": "L_eq = L1 + L2 + 2M." },
{ "en": "Kopling seri-melawan (series-opposing)?", "id": "Fluks magnetik saling melemahkan." },
{ "en": "Induktansi total seri-melawan?", "id": "L_eq = L1 + L2 - 2M." },
{ "en": "Cara menentukan M secara eksperimental?", "id": "Mengukur induktansi seri-membantu dan melawan." },
{ "en": "Transformator center-tap memiliki?", "id": "Satu terminal tambahan di tengah kumparan." },
{ "en": "Penggunaan transformator center-tap?", "id": "Pada catu daya dan penyearah gelombang penuh." },
{ "en": "Apakah plot Bode unik untuk satu rangkaian?", "id": "Tidak, rangkaian berbeda bisa punya plot sama." },
{ "en": "Plot Bode untuk RLC seri?", "id": "Menunjukkan puncak resonansi jika Q tinggi." },
{ "en": "Tinggi puncak resonansi berhubungan dengan?", "id": "Faktor kualitas Q." },
{ "en": "Skala logaritmik membantu menampilkan rentang frekuensi?", "id": "Sangat lebar." },
{ "en": "Frekuensi pojok untuk pole 1/(1+jω/p)?", "id": "ω = p." },
{ "en": "Kombinasi plot Bode individu?", "id": "Dijumlahkan (karena skala logaritmik)." },
{ "en": "Transformator pulsa dirancang untuk sinyal?", "id": "Non-sinusoidal seperti pulsa digital." },
{ "en": "Efek kulit (skin effect) pada frekuensi tinggi?", "id": "Arus mengalir di permukaan konduktor." },
{ "en": "Efek kulit meningkatkan?", "id": "Resistansi AC." },
{ "en": "Kapasitansi parasitik antar lilitan?", "id": "Penting pada frekuensi sangat tinggi." },
{ "en": "Resonansi sendiri (self-resonance) pada induktor?", "id": "Saat reaktansi induktif = reaktansi kapasitansi parasitik." },
{ "en": "Pada resonansi sendiri, impedansi induktor?", "id": "Sangat tinggi." },
{ "en": "Sinusoida adalah kasus khusus dari?", "id": "Eksponensial kompleks." },
{ "en": "Analisis rangkaian menggunakan s = σ + jω?", "id": "Analisis domain-s." },
{ "en": "Fungsi transfer H(s) menghubungkan?", "id": "Output dan input dalam domain-s." },
{ "en": "Pole dari H(s) adalah nilai s yang membuat?", "id": "H(s) tak hingga." },
{ "en": "Zero dari H(s) adalah nilai s yang membuat?", "id": "H(s) nol." },
{ "en": "Pole dan zero menentukan?", "id": "Respons frekuensi dan transien." },
{ "en": "Untuk keadaan tunak sinusoidal, kita set?", "id": "s = jω." },
{ "en": "Jaringan dengan dua pasang terminal?", "id": "Jaringan dua-port." },
{ "en": "Contoh jaringan dua-port?", "id": "Filter, amplifier, transformator." },
{ "en": "Port input biasanya diberi nomor?", "id": "Port 1." },
{ "en": "Port output biasanya diberi nomor?", "id": "Port 2." },
{ "en": "Parameter yang menghubungkan V1, V2 dengan I1, I2?", "id": "Parameter dua-port." },
{ "en": "Parameter impedansi disebut juga?", "id": "Parameter-Z." },
{ "en": "Definisi z11?", "id": "V1/I1 saat I2 = 0 (port 2 terbuka)." },
{ "en": "Definisi z21?", "id": "V2/I1 saat I2 = 0." },
{ "en": "Parameter-Z diukur dalam kondisi?", "id": "Rangkaian terbuka (open-circuit)." },
{ "en": "Parameter admitansi disebut juga?", "id": "Parameter-Y." },
{ "en": "Definisi y11?", "id": "I1/V1 saat V2 = 0 (port 2 dihubung singkat)." },
{ "en": "Definisi y21?", "id": "I2/V1 saat V2 = 0." },
{ "en": "Parameter-Y diukur dalam kondisi?", "id": "Hubung singkat (short-circuit)." },
{ "en": "Parameter hibrida disebut juga?", "id": "Parameter-h." },
{ "en": "Mengapa disebut parameter hibrida?", "id": "Kondisi pengukuran campuran (terbuka & singkat)." },
{ "en": "Parameter-h sering digunakan untuk memodelkan?", "id": "Transistor." },
{ "en": "Parameter transmisi disebut juga?", "id": "Parameter ABCD." },
{ "en": "Parameter ABCD menghubungkan variabel input dengan?", "id": "Variabel output." },
{ "en": "Parameter ABCD berguna untuk analisis?", "id": "Jaringan kaskade (cascade)." },
{ "en": "Menghubungkan dua jaringan secara seri?", "id": "Jumlahkan matriks Z-nya." },
{ "en": "Menghubungkan dua jaringan secara paralel?", "id": "Jumlahkan matriks Y-nya." },
{ "en": "Menghubungkan dua jaringan secara kaskade?", "id": "Kalikan matriks ABCD-nya." },
{ "en": "Jaringan bersifat resiprokal jika?", "id": "z12 = z21 atau y12 = y21." },
{ "en": "Jaringan bersifat simetris jika?", "id": "z11 = z22 atau y11 = y22." },
{ "en": "Apakah parameter Z selalu ada?", "id": "Tidak, jika tidak bisa diukur dengan port terbuka." },
{ "en": "Transformator ideal adalah contoh jaringan?", "id": "Non-resiprokal jika n tidak sama dengan 1." },
{ "en": "Representasi sinyal periodik non-sinusoidal?", "id": "Deret Fourier." },
{ "en": "Deret Fourier terdiri dari?", "id": "Komponen DC, fundamental, dan harmonisa." },
{ "en": "Frekuensi fundamental adalah?", "id": "Frekuensi terendah (sama dengan sinyal asli)." },
{ "en": "Harmonisa adalah?", "id": "Kelipatan bulat dari frekuensi fundamental." },
{ "en": "Bentuk trigonometrik deret Fourier?", "id": "Menggunakan suku cosinus dan sinus." },
{ "en": "Bentuk kompleks deret Fourier?", "id": "Menggunakan eksponensial kompleks." },
{ "en": "Analisis rangkaian dengan sumber non-sinusoidal?", "id": "Gunakan superposisi untuk tiap komponen harmonisa." },
{ "en": "Respons total adalah?", "id": "Jumlah respons dari DC, fundamental, harmonisa." },
{ "en": "Daya rata-rata total sinyal non-sinusoidal?", "id": "Jumlah daya rata-rata tiap komponen harmonisa." },
{ "en": "Nilai RMS sinyal non-sinusoidal?", "id": "Akar dari jumlah kuadrat RMS tiap komponen." },
{ "en": "Persen distorsi harmonisa total (THD)?", "id": "Ukuran seberapa terdistorsinya sinyal." },
{ "en": "Simetri ganjil pada gelombang?", "id": "Hanya mengandung komponen sinus." },
{ "en": "Simetri genap pada gelombang?", "id": "Hanya mengandung komponen cosinus." },
{ "en": "Simetri setengah gelombang?", "id": "Hanya mengandung harmonisa ganjil." },
{ "en": "Spektrum frekuensi dari sinyal periodik?", "id": "Diskrit (berupa garis-garis)." },
{ "en": "Representasi sinyal non-periodik?", "id": "Transformasi Fourier." },
{ "en": "Spektrum frekuensi sinyal non-periodik?", "id": "Kontinu." },
{ "en": "Hubungan antara deret dan transformasi Fourier?", "id": "Deret adalah kasus khusus untuk sinyal periodik." },
{ "en": "Fungsi transfer H(ω) mempengaruhi setiap harmonisa?", "id": "Ya, secara individual." },
{ "en": "Output Vout(nω) = ?", "id": "H(nω) dikali Vin(nω)." },
{ "en": "Filter dapat mengubah bentuk gelombang non-sinusoidal?", "id": "Ya, dengan melemahkan harmonisa tertentu." },
{ "en": "Gelombang persegi mengandung harmonisa?", "id": "Hanya harmonisa ganjil." },
{ "en": "Gelombang segitiga mengandung harmonisa?", "id": "Hanya harmonisa ganjil, amplitudo lebih kecil." },
{ "en": "Daya pada sistem tiga fasa tak seimbang?", "id": "Dihitung menggunakan komponen simetris." },
{ "en": "Daya urutan positif, negatif, dan nol?", "id": "Komponen daya pada sistem tak seimbang." },
{ "en": "Nilai rata-rata dari perkalian dua sinusoida?", "id": "Nol jika frekuensinya berbeda." },
{ "en": "Daya silang (cross-power) antara harmonisa?", "id": "Nol." },
{ "en": "Faktor daya untuk beban non-linier?", "id": "Disebut faktor daya distorsi." },
{ "en": "Daya distorsi adalah bagian dari?", "id": "Daya semu." },
{ "en": "Rangkaian interkoneksi dua-port yang umum?", "id": "Seri, Paralel, Kaskade, Seri-Paralel." },
{ "en": "Konversi antara parameter dua-port?", "id": "Ada formula standar untuk konversi." },
{ "en": "Parameter-Y disebut juga?", "id": "Parameter admitansi hubung singkat." },
{ "en": "Parameter-Z disebut juga?", "id": "Parameter impedansi rangkaian terbuka." },
{ "en": "Aplikasi deret Fourier dalam audio?", "id": "Equalizer grafis." },
{ "en": "Setiap pita pada equalizer adalah?", "id": "Filter lolos pita." },
{ "en": "Distorsi harmonisa disebabkan oleh?", "id": "Komponen non-linier dalam rangkaian." },
{ "en": "Contoh komponen non-linier?", "id": "Dioda, transistor, inti magnetik jenuh." },
{ "en": "Apakah analisis fasor bisa untuk non-linier?", "id": "Tidak." },
{ "en": "Model hibrida-pi transistor adalah model?", "id": "Dua-port sinyal kecil." },
{ "en": "Mengapa parameter-g ada?", "id": "Parameter hibrida invers." },
{ "en": "Apakah V1, I1 selalu variabel independen?", "id": "Tidak, tergantung jenis parameter." },
{ "en": "Kondisi V2=0 secara fisik berarti?", "id": "Terminal output dihubung-singkat." },
{ "en": "Kondisi I1=0 secara fisik berarti?", "id": "Terminal input dibiarkan terbuka." },
{ "en": "Parameter mana yang paling mudah diukur pada frekuensi tinggi?", "id": "Parameter-S (scattering)." },
{ "en": "Parameter-S berhubungan dengan?", "id": "Gelombang pantul dan transmisi." },
{ "en": "Koefisien Cn dalam deret Fourier kompleks?", "id": "Fasor dari harmonisa ke-n." },
{ "en": "Magnitudo |Cn| memberikan?", "id": "Amplitudo harmonisa." },
{ "en": "Sudut Cn memberikan?", "id": "Fasa harmonisa." },
{ "en": "Spektrum satu sisi vs dua sisi?", "id": "Dua sisi dari -∞ hingga +∞." },
{ "en": "Untuk sinyal riil, spektrum dua sisi?", "id": "Memiliki simetri konjugat." },
{ "en": "Daya nyata pada sumber non-linier?", "id": "P = Vdc*Idc + Σ Pn." },
{ "en": "Jaringan Lattice adalah jenis jaringan?", "id": "Dua-port simetris." },
{ "en": "Jaringan timbal-balik (reciprocal) tidak mengandung?", "id": "Sumber dependen." },
{ "en": "Rangkaian T adalah contoh jaringan?", "id": "Tiga terminal atau dua-port." },
{ "en": "Rangkaian Pi (π) adalah contoh jaringan?", "id": "Tiga terminal atau dua-port." },
{ "en": "Koefisien ao pada deret Fourier?", "id": "Nilai rata-rata atau komponen DC." },
{ "en": "Apakah semua sinyal periodik bisa direpresentasikan Fourier?", "id": "Ya, jika memenuhi kondisi Dirichlet." },
{ "en": "Analisis keadaan tunak AC mengasumsikan sumber?", "id": "Sinusoidal murni." },
{ "en": "Analisis Fourier memperluas ini untuk sumber?", "id": "Periodik non-sinusoidal." },
{ "en": "Respon rangkaian terhadap DC (ω=0)?", "id": "Perilaku kapasitor dan induktor berubah." },
{ "en": "Apa itu penyearah?", "id": "Contoh sistem non-linier." },
{ "en": "Output penyearah mengandung banyak?", "id": "Harmonisa." },
{ "en": "Filter setelah penyearah bertujuan?", "id": "Menghilangkan harmonisa AC." },
{ "en": "Alat ukur tegangan AC?", "id": "Voltmeter AC." },
{ "en": "Voltmeter AC umumnya mengukur nilai?", "id": "RMS." },
{ "en": "Alat ukur arus AC?", "id": "Ammeter AC." },
{ "en": "Ammeter AC umumnya mengukur nilai?", "id": "RMS." },
{ "en": "Alat untuk menampilkan bentuk gelombang?", "id": "Osiloskop." },
{ "en": "Osiloskop mengukur nilai?", "id": "Puncak, periode, sesaat." },
{ "en": "Alat untuk mengukur daya aktif?", "id": "Wattmeter." },
{ "en": "Alat untuk mengukur impedansi?", "id": "LCR meter atau jembatan impedansi." },
{ "en": "Alat untuk menghasilkan sinyal AC?", "id": "Generator fungsi (function generator)." },
{ "en": "Analisis rangkaian AC pada SPICE?", "id": "Analisis .AC." },
{ "en": "SPICE adalah singkatan dari?", "id": "Simulasi Program dengan Penekanan Rangkaian Terpadu." },
{ "en": "Apa itu analisis 'sweep' frekuensi?", "id": "Analisis pada rentang frekuensi." },
{ "en": "Hasil analisis .AC pada SPICE?", "id": "Magnitudo dan fasa." },
{ "en": "Apakah SPICE menggunakan fasor?", "id": "Ya, untuk analisis .AC." },
{ "en": "Daya maksimum yang bisa ditangani resistor?", "id": "Peringkat daya (power rating)." },
{ "en": "Tegangan maksimum yang bisa ditangani kapasitor?", "id": "Peringkat tegangan (voltage rating)." },
{ "en": "Model praktis induktor termasuk?", "id": "Resistansi seri (resistansi lilitan)." },
{ "en": "Model praktis kapasitor termasuk?", "id": "Resistansi bocor paralel (leakage)." },
{ "en": "Pada frekuensi tinggi, semua komponen bersifat?", "id": "Kompleks karena efek parasitik." },
{ "en": "Apa itu efek parasitik?", "id": "Induktansi, kapasitansi, resistansi tak diinginkan." },
{ "en": "Setiap kabel memiliki?", "id": "Induktansi dan resistansi." },
{ "en": "Dua konduktor berdekatan memiliki?", "id": "Kapasitansi." },
{ "en": "Grounding yang baik penting untuk?", "id": "Mengurangi noise dan loop tanah." },
{ "en": "Apa itu 'noise' dalam rangkaian?", "id": "Sinyal acak yang tidak diinginkan." },
{ "en": "Sumber utama noise termal?", "id": "Agitasi termal elektron di resistor." },
{ "en": "Noise termal juga disebut?", "id": "Noise Johnson-Nyquist." },
{ "en": "Efek pembebanan (loading effect) alat ukur?", "id": "Impedansi alat ukur mempengaruhi rangkaian." },
{ "en": "Voltmeter ideal punya impedansi input?", "id": "Tak hingga." },
{ "en": "Ammeter ideal punya impedansi input?", "id": "Nol." },
{ "en": "Probe osiloskop 10X berfungsi?", "id": "Meningkatkan impedansi input." },
{ "en": "Keselamatan saat bekerja dengan AC?", "id": "Sangat penting, bisa mematikan." },
{ "en": "Mengapa AC lebih berbahaya dari DC?", "id": "Menyebabkan fibrilasi ventrikel." },
{ "en": "Perangkat proteksi arus lebih?", "id": "Sekering (fuse) atau pemutus sirkuit (breaker)." },
{ "en": "Perangkat proteksi kejut listrik?", "id": "GFCI atau RCD." },
{ "en": "Transformator isolasi digunakan untuk?", "id": "Keselamatan, memisahkan ground." },
{ "en": "Impedansi tubuh manusia?", "id": "Bervariasi, tapi tidak tak hingga." },
{ "en": "Tiga kabel pada steker listrik?", "id": "Fasa (live), netral, dan tanah (ground)." },
{ "en": "Fungsi kabel tanah?", "id": "Jalur aman jika terjadi hubung singkat." },
{ "en": "Nilai RMS dari gelombang persegi simetris?", "id": "Sama dengan amplitudonya." },
{ "en": "Nilai RMS gelombang gigi gergaji?", "id": "Amplitudo dibagi akar tiga." },
{ "en": "Faktor puncak (crest factor)?", "id": "Rasio nilai puncak terhadap nilai RMS." },
{ "en": "Faktor puncak sinusoida?", "id": "Akar dua (sekitar 1.414)." },
{ "en": "Voltmeter 'True RMS' dapat mengukur?", "id": "Nilai RMS bentuk gelombang non-sinusoidal." },
{ "en": "Daya reaktif pada motor untuk?", "id": "Membangun medan magnet." },
{ "en": "Apakah keadaan tunak tercapai seketika?", "id": "Tidak, ada periode transien." },
{ "en": "Analisis transien menggunakan?", "id": "Persamaan diferensial atau Transformasi Laplace." },
{ "en": "Analisis keadaan tunak AC mengabaikan?", "id": "Respon transien." },
{ "en": "Konstanta waktu pada rangkaian RC?", "id": "Tau = RC." },
{ "en": "Konstanta waktu pada rangkaian RL?", "id": "Tau = L/R." },
{ "en": "Transien dianggap hilang setelah?", "id": "Sekitar 5 kali konstanta waktu." },
{ "en": "Respons lengkap = ?", "id": "Respons transien + respons keadaan tunak." },
{ "en": "Jika sumber AC tiba-tiba dinyalakan?", "id": "Akan ada komponen transien dan tunak." },
{ "en": "Mengapa lampu pijar sering putus saat dinyalakan?", "id": "Karena lonjakan arus (inrush current)." },
{ "en": "Daya rata-rata pada sistem tiga fasa?", "id": "Selalu konstan jika seimbang." },
{ "en": "Sistem AC lebih mudah ditransmisikan karena?", "id": "Tegangan mudah diubah dengan transformator." },
{ "en": "Apa itu 'brownout'?", "id": "Penurunan tegangan RMS yang disengaja." },
{ "en": "Apa itu 'surge'?", "id": "Kenaikan tegangan sesaat." },
{ "en": "Perangkat untuk melindungi dari surge?", "id": "Surge protector atau MOV." },
{ "en": "Impedansi karakteristik saluran transmisi?", "id": "Rasio V/I gelombang berjalan." },
{ "en": "Kualitas sinyal AC diukur dengan?", "id": "Kualitas daya (power quality)." },
{ "en": "Masalah kualitas daya yang umum?", "id": "Harmonisa, sag, swell, flicker." },
{ "en": "Metode fasa-terpisah (split-phase)?", "id": "Digunakan pada sistem listrik rumah tangga AS." },
{ "en": "Frekuensi listrik di Amerika Utara?", "id": "60 Hertz." },
{ "en": "Mengapa frekuensi berbeda di berbagai negara?", "id": "Alasan historis dan teknis." },
{ "en": "Perbedaan fasa antara V dan I disebut?", "id": "Pergeseran fasa." },
{ "en": "Representasi matematis dari sinusoida?", "id": "Fungsi sinus atau cosinus." },
{ "en": "Perbedaan antara reaktansi dan resistansi?", "id": "Reaktansi menyimpan, resistansi mendisipasi energi." },
{ "en": "Satuan dari XL, XC, R, Z?", "id": "Semuanya dalam Ohm." },
{ "en": "Satuan dari BL, BC, G, Y?", "id": "Semuanya dalam Siemens." },
{ "en": "Hubungan antara Ohm dan Siemens?", "id": "Siemens adalah kebalikan dari Ohm." },
{ "en": "Rangkaian dual satu sama lain?", "id": "RLC seri dan RLC paralel." },
{ "en": "Dualitas antara L dan C?", "id": "Induktor dual dengan kapasitor." },
{ "en": "Kapasitor pada DC berfungsi sebagai?", "id": "Saklar terbuka." },
{ "en": "Induktor pada DC berfungsi sebagai?", "id": "Kawat (hubung singkat)." },
{ "en": "Daya sesaat pada resistor selalu?", "id": "Positif atau nol." },
{ "en": "Daya sesaat pada reaktansi?", "id": "Berosilasi antara positif dan negatif." },
{ "en": "Daya rata-rata pada reaktansi murni?", "id": "Selalu nol." },
{ "en": "Daya semu adalah batas atas dari?", "id": "Daya rata-rata." },
{ "en": "Apa itu phasor?", "id": "Bukan fasor, ejaan yang salah." },
{ "en": "Apakah frekuensi negatif punya arti fisik?", "id": "Tidak, hanya konstruksi matematis." },
{ "en": "Kapan analisis fasor valid?", "id": "Hanya untuk rangkaian linier, invarian waktu." },
{ "en": "Jika ada beberapa sumber frekuensi berbeda?", "id": "Gunakan superposisi." },
{ "en": "Dapatkah kita menjumlahkan fasor frekuensi berbeda?", "id": "Tidak." },
{ "en": "Langkah akhir setelah superposisi frekuensi beda?", "id": "Jumlahkan hasilnya dalam domain waktu." },
{ "en": "Rangkaian yang mensintesis bentuk gelombang?", "id": "Osilator." },
{ "en": "Rangkaian yang memisahkan sinyal?", "id": "Filter." },
{ "en": "Rangkaian yang menguatkan sinyal?", "id": "Amplifier." },
{ "en": "Listrik di pesawat terbang menggunakan frekuensi?", "id": "400 Hz (untuk trafo lebih kecil)." },
{ "en": "Mengapa analisis keadaan tunak penting?", "id": "Perilaku jangka panjang rangkaian." },
{ "en": "Apakah rangkaian AC memiliki nilai 'searah'?", "id": "Ya, disebut komponen DC." },
{ "en": "Bagaimana komponen DC dianalisis?", "id": "Secara terpisah dengan analisis DC." },
{ "en": "Rangkaian RLC seri Q tinggi berarti R?", "id": "Sangat kecil." },
{ "en": "Rangkaian RLC paralel Q tinggi berarti R?", "id": "Sangat besar." },
{ "en": "Jaringan yang menghubungkan sumber dan beban?", "id": "Jaringan dua-port." },
{ "en": "Parameter Z adalah rasio?", "id": "Tegangan terhadap arus." },
{ "en": "Parameter Y adalah rasio?", "id": "Arus terhadap tegangan." },
{ "en": "Parameter Z diukur saat port output?", "id": "Terbuka (open-circuit)." },
{ "en": "Parameter Y diukur saat port output?", "id": "Dihubung singkat (short-circuit)." },
{ "en": "z12 sama dengan z21 menunjukkan jaringan?", "id": "Resiprokal." },
{ "en": "y11 sama dengan y22 menunjukkan jaringan?", "id": "Simetris." },
{ "en": "Parameter untuk menganalisis jaringan kaskade?", "id": "Parameter ABCD (transmisi)." },
{ "en": "Parameter yang memodelkan transistor?", "id": "Parameter hibrida (h)." },
{ "en": "Rangkaian T adalah ekuivalen dari?", "id": "Jaringan dua-port." },
{ "en": "Rangkaian Pi (π) adalah ekuivalen dari?", "id": "Jaringan dua-port." },
{ "en": "Koneksi kaskade berarti output satu ke?", "id": "Input jaringan berikutnya." },
{ "en": "Matriks ABCD total untuk koneksi kaskade?", "id": "Perkalian matriks ABCD individu." },
{ "en": "Matriks Z total untuk koneksi seri?", "id": "Penjumlahan matriks Z individu." },
{ "en": "Matriks Y total untuk koneksi paralel?", "id": "Penjumlahan matriks Y individu." },
{ "en": "Apa itu beban tak seimbang?", "id": "Impedansi antar fasa tidak sama." },
{ "en": "Arus pada netral sistem Y tak seimbang?", "id": "Tidak nol (IN = IA+IB+IC)." },
{ "en": "Tegangan fasa pada beban Y tak seimbang?", "id": "Tidak lagi seimbang." },
{ "en": "Analisis sistem tak seimbang memerlukan?", "id": "Metode analisis jala atau simpul." },
{ "en": "Apakah analisis per fasa berlaku untuk tak seimbang?", "id": "Tidak." },
{ "en": "Pergeseran titik netral terjadi pada?", "id": "Beban Y tak seimbang tanpa netral." },
{ "en": "Metode untuk menganalisis sistem tak seimbang?", "id": "Komponen simetris." },
{ "en": "Komponen simetris terdiri dari urutan?", "id": "Positif, negatif, dan nol." },
{ "en": "Komponen urutan positif mewakili?", "id": "Sistem seimbang normal." },
{ "en": "Komponen urutan negatif mewakili?", "id": "Sistem seimbang dengan urutan fasa terbalik." },
{ "en": "Komponen urutan nol mewakili?", "id": "Tiga fasor sefasa." },
{ "en": "Arus urutan nol hanya ada jika?", "id": "Ada jalur netral." },
{ "en": "Daya pada sistem tak seimbang?", "id": "Jumlah daya dari setiap fasa." },
{ "en": "Metode dua wattmeter mengukur daya pada?", "id": "Sistem tiga kawat (seimbang/tak seimbang)." },
{ "en": "Jika beban kapasitif, pembacaan wattmeter (P1-P2)?", "id": "Bisa negatif." },
{ "en": "Sinyal periodik non-sinusoidal dapat diurai menjadi?", "id": "Deret Fourier." },
{ "en": "Deret Fourier terdiri dari harmonisa dan?", "id": "Komponen DC." },
{ "en": "Frekuensi harmonisa ketiga adalah?", "id": "Tiga kali frekuensi fundamental." },
{ "en": "Analisis rangkaian dengan sumber non-sinusoidal?", "id": "Menggunakan teorema superposisi per frekuensi." },
{ "en": "Respons total adalah jumlah dari?", "id": "Respons DC dan semua respons harmonisa." },
{ "en": "Daya rata-rata total adalah jumlah?", "id": "Daya rata-rata setiap komponen." },
{ "en": "Nilai RMS total adalah akar dari?", "id": "Jumlah kuadrat nilai RMS komponen." },
{ "en": "Bentuk gelombang persegi hanya memiliki harmonisa?", "id": "Ganjil." },
{ "en": "Filter dapat digunakan untuk menghilangkan?", "id": "Harmonisa yang tidak diinginkan." },
{ "en": "Distorsi Harmonisa Total (THD) mengukur?", "id": "Tingkat distorsi bentuk gelombang." },
{ "en": "Beban non-linier adalah sumber dari?", "id": "Arus harmonisa." },
{ "en": "Contoh beban non-linier?", "id": "Penyearah, catu daya switching." },
{ "en": "Kopling magnetis nol berarti M?", "id": "Sama dengan nol." },
{ "en": "Transformator ideal tidak memiliki?", "id": "Kerugian daya." },
{ "en": "Permeabilitas inti trafo ideal?", "id": "Tak hingga." },
{ "en": "Lilitan trafo ideal memiliki resistansi?", "id": "Nol." },
{ "en": "Apa itu arus magnetisasi?", "id": "Arus untuk menghasilkan fluks di inti." },
{ "en": "Transformator frekuensi audio bekerja pada rentang?", "id": "20 Hz hingga 20 kHz." },
{ "en": "Plot respons frekuensi disebut juga?", "id": "Plot Bode." },
{ "en": "Pada frekuensi cut-off, daya berkurang menjadi?", "id": "Setengahnya." },
{ "en": "Pada frekuensi cut-off, tegangan berkurang menjadi?", "id": "70.7%." },
{ "en": "Tingkat penurunan filter disebut?", "id": "Roll-off rate." },
{ "en": "Filter orde kedua memiliki roll-off?", "id": "-40 dB/dekade." },
{ "en": "Fasa pada filter lolos bawah orde satu?", "id": "Berubah dari 0 hingga -90 derajat." },
{ "en": "Fasa pada filter lolos atas orde satu?", "id": "Berubah dari +90 hingga 0 derajat." },
{ "en": "Faktor kualitas Q tinggi berarti?", "id": "Selektivitas tinggi." },
{ "en": "Resonansi seri kadang disebut?", "id": "Resonansi tegangan." },
{ "en": "Resonansi paralel kadang disebut?", "id": "Resonansi arus." },
{ "en": "Dalam resonansi paralel, impedansi?", "id": "Maksimum." },
{ "en": "Dalam resonansi seri, impedansi?", "id": "Minimum." },
{ "en": "Rangkaian anti-resonansi adalah nama lain dari?", "id": "Resonansi paralel." },
{ "en": "Rangkaian penolak (rejector circuit)?", "id": "Resonansi paralel." },
{ "en": "Rangkaian penerima (acceptor circuit)?", "id": "Resonansi seri." },
{ "en": "Impedansi karakteristik saluran transmisi?", "id": "Akar dari L per C." },
{ "en": "Analisis keadaan tunak mengasumsikan waktu?", "id": "Setelah kondisi transien berakhir." },
{ "en": "Sumber tegangan dependen dikontrol oleh?", "id": "Tegangan atau arus lain." },
{ "en": "Sumber arus dependen dikontrol oleh?", "id": "Tegangan atau arus lain." },
{ "en": "VCVS adalah singkatan dari?", "id": "Voltage-Controlled Voltage Source." },
{ "en": "CCCS adalah singkatan dari?", "id": "Current-Controlled Current Source." },
{ "en": "Dualitas antara kapasitor dan?", "id": "Induktor." },
{ "en": "Dualitas antara resistansi dan?", "id": "Konduktansi." },
{ "en": "Dualitas antara simpul dan?", "id": "Jala (mesh)." },
{ "en": "Dualitas antara hubung singkat dan?", "id": "Rangkaian terbuka." },
{ "en": "Bentuk gelombang trapezoid mengandung harmonisa?", "id": "Ganjil dan genap." },
{ "en": "Simetri gelombang membantu menyederhanakan?", "id": "Perhitungan koefisien Fourier." },
{ "en": "Nilai rata-rata gelombang disebut juga?", "id": "Komponen DC." },
{ "en": "Efek Gibbs terjadi pada?", "id": "Sinyal dengan diskontinuitas." },
{ "en": "Apa itu Efek Gibbs?", "id": "Overshoot pada aproksimasi Fourier." },
{ "en": "Transformasi Fourier Cepat (FFT) adalah?", "id": "Algoritma untuk menghitung spektrum." },
{ "en": "Sistem LTI adalah singkatan dari?", "id": "Linear Time-Invariant." },
{ "en": "Respons sistem LTI terhadap sinusoida?", "id": "Sinusoida dengan frekuensi sama." },
{ "en": "Hanya amplitudo dan fasa yang?", "id": "Berubah." },
{ "en": "Daya tiga fasa lebih halus karena?", "id": "Daya sesaatnya konstan." },
{ "en": "Tegangan fasa pada beban delta?", "id": "Sama dengan tegangan line." },
{ "en": "Arus fasa pada beban bintang?", "id": "Sama dengan arus line." },
{ "en": "Konversi Y-Δ digunakan untuk menyederhanakan?", "id": "Rangkaian tiga fasa." },
{ "en": "Daya reaktif dalam sistem tak seimbang?", "id": "Didefinisikan dengan berbagai cara." },
{ "en": "Faktor daya pada sistem tak seimbang?", "id": "Bisa didefinisikan secara berbeda." },
{ "en": "Urutan fasa menentukan arah putaran?", "id": "Motor tiga fasa." },
{ "en": "Indikator urutan fasa digunakan untuk?", "id": "Menentukan urutan fasa ABC atau ACB." },
{ "en": "Daya pada beban Y tak seimbang?", "id": "P = |VA| |IA| cos(θA) + ... " },
{ "en": "Transformator ideal pada DC?", "id": "Berperilaku seperti hubung singkat." },
{ "en": "Kopling kritis terjadi saat?", "id": "Transfer energi maksimum antar kumparan." },
{ "en": "Filter Notch adalah nama lain untuk?", "id": "Filter tolak pita (band-stop)." },
{ "en": "Gain filter pasif selalu?", "id": "Kurang dari atau sama dengan satu." },
{ "en": "Gain filter aktif bisa?", "id": "Lebih dari satu." },
{ "en": "Rangkaian yang membedakan sinyal input?", "id": "Differentiator." },
{ "en": "Differentiator sederhana dibuat dari?", "id": "Resistor dan kapasitor (output di R)." },
{ "en": "Rangkaian yang mengintegrasikan sinyal input?", "id": "Integrator." },
{ "en": "Integrator sederhana dibuat dari?", "id": "Resistor dan kapasitor (output di C)." },
{ "en": "Fungsi transfer differentiator ideal?", "id": "H(s) = s." },
{ "en": "Fungsi transfer integrator ideal?", "id": "H(s) = 1/s." },
{ "en": "Respons frekuensi integrator?", "id": "Seperti filter lolos bawah." },
{ "en": "Respons frekuensi differentiator?", "id": "Seperti filter lolos atas." },
{ "en": "Masalah pada differentiator praktis?", "id": "Menguatkan noise frekuensi tinggi." },
{ "en": "Plot lokasi pole dan zero di bidang-s?", "id": "Diagram Pole-Zero." },
{ "en": "Sistem stabil jika semua pole berada di?", "id": "Separuh bidang kiri (LHP)." },
{ "en": "Sumbu imajiner pada bidang-s merepresentasikan?", "id": "Keadaan tunak sinusoidal (s = jω)." },
{ "en": "Jarak pole dari sumbu imajiner menentukan?", "id": "Laju peluruhan transien." },
{ "en": "Jarak pole dari sumbu riil menentukan?", "id": "Frekuensi osilasi transien." },
{ "en": "Zero pada sumbu jω menyebabkan?", "id": "Output nol pada frekuensi tersebut." },
{ "en": "Plot Nyquist digunakan untuk menentukan?", "id": "Stabilitas sistem umpan balik." },
{ "en": "Kriteria Stabilitas Nyquist berhubungan dengan?", "id": "Jumlah putaran mengelilingi titik -1." },
{ "en": "Gain margin adalah ukuran?", "id": "Seberapa jauh dari ketidakstabilan (gain)." },
{ "en": "Phase margin adalah ukuran?", "id": "Seberapa jauh dari ketidakstabilan (fasa)." },
{ "en": "Transformasi Laplace mengubah persamaan diferensial menjadi?", "id": "Persamaan aljabar." },
{ "en": "Variabel dalam domain Laplace?", "id": "s." },
{ "en": "Impedansi L dalam domain Laplace?", "id": "sL." },
{ "en": "Impedansi C dalam domain Laplace?", "id": "1/(sC)." },
{ "en": "Analisis Laplace bisa menangani?", "id": "Respons transien dan tunak." },
{ "en": "Kondisi awal pada kapasitor/induktor dimodelkan sebagai?", "id": "Sumber tegangan atau arus." },
{ "en": "Invers Transformasi Laplace mengembalikan sinyal ke?", "id": "Domain waktu." },
{ "en": "Metode untuk invers Laplace?", "id": "Ekspansi pecahan parsial." },
{ "en": "Teorema Nilai Awal menghubungkan?", "id": "Perilaku s → ∞ dengan t = 0+." },
{ "en": "Teorema Nilai Akhir menghubungkan?", "id": "Perilaku s → 0 dengan t → ∞." },
{ "en": "Syarat Teorema Nilai Akhir?", "id": "Sistem harus stabil." },
{ "en": "Apa itu konvolusi di domain waktu?", "id": "Perkalian di domain frekuensi/Laplace." },
{ "en": "Respons impuls h(t) adalah?", "id": "Output sistem untuk input delta Dirac." },
{ "en": "Transformasi Laplace dari respons impuls?", "id": "Fungsi transfer H(s)." },
{ "en": "Rangkaian all-pass filter mengubah?", "id": "Hanya fasa, bukan magnitudo." },
{ "en": "Equalizer digunakan untuk?", "id": "Mengubah respons frekuensi sistem." },
{ "en": "Analisis AC mengasumsikan komponen?", "id": "Linier dan invarian waktu." },
{ "en": "Resistor adalah komponen linier?", "id": "Ya." },
{ "en": "Dioda adalah komponen linier?", "id": "Tidak, bersifat non-linier." },
{ "en": "Model sinyal kecil untuk transistor?", "id": "Melinierkan perilaku di sekitar titik operasi." },
{ "en": "Gelombang termodulasi (AM/FM) adalah?", "id": "Sinyal non-sinusoidal." },
{ "en": "Analisis spektrum sinyal AM menunjukkan?", "id": "Frekuensi pembawa dan sideband." },
{ "en": "Filter kristal memiliki faktor Q?", "id": "Sangat tinggi." },
{ "en": "Penggunaan filter kristal?", "id": "Filter IF radio yang sangat selektif." },
{ "en": "Jaringan pencocokan impedansi tipe-L?", "id": "Menggunakan dua elemen reaktif (L dan C)." },
{ "en": "Diagram Smith adalah alat grafis untuk?", "id": "Analisis saluran transmisi dan impedansi." },
{ "en": "Diagram Smith memetakan bidang impedansi ke?", "id": "Lingkaran." },
{ "en": "Pusat Diagram Smith merepresentasikan?", "id": "Impedansi karakteristik (misal: 50 Ohm)." },
{ "en": "Lingkaran luar Diagram Smith merepresentasikan?", "id": "Reaktansi murni." },
{ "en": "Gerakan pada Diagram Smith?", "id": "Menambahkan panjang saluran atau komponen." },
{ "en": "Parameter hamburan (S-parameters) digunakan pada?", "id": "Frekuensi tinggi (RF/Microwave)." },
{ "en": "S11 pada S-parameters?", "id": "Koefisien pantul input." },
{ "en": "S21 pada S-parameters?", "id": "Gain transmisi maju." },
{ "en": "Keuntungan S-parameters?", "id": "Lebih mudah diukur daripada Z atau Y." },
{ "en": "Analisis vektor jaringan (VNA) mengukur?", "id": "S-parameters." },
{ "en": "Apa itu isolasi pada jaringan dua-port?", "id": "Ukuran seberapa baik sinyal output terisolasi." },
{ "en": "Apa itu 'group delay'?", "id": "Ukuran distorsi fasa." },
{ "en": "Group delay konstan berarti?", "id": "Semua frekuensi tertunda sama, bentuk sinyal terjaga." },
{ "en": "Filter Bessel dirancang untuk memiliki?", "id": "Group delay yang paling datar." },
{ "en": "Dualitas antara Thevenin dan?", "id": "Norton." },
{ "en": "Dualitas antara transformasi Y-Δ dan?", "id": "Transformasi Δ-Y." },
{ "en": "Kapasitansi Miller adalah efek?", "id": "Kapasitansi umpan balik pada penguat pembalik." },
{ "en": "Efek Miller menyebabkan impedansi input?", "id": "Terlihat lebih kecil." },
{ "en": "Frekuensi transisi (fT) pada transistor?", "id": "Frekuensi saat gain arus menjadi satu." },
{ "en": "Osilator adalah rangkaian yang?", "id": "Menghasilkan sinyal periodik tanpa input." },
{ "en": "Kriteria osilasi Barkhausen?", "id": "Gain loop = 1, pergeseran fasa loop = 0°." },
{ "en": "Osilator jembatan Wien menggunakan umpan balik?", "id": "Positif dan negatif." },
{ "en": "Osilator Colpitts dan Hartley adalah osilator?", "id": "LC." },
{ "en": "Osilator kristal menggunakan?", "id": "Resonansi mekanis kristal kuarsa." },
{ "en": "Phase-Locked Loop (PLL) digunakan untuk?", "id": "Sintesis frekuensi dan demodulasi FM." },
{ "en": "Resonansi pada saluran transmisi?", "id": "Terjadi pada kelipatan setengah panjang gelombang." },
{ "en": "Voltage Standing Wave Ratio (VSWR)?", "id": "Ukuran ketidakcocokan impedansi." },
{ "en": "VSWR = 1 berarti?", "id": "Pencocokan sempurna, tidak ada pantulan." },
{ "en": "VSWR tak hingga berarti?", "id": "Pantulan total (terbuka atau hubung singkat)." },
{ "en": "Efek Doppler pada sinyal AC?", "id": "Pergeseran frekuensi akibat gerakan." },
{ "en": "Kabel koaksial adalah jenis?", "id": "Saluran transmisi." },
{ "en": "Mode propagasi utama di kabel koaksial?", "id": "Mode TEM." },
{ "en": "Pemandu gelombang (waveguide) digunakan pada?", "id": "Frekuensi sangat tinggi (microwave)." },
{ "en": "Pemandu gelombang meloloskan frekuensi di atas?", "id": "Frekuensi cutoff." },
{ "en": "Antena mengubah gelombang terpandu menjadi?", "id": "Gelombang ruang bebas." },
{ "en": "Impedansi input antena harus?", "id": "Cocok dengan saluran transmisi." },
{ "en": "Balun digunakan untuk menghubungkan?", "id": "Saluran seimbang dan tak seimbang." },
{ "en": "Contoh saluran seimbang?", "id": "Kabel twin-lead." },
{ "en": "Contoh saluran tak seimbang?", "id": "Kabel koaksial." },
{ "en": "Analisis AC mengasumsikan parameter rangkaian?", "id": "Terkumpul (lumped)." },
{ "en": "Kapan asumsi parameter terkumpul tidak valid?", "id": "Saat dimensi fisik sebanding panjang gelombang." },
{ "en": "Analisis parameter terdistribusi menggunakan?", "id": "Persamaan Telegrapher." },
{ "en": "Daya aktif total tiga fasa seimbang?", "id": "3 * Vp * Ip * cos(θ)." },
{ "en": "Konduktor netral pada sistem delta?", "id": "Tidak ada." },
{ "en": "Daya reaktif dapat membangkitkan medan?", "id": "Magnetik dan listrik." },
{ "en": "Arus sirkulasi dapat terjadi pada?", "id": "Transformator paralel yang tidak identik." },
{ "en": "Ferrite bead digunakan untuk?", "id": "Menekan noise frekuensi tinggi." },
{ "en": "Apa itu konduktansi (G)?", "id": "Kebalikan dari resistansi." },
{ "en": "Apa itu suseptansi (B)?", "id": "Kebalikan dari reaktansi." },
{ "en": "Apa itu admitansi (Y)?", "id": "Kebalikan dari impedansi." },
{ "en": "Rumus Y dalam G dan B?", "id": "Y = G + jB." },
{ "en": "Suseptansi induktif (BL) nilainya?", "id": "Negatif." },
{ "en": "Suseptansi kapasitif (BC) nilainya?", "id": "Positif." },
{ "en": "Satuan G, B, dan Y?", "id": "Siemens (S) atau mho." },
{ "en": "Konsep admitansi berguna untuk rangkaian?", "id": "Paralel." },
{ "en": "Admitansi ekuivalen paralel adalah?", "id": "Jumlah dari semua admitansi." },
{ "en": "Faktor daya dapat dihitung dari admitansi?", "id": "Ya, cos(sudut Y)." },
{ "en": "Segitiga admitansi terdiri dari?", "id": "G, B, dan Y." },
{ "en": "Fasor tegangan pada komponen paralel?", "id": "Sama." },
{ "en": "Fasor arus pada komponen seri?", "id": "Sama." },
{ "en": "Apa itu sirkuit terbuka (open circuit)?", "id": "Impedansi tak hingga, arus nol." },
{ "en": "Apa itu hubung singkat (short circuit)?", "id": "Impedansi nol, tegangan nol." },
{ "en": "Representasi sinusoida dalam bentuk eksponensial?", "id": "Menggunakan rumus Euler." },
{ "en": "A cos(ωt + φ) dalam bentuk eksponensial?", "id": "Bagian riil dari A*e^(j(ωt+φ))." },
{ "en": "Keuntungan notasi eksponensial?", "id": "Menyederhanakan perkalian dan pembagian." },
{ "en": "Apa itu frekuensi angular?", "id": "Frekuensi sudut (ω)." },
{ "en": "Identitas trigonometri berguna untuk?", "id": "Menjumlahkan sinusoida frekuensi sama." },
{ "en": "Acos(x) + Bsin(x) dapat diubah menjadi?", "id": "Rcos(x - α)." },
{ "en": "Plot lintasan ujung fasor resultante?", "id": "Lokus." },
{ "en": "Lokus impedansi RLC seri?", "id": "Garis lurus vertikal." },
{ "en": "Lokus admitansi RLC paralel?", "id": "Garis lurus vertikal." },
{ "en": "Lokus impedansi RC seri (R variabel)?", "id": "Garis lurus horizontal." },
{ "en": "Lokus impedansi RC seri (C variabel)?", "id": "Setengah lingkaran." },
{ "en": "Daya rata-rata maksimum dari sumber?", "id": "Terjadi saat pencocokan konjugat." },
{ "en": "Energi yang tersimpan di induktor?", "id": "W = 1/2 * L * I^2." },
{ "en": "Energi yang tersimpan di kapasitor?", "id": "W = 1/2 * C * V^2." },
{ "en": "Energi dalam rangkaian AC?", "id": "Berosilasi antara komponen reaktif." },
{ "en": "Daya terdisipasi dalam rangkaian AC?", "id": "Hanya pada resistansi." },
{ "en": "Daya kompleks pada kapasitor?", "id": "S = -j * |I|^2 * XC." },
{ "en": "Daya kompleks pada induktor?", "id": "S = +j * |I|^2 * XL." },
{ "en": "Keseimbangan daya reaktif berarti?", "id": "Total Q disuplai = total Q diserap." },
{ "en": "Sumber tegangan bisa menyerap daya?", "id": "Ya, jika arus mengalir ke terminal positif." },
{ "en": "Daya kompleks S = VI* kenapa pakai I*?", "id": "Untuk mendapatkan selisih sudut fasa yang benar." },
{ "en": "Jika S = 100 ∠30°, P adalah?", "id": "100 * cos(30°) = 86.6 W." },
{ "en": "Jika S = 100 ∠30°, Q adalah?", "id": "100 * sin(30°) = 50 VAR." },
{ "en": "Jika S = 100 ∠-30°, beban bersifat?", "id": "Kapasitif." },
{ "en": "Daya reaktif yang disuplai oleh sumber?", "id": "Bisa positif atau negatif." },
{ "en": "Daya total tiga fasa dari wattmeter?", "id": "P_total = P1 + P2." },
{ "en": "Sudut faktor daya dari pembacaan wattmeter?", "id": "θ = arctan(akar(3)*(P1-P2)/(P1+P2))." },
{ "en": "Metode ini valid untuk urutan fasa apa?", "id": "ABC atau ACB, tapi rumus Q berbeda." },
{ "en": "Harmonisa triplen adalah?", "id": "Harmonisa kelipatan tiga (3, 6, 9, ...)." },
{ "en": "Sifat harmonisa triplen pada sistem Y?", "id": "Sefasa di ketiga fasa." },
{ "en": "Arus harmonisa triplen mengalir di?", "id": "Kawat netral." },
{ "en": "Arus netral bisa lebih besar dari arus fasa?", "id": "Ya, karena adanya harmonisa triplen." },
{ "en": "Harmonisa triplen pada koneksi delta?", "id": "Terperangkap dan bersirkulasi dalam loop delta." },
{ "en": "Transformator Y-Δ dapat memblokir?", "id": "Harmonisa triplen." },
{ "en": "Filter K-faktor pada transformator?", "id": "Dirancang untuk menangani arus harmonisa." },
{ "en": "Analisis rangkaian magnetik analog dengan?", "id": "Analisis rangkaian listrik DC." },
{ "en": "Gaya gerak magnet (MMF) analog dengan?", "id": "Tegangan (EMF)." },
{ "en": "Fluks magnetik (Φ) analog dengan?", "id": "Arus listrik." },
{ "en": "Reluktansi (ℜ) analog dengan?", "id": "Resistansi (R)." },
{ "en": "Hukum Hopkinson untuk rangkaian magnetik?", "id": "MMF = Φ * ℜ." },
{ "en": "Bahan dengan reluktansi rendah?", "id": "Bahan feromagnetik (besi)." },
{ "en": "Celah udara pada inti magnetik?", "id": "Memiliki reluktansi sangat tinggi." },
{ "en": "Histeresis pada inti magnetik menyebabkan?", "id": "Kerugian daya (rugi histeresis)." },
{ "en": "Arus Eddy pada inti magnetik menyebabkan?", "id": "Kerugian daya (rugi eddy)." },
{ "en": "Inti dilaminasi untuk mengurangi?", "id": "Kerugian arus Eddy." },
{ "en": "Kurva B-H menunjukkan hubungan antara?", "id": "Kerapatan fluks dan intensitas medan." },
{ "en": "Saturasi pada inti magnetik?", "id": "Saat peningkatan MMF tidak menambah fluks." },
{ "en": "Induktansi adalah sifat rangkaian yang?", "id": "Menentang perubahan arus." },
{ "en": "Kapasitansi adalah sifat rangkaian yang?", "id": "Menentang perubahan tegangan." },
{ "en": "Apa itu 'ringing' pada sinyal?", "id": "Osilasi teredam setelah transien." },
{ "en": "Ringing disebabkan oleh?", "id": "Resonansi pada komponen parasitik." },
{ "en": "Bypass capacitor digunakan untuk?", "id": "Menyediakan jalur impedansi rendah untuk noise AC." },
{ "en": "Decoupling capacitor digunakan untuk?", "id": "Mengisolasi bagian rangkaian dari noise catu daya." },
{ "en": "Kapasitor keramik baik untuk frekuensi?", "id": "Tinggi." },
{ "en": "Kapasitor elektrolit baik untuk?", "id": "Nilai kapasitansi besar, frekuensi rendah." },
{ "en": "Resistor film karbon vs film logam?", "id": "Film logam lebih stabil dan presisi." },
{ "en": "Resistor wirewound baik untuk?", "id": "Daya tinggi, tapi punya induktansi parasitik." },
{ "en": "Induktor inti udara vs inti besi?", "id": "Inti besi punya induktansi lebih tinggi." },
{ "en": "Kerugian pada kapasitor disebut?", "id": "Equivalent Series Resistance (ESR)." },
{ "en": "Q-faktor induktor adalah rasio?", "id": "Reaktansi terhadap resistansi serinya." },
{ "en": "Model rangkaian untuk kristal kuarsa?", "id": "RLC seri paralel dengan kapasitor." },
{ "en": "Kristal memiliki resonansi?", "id": "Seri dan paralel yang sangat berdekatan." },
{ "en": "Sirkuit terpadu (IC) adalah contoh?", "id": "Rangkaian yang sangat kompleks." },
{ "en": "Analisis AC pada IC?", "id": "Menggunakan model sinyal kecil untuk transistor." },
{ "en": "Impedansi output dari emitor follower?", "id": "Rendah." },
{ "en": "Impedansi input dari common base amplifier?", "id": "Rendah." },
{ "en": "Impedansi input dari common emitter amplifier?", "id": "Sedang." },
{ "en": "Penguat diferensial dirancang untuk menguatkan?", "id": "Selisih antara dua sinyal." },
{ "en": "Common-Mode Rejection Ratio (CMRR)?", "id": "Kemampuan menolak sinyal yang sama di kedua input." },
{ "en": "CMRR yang baik nilainya?", "id": "Sangat tinggi (dalam dB)." },
{ "en": "Slew rate pada op-amp membatasi?", "id": "Respons frekuensi sinyal besar." },
{ "en": "Gain-Bandwidth Product (GBW) pada op-amp?", "id": "Konstan." },
{ "en": "Jika gain dinaikkan, bandwidth op-amp?", "id": "Menurun." },
{ "en": "Umpan balik negatif pada penguat?", "id": "Menstabilkan gain, mengubah impedansi." },
{ "en": "Umpan balik positif pada penguat?", "id": "Menyebabkan osilasi." },
{ "en": "Apakah transformator bekerja pada DC?", "id": "Tidak." },
{ "en": "Respons frekuensi adalah plot dari?", "id": "Fungsi transfer H(jω)." },
{ "en": "Orde sebuah filter ditentukan oleh?", "id": "Jumlah total kapasitor dan induktor." },
{ "en": "Filter orde tinggi memberikan roll-off?", "id": "Lebih curam." },
{ "en": "Topologi filter Sallen-Key adalah filter?", "id": "Aktif orde kedua." },
{ "en": "Topologi filter Multiple Feedback (MFB)?", "id": "Filter aktif." },
{ "en": "Filter state-variable dapat menghasilkan output?", "id": "LP, HP, dan BP secara bersamaan." },
{ "en": "Filter switched-capacitor menggunakan?", "id": "Kapasitor yang di-switch untuk mensimulasikan resistor." },
{ "en": "Keuntungan filter switched-capacitor?", "id": "Frekuensi cut-off dapat diatur oleh clock." },
{ "en": "Filter digital memproses sinyal dalam bentuk?", "id": "Sampel digital." },
{ "en": "Respon Impuls Hingga (FIR) filter?", "id": "Jenis filter digital." },
{ "en": "Respon Impuls Tak Hingga (IIR) filter?", "id": "Jenis filter digital dengan umpan balik." },
{ "en": "Penormalan dalam desain filter?", "id": "Menskalakan komponen untuk R=1 atau ωc=1." },
{ "en": "Konversi dari LPF ke HPF?", "id": "Mengganti s dengan 1/s." },
{ "en": "Penyalaan tiba-tiba sumber AC menyebabkan?", "id": "Respons transien." },
{ "en": "Bagian transien dari solusi disebut?", "id": "Respons natural atau homogen." },
{ "en": "Bagian keadaan tunak dari solusi disebut?", "id": "Respons paksa atau partikular." },
{ "en": "Respons natural RLC seri bisa berbentuk?", "id": "Overdamped, critically damped, underdamped." },
{ "en": "Bentuk respons natural tergantung pada?", "id": "Rasio redaman (damping ratio)." },
{ "en": "Respon underdamped berarti?", "id": "Osilasi teredam." },
{ "en": "Respon overdamped berarti?", "id": "Peluruhan lambat tanpa osilasi." },
{ "en": "Arus DC pada induktor dalam keadaan tunak?", "id": "Konstan." },
{ "en": "Tegangan DC pada kapasitor dalam keadaan tunak?", "id": "Konstan." },
{ "en": "Aturan kontinuitas untuk kapasitor?", "id": "Tegangan tidak bisa berubah seketika." },
{ "en": "Aturan kontinuitas untuk induktor?", "id": "Arus tidak bisa berubah seketika." },
{ "en": "Analisis untuk t=0+ berarti?", "id": "Sesaat setelah saklar beraksi." },
{ "en": "Analisis untuk t=∞ berarti?", "id": "Setelah waktu yang sangat lama (keadaan tunak DC)." },
{ "en": "Faktor daya perpindahan (displacement PF)?", "id": "Faktor daya pada frekuensi fundamental." },
{ "en": "Faktor daya sebenarnya (true PF)?", "id": "Memperhitungkan distorsi harmonisa." },
{ "en": "True PF selalu ... dari displacement PF?", "id": "Lebih kecil atau sama dengan." },
{ "en": "P = Vrms * Irms * PF hanya berlaku jika?", "id": "Tegangan dan arus sinusoidal." },
{ "en": "Sistem tenaga listrik di kapal?", "id": "Bisa tidak diground-kan (IT system)." },
{ "en": "Sistem pentanahan TT, TN, IT?", "id": "Standar sistem grounding." },
{ "en": "Ferroresonansi adalah fenomena non-linier pada?", "id": "Transformator dan kapasitor." },
{ "en": "Ferroresonansi dapat menyebabkan?", "id": "Tegangan lebih yang sangat tinggi." },
{ "en": "Arus inrush pada transformator?", "id": "Lonjakan arus saat pertama kali dihidupkan." },
{ "en": "Arus inrush kaya akan?", "id": "Harmonisa, terutama harmonisa kedua." },
{ "en": "Proteksi diferensial transformator harus?", "id": "Tahan terhadap arus inrush." },
{ "en": "Medan magnet berputar dihasilkan oleh?", "id": "Arus tiga fasa pada lilitan stator." },
{ "en": "Kecepatan medan putar disebut?", "id": "Kecepatan sinkron." },
{ "en": "Kecepatan sinkron bergantung pada?", "id": "Frekuensi dan jumlah kutub." },
{ "en": "Slip pada motor induksi?", "id": "Perbedaan antara kecepatan sinkron dan rotor." },
{ "en": "Rangkaian ekuivalen motor induksi mirip?", "id": "Rangkaian ekuivalen transformator." },
{ "en": "Beban pada motor dimodelkan sebagai?", "id": "Resistansi variabel yang bergantung slip." },
{ "en": "Motor sinkron berputar pada kecepatan?", "id": "Sinkron (slip = 0)." },
{ "en": "Eksitasi pada motor sinkron mengontrol?", "id": "Faktor daya (daya reaktif)." },
{ "en": "Generator sinkron adalah sumber utama?", "id": "Listrik di pembangkit listrik." },
{ "en": "Kurva kapabilitas generator menunjukkan batasan?", "id": "Operasi daya aktif dan reaktif." },
{ "en": "Jaringan interkoneksi listrik (grid) memerlukan?", "id": "Sinkronisasi antar generator." },
{ "en": "Kondisi sinkronisasi?", "id": "Tegangan sama, frekuensi sama, fasa sama." },
{ "en": "Stabilitas sistem tenaga adalah kemampuan?", "id": "Untuk tetap sinkron setelah gangguan." },
{ "en": "Insulator pada saluran transmisi berfungsi?", "id": "Mengisolasi konduktor dari menara." },
{ "en": "Kabel OPGW pada saluran transmisi?", "id": "Kabel tanah dengan serat optik." },
{ "en": "Kompensasi seri pada saluran transmisi?", "id": "Menggunakan kapasitor seri untuk mengurangi reaktansi." },
{ "en": "Kompensasi shunt pada saluran transmisi?", "id": "Menggunakan reaktor/kapasitor untuk mengontrol tegangan." },
{ "en": "Efek Ferranti adalah?", "id": "Kenaikan tegangan di ujung saluran." },
{ "en": "Efek Ferranti terjadi pada?", "id": "Saluran panjang tanpa beban atau beban ringan." },
{ "en": "Penyebab efek Ferranti?", "id": "Kapasitansi saluran." },
{ "en": "Reaktor shunt digunakan untuk mengatasi?", "id": "Efek Ferranti." },
{ "en": "Daya aktif mengalir dari sudut fasa?", "id": "Tinggi ke rendah." },
{ "en": "Daya reaktif mengalir dari tegangan?", "id": "Tinggi ke rendah." },
{ "en": "Studi aliran daya (load flow) menentukan?", "id": "Tegangan, arus, dan aliran daya di jaringan." },
{ "en": "Bus slack atau swing pada studi aliran daya?", "id": "Bus referensi yang menyeimbangkan daya." },
{ "en": "Bus PQ atau beban?", "id": "Daya aktif dan reaktifnya diketahui." },
{ "en": "Bus PV atau generator?", "id": "Daya aktif dan magnitudo tegangannya diketahui." },
{ "en": "Metode Newton-Raphson digunakan untuk?", "id": "Menyelesaikan persamaan aliran daya." },
{ "en": "Matriks admitansi bus (Y-bus) digunakan dalam?", "id": "Studi aliran daya." },
{ "en": "Analisis hubung singkat menentukan?", "id": "Arus gangguan maksimum." },
{ "en": "Arus hubung singkat digunakan untuk?", "id": "Menentukan peringkat peralatan proteksi." },
{ "en": "Impedansi urutan nol, positif, negatif?", "id": "Impedansi komponen sistem terhadap arus urutan." },
{ "en": "Impedansi urutan nol seringkali berbeda?", "id": "Sangat berbeda dari urutan positif/negatif." },
{ "en": "Hubungan V/I pada titik resonansi?", "id": "Resistif." },
{ "en": "Apakah nilai RMS bisa dihitung untuk sinyal non-periodik?", "id": "Tidak, konsepnya untuk sinyal periodik." },
{ "en": "Energi sinyal non-periodik?", "id": "Dihitung menggunakan integral kuadratnya." },
{ "en": "Daya sinyal periodik?", "id": "Dihitung sebagai rata-rata kuadratnya." },
{ "en": "Gelombang sinus murni adalah sinyal?", "id": "Sinyal daya." },
{ "en": "Pulsa tunggal adalah sinyal?", "id": "Sinyal energi." },
{ "en": "Fungsi autokorelasi mengukur?", "id": "Kemiripan sinyal dengan versi dirinya yang bergeser." },
{ "en": "Kepadatan Spektral Daya (PSD) adalah?", "id": "Transformasi Fourier dari fungsi autokorelasi." },
{ "en": "PSD menunjukkan distribusi?", "id": "Daya sinyal pada berbagai frekuensi." },
{ "en": "Derau putih (white noise) memiliki PSD?", "id": "Datar (konstan)." },
{ "en": "Filter Wiener adalah filter optimal untuk?", "id": "Mengurangi noise dari sinyal." },
{ "en": "Skematik rangkaian vs diagram pengkabelan?", "id": "Skematik untuk fungsi, pengkabelan untuk koneksi fisik." },
{ "en": "Rangkaian cetak (PCB) adalah?", "id": "Papan untuk memasang dan menghubungkan komponen." },
{ "en": "Jejak (trace) pada PCB memiliki?", "id": "Resistansi dan induktansi parasitik." },
{ "en": "Bidang tanah (ground plane) pada PCB?", "id": "Membantu mengurangi noise dan impedansi." },
{ "en": "Resistor SMD adalah?", "id": "Resistor untuk pemasangan di permukaan." },
{ "en": "Kode pada resistor SMD menunjukkan?", "id": "Nilai resistansinya." },
{ "en": "Kelistrikan statis (ESD) dapat?", "id": "Merusak komponen elektronik sensitif." },
{ "en": "Gelang anti-statis digunakan untuk?", "id": "Mencegah kerusakan akibat ESD." },
{ "en": "Lampu neon adalah beban?", "id": "Non-linier dan kapasitif." },
{ "en": "Tegangan line-to-line sama dengan?", "id": "Tegangan antar-fasa." },
{ "en": "Tegangan line-to-neutral sama dengan?", "id": "Tegangan fasa." },
{ "en": "Fasor adalah bilangan kompleks yang merepresentasikan?", "id": "Amplitudo dan fasa sinusoida." },
{ "en": "Keadaan tunak sinusoidal berarti?", "id": "Respons paksa terhadap input sinusoidal." },
{ "en": "Semua transien telah?", "id": "Meluruh menjadi nol." },
{ "en": "Frekuensi respons sama dengan?", "id": "Frekuensi sumber." },
{ "en": "Resistansi tidak menyimpan?", "id": "Energi." },
{ "en": "Reaktansi tidak mendisipasikan?", "id": "Daya rata-rata." },
{ "en": "Lagging PF berarti arus?", "id": "Tertinggal dari tegangan." },
{ "en": "Leading PF berarti arus?", "id": "Mendahului tegangan." },
{ "en": "Daya reaktif positif berarti?", "id": "Beban induktif (menyerap Q)." },
{ "en": "Daya reaktif negatif berarti?", "id": "Beban kapasitif (menyuplai Q)." },
{ "en": "Daya semu adalah batas geometris dari?", "id": "Daya yang bisa dikirimkan." },
{ "en": "Koreksi PF mengurangi komponen?", "id": "Imajiner dari arus total." },
{ "en": "Resonansi adalah kondisi keseimbangan energi?", "id": "Antara induktor dan kapasitor." },
{ "en": "Faktor kualitas Q juga disebut?", "id": "Faktor magnifikasi." },
{ "en": "Bandwidth yang sempit berarti selektivitas?", "id": "Tinggi." },
{ "en": "Filter pasif tidak memerlukan?", "id": "Sumber daya eksternal." },
{ "en": "Transformator bekerja berdasarkan prinsip?", "id": "Induksi elektromagnetik." },
{ "en": "Sistem tiga fasa seimbang dapat dianalisis?", "id": "Secara per fasa." },
{ "en": "Daya total tiga fasa diukur dengan?", "id": "Dua wattmeter." },
{ "en": "Harmonisa adalah kelipatan dari?", "id": "Frekuensi fundamental." },
{ "en": "Jaringan dua-port mengkarakterisasi?", "id": "Hubungan input-output." },
{ "en": "Analisis Laplace lebih umum daripada?", "id": "Analisis fasor." },
{ "en": "Respons frekuensi adalah kasus khusus dari?", "id": "Fungsi transfer." },
{ "en": "Nilai RMS adalah ukuran?", "id": "Efek pemanasan dari sinyal AC." },
{ "en": "Nilai rata-rata gelombang sinus?", "id": "Nol." },
{ "en": "Impedansi adalah rasio fasor?", "id": "Tegangan terhadap arus." },
{ "en": "Admitansi adalah rasio fasor?", "id": "Arus terhadap tegangan." },
{ "en": "KVL versi fasor?", "id": "Jumlah fasor tegangan dalam loop adalah nol." },
{ "en": "KCL versi fasor?", "id": "Jumlah fasor arus di simpul adalah nol." },
{ "en": "Thevenin menyederhanakan rangkaian menjadi?", "id": "Sumber tegangan praktis." },
{ "en": "Norton menyederhanakan rangkaian menjadi?", "id": "Sumber arus praktis." },
{ "en": "Superposisi hanya berlaku untuk?", "id": "Rangkaian linier." },
{ "en": "Transfer daya maksimum mensyaratkan?", "id": "Pencocokan konjugat." },
{ "en": "Daya aktif adalah daya?", "id": "Nyata yang melakukan kerja." },
{ "en": "Daya reaktif adalah daya?", "id": "Imajiner untuk medan." },
{ "en": "Daya kompleks adalah jumlah vektor dari?", "id": "Daya aktif dan reaktif." },
{ "en": "Faktor daya adalah rasio?", "id": "Daya aktif terhadap daya semu." },
{ "en": "Koreksi PF menggunakan kapasitor?", "id": "Untuk menyuplai daya reaktif." },
{ "en": "Resonansi seri terjadi pada impedansi?", "id": "Minimum." },
{ "en": "Resonansi paralel terjadi pada impedansi?", "id": "Maksimum." },
{ "en": "Faktor Q tinggi berarti redaman?", "id": "Rendah." },
{ "en": "Filter lolos bawah melewatkan?", "id": "Frekuensi rendah." },
{ "en": "Filter lolos atas melewatkan?", "id": "Frekuensi tinggi." },
{ "en": "Filter lolos pita melewatkan?", "id": "Rentang frekuensi tertentu." },
{ "en": "Filter tolak pita menolak?", "id": "Rentang frekuensi tertentu." },
{ "en": "Plot Bode menyajikan respons frekuensi dalam?", "id": "Skala logaritmik." },
{ "en": "Induktansi timbal balik (M) menyebabkan?", "id": "Kopling antar kumparan." },
{ "en": "Transformator step-up menaikkan?", "id": "Tegangan." },
{ "en": "Transformator step-down menurunkan?", "id": "Tegangan." },
{ "en": "Sistem Y memiliki titik?", "id": "Netral." },
{ "en": "Sistem Δ tidak memiliki titik?", "id": "Netral." },
{ "en": "Pada sistem Y, VL ... dari Vp?", "id": "Lebih besar (akar 3 kali)." },
{ "en": "Pada sistem Δ, IL ... dari Ip?", "id": "Lebih besar (akar 3 kali)." },
{ "en": "Deret Fourier mengurai sinyal menjadi?", "id": "Sinusoida." },
{ "en": "Parameter Z, Y, h, ABCD digunakan untuk?", "id": "Menganalisis jaringan dua-port." },
{ "en": "Respons transien adalah respons?", "id": "Sementara." },
{ "en": "Respons keadaan tunak adalah respons?", "id": "Jangka panjang." },
{ "en": "Analisis fasor adalah untuk?", "id": "Keadaan tunak sinusoidal." },
{ "en": "Impedansi Z = R + jX, X adalah?", "id": "Reaktansi." },
{ "en": "Admitansi Y = G + jB, B adalah?", "id": "Suseptansi." },
{ "en": "Jika sudut impedansi 45°, maka?", "id": "R = X." },
{ "en": "Jika PF 0.707, maka?", "id": "|P| = |Q|." },
{ "en": "Puncak resonansi yang tajam berarti BW?", "id": "Sempit." },
{ "en": "Daya tiga fasa lebih efisien karena?", "id": "Membutuhkan lebih sedikit tembaga." },
{ "en": "Transformasi Laplace menangani kondisi?", "id": "Awal." },
{ "en": "Fungsi transfer H(s) tidak bergantung pada?", "id": "Input." },
{ "en": "Sinyal kausal berarti?", "id": "Nol untuk t < 0." },
{ "en": "Rangkaian pasif tidak bisa menghasilkan?", "id": "Gain daya." },
{ "en": "Rangkaian aktif mengandung?", "id": "Sumber energi (misal: op-amp)." },
{ "en": "Apakah frekuensi bisa negatif?", "id": "Secara matematis ya, secara fisik tidak." },
{ "en": "Gelombang sinus dan cosinus berbeda fasa?", "id": "90 derajat." },
{ "en": "Kapasitor memblokir?", "id": "Arus DC." },
{ "en": "Induktor memblokir?", "id": "Perubahan arus yang cepat." },
{ "en": "Watt adalah satuan untuk daya?", "id": "Aktif." },
{ "en": "VAR adalah satuan untuk daya?", "id": "Reaktif." },
{ "en": "VA adalah satuan untuk daya?", "id": "Semu." },
{ "en": "Jika beban seimbang, arus netral?", "id": "Nol." },
{ "en": "Daya sesaat negatif berarti?", "id": "Daya kembali ke sumber." },
{ "en": "Resonansi adalah fenomena domain?", "id": "Frekuensi." },
{ "en": "Analisis mesh berdasarkan?", "id": "KVL." },
{ "en": "Analisis node berdasarkan?", "id": "KCL." },
{ "en": "Jembatan Wheatstone AC setimbang jika?", "id": "Perkalian impedansi silang sama." },
{ "en": "Kopling sempurna berarti k=?", "id": "Satu." },
{ "en": "Tidak ada kopling berarti k=?", "id": "Nol." },
{ "en": "Simetri setengah gelombang berarti?", "id": "f(t) = -f(t + T/2)." },
{ "en": "Parameter h12 adalah?", "id": "Gain tegangan balik." },
{ "en": "Parameter y21 adalah?", "id": "Admitansi transfer maju." },
{ "en": "Filter Butterworth bersifat?", "id": "Monotonik di passband." },
{ "en": "Filter Chebyshev memiliki?", "id": "Riak di passband." },
{ "en": "Rangkaian linier mematuhi prinsip?", "id": "Superposisi." },
{ "en": "Analisis AC adalah tulang punggung dari?", "id": "Teknik elektro." }




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
