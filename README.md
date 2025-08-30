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


  { "en": "Komponen elektronika penyearah arus?", "id": "Dioda." },
  { "en": "Alat penguat sinyal listrik?", "id": "Transistor atau Op-Amp." },
  { "en": "Satuan hambatan listrik?", "id": "Ohm (Ω)." },
  { "en": "Satuan tegangan listrik?", "id": "Volt (V)." },
  { "en": "Satuan arus listrik?", "id": "Ampere (A)." },
  { "en": "Bunyi hukum Ohm?", "id": "V = I x R." },
  { "en": "Bunyi hukum Kirchhoff tentang arus?", "id": "Arus masuk sama dengan arus keluar." },
  { "en": "Kepanjangan dari AC (Alternating Current)?", "id": "Arus Bolak-balik." },
  { "en": "Kepanjangan dari DC (Direct Current)?", "id": "Arus Searah." },
  { "en": "Sumber listrik DC (Direct Current)?", "id": "Baterai, aki, adaptor." },
  { "en": "Pembangkit listrik tenaga air disingkat?", "id": "PLTA." },
  { "en": "Fungsi utama transformator (trafo)?", "id": "Menaikkan atau menurunkan tegangan." },
  { "en": "Alat pengubah energi listrik ke gerak?", "id": "Motor listrik." },
  { "en": "Alat pengubah energi gerak ke listrik?", "id": "Generator." },
  { "en": "Gerbang logika dasar?", "id": "AND, OR, NOT." },
  { "en": "Gerbang AND bernilai 1 jika?", "id": "Semua input bernilai 1." },
  { "en": "Gerbang OR bernilai 1 jika?", "id": "Salah satu input bernilai 1." },
  { "en": "Fungsi gerbang NOT?", "id": "Membalik nilai input (inverter)." },
  { "en": "Proses menumpangkan sinyal informasi ke sinyal pembawa?", "id": "Modulasi." },
  { "en": "Kepanjangan dari AM (Amplitude Modulation)?", "id": "Modulasi Amplitudo." },
  { "en": "Kepanjangan dari FM (Frequency Modulation)?", "id": "Modulasi Frekuensi." },
  { "en": "Keunggulan modulasi FM (Frequency Modulation) dibanding AM?", "id": "Lebih tahan terhadap noise." },
  { "en": "Media perambatan gelombang radio?", "id": "Udara atau ruang hampa." },
  { "en": "Alat untuk memancarkan gelombang radio?", "id": "Antena." },
  { "en": "Satuan frekuensi?", "id": "Hertz (Hz)." },
  { "en": "Model referensi jaringan komputer?", "id": "Model OSI atau TCP/IP." },
  { "en": "Kepanjangan dari LAN (Local Area Network)?", "id": "Jaringan Area Lokal." },
  { "en": "Kepanjangan dari WAN (Wide Area Network)?", "id": "Jaringan Area Luas." },
  { "en": "Alamat unik sebuah komputer di jaringan?", "id": "Alamat IP (Internet Protocol)." },
  { "en": "Perangkat penghubung antar jaringan berbeda?", "id": "Router." },
  { "en": "Sistem kontrol loop terbuka artinya?", "id": "Output tidak mempengaruhi input." },
  { "en": "Sistem kontrol loop tertutup artinya?", "id": "Output memiliki umpan balik (feedback)." },
  { "en": "Contoh sistem loop terbuka?", "id": "Kipas angin, lampu lalu lintas." },
  { "en": "Contoh sistem loop tertutup?", "id": "AC (Air Conditioner), setrika otomatis." },
  { "en": "Komponen pendeteksi pada sistem kontrol?", "id": "Sensor." },
  { "en": "Komponen penggerak pada sistem kontrol?", "id": "Aktuator." },
  { "en": "Dioda yang memancarkan cahaya?", "id": "LED (Light Emitting Diode)." },
  { "en": "Alat untuk mengubah AC ke DC?", "id": "Penyearah (rectifier)." },
  { "en": "Alat untuk mengubah DC ke AC?", "id": "Inverter." },
  { "en": "Tiga terminal pada transistor BJT?", "id": "Basis, Kolektor, Emitor." },
  { "en": "Fungsi transistor sebagai saklar?", "id": "Memutus atau menyambung arus." },
  { "en": "Bilangan dasar sistem digital?", "id": "Biner (0 dan 1)." },
  { "en": "Satuan daya listrik?", "id": "Watt (W)." },
  { "en": "Frekuensi listrik PLN di Indonesia?", "id": "50 Hertz (Hz)." },
  { "en": "Tegangan listrik satu fasa di Indonesia?", "id": "220 Volt." },
  { "en": "Tegangan listrik tiga fasa di Indonesia?", "id": "380 Volt." },
  { "en": "Bagian berputar pada motor/generator?", "id": "Rotor." },
  { "en": "Bagian diam pada motor/generator?", "id": "Stator." },
  { "en": "Pengaman listrik dari arus lebih?", "id": "Sekring atau MCB." },
  { "en": "Jaringan listrik tegangan tinggi disebut?", "id": "Jaringan transmisi." },
  { "en": "Jaringan listrik ke pelanggan disebut?", "id": "Jaringan distribusi." },
  { "en": "Apa itu sinyal analog?", "id": "Sinyal kontinu." },
  { "en": "Apa itu sinyal digital?", "id": "Sinyal diskrit (0 dan 1)." },
  { "en": "Contoh media transmisi terpandu (guided)?", "id": "Kabel tembaga, serat optik." },
  { "en": "Kelebihan utama serat optik?", "id": "Kecepatan tinggi, tahan interferensi." },
  { "en": "Kepanjangan dari GPS (Global Positioning System)?", "id": "Sistem Pemosisi Global." },
  { "en": "Komunikasi tanpa kabel disebut?", "id": "Nirkabel (wireless)." },
  { "en": "Perangkat keras utama komputer?", "id": "CPU, RAM, storage." },
  { "en": "Kepanjangan dari CPU (Central Processing Unit)?", "id": "Unit Pemrosesan Sentral." },
  { "en": "Kepanjangan dari RAM (Random Access Memory)?", "id": "Memori Akses Acak." },
  { "en": "Perangkat lunak utama komputer?", "id": "Sistem Operasi (Operating System)." },
  { "en": "Topologi jaringan paling sederhana?", "id": "Topologi Bus atau Bintang." },
  { "en": "Sistem kontroler logika terprogram?", "id": "PLC (Programmable Logic Controller)." },
  { "en": "Aplikasi PLC (Programmable Logic Controller)?", "id": "Otomasi industri." },
  { "en": "Contoh sensor suhu?", "id": "Termokopel, termistor." },
  { "en": "Contoh aktuator?", "id": "Motor, solenoid, pemanas." },
  { "en": "Komponen pasif elektronika?", "id": "Resistor, Kapasitor, Induktor." },
  { "en": "Fungsi kapasitor?", "id": "Menyimpan muatan listrik." },
  { "en": "Satuan kapasitansi?", "id": "Farad (F)." },
  { "en": "Fungsi induktor?", "id": "Menyimpan energi medan magnet." },
  { "en": "Satuan induktansi?", "id": "Henry (H)." },
  { "en": "Rangkaian seri artinya?", "id": "Komponen terhubung berderet." },
  { "en": "Rangkaian paralel artinya?", "id": "Komponen terhubung sejajar." },
  { "en": "Arus pada rangkaian seri?", "id": "Nilainya sama di semua titik." },
  { "en": "Tegangan pada rangkaian paralel?", "id": "Nilainya sama di semua cabang." },
  { "en": "Pembangkit listrik tenaga uap disingkat?", "id": "PLTU." },
  { "en": "Bahan bakar utama PLTU?", "id": "Batu bara." },
  { "en": "Pembangkit listrik tenaga surya disingkat?", "id": "PLTS." },
  { "en": "Komponen utama PLTS?", "id": "Panel surya." },
  { "en": "Sistem proteksi dari petir?", "id": "Penangkal petir (arrester)." },
  { "en": "Proses mengubah modulasi kembali ke sinyal asli?", "id": "Demodulasi." },
  { "en": "Contoh komunikasi simpleks?", "id": "Siaran radio atau televisi." },
  { "en": "Contoh komunikasi half-duplex?", "id": "Walkie-talkie." },
  { "en": "Contoh komunikasi full-duplex?", "id": "Telepon." },
  { "en": "Perangkat untuk terhubung ke internet?", "id": "Modem." },
  { "en": "Protokol standar untuk web?", "id": "HTTP atau HTTPS." },
  { "en": "Protokol untuk mengirim email?", "id": "SMTP (Simple Mail Transfer Protocol)." },
  { "en": "Sistem kontrol yang meniru manusia?", "id": "Kecerdasan buatan (AI)." },
  { "en": "Komponen elektronika dengan 3 terminal?", "id": "Transistor." },
  { "en": "Rangkaian terpadu disebut juga?", "id": "IC (Integrated Circuit)." },
  { "en": "Hambatan total pada rangkaian AC?", "id": "Impedansi." },
  { "en": "Daya yang benar-benar terpakai?", "id": "Daya nyata (Watt)." },
  { "en": "Faktor daya yang baik nilainya?", "id": "Mendekati 1." },
  { "en": "Gelombang pembawa sinyal disebut?", "id": "Sinyal carrier." },
  { "en": "Sistem bilangan yang digunakan komputer?", "id": "Sistem biner." },
  { "en": "Apa itu feedback dalam sistem kontrol?", "id": "Sinyal keluaran yang dikembalikan ke masukan." },
  { "en": "Komponen yang menentang perubahan arus?", "id": "Induktor." },
  { "en": "Komponen yang menentang perubahan tegangan?", "id": "Kapasitor." },
  { "en": "Komponen yang menghambat aliran arus?", "id": "Resistor." },
  { "en": "Apa itu impedansi?", "id": "Hambatan total pada rangkaian AC." },
  { "en": "Satuan impedansi?", "id": "Ohm (Ω)." },
  { "en": "Dioda yang berfungsi sebagai kapasitor variabel?", "id": "Dioda Varaktor." },
  { "en": "Tiga terminal pada FET (Field Effect Transistor)?", "id": "Gate, Drain, Source." },
  { "en": "Rangkaian elektronika pembangkit gelombang?", "id": "Osilator." },
  { "en": "Penguat operasional idealnya memiliki impedansi masukan?", "id": "Sangat besar (tak terhingga)." },
  { "en": "Penguat operasional idealnya memiliki impedansi keluaran?", "id": "Sangat kecil (nol)." },
  { "en": "Gerbang logika universal?", "id": "NAND dan NOR." },
  { "en": "Gerbang XOR bernilai 1 jika?", "id": "Inputnya berbeda (ganjil)." },
  { "en": "Rangkaian digital penyimpan satu bit data?", "id": "Flip-Flop." },
  { "en": "Apa itu multiplexer (MUX)?", "id": "Memilih satu dari banyak input." },
  { "en": "Apa itu demultiplexer (DEMUX)?", "id": "Mengirim input ke banyak output." },
  { "en": "Proses mengubah sinyal analog ke digital?", "id": "Sampling, kuantisasi, dan pengkodean." },
  { "en": "Alat pengubah sinyal analog ke digital?", "id": "ADC (Analog-to-Digital Converter)." },
  { "en": "Alat pengubah sinyal digital ke analog?", "id": "DAC (Digital-to-Analog Converter)." },
  { "en": "Apa itu bandwidth?", "id": "Rentang frekuensi yang digunakan." },
  { "en": "Gangguan yang tidak diinginkan pada sinyal?", "id": "Noise atau derau." },
  { "en": "Rasio antara sinyal dan noise?", "id": "SNR (Signal-to-Noise Ratio)." },
  { "en": "Modulasi digital paling sederhana?", "id": "ASK (Amplitude Shift Keying)." },
  { "en": "Modulasi dengan mengubah frekuensi sinyal pembawa?", "id": "FSK (Frequency Shift Keying)." },
  { "en": "Modulasi dengan mengubah fasa sinyal pembawa?", "id": "PSK (Phase Shift Keying)." },
  { "en": "Media transmisi berupa kabel sepaksi?", "id": "Kabel koaksial." },
  { "en": "Contoh penggunaan kabel koaksial?", "id": "Antena TV, jaringan CCTV." },
  { "en": "Pelemahan sinyal di sepanjang saluran?", "id": "Atenuasi atau redaman." },
  { "en": "Sinyal pembawa informasi disebut?", "id": "Sinyal baseband." },
  { "en": "Hukum Kirchhoff tentang tegangan?", "id": "Jumlah tegangan dalam loop tertutup nol." },
  { "en": "Daya yang hilang dalam bentuk panas?", "id": "Rugi-rugi daya." },
  { "en": "Perbandingan daya nyata dan daya semu?", "id": "Faktor daya (Power Factor)." },
  { "en": "Alat untuk memperbaiki faktor daya?", "id": "Kapasitor bank." },
  { "en": "Daya reaktif satuannya?", "id": "VAR (Volt-Ampere Reactive)." },
  { "en": "Daya semu satuannya?", "id": "VA (Volt-Ampere)." },
  { "en": "Teorema untuk menyederhanakan rangkaian linier?", "id": "Teorema Thevenin dan Norton." },
  { "en": "Pembangkit listrik tenaga gas disingkat?", "id": "PLTG." },
  { "en": "Pembangkit listrik tenaga diesel disingkat?", "id": "PLTD." },
  { "en": "Fungsi utama gardu induk?", "id": "Menaikkan dan menurunkan tegangan." },
  { "en": "Bagian saluran transmisi untuk melindungi dari petir?", "id": "Kawat tanah (ground wire)." },
  { "en": "Bahan isolator pada jaringan listrik?", "id": "Keramik, kaca, atau polimer." },
  { "en": "Sistem yang menghubungkan beberapa pembangkit?", "id": "Sistem interkoneksi." },
  { "en": "Apa itu hubung singkat?", "id": "Hubungan arus listrik tanpa hambatan." },
  { "en": "Apa itu beban lebih?", "id": "Arus yang melebihi kapasitas normal." },
  { "en": "Kepanjangan dari SUTET?", "id": "Saluran Udara Tegangan Ekstra Tinggi." },
  { "en": "Alat pengaman dari sengatan listrik?", "id": "ELCB (Earth Leakage Circuit Breaker)." },
  { "en": "Perangkat jaringan untuk menghubungkan banyak komputer dalam LAN?", "id": "Switch atau Hub." },
  { "en": "Perbedaan Switch dan Hub?", "id": "Switch lebih cerdas dari Hub." },
  { "en": "Kabel yang umum digunakan untuk LAN?", "id": "Kabel UTP (Unshielded Twisted Pair)." },
  { "en": "Kepanjangan dari IP (Internet Protocol)?", "id": "Protokol Internet." },
  { "en": "Sistem penamaan domain di internet?", "id": "DNS (Domain Name System)." },
  { "en": "Protokol untuk transfer file?", "id": "FTP (File Transfer Protocol)." },
  { "en": "Lapisan fisik pada model OSI?", "id": "Layer 1." },
  { "en": "Lapisan aplikasi pada model OSI?", "id": "Layer 7." },
  { "en": "Pengamanan jaringan dari akses luar?", "id": "Firewall." },
  { "en": "Jaringan pribadi melalui jaringan publik?", "id": "VPN (Virtual Private Network)." },
  { "en": "Apa itu umpan balik negatif (negative feedback)?", "id": "Memperkecil kesalahan (error)." },
  { "en": "Apa itu umpan balik positif (positive feedback)?", "id": "Memperbesar sinyal (osilator)." },
  { "en": "Variabel yang diukur dalam sistem kontrol?", "id": "Process variable." },
  { "en": "Nilai yang diinginkan dalam sistem kontrol?", "id": "Setpoint." },
  { "en": "Selisih antara setpoint dan process variable?", "id": "Error." },
  { "en": "Contoh sensor cahaya?", "id": "LDR (Light Dependent Resistor)." },
  { "en": "Apa itu mikrokontroler?", "id": "Komputer kecil dalam satu chip." },
  { "en": "Contoh mikrokontroler populer?", "id": "Arduino, Raspberry Pi." },
  { "en": "Bagaimana cara menghitung resistansi total rangkaian seri?", "id": "Dijumlahkan (R1 + R2 + ...)." },
  { "en": "Bagaimana cara menghitung kapasitansi total rangkaian paralel?", "id": "Dijumlahkan (C1 + C2 + ...)." },
  { "en": "Apa itu penyearah setengah gelombang?", "id": "Menggunakan satu dioda." },
  { "en": "Apa itu penyearah gelombang penuh?", "id": "Menggunakan dua atau empat dioda." },
  { "en": "Komponen untuk menstabilkan tegangan DC?", "id": "Dioda Zener atau IC Regulator." },
  { "en": "Apa itu osiloskop?", "id": "Alat untuk melihat bentuk gelombang." },
  { "en": "Apa itu multimeter?", "id": "Mengukur tegangan, arus, dan hambatan." },
  { "en": "Sistem tiga fasa umumnya memiliki berapa kabel?", "id": "Tiga atau empat kabel." },
  { "en": "Kabel keempat pada sistem tiga fasa?", "id": "Kabel netral." },
  { "en": "Beda fasa antara tegangan pada sistem tiga fasa?", "id": "120 derajat." },
  { "en": "Apa itu sistem pentanahan (grounding)?", "id": "Koneksi ke tanah untuk keselamatan." },
  { "en": "Apa itu antena Yagi?", "id": "Antena directional untuk TV." },
  { "en": "Apa itu antena omnidirectional?", "id": "Memancarkan ke segala arah." },
  { "en": "Gelombang radio merambat dengan kecepatan?", "id": "Kecepatan cahaya." },
  { "en": "Sistem bilangan berbasis 16?", "id": "Heksadesimal." },
  { "en": "Sistem bilangan berbasis 8?", "id": "Oktal." },
  { "en": "1 byte terdiri dari berapa bit?", "id": "8 bit." },
  { "en": "Perangkat input standar komputer?", "id": "Keyboard dan mouse." },
  { "en": "Perangkat output standar komputer?", "id": "Monitor dan printer." },
  { "en": "Apa itu error dalam sistem kontrol?", "id": "Perbedaan antara nilai yang diinginkan dan aktual." },
  { "en": "Kontroler yang umum di industri?", "id": "Kontroler PID." },
  { "en": "Kepanjangan dari PID?", "id": "Proportional, Integral, Derivative." },
  { "en": "Apa itu 'transient response'?", "id": "Respon sistem terhadap perubahan mendadak." },
  { "en": "Apa itu 'steady state'?", "id": "Kondisi sistem setelah stabil." },
  { "en": "Apa itu op-amp (operational amplifier)?", "id": "Penguat diferensial tegangan tinggi." },
  { "en": "Apa itu semikonduktor?", "id": "Bahan antara konduktor dan isolator." },
  { "en": "Contoh bahan semikonduktor?", "id": "Silikon (Si) dan Germanium (Ge)." },
  { "en": "Proses penambahan ketidakmurnian pada semikonduktor?", "id": "Doping." },
  { "en": "Semikonduktor tipe-N kelebihan?", "id": "Elektron." },
  { "en": "Semikonduktor tipe-P kelebihan?", "id": "Hole." },
  { "en": "Gabungan semikonduktor P dan N?", "id": "Dioda." },
  { "en": "Resistor yang nilainya dapat diubah-ubah?", "id": "Potensiometer atau variabel resistor." },
  { "en": "Dua jenis dioda berdasarkan bahannya?", "id": "Silikon dan Germanium." },
  { "en": "Tegangan maju (forward bias) dioda silikon?", "id": "Sekitar 0.7 Volt." },
  { "en": "Apa fungsi dioda Zener?", "id": "Sebagai penstabil (regulator) tegangan." },
  { "en": "Apa fungsi Optocoupler?", "id": "Mengisolasi dua rangkaian secara optik." },
  { "en": "Konfigurasi Op-Amp (Operational Amplifier) pembalik fasa?", "id": "Inverting amplifier." },
  { "en": "Konfigurasi Op-Amp (Operational Amplifier) penguat tak pembalik?", "id": "Non-inverting amplifier." },
  { "en": "Apa itu CMRR (Common-Mode Rejection Ratio) pada Op-Amp?", "id": "Kemampuan menolak sinyal yang sama." },
  { "en": "Rangkaian digital yang melakukan operasi aritmatika?", "id": "ALU (Arithmetic Logic Unit)." },
  { "en": "Apa itu register dalam sistem digital?", "id": "Sekelompok flip-flop penyimpan data." },
  { "en": "Apa itu counter (pencacah)?", "id": "Rangkaian yang menghitung pulsa." },
  { "en": "Spektrum gelombang elektromagnetik diurutkan berdasarkan?", "id": "Frekuensi atau panjang gelombang." },
  { "en": "Gelombang radio dengan frekuensi sangat tinggi?", "id": "Microwave (gelombang mikro)." },
  { "en": "Cahaya yang tidak terlihat dengan frekuensi di bawah merah?", "id": "Inframerah (infrared)." },
  { "en": "Cahaya yang tidak terlihat dengan frekuensi di atas ungu?", "id": "Ultraviolet (UV)." },
  { "en": "Rentang frekuensi radio HF (High Frequency)?", "id": "3 hingga 30 MHz." },
  { "en": "Rentang frekuensi radio VHF (Very High Frequency)?", "id": "30 hingga 300 MHz." },
  { "en": "Rentang frekuensi radio UHF (Ultra High Frequency)?", "id": "300 MHz hingga 3 GHz." },
  { "en": "Kepanjangan dari RADAR?", "id": "Radio Detection and Ranging." },
  { "en": "Apa itu propagasi gelombang tanah (ground wave)?", "id": "Gelombang radio mengikuti permukaan bumi." },
  { "en": "Apa itu propagasi gelombang langit (sky wave)?", "id": "Gelombang radio dipantulkan ionosfer." },
  { "en": "Apa itu 'line-of-sight' (LOS) propagation?", "id": "Perambatan lurus tanpa halangan." },
  { "en": "Proses menggabungkan beberapa sinyal dalam satu saluran?", "id": "Multiplexing." },
  { "en": "Jenis multiplexing berdasarkan frekuensi?", "id": "FDM (Frequency Division Multiplexing)." },
  { "en": "Jenis multiplexing berdasarkan waktu?", "id": "TDM (Time Division Multiplexing)." },
  { "en": "Bagaimana cara menghitung kapasitansi total rangkaian seri?", "id": "Dengan rumus 1/Ctotal." },
  { "en": "Bagaimana cara menghitung induktansi total rangkaian seri?", "id": "Dijumlahkan (L1 + L2 + ...)." },
  { "en": "Apa itu daya nyata (active power)?", "id": "Daya yang melakukan kerja nyata." },
  { "en": "Apa itu daya reaktif (reactive power)?", "id": "Daya untuk medan magnet/listrik." },
  { "en": "Apa itu daya semu (apparent power)?", "id": "Gabungan vektor daya nyata dan reaktif." },
  { "en": "Hubungan ketiga daya digambarkan dalam?", "id": "Segitiga daya (power triangle)." },
  { "en": "Satuan energi listrik yang dibayar ke PLN?", "id": "kWh (kiloWatt-hour)." },
  { "en": "Apa itu 'root mean square' (RMS)?", "id": "Nilai efektif dari tegangan/arus AC." },
  { "en": "Nilai tegangan yang diukur oleh multimeter?", "id": "Nilai RMS (Root Mean Square)." },
  { "en": "Sistem pembangkitan listrik disebut?", "id": "Generation." },
  { "en": "Sistem penyaluran listrik jarak jauh?", "id": "Transmisi." },
  { "en": "Sistem pembagian listrik ke pelanggan?", "id": "Distribusi." },
  { "en": "Bahan utama kabel konduktor transmisi?", "id": "ACSR (Aluminum Conductor Steel Reinforced)." },
  { "en": "Fungsi inti baja pada kabel ACSR?", "id": "Memberikan kekuatan mekanik." },
  { "en": "Fenomena loncatan api pada isolator?", "id": "Flashover." },
  { "en": "Apa itu busbar?", "id": "Konduktor pusat pengumpul/distribusi daya." },
  { "en": "Bahan utama busbar?", "id": "Tembaga atau aluminium." },
  { "en": "Peralatan untuk memutus dan menghubungkan sirkuit?", "id": "PMT (Pemutus Tenaga) atau saklar." },
  { "en": "Peralatan untuk memisahkan sirkuit tanpa beban?", "id": "PMS (Pemisah) atau Disconnecting Switch." },
  { "en": "Alamat fisik sebuah kartu jaringan?", "id": "Alamat MAC (Media Access Control)." },
  { "en": "Perangkat untuk mengubah sinyal digital ke analog untuk telepon?", "id": "Modem." },
  { "en": "Protokol yang bersifat 'connection-oriented'?", "id": "TCP (Transmission Control Protocol)." },
  { "en": "Protokol yang bersifat 'connectionless'?", "id": "UDP (User Datagram Protocol)." },
  { "en": "Mana yang lebih andal, TCP atau UDP?", "id": "TCP (Transmission Control Protocol)." },
  { "en": "Mana yang lebih cepat, TCP atau UDP?", "id": "UDP (User Datagram Protocol)." },
  { "en": "Contoh aplikasi yang menggunakan UDP?", "id": "Streaming video, game online." },
  { "en": "Sistem operasi jaringan yang populer?", "id": "Windows Server, Linux." },
  { "en": "Perintah untuk memeriksa koneksi jaringan?", "id": "Ping." },
  { "en": "Respon sistem dari kondisi awal hingga stabil?", "id": "Respon transien." },
  { "en": "Respon sistem setelah mencapai kondisi stabil?", "id": "Respon steady-state." },
  { "en": "Waktu yang dibutuhkan sistem untuk mencapai target?", "id": "Rise time atau waktu naik." },
  { "en": "Kelebihan output dari nilai akhir saat respon transien?", "id": "Overshoot." },
  { "en": "Apa itu kestabilan sistem kontrol?", "id": "Kemampuan sistem kembali ke setimbang." },
  { "en": "Bagian dari PLC (Programmable Logic Controller) yang menerima sinyal?", "id": "Modul input." },
  { "en": "Bagian dari PLC (Programmable Logic Controller) yang mengirim sinyal?", "id": "Modul output." },
  { "en": "Bahasa pemrograman grafis untuk PLC?", "id": "Ladder Diagram." },
  { "en": "Elektronika yang menggunakan komponen tabung vakum?", "id": "Elektronika tabung." },
  { "en": "Elektronika yang menggunakan komponen solid-state?", "id": "Elektronika modern." },
  { "en": "Apa itu 'gain' pada penguat?", "id": "Perbandingan sinyal output dan input." },
  { "en": "Satuan 'gain'?", "id": "Tidak ada (rasio) atau desibel (dB)." },
  { "en": "Rangkaian yang menyaring frekuensi tertentu?", "id": "Filter." },
  { "en": "Filter yang meloloskan frekuensi rendah?", "id": "LPF (Low Pass Filter)." },
  { "en": "Filter yang meloloskan frekuensi tinggi?", "id": "HPF (High Pass Filter)." },
  { "en": "Filter yang meloloskan rentang frekuensi tertentu?", "id": "BPF (Band Pass Filter)." },
  { "en": "Aljabar untuk menyederhanakan logika digital?", "id": "Aljabar Boolean." },
  { "en": "Apa itu 'clock' dalam sistem digital?", "id": "Sinyal denyut untuk sinkronisasi." },
  { "en": "Ponsel menggunakan sistem komunikasi apa?", "id": "Full-duplex." },
  { "en": "Teknologi di balik komunikasi seluler?", "id": "Jaringan seluler (misal 4G, 5G)." },
  { "en": "Apa itu 'noise figure'?", "id": "Ukuran noise yang ditambahkan perangkat." },
  { "en": "Proses konversi frekuensi radio ke frekuensi menengah?", "id": "Superheterodyne." },
  { "en": "Apa itu pembumian (grounding)?", "id": "Menghubungkan sistem ke tanah." },
  { "en": "Tujuan utama pembumian?", "id": "Keselamatan dan referensi tegangan." },
  { "en": "Apa itu 'drop' tegangan?", "id": "Penurunan tegangan di sepanjang kabel." },
  { "en": "Penyebab 'drop' tegangan?", "id": "Resistansi kabel." },
  { "en": "Satuan energi yang umum?", "id": "Joule (J)." },
  { "en": "Hubungan antara daya dan energi?", "id": "Energi adalah daya dikali waktu." },
  { "en": "Apa itu transformator step-up?", "id": "Menaikkan tegangan, menurunkan arus." },
  { "en": "Apa itu transformator step-down?", "id": "Menurunkan tegangan, menaikkan arus." },
  { "en": "Apa itu efisiensi?", "id": "Rasio daya keluaran terhadap masukan." },
  { "en": "Nilai efisiensi ideal?", "id": "100 persen." },
  { "en": "Topologi jaringan dimana semua terhubung ke pusat?", "id": "Topologi bintang (star)." },
  { "en": "Topologi jaringan berbentuk cincin?", "id": "Topologi cincin (ring)." },
  { "en": "Apa itu 'server'?", "id": "Komputer penyedia layanan di jaringan." },
  { "en": "Apa itu 'client'?", "id": "Komputer pengguna layanan di jaringan." },
  { "en": "Apa itu 'transfer function'?", "id": "Model matematis dari sistem kontrol." },
  { "en": "Apa itu 'actuator' pneumatik?", "id": "Aktuator yang digerakkan udara bertekanan." },
  { "en": "Apa itu 'actuator' hidrolik?", "id": "Aktuator yang digerakkan cairan bertekanan." },
  { "en": "Contoh sensor jarak?", "id": "Ultrasonik, inframerah." },
  { "en": "Apa itu 'open loop control'?", "id": "Sistem kontrol tanpa umpan balik." },
  { "en": "Apa itu 'closed loop control'?", "id": "Sistem kontrol dengan umpan balik." },
  { "en": "Apa itu IC (Integrated Circuit)?", "id": "Banyak komponen dalam satu chip." },
  { "en": "Fungsi utama IC (Integrated Circuit) 555?", "id": "Timer, osilator, atau multivibrator." },
  { "en": "Kelas penguat (amplifier) paling efisien?", "id": "Kelas D." },
  { "en": "Kelas penguat (amplifier) dengan fidelitas terbaik?", "id": "Kelas A." },
  { "en": "Gabungan gerbang AND dan NOT?", "id": "Gerbang NAND." },
  { "en": "Gabungan gerbang OR dan NOT?", "id": "Gerbang NOR." },
  { "en": "Mengapa NAND dan NOR disebut gerbang universal?", "id": "Bisa membentuk semua gerbang lain." },
  { "en": "Alat untuk menstabilkan tegangan DC?", "id": "Regulator tegangan." },
  { "en": "Contoh IC (Integrated Circuit) regulator tegangan?", "id": "IC 7805 (untuk 5V)." },
  { "en": "Apa itu 'clipping' pada sinyal?", "id": "Puncak gelombang terpotong." },
  { "en": "Penyebab 'clipping' pada penguat?", "id": "Penguatan terlalu besar (overdrive)." },
  { "en": "Komponen yang mengubah getaran suara menjadi sinyal listrik?", "id": "Mikrofon." },
  { "en": "Komponen yang mengubah sinyal listrik menjadi suara?", "id": "Loudspeaker atau speaker." },
  { "en": "Apa itu 'crossover' pada speaker?", "id": "Rangkaian pemisah frekuensi suara." },
  { "en": "Perbandingan kekuatan sinyal terhadap noise?", "id": "SNR (Signal-to-Noise Ratio)." },
  { "en": "Nilai SNR (Signal-to-Noise Ratio) yang baik?", "id": "Nilai yang tinggi." },
  { "en": "Antena yang memancar ke segala arah?", "id": "Antena omnidirectional." },
  { "en": "Antena yang memancar ke satu arah tertentu?", "id": "Antena directional." },
  { "en": "Contoh antena directional?", "id": "Antena Yagi, antena parabola." },
  { "en": "Apa itu stasiun pangkalan dalam komunikasi seluler?", "id": "BTS (Base Transceiver Station)." },
  { "en": "Wilayah cakupan sebuah BTS (Base Transceiver Station)?", "id": "Sel (cell)." },
  { "en": "Proses perpindahan koneksi antar sel?", "id": "Handover atau handoff." },
  { "en": "Teknologi di balik TV digital?", "id": "Modulasi digital dan kompresi data." },
  { "en": "Apa itu 'ghosting' pada TV analog?", "id": "Bayangan gambar akibat pantulan sinyal." },
  { "en": "Hubungan antara frekuensi dan panjang gelombang?", "id": "Berbanding terbalik." },
  { "en": "Apa itu nilai puncak (peak value) tegangan AC?", "id": "Nilai maksimum gelombang sinus." },
  { "en": "Rumus hubungan nilai puncak dan RMS (Root Mean Square)?", "id": "Vpuncak = Vrms x √2." },
  { "en": "Dua jenis hubungan pada sistem tiga fasa?", "id": "Hubungan Bintang (Y) dan Segitiga (Δ)." },
  { "en": "Pada hubungan Bintang (Y), apakah ada titik netral?", "id": "Ya, ada titik netral." },
  { "en": "Pada hubungan Segitiga (Δ), apakah ada titik netral?", "id": "Tidak ada titik netral." },
  { "en": "Alat ukur energi listrik di rumah?", "id": "Meteran kWh (kiloWatt-hour)." },
  { "en": "Perhitungan daya pada rangkaian DC?", "id": "P = V x I." },
  { "en": "Perhitungan daya pada rangkaian AC satu fasa?", "id": "P = V x I x cos(φ)." },
  { "en": "Simbol 'cos(φ)' dalam rumus daya AC?", "id": "Faktor daya (power factor)." },
  { "en": "Apa fungsi sekring (fuse)?", "id": "Memutus sirkuit dengan melebur." },
  { "en": "Apa kelebihan MCB (Miniature Circuit Breaker) dari sekring?", "id": "Bisa di-reset kembali." },
  { "en": "Alat deteksi gangguan yang memberi sinyal ke PMT?", "id": "Relay proteksi." },
  { "en": "Relay yang bekerja berdasarkan arus lebih?", "id": "Overcurrent Relay (OCR)." },
  { "en": "Relay yang bekerja berdasarkan tegangan lebih/kurang?", "id": "Voltage Relay." },
  { "en": "Pembangkit listrik panas bumi disingkat?", "id": "PLTP." },
  { "en": "Sumber energi PLTP?", "id": "Uap panas dari dalam bumi." },
  { "en": "Apa itu 'blackout'?", "id": "Pemadaman listrik skala luas." },
  { "en": "Sistem pendingin utama pada trafo besar?", "id": "Oli transformator." },
  { "en": "Fungsi ganda oli trafo?", "id": "Sebagai pendingin dan isolasi." },
  { "en": "Aturan atau standar dalam komunikasi jaringan?", "id": "Protokol." },
  { "en": "Lapisan pada model OSI yang menangani pengalamatan logis?", "id": "Lapisan Jaringan (Network Layer)." },
  { "en": "Lapisan pada model OSI yang menangani pengalamatan fisik?", "id": "Lapisan Data Link (Data Link Layer)." },
  { "en": "Lapisan pada model OSI yang mengatur sesi komunikasi?", "id": "Lapisan Sesi (Session Layer)." },
  { "en": "Lapisan pada model OSI yang menangani presentasi data?", "id": "Lapisan Presentasi (Presentation Layer)." },
  { "en": "Fungsi utama firewall?", "id": "Menyaring lalu lintas jaringan." },
  { "en": "Fungsi utama VPN (Virtual Private Network)?", "id": "Membuat koneksi aman lewat internet." },
  { "en": "Perangkat lunak jahat disingkat?", "id": "Malware." },
  { "en": "Serangan yang membanjiri server dengan lalu lintas data?", "id": "Serangan DDoS (Distributed Denial of Service)." },
  { "en": "Elemen utama dalam sistem kontrol?", "id": "Plant, sensor, controller, aktuator." },
  { "en": "Bagian dari sistem kontrol yang dikendalikan?", "id": "Plant atau proses." },
  { "en": "Bagian yang memproses error dan menghasilkan sinyal kontrol?", "id": "Controller (pengendali)." },
  { "en": "Apa itu 'disturbance' dalam sistem kontrol?", "id": "Sinyal gangguan yang tidak diinginkan." },
  { "en": "Contoh 'disturbance' pada AC (Air Conditioner)?", "id": "Membuka pintu atau jendela." },
  { "en": "Sistem kontrol yang bekerja secara on-off?", "id": "Kontrol bang-bang." },
  { "en": "Contoh kontrol bang-bang?", "id": "Termostat pada kulkas." },
  { "en": "Apa itu robotika?", "id": "Ilmu tentang desain dan operasi robot." },
  { "en": "Bahan dasar pembuatan chip IC?", "id": "Silikon (silicon)." },
  { "en": "Proses pencetakan rangkaian ke chip silikon?", "id": "Litografi." },
  { "en": "Papan sirkuit tempat komponen elektronika dipasang?", "id": "PCB (Printed Circuit Board)." },
  { "en": "Apa itu 'datasheet'?", "id": "Dokumen spesifikasi teknis komponen." },
  { "en": "Apa itu catu daya (power supply)?", "id": "Penyedia daya untuk rangkaian elektronik." },
  { "en": "Apa itu 'ripple' pada catu daya DC?", "id": "Sisa gelombang AC yang tidak rata." },
  { "en": "Komponen untuk mengurangi 'ripple'?", "id": "Kapasitor filter." },
  { "en": "Apa itu 'breadboard'?", "id": "Papan untuk merangkai prototipe sirkuit." },
  { "en": "Alat untuk menyolder komponen?", "id": "Solder dan timah." },
  { "en": "Apa itu 'heat sink'?", "id": "Pendingin untuk komponen elektronika." },
  { "en": "Komponen yang memerlukan 'heat sink'?", "id": "Transistor daya, IC regulator." },
  { "en": "Sistem komunikasi yang menggunakan cahaya lewat kabel?", "id": "Serat optik." },
  { "en": "Sumber cahaya pada sistem serat optik?", "id": "LED atau Laser." },
  { "en": "Penerima cahaya pada sistem serat optik?", "id": "Photodiode." },
  { "en": "Penyambungan permanen dua serat optik?", "id": "Fusion splicing." },
  { "en": "Apa itu 'grounding' atau pentanahan?", "id": "Koneksi ke bumi untuk keselamatan." },
  { "en": "Warna standar kabel ground?", "id": "Hijau-kuning." },
  { "en": "Warna standar kabel netral?", "id": "Biru." },
  { "en": "Warna standar kabel fasa?", "id": "Merah, kuning, hitam." },
  { "en": "Peralatan untuk mengukur resistansi tanah?", "id": "Earth Tester." },
  { "en": "Apa itu 'step-up transformer'?", "id": "Trafo penaik tegangan." },
  { "en": "Apa itu 'step-down transformer'?", "id": "Trafo penurun tegangan." },
  { "en": "Apa itu 'IP address' dinamis?", "id": "Alamat IP yang bisa berubah." },
  { "en": "Apa itu 'IP address' statis?", "id": "Alamat IP yang tetap." },
  { "en": "Perangkat yang memperluas jangkauan sinyal Wi-Fi?", "id": "Wi-Fi extender atau repeater." },
  { "en": "Apa itu 'Internet of Things' (IoT)?", "id": "Jaringan perangkat pintar yang terhubung." },
  { "en": "Apa itu 'actuator' solenoid?", "id": "Aktuator yang menghasilkan gerakan linier." },
  { "en": "Sistem kontrol yang bisa belajar?", "id": "Sistem kontrol adaptif atau cerdas." },
  { "en": "Apa itu 'transfer function'?", "id": "Representasi matematis input-output sistem." },
  { "en": "Apa itu 'time delay' dalam sistem kontrol?", "id": "Waktu tunda antara input dan output." },
  { "en": "Dampak 'time delay' pada sistem?", "id": "Bisa menyebabkan ketidakstabilan." },
  { "en": "Apa itu 'system identification'?", "id": "Proses membuat model matematis sistem." },
  { "en": "Apa itu semikonduktor intrinsik?", "id": "Semikonduktor murni tanpa doping." },
  { "en": "Apa itu semikonduktor ekstrinsik?", "id": "Semikonduktor yang sudah diberi doping." },
  { "en": "Daerah deplesi pada dioda adalah?", "id": "Daerah tanpa pembawa muatan." },
  { "en": "Apa itu 'thermal runaway' pada transistor?", "id": "Kerusakan akibat panas berlebih." },
  { "en": "Karakteristik Op-Amp (Operational Amplifier) ideal?", "id": "Gain tak hingga, impedansi input tak hingga." },
  { "en": "Apa itu 'slew rate' pada Op-Amp?", "id": "Kecepatan perubahan tegangan output maksimum." },
  { "en": "Memori yang isinya tidak bisa diubah?", "id": "ROM (Read-Only Memory)." },
  { "en": "Memori yang datanya hilang saat listrik padam?", "id": "RAM (Random Access Memory) jenis volatil." },
  { "en": "Jenis flip-flop yang paling dasar?", "id": "SR (Set-Reset) Flip-Flop." },
  { "en": "Apa itu 'race condition'?", "id": "Kondisi output tak menentu akibat input." },
  { "en": "Rangkaian untuk mengubah level tegangan logika?", "id": "Level shifter." },
  { "en": "Apa itu 'decoder'?", "id": "Mengubah kode biner ke output spesifik." },
  { "en": "Apa itu 'encoder'?", "id": "Mengubah input aktif ke kode biner." },
  { "en": "Apa itu 'attenuation' dalam telekomunikasi?", "id": "Pelemahan kekuatan sinyal." },
  { "en": "Apa itu 'amplification' dalam telekomunikasi?", "id": "Penguatan kekuatan sinyal." },
  { "en": "Alat untuk mengatasi 'attenuation'?", "id": "Amplifier atau repeater." },
  { "en": "Jenis kabel jaringan yang terdiri dari pasangan terpilin?", "id": "Kabel twisted pair." },
  { "en": "Tujuan memilin kabel pada 'twisted pair'?", "id": "Mengurangi interferensi elektromagnetik." },
  { "en": "Bahan utama pembuat serat optik?", "id": "Kaca silika murni." },
  { "en": "Cahaya merambat dalam serat optik karena prinsip?", "id": "Pemantulan internal total." },
  { "en": "Apa itu FDM (Frequency Division Multiplexing)?", "id": "Berbagi saluran berdasarkan frekuensi." },
  { "en": "Apa itu TDM (Time Division Multiplexing)?", "id": "Berbagi saluran berdasarkan waktu." },
  { "en": "Contoh aplikasi FDM (Frequency Division Multiplexing)?", "id": "Siaran radio FM." },
  { "en": "Apa itu 'crosstalk'?", "id": "Bocoran sinyal antar saluran." },
  { "en": "Apa itu 'echo' dalam komunikasi?", "id": "Sinyal yang terpantul kembali." },
  { "en": "Kondisi saat reaktansi induktif sama dengan kapasitif?", "id": "Resonansi." },
  { "en": "Pada saat resonansi seri, impedansi rangkaian bernilai?", "id": "Minimum (sama dengan resistansi)." },
  { "en": "Pada saat resonansi paralel, impedansi rangkaian bernilai?", "id": "Maksimum." },
  { "en": "Frekuensi saat terjadi resonansi disebut?", "id": "Frekuensi resonansi." },
  { "en": "Ukuran kualitas rangkaian resonansi?", "id": "Faktor Kualitas atau Q factor." },
  { "en": "Nilai tegangan rata-rata (average) gelombang sinus murni?", "id": "Nol." },
  { "en": "Nilai tegangan AC yang setara dengan DC?", "id": "Nilai RMS (Root Mean Square)." },
  { "en": "Apa itu 'form factor'?", "id": "Rasio nilai RMS dan nilai rata-rata." },
  { "en": "Apa itu 'crest factor'?", "id": "Rasio nilai puncak dan nilai RMS." },
  { "en": "Gabungan jaringan listrik regional atau nasional?", "id": "Grid atau jaringan listrik." },
  { "en": "Apa itu sistem distribusi radial?", "id": "Sistem dengan satu jalur suplai." },
  { "en": "Kelemahan sistem distribusi radial?", "id": "Tidak andal, jika putus semua padam." },
  { "en": "Apa itu sistem distribusi loop?", "id": "Sistem dengan suplai dari dua arah." },
  { "en": "Keuntungan sistem distribusi loop?", "id": "Lebih andal jika terjadi gangguan." },
  { "en": "Peralatan di gardu induk yang dibungkus logam?", "id": "Switchgear." },
  { "en": "Media isolasi pada switchgear modern?", "id": "Gas SF6 (Sulfur Hexafluoride)." },
  { "en": "Apa itu 'arus inrush' pada trafo?", "id": "Lonjakan arus sesaat saat dihidupkan." },
  { "en": "Apa itu 'rugi tembaga' pada trafo?", "id": "Rugi daya akibat resistansi belitan." },
  { "en": "Apa itu 'rugi inti' pada trafo?", "id": "Rugi daya akibat histeresis dan eddy." },
  { "en": "Jaringan komputer personal?", "id": "PAN (Personal Area Network)." },
  { "en": "Contoh PAN (Personal Area Network)?", "id": "Bluetooth, koneksi HP ke laptop." },
  { "en": "Jaringan komputer skala kota?", "id": "MAN (Metropolitan Area Network)." },
  { "en": "Server yang menerjemahkan nama domain ke alamat IP?", "id": "Server DNS (Domain Name System)." },
  { "en": "Program komputer yang merusak?", "id": "Virus atau malware." },
  { "en": "Email penipuan untuk mencuri data?", "id": "Phishing." },
  { "en": "Enkripsi digunakan untuk?", "id": "Mengamankan data." },
  { "en": "Perangkat lunak untuk menjelajahi web?", "id": "Browser web." },
  { "en": "Apa itu 'cache' pada browser?", "id": "Penyimpanan data situs sementara." },
  { "en": "Tujuan 'cache'?", "id": "Mempercepat waktu muat halaman." },
  { "en": "Apa itu 'setpoint' dalam sistem kontrol?", "id": "Nilai acuan yang diinginkan." },
  { "en": "Apa itu 'process variable'?", "id": "Nilai aktual yang diukur." },
  { "en": "Apa itu 'controller'?", "id": "Otak yang membandingkan setpoint dan variabel." },
  { "en": "Apa itu 'actuator'?", "id": "Otot yang melakukan aksi fisik." },
  { "en": "Sistem kontrol yang stabil artinya?", "id": "Outputnya tidak berosilasi liar." },
  { "en": "Sistem kontrol yang tidak stabil artinya?", "id": "Outputnya terus membesar tak terkendali." },
  { "en": "Alat untuk mengukur kecepatan putaran motor?", "id": "Tachometer." },
  { "en": "Apa itu 'closed-loop transfer function'?", "id": "Fungsi transfer sistem loop tertutup." },
  { "en": "Apa itu 'thyristor' atau SCR (Silicon Controlled Rectifier)?", "id": "Dioda yang bisa dikontrol gerbangnya." },
  { "en": "Fungsi utama SCR (Silicon Controlled Rectifier)?", "id": "Saklar daya tinggi, kontrol daya AC." },
  { "en": "Komponen elektronika daya lainnya selain SCR?", "id": "TRIAC, GTO, IGBT." },
  { "en": "Apa fungsi TRIAC?", "id": "Mengontrol daya AC dua arah." },
  { "en": "Contoh aplikasi TRIAC?", "id": "Dimmer lampu, kontrol kecepatan kipas." },
  { "en": "Apa itu 'datasheet' komponen?", "id": "Dokumen berisi spesifikasi teknis." },
  { "en": "Apa itu 'bit'?", "id": "Satuan data digital terkecil." },
  { "en": "Apa itu 'byte'?", "id": "Kumpulan 8 bit." },
  { "en": "Sistem bilangan heksadesimal menggunakan basis?", "id": "Basis 16." },
  { "en": "Apa itu 'firmware'?", "id": "Perangkat lunak yang tertanam di perangkat keras." },
  { "en": "Apa itu 'embedded system'?", "id": "Sistem komputer khusus dalam perangkat." },
  { "en": "Contoh 'embedded system'?", "id": "Mikrokontroler di mesin cuci." },
  { "en": "Apa itu 'spectrum analyzer'?", "id": "Alat ukur sinyal dalam domain frekuensi." },
  { "en": "Peralatan untuk proteksi tegangan lebih transien?", "id": "Varistor atau TVS Diode." },
  { "en": "Pita frekuensi untuk siaran radio AM?", "id": "Medium Frequency (MF)." },
  { "en": "Pita frekuensi untuk siaran radio FM?", "id": "Very High Frequency (VHF)." },
  { "en": "Pita frekuensi untuk Wi-Fi dan microwave?", "id": "Super High Frequency (SHF)." },
  { "en": "Apa itu 'return loss'?", "id": "Ukuran sinyal yang dipantulkan balik." },
  { "en": "Apa itu VSWR (Voltage Standing Wave Ratio)?", "id": "Rasio gelombang berdiri pada saluran." },
  { "en": "Nilai VSWR (Voltage Standing Wave Ratio) yang ideal?", "id": "1:1." },
  { "en": "Penyebab VSWR (Voltage Standing Wave Ratio) yang buruk?", "id": "Ketidakcocokan impedansi." },
  { "en": "Kondisi saat impedansi beban sama dengan saluran?", "id": "Matching impedance." },
  { "en": "Apa itu 'wattmeter'?", "id": "Alat untuk mengukur daya listrik." },
  { "en": "Apa itu 'LCR meter'?", "id": "Mengukur Induktansi, Kapasitansi, dan Resistansi." },
  { "en": "Bagaimana cara menghitung induktansi total rangkaian paralel?", "id": "Dengan rumus 1/Ltotal." },
  { "en": "Gelombang listrik di PLN berbentuk?", "id": "Gelombang sinus (sinusoidal)." },
  { "en": "Penyebab gelombang listrik tidak sinus murni?", "id": "Beban non-linear (harmonisa)." },
  { "en": "Jaringan kabel listrik di bawah tanah disebut?", "id": "Saluran Kabel Bawah Tanah (SKBT)." },
  { "en": "Jaringan kabel listrik di udara disebut?", "id": "Saluran Udara (overhead line)." },
  { "en": "Alat untuk menghubungkan dan memutuskan sambungan listrik?", "id": "Konektor (connector)." },
  { "en": "Apa itu 'binary code'?", "id": "Sistem pengkodean menggunakan biner." },
  { "en": "Apa itu 'ASCII'?", "id": "Standar kode untuk karakter teks." },
  { "en": "Apa itu 'port' dalam jaringan komputer?", "id": "Titik akhir komunikasi logis." },
  { "en": "Port 80 digunakan untuk protokol apa?", "id": "HTTP (Hypertext Transfer Protocol)." },
  { "en": "Port 443 digunakan untuk protokol apa?", "id": "HTTPS (HTTP Secure)." },
  { "en": "Apa itu 'cybersecurity'?", "id": "Keamanan dalam dunia siber." },
  { "en": "Apa itu SCR (Silicon Controlled Rectifier)?", "id": "Dioda terkendali untuk switching daya." },
  { "en": "Apa itu TRIAC (Triode for Alternating Current)?", "id": "SCR dua arah untuk kontrol AC." },
  { "en": "Kepanjangan dari MOSFET?", "id": "Metal-Oxide-Semiconductor Field-Effect Transistor." },
  { "en": "Kepanjangan dari IGBT (Insulated-Gate Bipolar Transistor)?", "id": "Insulated-Gate Bipolar Transistor." },
  { "en": "Komponen yang menggabungkan kelebihan BJT dan MOSFET?", "id": "IGBT (Insulated-Gate Bipolar Transistor)." },
  { "en": "Apa fungsi 'freewheeling diode'?", "id": "Melindungi dari tegangan induktif balik." },
  { "en": "Apa itu 'snubber circuit'?", "id": "Rangkaian peredam lonjakan tegangan." },
  { "en": "Apa itu ADC (Analog-to-Digital Converter)?", "id": "Pengubah sinyal analog ke digital." },
  { "en": "Apa itu DAC (Digital-to-Analog Converter)?", "id": "Pengubah sinyal digital ke analog." },
  { "en": "Resolusi ADC (Analog-to-Digital Converter) diukur dalam?", "id": "Bit (contoh: 8-bit, 10-bit)." },
  { "en": "Apa itu 'sampling rate'?", "id": "Seberapa sering sinyal diukur." },
  { "en": "Teorema yang mendasari 'sampling rate'?", "id": "Teorema Nyquist-Shannon." },
  { "en": "Apa itu 'aliasing'?", "id": "Error akibat sampling rate rendah." },
  { "en": "Berapa 'sampling rate' minimum menurut Nyquist?", "id": "Dua kali frekuensi tertinggi sinyal." },
  { "en": "Sistem memori tercepat dalam CPU?", "id": "Register." },
  { "en": "Memori perantara antara CPU dan RAM?", "id": "Cache memory." },
  { "en": "Apa itu 'bus' dalam sistem komputer?", "id": "Jalur komunikasi data internal." },
  { "en": "Tiga jenis 'bus' utama?", "id": "Address, Data, dan Control Bus." },
  { "en": "Apa itu 'polling' dalam mikrokontroler?", "id": "Memeriksa status input secara terus-menerus." },
  { "en": "Apa itu 'interrupt' dalam mikrokontroler?", "id": "Sinyal yang menginterupsi program utama." },
  { "en": "Apa itu PWM (Pulse Width Modulation)?", "id": "Modulasi lebar pulsa." },
  { "en": "Aplikasi PWM (Pulse Width Modulation)?", "id": "Kontrol kecerahan LED, kecepatan motor." },
  { "en": "Apa itu 'duty cycle' dalam PWM?", "id": "Persentase waktu pulsa 'ON'." },
  { "en": "Polarisasi gelombang radio ditentukan oleh arah?", "id": "Medan listriknya." },
  { "en": "Dua jenis polarisasi utama?", "id": "Linear (vertikal/horizontal) dan sirkular." },
  { "en": "Apa itu 'gain' antena?", "id": "Ukuran kemampuan antena memfokuskan sinyal." },
  { "en": "Satuan 'gain' antena?", "id": "dBi atau dBd." },
  { "en": "Apa itu 'beamwidth' antena?", "id": "Sudut pancaran utama antena." },
  { "en": "Apa itu 'fading' dalam komunikasi nirkabel?", "id": "Fluktuasi kekuatan sinyal diterima." },
  { "en": "Penyebab 'fading'?", "id": "Pantulan sinyal (multipath)." },
  { "en": "Teknologi seluler generasi pertama (1G) bersifat?", "id": "Analog." },
  { "en": "Teknologi seluler generasi kedua (2G) bersifat?", "id": "Digital (misal: GSM)." },
  { "en": "Kepanjangan dari GSM?", "id": "Global System for Mobiles." },
  { "en": "Teknologi seluler generasi ketiga (3G) menawarkan?", "id": "Layanan data mobile." },
  { "en": "Teknologi seluler generasi keempat (4G) menawarkan?", "id": "Kecepatan data sangat tinggi." },
  { "en": "Apa itu 'harmonics' dalam sistem tenaga?", "id": "Gelombang pengganggu kelipatan frekuensi dasar." },
  { "en": "Penyebab utama 'harmonics'?", "id": "Beban non-linear (misal: catu daya)." },
  { "en": "Dampak buruk 'harmonics'?", "id": "Pemanasan berlebih, kerusakan peralatan." },
  { "en": "Ukuran total distorsi harmonik?", "id": "THD (Total Harmonic Distortion)." },
  { "en": "Apa itu 'voltage sag'?", "id": "Penurunan tegangan sesaat." },
  { "en": "Apa itu 'voltage swell'?", "id": "Kenaikan tegangan sesaat." },
  { "en": "Apa itu 'transient' dalam sistem tenaga?", "id": "Lonjakan tegangan sangat cepat." },
  { "en": "Sumber utama 'transient'?", "id": "Sambaran petir atau switching." },
  { "en": "Apa itu 'slip' pada motor induksi?", "id": "Perbedaan kecepatan rotor dan medan putar." },
  { "en": "Kecepatan medan putar stator disebut?", "id": "Kecepatan sinkron." },
  { "en": "Bagaimana cara membalik putaran motor 3 fasa?", "id": "Menukar koneksi dua dari tiga fasanya." },
  { "en": "Metode starting motor induksi untuk mengurangi arus?", "id": "Star-Delta starting." },
  { "en": "Alat proteksi motor dari beban lebih?", "id": "TOR (Thermal Overload Relay)." },
  { "en": "Kepanjangan TOR (Thermal Overload Relay)?", "id": "Thermal Overload Relay." },
  { "en": "Protokol standar email?", "id": "SMTP, POP3, IMAP." },
  { "en": "Kepanjangan dari HTTP?", "id": "Hypertext Transfer Protocol." },
  { "en": "Versi aman dari HTTP?", "id": "HTTPS (HTTP Secure)." },
  { "en": "Apa fungsi 'S' pada HTTPS?", "id": "Secure (menggunakan enkripsi)." },
  { "en": "Perangkat yang terhubung ke jaringan disebut?", "id": "Node atau host." },
  { "en": "Kumpulan aturan dalam jaringan disebut?", "id": "Protokol." },
  { "en": "Standar untuk jaringan Wi-Fi?", "id": "IEEE 802.11." },
  { "en": "Standar untuk jaringan Ethernet (LAN)?", "id": "IEEE 802.3." },
  { "en": "Apa itu 'bandwidth throttling'?", "id": "Pembatasan kecepatan transfer data." },
  { "en": "Apa itu 'latency'?", "id": "Waktu tunda dalam pengiriman data." },
  { "en": "Apa itu 'jitter'?", "id": "Variasi dalam latency." },
  { "en": "Model matematis dari sebuah sistem?", "id": "Fungsi transfer." },
  { "en": "Apa itu kestabilan BIBO (Bounded-Input, Bounded-Output)?", "id": "Input terbatas menghasilkan output terbatas." },
  { "en": "Apa itu 'root locus'?", "id": "Metode grafis untuk analisis kestabilan." },
  { "en": "Apa itu 'Bode plot'?", "id": "Plot respon frekuensi sistem." },
  { "en": "Apa itu 'gain margin'?", "id": "Ukuran cadangan kestabilan (gain)." },
  { "en": "Apa itu 'phase margin'?", "id": "Ukuran cadangan kestabilan (fasa)." },
  { "en": "Sistem kontrol untuk mengatur posisi?", "id": "Sistem servo." },
  { "en": "Sensor umpan balik pada sistem servo?", "id": "Encoder atau potensiometer." },
  { "en": "Sistem kontrol untuk mengatur proses industri?", "id": "Sistem kontrol proses." },
  { "en": "Apa itu 'dead time'?", "id": "Waktu tunda murni dalam sistem." },
  { "en": "Filter yang meloloskan semua frekuensi kecuali rentang tertentu?", "id": "BSF (Band Stop Filter)." },
  { "en": "Apa itu 'noise filter'?", "id": "Filter untuk mengurangi noise." },
  { "en": "Apa itu 'power supply switching'?", "id": "Catu daya efisiensi tinggi." },
  { "en": "Kepanjangan SMPS (Switched-Mode Power Supply)?", "id": "Switched-Mode Power Supply." },
  { "en": "Apa itu 'rectifier'?", "id": "Rangkaian penyearah AC ke DC." },
  { "en": "Penyearah jembatan menggunakan berapa dioda?", "id": "Empat dioda." },
  { "en": "Apa itu 'logic probe'?", "id": "Alat untuk memeriksa level logika." },
  { "en": "Level logika 'high' biasanya berapa volt?", "id": "Umumnya 5V atau 3.3V." },
  { "en": "Level logika 'low' biasanya berapa volt?", "id": "Umumnya 0V." },
  { "en": "Apa itu 'floating input'?", "id": "Input yang tidak terhubung kemana-mana." },
  { "en": "Apa itu 'pull-up resistor'?", "id": "Menarik input ke level high." },
  { "en": "Apa itu 'pull-down resistor'?", "id": "Menarik input ke level low." },
  { "en": "Apa itu 'multipath propagation'?", "id": "Sinyal diterima dari banyak lintasan." },
  { "en": "Efek 'multipath propagation'?", "id": "Fading dan distorsi." },
  { "en": "Apa itu 'diversity' dalam komunikasi?", "id": "Menggunakan banyak jalur untuk keandalan." },
  { "en": "Contoh 'diversity'?", "id": "Menggunakan dua antena penerima." },
  { "en": "Apa itu 'duplexer'?", "id": "Memungkinkan antena dipakai mengirim dan menerima." },
  { "en": "Apa itu 'repeater'?", "id": "Menerima, menguatkan, dan memancarkan ulang sinyal." },
  { "en": "Apa itu CT (Current Transformer)?", "id": "Trafo untuk mengukur arus tinggi." },
  { "en": "Apa itu PT (Potential Transformer)?", "id": "Trafo untuk mengukur tegangan tinggi." },
  { "en": "Tujuan CT (Current Transformer) dan PT (Potential Transformer)?", "id": "Untuk pengukuran dan proteksi." },
  { "en": "Sisi sekunder CT (Current Transformer) tidak boleh?", "id": "Dibiarkan terbuka (open circuit)." },
  { "en": "Bahaya membuka sekunder CT (Current Transformer)?", "id": "Menghasilkan tegangan sangat tinggi." },
  { "en": "Sistem SCADA (Supervisory Control and Data Acquisition) digunakan untuk?", "id": "Monitoring dan kontrol jarak jauh." },
  { "en": "Kepanjangan dari SCADA?", "id": "Supervisory Control and Data Acquisition." },
  { "en": "Apa itu 'voltage doubler'?", "id": "Rangkaian pengganda tegangan DC." },
  { "en": "Apa itu 'voltage divider'?", "id": "Rangkaian pembagi tegangan." },
  { "en": "Rangkaian pembagi tegangan menggunakan komponen apa?", "id": "Dua atau lebih resistor seri." },
  { "en": "Apa itu 'current divider'?", "id": "Rangkaian pembagi arus." },
  { "en": "Rangkaian pembagi arus menggunakan komponen apa?", "id": "Dua atau lebih resistor paralel." },
  { "en": "Keluarga logika digital yang umum?", "id": "TTL dan CMOS." },
  { "en": "Kepanjangan dari TTL (Transistor-Transistor Logic)?", "id": "Transistor-Transistor Logic." },
  { "en": "Kepanjangan dari CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Complementary Metal-Oxide-Semiconductor." },
  { "en": "Keunggulan utama CMOS (Complementary Metal-Oxide-Semiconductor) dibanding TTL?", "id": "Konsumsi daya sangat rendah." },
  { "en": "Apa itu 'latch'?", "id": "Flip-flop yang tidak sinkron (asynchronous)." },
  { "en": "Apa itu D Flip-Flop?", "id": "Menyimpan data (D) saat ada clock." },
  { "en": "Apa itu JK Flip-Flop?", "id": "Flip-flop serbaguna dengan input J-K." },
  { "en": "Apa itu T Flip-Flop?", "id": "Membalik (toggle) output saat ada clock." },
  { "en": "Konversi dari bilangan biner ke desimal?", "id": "Menggunakan perkalian pangkat dua." },
  { "en": "Konversi dari desimal ke biner?", "id": "Menggunakan pembagian dengan dua." },
  { "en": "Sistem komunikasi yang menggunakan satelit?", "id": "Komunikasi satelit." },
  { "en": "Orbit satelit geostasioner (GEO)?", "id": "Tetap di atas satu titik bumi." },
  { "en": "Ketinggian orbit GEO (Geostationary Earth Orbit)?", "id": "Sekitar 36.000 km." },
  { "en": "Kelemahan satelit GEO (Geostationary Earth Orbit)?", "id": "Latensi (delay) sinyal tinggi." },
  { "en": "Orbit satelit LEO (Low Earth Orbit)?", "id": "Orbit rendah, bergerak cepat." },
  { "en": "Keunggulan satelit LEO (Low Earth Orbit)?", "id": "Latensi rendah, sinyal kuat." },
  { "en": "Aplikasi satelit LEO (Low Earth Orbit)?", "id": "Internet satelit (Starlink)." },
  { "en": "Transmisi sinyal dari bumi ke satelit?", "id": "Uplink." },
  { "en": "Transmisi sinyal dari satelit ke bumi?", "id": "Downlink." },
  { "en": "Perangkat di satelit yang menerima dan memancarkan sinyal?", "id": "Transponder." },
  { "en": "Apa itu 'footprint' satelit?", "id": "Area cakupan sinyal di bumi." },
  { "en": "Apa itu RC (Resistor-Capacitor) circuit?", "id": "Rangkaian yang terdiri dari R dan C." },
  { "en": "Aplikasi rangkaian RC (Resistor-Capacitor)?", "id": "Timer, filter, osilator." },
  { "en": "Apa itu RL (Resistor-Inductor) circuit?", "id": "Rangkaian yang terdiri dari R dan L." },
  { "en": "Apa itu 'time constant' (konstanta waktu)?", "id": "Ukuran kecepatan respon rangkaian RC/RL." },
  { "en": "Rumus konstanta waktu rangkaian RC?", "id": "τ = R x C." },
  { "en": "Rumus konstanta waktu rangkaian RL?", "id": "τ = L / R." },
  { "en": "Kondisi dimana rangkaian RLC berosilasi?", "id": "Underdamped." },
  { "en": "Apa itu 'damping'?", "id": "Peredaman osilasi dalam sistem." },
  { "en": "Apa itu 'switchgear'?", "id": "Peralatan hubung bagi tegangan menengah/tinggi." },
  { "en": "Isolasi pada 'switchgear' bebas SF6 (Sulfur Hexafluoride)?", "id": "Udara kering atau vakum." },
  { "en": "Peralatan untuk memonitor kualitas daya?", "id": "Power Quality Analyzer." },
  { "en": "Apa itu 'brownout'?", "id": "Penurunan tegangan jangka panjang." },
  { "en": "Apa itu 'interrupt' atau 'outage'?", "id": "Pemadaman listrik total." },
  { "en": "Sistem penyimpan energi untuk menstabilkan grid?", "id": "BESS (Battery Energy Storage System)." },
  { "en": "Kepanjangan dari BESS?", "id": "Battery Energy Storage System." },
  { "en": "Fungsi BESS (Battery Energy Storage System)?", "id": "Menyimpan energi saat surplus, melepas saat kurang." },
  { "en": "Pembangkit yang menggunakan dua siklus termal?", "id": "PLTGU (Pembangkit Listrik Tenaga Gas dan Uap)." },
  { "en": "Kepanjangan PLTGU?", "id": "Pembangkit Listrik Tenaga Gas dan Uap." },
  { "en": "Keunggulan PLTGU (Pembangkit Listrik Tenaga Gas dan Uap)?", "id": "Efisiensi sangat tinggi." },
  { "en": "Apa itu 'smart grid'?", "id": "Jaringan listrik pintar dan otomatis." },
  { "en": "Teknologi kunci 'smart grid'?", "id": "Komunikasi dua arah, sensor, otomasi." },
  { "en": "Apa itu 'distributed generation'?", "id": "Pembangkit listrik tersebar skala kecil." },
  { "en": "Contoh 'distributed generation'?", "id": "PLTS atap, mikrohidro." },
  { "en": "Apa itu 'malware'?", "id": "Perangkat lunak berbahaya." },
  { "en": "Contoh 'malware'?", "id": "Virus, worm, trojan, ransomware." },
  { "en": "Apa itu 'ransomware'?", "id": "Malware yang mengenkripsi data dan minta tebusan." },
  { "en": "Apa itu 'authentication'?", "id": "Proses verifikasi identitas pengguna." },
  { "en": "Contoh metode 'authentication'?", "id": "Password, sidik jari, OTP." },
  { "en": "Kepanjangan OTP (One-Time Password)?", "id": "One-Time Password." },
  { "en": "Apa itu 'authorization'?", "id": "Proses pemberian hak akses." },
  { "en": "Apa itu 'encryption'?", "id": "Proses mengacak data agar aman." },
  { "en": "Apa itu 'decryption'?", "id": "Proses mengembalikan data teracak ke asli." },
  { "en": "Kunci untuk enkripsi dan dekripsi?", "id": "Kunci kriptografi (cryptographic key)." },
  { "en": "Apa itu 'steady-state error'?", "id": "Error sistem setelah kondisi stabil." },
  { "en": "Sistem kontrol tipe 0 memiliki error steady-state terhadap input step?", "id": "Error konstan." },
  { "en": "Sistem kontrol tipe 1 memiliki error steady-state terhadap input step?", "id": "Error nol." },
  { "en": "Apa itu 'actuator' hidrolik?", "id": "Aktuator bertenaga cairan (oli)." },
  { "en": "Apa itu 'actuator' pneumatik?", "id": "Aktuator bertenaga udara bertekanan." },
  { "en": "Keunggulan aktuator hidrolik?", "id": "Gaya sangat besar." },
  { "en": "Keunggulan aktuator pneumatik?", "id": "Cepat dan bersih." },
  { "en": "Sensor untuk mengukur tekanan?", "id": "Pressure sensor/transducer." },
  { "en": "Sensor untuk mengukur aliran fluida?", "id": "Flow meter." },
  { "en": "Apa itu 'amplifier' diferensial?", "id": "Menguatkan selisih dua sinyal input." },
  { "en": "Apa itu 'filter' aktif?", "id": "Filter yang menggunakan komponen aktif (Op-Amp)." },
  { "en": "Apa itu 'filter' pasif?", "id": "Filter dari komponen pasif (R, L, C)." },
  { "en": "Apa itu 'shielding' pada kabel?", "id": "Lapisan pelindung dari interferensi." },
  { "en": "Jenis kabel UTP (Unshielded Twisted Pair) vs STP (Shielded Twisted Pair)?", "id": "STP memiliki lapisan pelindung." },
  { "en": "Apa itu 'propagation delay'?", "id": "Waktu yang dibutuhkan sinyal untuk merambat." },
  { "en": "Apa itu 'logic family'?", "id": "Kelompok IC digital dengan karakteristik sama." },
  { "en": "Apa itu 'fan-out'?", "id": "Jumlah input yang bisa di-drive satu output." },
  { "en": "Apa itu 'noise margin'?", "id": "Batas noise sebelum level logika berubah." },
  { "en": "Apa itu 'power dissipation'?", "id": "Daya yang hilang menjadi panas." },
  { "en": "Perangkat lunak simulasi sirkuit elektronik?", "id": "SPICE, Multisim, Proteus." },
  { "en": "Kepanjangan dari SPICE?", "id": "Simulation Program with Integrated Circuit Emphasis." },
  { "en": "Apa itu 'virtual instrument'?", "id": "Instrumen ukur berbasis perangkat lunak." },
  { "en": "Contoh platform 'virtual instrument'?", "id": "LabVIEW." },
  { "en": "Peralatan untuk memprogram IC (Integrated Circuit) logika?", "id": "Universal programmer." },
  { "en": "Apa itu FPGA (Field-Programmable Gate Array)?", "id": "IC yang logikanya bisa diprogram." },
  { "en": "Kepanjangan FPGA?", "id": "Field-Programmable Gate Array." },
  { "en": "Apa itu CPLD (Complex Programmable Logic Device)?", "id": "Perangkat logika terprogram kompleks." },
  { "en": "Bahasa deskripsi perangkat keras?", "id": "VHDL atau Verilog." },
  { "en": "Kepanjangan VHDL?", "id": "VHSIC Hardware Description Language." },
  { "en": "Apa itu 'synthesis' dalam desain digital?", "id": "Proses mengubah kode menjadi rangkaian logika." },
  { "en": "Peralatan untuk menguji koneksi kabel jaringan?", "id": "LAN tester." },
  { "en": "Apa itu 'crossover cable'?", "id": "Kabel untuk menghubungkan dua perangkat sejenis." },
  { "en": "Apa itu 'straight-through cable'?", "id": "Kabel untuk menghubungkan perangkat berbeda." },
  { "en": "Apa itu 'packet' dalam jaringan?", "id": "Unit data kecil yang dikirim." },
  { "en": "Apa itu 'packet switching'?", "id": "Metode pengiriman data dalam bentuk paket." },
  { "en": "Apa itu 'circuit switching'?", "id": "Metode membuat jalur komunikasi dedicated." },
  { "en": "Contoh 'circuit switching'?", "id": "Jaringan telepon lama." },
  { "en": "Apa itu 'printed circuit board' (PCB)?", "id": "Papan sirkuit cetak." },
  { "en": "Fungsi utama PCB (Printed Circuit Board)?", "id": "Menghubungkan komponen elektronika secara fisik." },
  { "en": "Jalur tembaga pada PCB (Printed Circuit Board) disebut?", "id": "Jejak (trace)." },
  { "en": "Lapisan pelindung PCB (Printed Circuit Board) berwarna hijau?", "id": "Solder mask." },
  { "en": "Teknik pemasangan komponen menembus PCB?", "id": "Through-Hole Technology (THT)." },
  { "en": "Teknik pemasangan komponen di permukaan PCB?", "id": "Surface-Mount Technology (SMT)." },
  { "en": "Komponen untuk SMT (Surface-Mount Technology) disebut?", "id": "SMD (Surface-Mount Device)." },
  { "en": "Apa itu 'multivibrator'?", "id": "Rangkaian untuk menghasilkan sinyal kotak." },
  { "en": "Jenis 'multivibrator' yang stabil di dua keadaan?", "id": "Bistable (flip-flop)." },
  { "en": "Jenis 'multivibrator' yang tidak stabil sama sekali?", "id": "Astable (osilator)." },
  { "en": "Jenis 'multivibrator' yang stabil di satu keadaan?", "id": "Monostable (one-shot)." },
  { "en": "Apa itu 'Schmitt trigger'?", "id": "Rangkaian pembanding dengan histeresis." },
  { "en": "Fungsi 'Schmitt trigger'?", "id": "Mengubah sinyal berisik menjadi bersih." },
  { "en": "Apa itu 'crowbar circuit'?", "id": "Rangkaian proteksi tegangan lebih." },
  { "en": "Cara kerja 'crowbar circuit'?", "id": "Menghubung-singkatkan catu daya." },
  { "en": "Apa itu modulasi PCM (Pulse Code Modulation)?", "id": "Representasi digital dari sinyal analog." },
  { "en": "Kepanjangan dari PCM?", "id": "Pulse Code Modulation." },
  { "en": "Apa itu PAM (Pulse Amplitude Modulation)?", "id": "Modulasi dengan mengubah amplitudo pulsa." },
  { "en": "Apa itu PPM (Pulse Position Modulation)?", "id": "Modulasi dengan mengubah posisi pulsa." },
  { "en": "Apa itu 'codec'?", "id": "Perangkat coder dan decoder." },
  { "en": "Fungsi 'codec' dalam audio/video?", "id": "Mengompres dan mendekompres data." },
  { "en": "Contoh 'codec' video?", "id": "H.264, HEVC, AV1." },
  { "en": "Contoh 'codec' audio?", "id": "MP3, AAC." },
  { "en": "Apa itu 'sampling'?", "id": "Mengambil sampel sinyal pada interval waktu." },
  { "en": "Apa itu 'quantization'?", "id": "Membulatkan nilai sampel ke level terdekat." },
  { "en": "Error yang timbul akibat 'quantization'?", "id": "Quantization error atau noise." },
  { "en": "Frekuensi dimana reaktansi induktif sama dengan kapasitif?", "id": "Frekuensi resonansi." },
  { "en": "Apa itu 'bandwidth' rangkaian RLC?", "id": "Lebar pita frekuensi di sekitar resonansi." },
  { "en": "Apa itu faktor Q (Quality Factor)?", "id": "Ukuran selektivitas rangkaian resonansi." },
  { "en": "Faktor Q (Quality Factor) tinggi berarti?", "id": "Sangat selektif (bandwidth sempit)." },
  { "en": "Faktor Q (Quality Factor) rendah berarti?", "id": "Kurang selektif (bandwidth lebar)." },
  { "en": "Apa itu 'skin effect'?", "id": "Arus AC cenderung mengalir di permukaan." },
  { "en": "Dampak 'skin effect'?", "id": "Meningkatkan resistansi efektif konduktor." },
  { "en": "Apa itu 'Litz wire'?", "id": "Kabel untuk mengurangi 'skin effect'." },
  { "en": "Apa itu 'Ferranti effect'?", "id": "Tegangan ujung saluran lebih tinggi." },
  { "en": "Kapan 'Ferranti effect' terjadi?", "id": "Saluran transmisi panjang tanpa beban." },
  { "en": "Apa itu 'corona discharge'?", "id": "Lucutan listrik di sekitar konduktor." },
  { "en": "Tanda-tanda 'corona discharge'?", "id": "Cahaya ungu, suara mendesis, bau ozon." },
  { "en": "Dampak 'corona discharge'?", "id": "Rugi daya, interferensi radio." },
  { "en": "Bagaimana cara mengurangi korona?", "id": "Menggunakan konduktor bundle." },
  { "en": "Relay yang membandingkan dua arus?", "id": "Relay diferensial." },
  { "en": "Aplikasi utama relay diferensial?", "id": "Proteksi trafo, generator, busbar." },
  { "en": "Relay yang mengukur impedansi saluran?", "id": "Relay jarak (distance relay)." },
  { "en": "Aplikasi utama relay jarak?", "id": "Proteksi saluran transmisi." },
  { "en": "Zona proteksi pada relay jarak?", "id": "Zona 1, 2, 3." },
  { "en": "Apa itu 'andongan' atau 'sag'?", "id": "Lendutan kabel di antara dua tiang." },
  { "en": "Mengapa 'andongan' atau 'sag' diperlukan?", "id": "Mengurangi tegangan tarik mekanis." },
  { "en": "Apa itu 'phishing'?", "id": "Upaya penipuan untuk mencuri data sensitif." },
  { "en": "Apa itu 'spyware'?", "id": "Malware untuk memata-matai aktivitas pengguna." },
  { "en": "Apa itu 'adware'?", "id": "Malware yang menampilkan iklan tak diinginkan." },
  { "en": "Apa itu 'botnet'?", "id": "Jaringan komputer yang terinfeksi malware." },
  { "en": "Apa itu 'patch' perangkat lunak?", "id": "Pembaruan untuk memperbaiki bug/keamanan." },
  { "en": "Apa itu 'zero-day exploit'?", "id": "Serangan yang memanfaatkan celah keamanan baru." },
  { "en": "Sistem operasi berbasis kernel Linux?", "id": "Android, Ubuntu, Debian." },
  { "en": "Sistem operasi untuk produk Apple?", "id": "iOS, macOS." },
  { "en": "Apa itu 'open-source' software?", "id": "Perangkat lunak dengan kode sumber terbuka." },
  { "en": "Apa itu 'proprietary' software?", "id": "Perangkat lunak berpemilik, kode tertutup." },
  { "en": "Apa itu 'stability' dalam sistem kontrol?", "id": "Kemampuan sistem untuk tetap terkendali." },
  { "en": "Sistem yang output-nya terus membesar tak terbatas?", "id": "Sistem tidak stabil (unstable)." },
  { "en": "Apa itu 'rise time'?", "id": "Waktu respon mencapai nilai akhir." },
  { "en": "Apa itu 'settling time'?", "id": "Waktu respon untuk stabil di nilai akhir." },
  { "en": "Apa itu 'overshoot'?", "id": "Output yang melampaui nilai akhir." },
  { "en": "Apa itu 'controller gain'?", "id": "Parameter penguatan pada kontroler." },
  { "en": "Dampak 'controller gain' terlalu tinggi?", "id": "Sistem bisa menjadi tidak stabil." },
  { "en": "Sistem kontroler yang paling umum?", "id": "PID (Proportional-Integral-Derivative) controller." },
  { "en": "Aksi P (Proportional) pada PID?", "id": "Mengurangi error saat ini." },
  { "en": "Aksi I (Integral) pada PID?", "id": "Menghilangkan error steady-state." },
  { "en": "Aksi D (Derivative) pada PID?", "id": "Mempercepat respon, mengurangi overshoot." },
  { "en": "Proses penyesuaian parameter PID?", "id": "Tuning." },
  { "en": "Apa itu 'photodiode'?", "id": "Dioda yang mendeteksi cahaya." },
  { "en": "Apa itu 'phototransistor'?", "id": "Transistor yang dikontrol oleh cahaya." },
  { "en": "Apa itu 'solar cell'?", "id": "Dioda P-N besar pengubah cahaya." },
  { "en": "Fungsi 'solar cell'?", "id": "Mengubah energi cahaya matahari menjadi listrik." },
  { "en": "Apa itu 'crystal oscillator'?", "id": "Osilator sangat stabil menggunakan kristal." },
  { "en": "Bahan kristal pada osilator?", "id": "Kuarsa (quartz)." },
  { "en": "Apa itu 'phase-locked loop' (PLL)?", "id": "Sirkuit untuk sinkronisasi sinyal." },
  { "en": "Aplikasi PLL (Phase-Locked Loop)?", "id": "Sintesis frekuensi, demodulasi FM." },
  { "en": "Apa itu 'mixer' dalam radio?", "id": "Rangkaian untuk mengubah frekuensi sinyal." },
  { "en": "Apa itu 'intermediate frequency' (IF)?", "id": "Frekuensi perantara dalam penerima superheterodyne." },
  { "en": "Apa itu 'automatic gain control' (AGC)?", "id": "Kontrol penguatan otomatis." },
  { "en": "Tujuan AGC (Automatic Gain Control)?", "id": "Menjaga level sinyal output konstan." },
  { "en": "Apa itu 'balanced line'?", "id": "Saluran transmisi dengan dua konduktor simetris." },
  { "en": "Contoh 'balanced line'?", "id": "Kabel twisted pair." },
  { "en": "Apa itu 'unbalanced line'?", "id": "Saluran transmisi dengan satu konduktor dan ground." },
  { "en": "Contoh 'unbalanced line'?", "id": "Kabel koaksial." },
  { "en": "Alat untuk mengubah 'balanced' ke 'unbalanced'?", "id": "Balun (Balanced to Unbalanced)." },
  { "en": "Apa itu 'switch mode power supply' (SMPS)?", "id": "Catu daya yang sangat efisien." },
  { "en": "Apa itu 'linear power supply'?", "id": "Catu daya yang kurang efisien tapi bersih." },
  { "en": "Komponen utama 'linear power supply'?", "id": "Trafo, penyearah, filter, regulator." },
  { "en": "Apa itu 'voltage reference'?", "id": "Sirkuit yang menghasilkan tegangan sangat stabil." },
  { "en": "Apa itu 'current mirror'?", "id": "Sirkuit untuk menyalin arus." },
  { "en": "Apa itu 'differential amplifier'?", "id": "Penguat yang menguatkan selisih dua input." },
  { "en": "Apa itu 'common-mode signal'?", "id": "Sinyal yang sama pada kedua input." },
  { "en": "Apa itu 'differential-mode signal'?", "id": "Sinyal yang berbeda pada kedua input." },
  { "en": "Apa itu 'operational amplifier' (Op-Amp)?", "id": "Penguat diferensial gain sangat tinggi." },
  { "en": "Input pada Op-Amp (Operational Amplifier) pembalik disebut?", "id": "Input inverting (-)." },
  { "en": "Input pada Op-Amp (Operational Amplifier) tak pembalik disebut?", "id": "Input non-inverting (+)." },
  { "en": "Kondisi ideal Op-Amp (Operational Amplifier)?", "id": "Impedansi input tak hingga, output nol." },
  { "en": "Rangkaian Op-Amp (Operational Amplifier) sebagai penjumlah?", "id": "Summing amplifier." },
  { "en": "Rangkaian Op-Amp (Operational Amplifier) sebagai pengurang?", "id": "Differential amplifier." },
  { "en": "Rangkaian Op-Amp (Operational Amplifier) sebagai pembanding?", "id": "Komparator." },
  { "en": "Apa itu 'virtual ground'?", "id": "Titik yang tegangannya 0V tapi tidak terhubung ground." },
  { "en": "Di mana 'virtual ground' ditemukan?", "id": "Pada input inverting Op-Amp." },
  { "en": "Apa itu 'slew rate'?", "id": "Kecepatan perubahan output maksimum Op-Amp." },
  { "en": "Apa itu 'class-A amplifier'?", "id": "Transistor aktif sepanjang siklus." },
  { "en": "Keunggulan 'class-A amplifier'?", "id": "Linearitas dan fidelitas tinggi." },
  { "en": "Kelemahan 'class-A amplifier'?", "id": "Efisiensi sangat rendah, panas." },
  { "en": "Apa itu 'class-B amplifier'?", "id": "Dua transistor aktif bergantian." },
  { "en": "Masalah pada 'class-B amplifier'?", "id": "Crossover distortion." },
  { "en": "Apa itu 'class-AB amplifier'?", "id": "Gabungan kelas A dan B." },
  { "en": "Tujuan 'class-AB amplifier'?", "id": "Mengurangi crossover distortion." },
  { "en": "Apa itu 'class-D amplifier'?", "id": "Penguat switching (sangat efisien)." },
  { "en": "Dasar dari 'class-D amplifier'?", "id": "Teknik PWM (Pulse Width Modulation)." },
  { "en": "Apa itu 'feedback' dalam penguat?", "id": "Mengembalikan sebagian sinyal output ke input." },
  { "en": "'Negative feedback' digunakan untuk?", "id": "Meningkatkan stabilitas, mengurangi distorsi." },
  { "en": "'Positive feedback' digunakan untuk?", "id": "Membuat osilator." },
  { "en": "Apa itu 'baud rate'?", "id": "Jumlah perubahan simbol per detik." },
  { "en": "Perbedaan 'baud rate' dan 'bit rate'?", "id": "Tidak selalu sama, bit rate bisa lebih tinggi." },
  { "en": "Teknik modulasi yang menggabungkan amplitudo dan fasa?", "id": "QAM (Quadrature Amplitude Modulation)." },
  { "en": "Keuntungan QAM (Quadrature Amplitude Modulation)?", "id": "Sangat efisien dalam penggunaan bandwidth." },
  { "en": "Apa itu 'error detection'?", "id": "Metode untuk mendeteksi kesalahan data." },
  { "en": "Contoh metode 'error detection'?", "id": "Parity check, CRC." },
  { "en": "Kepanjangan dari CRC?", "id": "Cyclic Redundancy Check." },
  { "en": "Apa itu 'error correction'?", "id": "Metode untuk memperbaiki kesalahan data." },
  { "en": "Contoh kode 'error correction'?", "id": "Kode Hamming." },
  { "en": "Standar komunikasi serial yang umum?", "id": "RS-232, RS-485." },
  { "en": "Standar komunikasi nirkabel jarak dekat?", "id": "Bluetooth, Zigbee." },
  { "en": "Standar komunikasi nirkabel untuk LAN?", "id": "Wi-Fi (IEEE 802.11)." },
  { "en": "Apa itu 'handshaking'?", "id": "Proses negosiasi parameter komunikasi." },
  { "en": "Apa itu 'buffer'?", "id": "Area memori untuk penyimpanan sementara." },
  { "en": "Daya sesaat (instantaneous power) adalah?", "id": "Perkalian tegangan dan arus sesaat." },
  { "en": "Daya rata-rata (average power) adalah?", "id": "Daya nyata (P) dalam Watt." },
  { "en": "Apa itu 'power factor correction' (PFC)?", "id": "Upaya memperbaiki faktor daya." },
  { "en": "Tujuan PFC (Power Factor Correction)?", "id": "Mengurangi rugi-rugi dan denda PLN." },
  { "en": "Beban yang menyebabkan faktor daya 'leading'?", "id": "Beban kapasitif." },
  { "en": "Beban yang menyebabkan faktor daya 'lagging'?", "id": "Beban induktif." },
  { "en": "Beban dengan faktor daya sama dengan 1?", "id": "Beban resistif murni." },
  { "en": "Apa itu 'apparent power'?", "id": "Daya semu (S) dalam VA." },
  { "en": "Apa itu 'reactive power'?", "id": "Daya reaktif (Q) dalam VAR." },
  { "en": "Apa itu 'real power'?", "id": "Daya nyata (P) dalam Watt." },
  { "en": "Hubungan matematis ketiga daya?", "id": "S² = P² + Q²." },
  { "en": "Apa itu 'insulator' (isolator)?", "id": "Bahan yang tidak menghantarkan listrik." },
  { "en": "Apa itu 'conductor' (konduktor)?", "id": "Bahan yang menghantarkan listrik." },
  { "en": "Contoh konduktor yang baik?", "id": "Perak, tembaga, emas, aluminum." },
  { "en": "Contoh isolator yang baik?", "id": "Karet, porselen, kaca, plastik." },
  { "en": "Apa itu 'dielectric strength'?", "id": "Kekuatan isolator menahan tegangan." },
  { "en": "Satuan 'dielectric strength'?", "id": "kV/mm." },
  { "en": "Proses kegagalan isolator akibat tegangan tinggi?", "id": "Breakdown." },
  { "en": "Apa itu 'bus duct' atau 'busway'?", "id": "Sistem distribusi daya menggunakan busbar terisolasi." },
  { "en": "Keunggulan 'bus duct' dibanding kabel?", "id": "Instalasi rapi, kapasitas arus besar." },
  { "en": "Sistem pembumian (grounding) untuk proteksi petir?", "id": "Lightning protection system." },
  { "en": "Komponen utama proteksi petir eksternal?", "id": "Air terminal, down conductor, grounding." },
  { "en": "Apa itu 'equipotential bonding'?", "id": "Menghubungkan semua bagian konduktif." },
  { "en": "Tujuan 'equipotential bonding'?", "id": "Mencegah beda tegangan berbahaya." },
  { "en": "Apa itu 'surge protective device' (SPD)?", "id": "Perangkat pelindung surja." },
  { "en": "Fungsi SPD (Surge Protective Device)?", "id": "Membuang tegangan surja ke tanah." },
  { "en": "Kelas SPD (Surge Protective Device) untuk panel utama?", "id": "Tipe 1." },
  { "en": "Kelas SPD (Surge Protective Device) untuk sub-panel?", "id": "Tipe 2." },
  { "en": "Apa itu 'ethernet'?", "id": "Teknologi jaringan kabel (LAN) yang dominan." },
  { "en": "Kecepatan standar 'fast ethernet'?", "id": "100 Mbps." },
  { "en": "Kecepatan standar 'gigabit ethernet'?", "id": "1 Gbps (1000 Mbps)." },
  { "en": "Apa itu 'fiber to the home' (FTTH)?", "id": "Jaringan serat optik sampai ke rumah." },
  { "en": "Kepanjangan dari FTTH?", "id": "Fiber to the Home." },
  { "en": "Apa itu 'digital subscriber line' (DSL)?", "id": "Internet melalui saluran telepon tembaga." },
  { "en": "Kepanjangan dari DSL?", "id": "Digital Subscriber Line." },
  { "en": "Apa itu ' denial-of-service' (DoS) attack?", "id": "Serangan untuk melumpuhkan layanan." },
  { "en": "Apa itu 'man-in-the-middle' (MITM) attack?", "id": "Serangan menyadap di tengah komunikasi." },
  { "en": "Apa itu 'malware signature'?", "id": "Pola unik untuk mengidentifikasi malware." },
  { "en": "Perangkat lunak pendeteksi 'malware signature'?", "id": "Antivirus." },
  { "en": "Sistem kontrol yang menggunakan logika fuzzy?", "id": "Fuzzy logic control." },
  { "en": "Sistem kontrol yang menggunakan jaringan saraf tiruan?", "id": "Neural network control." },
  { "en": "Apa itu 'servomechanism'?", "id": "Sistem kontrol umpan balik untuk posisi/kecepatan." },
  { "en": "Apa itu 'regulator'?", "id": "Sistem kontrol umpan balik untuk menjaga variabel." },
  { "en": "Apa itu 'process control'?", "id": "Kontrol untuk proses industri (suhu, tekanan)." },
  { "en": "Diagram blok digunakan untuk?", "id": "Merepresentasikan sistem kontrol secara grafis." },
  { "en": "Apa itu 'summing point' di diagram blok?", "id": "Titik penjumlahan atau pengurangan sinyal." },
  { "en": "Apa itu 'takeoff point' di diagram blok?", "id": "Titik percabangan sinyal." },
  { "en": "Apa itu 'Laplace transform'?", "id": "Transformasi matematis untuk analisis sistem." },
  { "en": "Domain waktu diubah menjadi apa oleh 'Laplace transform'?", "id": "Domain frekuensi kompleks (s-domain)." },
  { "en": "Apa itu 'poles' dari fungsi transfer?", "id": "Akar dari penyebut (denominator)." },
  { "en": "Apa itu 'zeros' dari fungsi transfer?", "id": "Akar dari pembilang (numerator)." },
  { "en": "Posisi 'poles' menentukan apa?", "id": "Kestabilan sistem." },
  { "en": "Sistem stabil jika semua 'poles' berada di?", "id": "Sisi kiri bidang-s." },
  { "en": "Apa itu 'final value theorem'?", "id": "Menentukan nilai akhir respon sistem." },
  { "en": "Apa itu 'initial value theorem'?", "id": "Menentukan nilai awal respon sistem." },
  { "en": "Apa itu 'multimeter'?", "id": "Alat ukur listrik serbaguna." },
  { "en": "Tiga besaran utama yang diukur multimeter?", "id": "Tegangan, arus, dan hambatan." },
  { "en": "Multimeter analog menggunakan apa untuk penunjukan?", "id": "Jarum penunjuk (pointer)." },
  { "en": "Multimeter digital menggunakan apa untuk penunjukan?", "id": "Tampilan digital (display)." },
  { "en": "Kepanjangan DMM?", "id": "Digital Multimeter." },
  { "en": "Bagaimana cara mengukur tegangan dengan multimeter?", "id": "Dipasang paralel dengan beban." },
  { "en": "Bagaimana cara mengukur arus dengan multimeter?", "id": "Dipasang seri dengan beban." },
  { "en": "Bagaimana cara mengukur hambatan dengan multimeter?", "id": "Beban harus dalam keadaan mati." },
  { "en": "Alat ukur bentuk gelombang listrik?", "id": "Osiloskop." },
  { "en": "Sumbu horizontal pada osiloskop merepresentasikan?", "id": "Waktu." },
  { "en": "Sumbu vertikal pada osiloskop merepresentasikan?", "id": "Tegangan." },
  { "en": "Alat untuk menghasilkan berbagai bentuk gelombang?", "id": "Function generator." },
  { "en": "Contoh bentuk gelombang dari 'function generator'?", "id": "Sinus, kotak, segitiga." },
  { "en": "Apa itu 'Fourier analysis'?", "id": "Memecah sinyal menjadi gelombang sinus." },
  { "en": "Komponen frekuensi terendah dalam 'Fourier analysis'?", "id": "Frekuensi fundamental." },
  { "en": "Komponen frekuensi kelipatan dari fundamental?", "id": "Harmonisa." },
  { "en": "Apa itu 'bandwidth' sinyal?", "id": "Rentang frekuensi yang dikandung sinyal." },
  { "en": "Apa itu 'baseband' signal?", "id": "Sinyal informasi sebelum modulasi." },
  { "en": "Apa itu 'broadband' signal?", "id": "Sinyal yang menempati bandwidth lebar." },
  { "en": "Apa itu 'passband' signal?", "id": "Sinyal yang sudah dimodulasi." },
  { "en": "Apa itu 'channel' dalam komunikasi?", "id": "Media fisik tempat sinyal merambat." },
  { "en": "Apa itu 'noise' dalam komunikasi?", "id": "Sinyal acak yang tidak diinginkan." },
  { "en": "Sumber 'thermal noise'?", "id": "Gerakan acak elektron dalam konduktor." },
  { "en": "Apa itu 'interference'?", "id": "Gangguan dari sinyal lain." },
  { "en": "Apa itu 'Shannon-Hartley theorem'?", "id": "Menentukan kapasitas kanal maksimum." },
  { "en": "Kapasitas kanal bergantung pada?", "id": "Bandwidth dan SNR (Signal-to-Noise Ratio)." },
  { "en": "Apa itu 'bit error rate' (BER)?", "id": "Tingkat kesalahan bit dalam transmisi." },
  { "en": "Kepanjangan dari BER?", "id": "Bit Error Rate." },
  { "en": "Nilai BER (Bit Error Rate) yang baik?", "id": "Nilai yang sangat rendah." },
  { "en": "Apa itu 'powerline communication' (PLC)?", "id": "Komunikasi data melalui jaringan listrik." },
  { "en": "Kepanjangan dari PLC (Powerline Communication)?", "id": "Powerline Communication." },
  { "en": "Apa itu 'magnetic field'?", "id": "Medan magnet." },
  { "en": "Apa itu 'electric field'?", "id": "Medan listrik." },
  { "en": "Gelombang yang terdiri dari medan listrik dan magnet?", "id": "Gelombang elektromagnetik." },
  { "en": "Hukum yang mengatur induksi elektromagnetik?", "id": "Hukum Faraday." },
  { "en": "Arah arus induksi ditentukan oleh hukum?", "id": "Hukum Lenz." },
  { "en": "Apa itu 'reluctance'?", "id": "Reluktansi (tahanan magnetik)." },
  { "en": "Apa itu 'permeability'?", "id": "Permeabilitas (kemampuan material dimagnetisasi)." },
  { "en": "Material dengan permeabilitas sangat tinggi?", "id": "Material feromagnetik (besi, nikel)." },
  { "en": "Apa itu 'hysteresis'?", "id": "Ketinggalan fluks magnet terhadap gaya magnet." },
  { "en": "Kerugian daya akibat histeresis disebut?", "id": "Rugi histeresis." },
  { "en": "Arus sirkuler dalam inti besi akibat medan magnet?", "id": "Arus eddy (eddy current)." },
  { "en": "Kerugian daya akibat arus eddy?", "id": "Rugi arus eddy." },
  { "en": "Cara mengurangi rugi arus eddy?", "id": "Menggunakan inti berlapis (laminasi)." },
  { "en": "Peralatan yang mengubah AC ke DC?", "id": "Rectifier (penyearah)." },
  { "en": "Peralatan yang mengubah DC ke AC?", "id": "Inverter." },
  { "en": "Peralatan yang mengubah DC ke DC tegangan lain?", "id": "DC-DC converter." },
  { "en": "Jenis DC-DC converter penaik tegangan?", "id": "Boost converter." },
  { "en": "Jenis DC-DC converter penurun tegangan?", "id": "Buck converter." },
  { "en": "Jenis DC-DC converter penaik/penurun tegangan?", "id": "Buck-boost converter." },
  { "en": "Elektronika yang berurusan dengan konversi daya?", "id": "Elektronika daya (power electronics)." },
  { "en": "Apa itu 'domain name'?", "id": "Nama unik untuk sebuah website." },
  { "en": "Contoh 'top-level domain' (TLD)?", "id": ".com, .id, .org." },
  { "en": "Apa itu 'web hosting'?", "id": "Layanan penyimpanan data website." },
  { "en": "Apa itu 'IP address' publik?", "id": "Alamat IP yang bisa diakses dari internet." },
  { "en": "Apa itu 'IP address' privat?", "id": "Alamat IP untuk jaringan lokal." },
  { "en": "Proses penerjemahan alamat IP privat ke publik?", "id": "NAT (Network Address Translation)." },
  { "en": "Kepanjangan dari NAT?", "id": "Network Address Translation." },
  { "en": "Apa itu 'cookie'?", "id": "Data kecil yang disimpan browser." },
  { "en": "Apa itu 'script'?", "id": "Program kecil di halaman web." },
  { "en": "Bahasa 'scripting' sisi klien yang populer?", "id": "JavaScript." },
  { "en": "Bahasa 'scripting' sisi server yang populer?", "id": "PHP, Python, Node.js." },
  { "en": "Apa itu 'database'?", "id": "Kumpulan data terstruktur." },
  { "en": "Sistem manajemen database yang umum?", "id": "MySQL, PostgreSQL, Oracle." },
  { "en": "Bahasa untuk berinteraksi dengan database relasional?", "id": "SQL (Structured Query Language)." },
  { "en": "Kepanjangan dari SQL?", "id": "Structured Query Language." },
  { "en": "Apa itu 'Nyquist plot'?", "id": "Plot untuk analisis kestabilan sistem." },
  { "en": "Kriteria kestabilan Nyquist didasarkan pada?", "id": "Encirclement titik (-1, 0)." },
  { "en": "Apa itu 'feedback control system'?", "id": "Sistem kontrol loop tertutup." },
  { "en": "Apa itu 'feedforward control'?", "id": "Kontrol untuk mengantisipasi gangguan." },
  { "en": "Apa itu 'cascade control'?", "id": "Sistem kontrol dengan dua loop." },
  { "en": "Apa itu 'adaptive control'?", "id": "Sistem kontrol yang bisa beradaptasi." },
  { "en": "Apa itu 'robust control'?", "id": "Sistem kontrol yang tangguh terhadap ketidakpastian." },
  { "en": "Apa itu 'optimal control'?", "id": "Sistem kontrol yang dioptimalkan." },
  { "en": "Apa itu 'state-space representation'?", "id": "Model sistem menggunakan variabel state." },
  { "en": "Apa itu 'controllability'?", "id": "Kemampuan mengontrol state sistem." },
  { "en": "Apa itu 'observability'?", "id": "Kemampuan mengamati state sistem." },
  { "en": "Apa itu 'sensor fusion'?", "id": "Menggabungkan data dari beberapa sensor." },
  { "en": "Sensor untuk mengukur percepatan?", "id": "Akselerometer." },
  { "en": "Sensor untuk mengukur orientasi sudut?", "id": "Giroskop (gyroscope)." },
  { "en": "Gabungan akselerometer dan giroskop?", "id": "IMU (Inertial Measurement Unit)." },
  { "en": "Kepanjangan dari IMU?", "id": "Inertial Measurement Unit." },
  { "en": "Apa itu 'Kalman filter'?", "id": "Algoritma untuk estimasi state sistem." },
  { "en": "Aplikasi 'Kalman filter'?", "id": "Navigasi, pelacakan objek." },
  { "en": "Sistem kontrol untuk robot?", "id": "Kontrol robotik." },
  { "en": "Ilmu tentang interaksi manusia dan mesin?", "id": "Human-Machine Interface (HMI)." },
  { "en": "Kepanjangan dari HMI?", "id": "Human-Machine Interface." },
  { "en": "Contoh HMI (Human-Machine Interface)?", "id": "Layar sentuh di panel kontrol." },
  { "en": "Sistem kontrol terdistribusi?", "id": "DCS (Distributed Control System)." },
  { "en": "Kepanjangan dari DCS?", "id": "Distributed Control System." },
  { "en": "Aplikasi DCS (Distributed Control System)?", "id": "Pabrik kimia, pembangkit listrik." },
  { "en": "Apa itu 'printed circuit board' (PCB)?", "id": "Papan sirkuit cetak." },
  { "en": "Fungsi utama PCB (Printed Circuit Board)?", "id": "Menghubungkan komponen elektronika secara fisik." },
  { "en": "Jalur tembaga pada PCB (Printed Circuit Board) disebut?", "id": "Jejak (trace)." },
  { "en": "Lapisan pelindung PCB (Printed Circuit Board) berwarna hijau?", "id": "Solder mask." },
  { "en": "Teknik pemasangan komponen menembus PCB?", "id": "Through-Hole Technology (THT)." },
  { "en": "Teknik pemasangan komponen di permukaan PCB?", "id": "Surface-Mount Technology (SMT)." },
  { "en": "Komponen untuk SMT (Surface-Mount Technology) disebut?", "id": "SMD (Surface-Mount Device)." },
  { "en": "Apa itu 'multivibrator'?", "id": "Rangkaian untuk menghasilkan sinyal kotak." },
  { "en": "Jenis 'multivibrator' yang stabil di dua keadaan?", "id": "Bistable (flip-flop)." },
  { "en": "Jenis 'multivibrator' yang tidak stabil sama sekali?", "id": "Astable (osilator)." },
  { "en": "Jenis 'multivibrator' yang stabil di satu keadaan?", "id": "Monostable (one-shot)." },
  { "en": "Apa itu 'Schmitt trigger'?", "id": "Rangkaian pembanding dengan histeresis." },
  { "en": "Fungsi 'Schmitt trigger'?", "id": "Mengubah sinyal berisik menjadi bersih." },
  { "en": "Apa itu 'crowbar circuit'?", "id": "Rangkaian proteksi tegangan lebih." },
  { "en": "Cara kerja 'crowbar circuit'?", "id": "Menghubung-singkatkan catu daya." },
  { "en": "Apa itu modulasi PCM (Pulse Code Modulation)?", "id": "Representasi digital dari sinyal analog." },
  { "en": "Kepanjangan dari PCM?", "id": "Pulse Code Modulation." },
  { "en": "Apa itu PAM (Pulse Amplitude Modulation)?", "id": "Modulasi dengan mengubah amplitudo pulsa." },
  { "en": "Apa itu PPM (Pulse Position Modulation)?", "id": "Modulasi dengan mengubah posisi pulsa." },
  { "en": "Apa itu 'codec'?", "id": "Perangkat coder dan decoder." },
  { "en": "Fungsi 'codec' dalam audio/video?", "id": "Mengompres dan mendekompres data." },
  { "en": "Contoh 'codec' video?", "id": "H.264, HEVC, AV1." },
  { "en": "Contoh 'codec' audio?", "id": "MP3, AAC." },
  { "en": "Apa itu 'sampling'?", "id": "Mengambil sampel sinyal pada interval waktu." },
  { "en": "Apa itu 'quantization'?", "id": "Membulatkan nilai sampel ke level terdekat." },
  { "en": "Error yang timbul akibat 'quantization'?", "id": "Quantization error atau noise." },
  { "en": "Frekuensi dimana reaktansi induktif sama dengan kapasitif?", "id": "Frekuensi resonansi." },
  { "en": "Apa itu 'bandwidth' rangkaian RLC?", "id": "Lebar pita frekuensi di sekitar resonansi." },
  { "en": "Apa itu faktor Q (Quality Factor)?", "id": "Ukuran selektivitas rangkaian resonansi." },
  { "en": "Faktor Q (Quality Factor) tinggi berarti?", "id": "Sangat selektif (bandwidth sempit)." },
  { "en": "Faktor Q (Quality Factor) rendah berarti?", "id": "Kurang selektif (bandwidth lebar)." },
  { "en": "Apa itu 'skin effect'?", "id": "Arus AC cenderung mengalir di permukaan." },
  { "en": "Dampak 'skin effect'?", "id": "Meningkatkan resistansi efektif konduktor." },
  { "en": "Apa itu 'Litz wire'?", "id": "Kabel untuk mengurangi 'skin effect'." },
  { "en": "Apa itu 'Ferranti effect'?", "id": "Tegangan ujung saluran lebih tinggi." },
  { "en": "Kapan 'Ferranti effect' terjadi?", "id": "Saluran transmisi panjang tanpa beban." },
  { "en": "Apa itu 'corona discharge'?", "id": "Lucutan listrik di sekitar konduktor." },
  { "en": "Tanda-tanda 'corona discharge'?", "id": "Cahaya ungu, suara mendesis, bau ozon." },
  { "en": "Dampak 'corona discharge'?", "id": "Rugi daya, interferensi radio." },
  { "en": "Bagaimana cara mengurangi korona?", "id": "Menggunakan konduktor bundle." },
  { "en": "Relay yang membandingkan dua arus?", "id": "Relay diferensial." },
  { "en": "Aplikasi utama relay diferensial?", "id": "Proteksi trafo, generator, busbar." },
  { "en": "Relay yang mengukur impedansi saluran?", "id": "Relay jarak (distance relay)." },
  { "en": "Aplikasi utama relay jarak?", "id": "Proteksi saluran transmisi." },
  { "en": "Zona proteksi pada relay jarak?", "id": "Zona 1, 2, 3." },
  { "en": "Apa itu 'andongan' atau 'sag'?", "id": "Lendutan kabel di antara dua tiang." },
  { "en": "Mengapa 'andongan' atau 'sag' diperlukan?", "id": "Mengurangi tegangan tarik mekanis." },
  { "en": "Apa itu 'phishing'?", "id": "Upaya penipuan untuk mencuri data sensitif." },
  { "en": "Apa itu 'spyware'?", "id": "Malware untuk memata-matai aktivitas pengguna." },
  { "en": "Apa itu 'adware'?", "id": "Malware yang menampilkan iklan tak diinginkan." },
  { "en": "Apa itu 'botnet'?", "id": "Jaringan komputer yang terinfeksi malware." },
  { "en": "Apa itu 'patch' perangkat lunak?", "id": "Pembaruan untuk memperbaiki bug/keamanan." },
  { "en": "Apa itu 'zero-day exploit'?", "id": "Serangan yang memanfaatkan celah keamanan baru." },
  { "en": "Sistem operasi berbasis kernel Linux?", "id": "Android, Ubuntu, Debian." },
  { "en": "Sistem operasi untuk produk Apple?", "id": "iOS, macOS." },
  { "en": "Apa itu 'open-source' software?", "id": "Perangkat lunak dengan kode sumber terbuka." },
  { "en": "Apa itu 'proprietary' software?", "id": "Perangkat lunak berpemilik, kode tertutup." },
  { "en": "Apa itu 'stability' dalam sistem kontrol?", "id": "Kemampuan sistem untuk tetap terkendali." },
  { "en": "Sistem yang output-nya terus membesar tak terbatas?", "id": "Sistem tidak stabil (unstable)." },
  { "en": "Apa itu 'rise time'?", "id": "Waktu respon mencapai nilai akhir." },
  { "en": "Apa itu 'settling time'?", "id": "Waktu respon untuk stabil di nilai akhir." },
  { "en": "Apa itu 'overshoot'?", "id": "Output yang melampaui nilai akhir." },
  { "en": "Apa itu 'controller gain'?", "id": "Parameter penguatan pada kontroler." },
  { "en": "Dampak 'controller gain' terlalu tinggi?", "id": "Sistem bisa menjadi tidak stabil." },
  { "en": "Sistem kontroler yang paling umum?", "id": "PID (Proportional-Integral-Derivative) controller." },
  { "en": "Aksi P (Proportional) pada PID?", "id": "Mengurangi error saat ini." },
  { "en": "Aksi I (Integral) pada PID?", "id": "Menghilangkan error steady-state." },
  { "en": "Aksi D (Derivative) pada PID?", "id": "Mempercepat respon, mengurangi overshoot." },
  { "en": "Proses penyesuaian parameter PID?", "id": "Tuning." },
  { "en": "Apa itu 'photodiode'?", "id": "Dioda yang mendeteksi cahaya." },
  { "en": "Apa itu 'phototransistor'?", "id": "Transistor yang dikontrol oleh cahaya." },
  { "en": "Apa itu 'solar cell'?", "id": "Dioda P-N besar pengubah cahaya." },
  { "en": "Fungsi 'solar cell'?", "id": "Mengubah energi cahaya matahari menjadi listrik." },
  { "en": "Apa itu 'crystal oscillator'?", "id": "Osilator sangat stabil menggunakan kristal." },
  { "en": "Bahan kristal pada osilator?", "id": "Kuarsa (quartz)." },
  { "en": "Apa itu 'phase-locked loop' (PLL)?", "id": "Sirkuit untuk sinkronisasi sinyal." },
  { "en": "Aplikasi PLL (Phase-Locked Loop)?", "id": "Sintesis frekuensi, demodulasi FM." },
  { "en": "Apa itu 'mixer' dalam radio?", "id": "Rangkaian untuk mengubah frekuensi sinyal." },
  { "en": "Apa itu 'intermediate frequency' (IF)?", "id": "Frekuensi perantara dalam penerima superheterodyne." },
  { "en": "Apa itu 'automatic gain control' (AGC)?", "id": "Kontrol penguatan otomatis." },
  { "en": "Tujuan AGC (Automatic Gain Control)?", "id": "Menjaga level sinyal output konstan." },
  { "en": "Apa itu 'balanced line'?", "id": "Saluran transmisi dengan dua konduktor simetris." },
  { "en": "Contoh 'balanced line'?", "id": "Kabel twisted pair." },
  { "en": "Apa itu 'unbalanced line'?", "id": "Saluran transmisi dengan satu konduktor dan ground." },
  { "en": "Contoh 'unbalanced line'?", "id": "Kabel koaksial." },
  { "en": "Alat untuk mengubah 'balanced' ke 'unbalanced'?", "id": "Balun (Balanced to Unbalanced)." },
  { "en": "Apa itu 'switch mode power supply' (SMPS)?", "id": "Catu daya yang sangat efisien." },
  { "en": "Apa itu 'linear power supply'?", "id": "Catu daya yang kurang efisien tapi bersih." },
  { "en": "Komponen utama 'linear power supply'?", "id": "Trafo, penyearah, filter, regulator." },
  { "en": "Apa itu 'voltage reference'?", "id": "Sirkuit yang menghasilkan tegangan sangat stabil." },
  { "en": "Apa itu 'current mirror'?", "id": "Sirkuit untuk menyalin arus." },
  { "en": "Apa itu 'differential amplifier'?", "id": "Penguat yang menguatkan selisih dua input." },
  { "en": "Apa itu 'common-mode signal'?", "id": "Sinyal yang sama pada kedua input." },
  { "en": "Apa itu 'differential-mode signal'?", "id": "Sinyal yang berbeda pada kedua input." },
  { "en": "Apa itu 'multimeter'?", "id": "Alat ukur listrik serbaguna." },
  { "en": "Tiga besaran utama yang diukur multimeter?", "id": "Tegangan, arus, dan hambatan." },
  { "en": "Multimeter analog menggunakan apa untuk penunjukan?", "id": "Jarum penunjuk (pointer)." },
  { "en": "Multimeter digital menggunakan apa untuk penunjukan?", "id": "Tampilan digital (display)." },
  { "en": "Kepanjangan DMM?", "id": "Digital Multimeter." },
  { "en": "Bagaimana cara mengukur tegangan dengan multimeter?", "id": "Dipasang paralel dengan beban." },
  { "en": "Bagaimana cara mengukur arus dengan multimeter?", "id": "Dipasang seri dengan beban." },
  { "en": "Bagaimana cara mengukur hambatan dengan multimeter?", "id": "Beban harus dalam keadaan mati." },
  { "en": "Alat ukur bentuk gelombang listrik?", "id": "Osiloskop." },
  { "en": "Sumbu horizontal pada osiloskop merepresentasikan?", "id": "Waktu." },
  { "en": "Sumbu vertikal pada osiloskop merepresentasikan?", "id": "Tegangan." },
  { "en": "Alat untuk menghasilkan berbagai bentuk gelombang?", "id": "Function generator." },
  { "en": "Contoh bentuk gelombang dari 'function generator'?", "id": "Sinus, kotak, segitiga." },
  { "en": "Apa itu 'Fourier analysis'?", "id": "Memecah sinyal menjadi gelombang sinus." },
  { "en": "Komponen frekuensi terendah dalam 'Fourier analysis'?", "id": "Frekuensi fundamental." },
  { "en": "Komponen frekuensi kelipatan dari fundamental?", "id": "Harmonisa." },
  { "en": "Apa itu 'bandwidth' sinyal?", "id": "Rentang frekuensi yang dikandung sinyal." },
  { "en": "Apa itu 'baseband' signal?", "id": "Sinyal informasi sebelum modulasi." },
  { "en": "Apa itu 'broadband' signal?", "id": "Sinyal yang menempati bandwidth lebar." },
  { "en": "Apa itu 'passband' signal?", "id": "Sinyal yang sudah dimodulasi." },
  { "en": "Apa itu 'channel' dalam komunikasi?", "id": "Media fisik tempat sinyal merambat." },
  { "en": "Apa itu 'noise' dalam komunikasi?", "id": "Sinyal acak yang tidak diinginkan." },
  { "en": "Sumber 'thermal noise'?", "id": "Gerakan acak elektron dalam konduktor." },
  { "en": "Apa itu 'interference'?", "id": "Gangguan dari sinyal lain." },
  { "en": "Apa itu 'Shannon-Hartley theorem'?", "id": "Menentukan kapasitas kanal maksimum." },
  { "en": "Kapasitas kanal bergantung pada?", "id": "Bandwidth dan SNR (Signal-to-Noise Ratio)." },
  { "en": "Apa itu 'bit error rate' (BER)?", "id": "Tingkat kesalahan bit dalam transmisi." },
  { "en": "Kepanjangan dari BER?", "id": "Bit Error Rate." },
  { "en": "Nilai BER (Bit Error Rate) yang baik?", "id": "Nilai yang sangat rendah." },
  { "en": "Apa itu 'powerline communication' (PLC)?", "id": "Komunikasi data melalui jaringan listrik." },
  { "en": "Kepanjangan dari PLC (Powerline Communication)?", "id": "Powerline Communication." },
  { "en": "Apa itu 'magnetic field'?", "id": "Medan magnet." },
  { "en": "Apa itu 'electric field'?", "id": "Medan listrik." },
  { "en": "Gelombang yang terdiri dari medan listrik dan magnet?", "id": "Gelombang elektromagnetik." },
  { "en": "Hukum yang mengatur induksi elektromagnetik?", "id": "Hukum Faraday." },
  { "en": "Arah arus induksi ditentukan oleh hukum?", "id": "Hukum Lenz." },
  { "en": "Apa itu 'reluctance'?", "id": "Reluktansi (tahanan magnetik)." },
  { "en": "Apa itu 'permeability'?", "id": "Permeabilitas (kemampuan material dimagnetisasi)." },
  { "en": "Material dengan permeabilitas sangat tinggi?", "id": "Material feromagnetik (besi, nikel)." },
  { "en": "Apa itu 'hysteresis'?", "id": "Ketinggalan fluks magnet terhadap gaya magnet." },
  { "en": "Kerugian daya akibat histeresis disebut?", "id": "Rugi histeresis." },
  { "en": "Arus sirkuler dalam inti besi akibat medan magnet?", "id": "Arus eddy (eddy current)." },
  { "en": "Kerugian daya akibat arus eddy?", "id": "Rugi arus eddy." },
  { "en": "Cara mengurangi rugi arus eddy?", "id": "Menggunakan inti berlapis (laminasi)." },
  { "en": "Peralatan yang mengubah AC ke DC?", "id": "Rectifier (penyearah)." },
  { "en": "Peralatan yang mengubah DC ke AC?", "id": "Inverter." },
  { "en": "Peralatan yang mengubah DC ke DC tegangan lain?", "id": "DC-DC converter." },
  { "en": "Jenis DC-DC converter penaik tegangan?", "id": "Boost converter." },
  { "en": "Jenis DC-DC converter penurun tegangan?", "id": "Buck converter." },
  { "en": "Jenis DC-DC converter penaik/penurun tegangan?", "id": "Buck-boost converter." },
  { "en": "Elektronika yang berurusan dengan konversi daya?", "id": "Elektronika daya (power electronics)." },
  { "en": "Apa itu 'domain name'?", "id": "Nama unik untuk sebuah website." },
  { "en": "Contoh 'top-level domain' (TLD)?", "id": ".com, .id, .org." },
  { "en": "Apa itu 'web hosting'?", "id": "Layanan penyimpanan data website." },
  { "en": "Apa itu 'IP address' publik?", "id": "Alamat IP yang bisa diakses dari internet." },
  { "en": "Apa itu 'IP address' privat?", "id": "Alamat IP untuk jaringan lokal." },
  { "en": "Proses penerjemahan alamat IP privat ke publik?", "id": "NAT (Network Address Translation)." },
  { "en": "Kepanjangan dari NAT?", "id": "Network Address Translation." },
  { "en": "Apa itu 'cookie'?", "id": "Data kecil yang disimpan browser." },
  { "en": "Apa itu 'script'?", "id": "Program kecil di halaman web." },
  { "en": "Bahasa 'scripting' sisi klien yang populer?", "id": "JavaScript." },
  { "en": "Bahasa 'scripting' sisi server yang populer?", "id": "PHP, Python, Node.js." },
  { "en": "Apa itu 'database'?", "id": "Kumpulan data terstruktur." },
  { "en": "Sistem manajemen database yang umum?", "id": "MySQL, PostgreSQL, Oracle." },
  { "en": "Bahasa untuk berinteraksi dengan database relasional?", "id": "SQL (Structured Query Language)." },
  { "en": "Kepanjangan dari SQL?", "id": "Structured Query Language." },
  { "en": "Apa itu 'Nyquist plot'?", "id": "Plot untuk analisis kestabilan sistem." },
  { "en": "Kriteria kestabilan Nyquist didasarkan pada?", "id": "Encirclement titik (-1, 0)." },
  { "en": "Apa itu 'feedback control system'?", "id": "Sistem kontrol loop tertutup." },
  { "en": "Apa itu 'feedforward control'?", "id": "Kontrol untuk mengantisipasi gangguan." },
  { "en": "Apa itu 'cascade control'?", "id": "Sistem kontrol dengan dua loop." },
  { "en": "Apa itu 'adaptive control'?", "id": "Sistem kontrol yang bisa beradaptasi." },
  { "en": "Apa itu 'robust control'?", "id": "Sistem kontrol yang tangguh terhadap ketidakpastian." },
  { "en": "Apa itu 'optimal control'?", "id": "Sistem kontrol yang dioptimalkan." },
  { "en": "Apa itu 'state-space representation'?", "id": "Model sistem menggunakan variabel state." },
  { "en": "Apa itu 'controllability'?", "id": "Kemampuan mengontrol state sistem." },
  { "en": "Apa itu 'observability'?", "id": "Kemampuan mengamati state sistem." },
  { "en": "Apa itu 'sensor fusion'?", "id": "Menggabungkan data dari beberapa sensor." },
  { "en": "Sensor untuk mengukur percepatan?", "id": "Akselerometer." },
  { "en": "Sensor untuk mengukur orientasi sudut?", "id": "Giroskop (gyroscope)." },
  { "en": "Gabungan akselerometer dan giroskop?", "id": "IMU (Inertial Measurement Unit)." },
  { "en": "Kepanjangan dari IMU?", "id": "Inertial Measurement Unit." },
  { "en": "Apa itu 'Kalman filter'?", "id": "Algoritma untuk estimasi state sistem." },
  { "en": "Aplikasi 'Kalman filter'?", "id": "Navigasi, pelacakan objek." },
  { "en": "Sistem kontrol untuk robot?", "id": "Kontrol robotik." },
  { "en": "Ilmu tentang interaksi manusia dan mesin?", "id": "Human-Machine Interface (HMI)." },
  { "en": "Kepanjangan dari HMI?", "id": "Human-Machine Interface." },
  { "en": "Contoh HMI (Human-Machine Interface)?", "id": "Layar sentuh di panel kontrol." },
  { "en": "Sistem kontrol terdistribusi?", "id": "DCS (Distributed Control System)." },
  { "en": "Kepanjangan dari DCS?", "id": "Distributed Control System." },
  { "en": "Aplikasi DCS (Distributed Control System)?", "id": "Pabrik kimia, pembangkit listrik." },
  { "en": "Apa itu 'operational amplifier' (Op-Amp)?", "id": "Penguat diferensial gain sangat tinggi." },
  { "en": "Input pada Op-Amp (Operational Amplifier) pembalik disebut?", "id": "Input inverting (-)." },
  { "en": "Input pada Op-Amp (Operational Amplifier) tak pembalik disebut?", "id": "Input non-inverting (+)." },
  { "en": "Kondisi ideal Op-Amp (Operational Amplifier)?", "id": "Impedansi input tak hingga, output nol." },
  { "en": "Rangkaian Op-Amp (Operational Amplifier) sebagai penjumlah?", "id": "Summing amplifier." },
  { "en": "Rangkaian Op-Amp (Operational Amplifier) sebagai pengurang?", "id": "Differential amplifier." },
  { "en": "Rangkaian Op-Amp (Operational Amplifier) sebagai pembanding?", "id": "Komparator." },
  { "en": "Apa itu 'virtual ground'?", "id": "Titik yang tegangannya 0V tapi tidak terhubung ground." },
  { "en": "Di mana 'virtual ground' ditemukan?", "id": "Pada input inverting Op-Amp." },
  { "en": "Apa itu 'slew rate'?", "id": "Kecepatan perubahan output maksimum Op-Amp." },
  { "en": "Apa itu 'class-A amplifier'?", "id": "Transistor aktif sepanjang siklus." },
  { "en": "Keunggulan 'class-A amplifier'?", "id": "Linearitas dan fidelitas tinggi." },
  { "en": "Kelemahan 'class-A amplifier'?", "id": "Efisiensi sangat rendah, panas." },
  { "en": "Apa itu 'class-B amplifier'?", "id": "Dua transistor aktif bergantian." },
  { "en": "Masalah pada 'class-B amplifier'?", "id": "Crossover distortion." },
  { "en": "Apa itu 'class-AB amplifier'?", "id": "Gabungan kelas A dan B." },
  { "en": "Tujuan 'class-AB amplifier'?", "id": "Mengurangi crossover distortion." },
  { "en": "Apa itu 'class-D amplifier'?", "id": "Penguat switching (sangat efisien)." },
  { "en": "Dasar dari 'class-D amplifier'?", "id": "Teknik PWM (Pulse Width Modulation)." },
  { "en": "Apa itu 'feedback' dalam penguat?", "id": "Mengembalikan sebagian sinyal output ke input." },
  { "en": "'Negative feedback' digunakan untuk?", "id": "Meningkatkan stabilitas, mengurangi distorsi." },
  { "en": "'Positive feedback' digunakan untuk?", "id": "Membuat osilator." },
  { "en": "Apa itu 'baud rate'?", "id": "Jumlah perubahan simbol per detik." },
  { "en": "Perbedaan 'baud rate' dan 'bit rate'?", "id": "Tidak selalu sama, bit rate bisa lebih tinggi." },
  { "en": "Teknik modulasi yang menggabungkan amplitudo dan fasa?", "id": "QAM (Quadrature Amplitude Modulation)." },
  { "en": "Keuntungan QAM (Quadrature Amplitude Modulation)?", "id": "Sangat efisien dalam penggunaan bandwidth." },
  { "en": "Apa itu 'error detection'?", "id": "Metode untuk mendeteksi kesalahan data." },
  { "en": "Contoh metode 'error detection'?", "id": "Parity check, CRC." },
  { "en": "Kepanjangan dari CRC?", "id": "Cyclic Redundancy Check." },
  { "en": "Apa itu 'error correction'?", "id": "Metode untuk memperbaiki kesalahan data." },
  { "en": "Contoh kode 'error correction'?", "id": "Kode Hamming." },
  { "en": "Standar komunikasi serial yang umum?", "id": "RS-232, RS-485." },
  { "en": "Standar komunikasi nirkabel jarak dekat?", "id": "Bluetooth, Zigbee." },
  { "en": "Standar komunikasi nirkabel untuk LAN?", "id": "Wi-Fi (IEEE 802.11)." },
  { "en": "Apa itu 'handshaking'?", "id": "Proses negosiasi parameter komunikasi." },
  { "en": "Apa itu 'buffer'?", "id": "Area memori untuk penyimpanan sementara." },
  { "en": "Daya sesaat (instantaneous power) adalah?", "id": "Perkalian tegangan dan arus sesaat." },
  { "en": "Daya rata-rata (average power) adalah?", "id": "Daya nyata (P) dalam Watt." },
  { "en": "Apa itu 'power factor correction' (PFC)?", "id": "Upaya memperbaiki faktor daya." },
  { "en": "Tujuan PFC (Power Factor Correction)?", "id": "Mengurangi rugi-rugi dan denda PLN." },
  { "en": "Beban yang menyebabkan faktor daya 'leading'?", "id": "Beban kapasitif." },
  { "en": "Beban yang menyebabkan faktor daya 'lagging'?", "id": "Beban induktif." },
  { "en": "Beban dengan faktor daya sama dengan 1?", "id": "Beban resistif murni." },
  { "en": "Apa itu 'apparent power'?", "id": "Daya semu (S) dalam VA." },
  { "en": "Apa itu 'reactive power'?", "id": "Daya reaktif (Q) dalam VAR." },
  { "en": "Apa itu 'real power'?", "id": "Daya nyata (P) dalam Watt." },
  { "en": "Hubungan matematis ketiga daya?", "id": "S² = P² + Q²." },
  { "en": "Apa itu 'insulator' (isolator)?", "id": "Bahan yang tidak menghantarkan listrik." },
  { "en": "Apa itu 'conductor' (konduktor)?", "id": "Bahan yang menghantarkan listrik." },
  { "en": "Contoh konduktor yang baik?", "id": "Perak, tembaga, emas, aluminum." },
  { "en": "Contoh isolator yang baik?", "id": "Karet, porselen, kaca, plastik." },
  { "en": "Apa itu 'dielectric strength'?", "id": "Kekuatan isolator menahan tegangan." },
  { "en": "Satuan 'dielectric strength'?", "id": "kV/mm." },
  { "en": "Proses kegagalan isolator akibat tegangan tinggi?", "id": "Breakdown." },
  { "en": "Apa itu 'bus duct' atau 'busway'?", "id": "Sistem distribusi daya menggunakan busbar terisolasi." },
  { "en": "Keunggulan 'bus duct' dibanding kabel?", "id": "Instalasi rapi, kapasitas arus besar." },
  { "en": "Sistem pembumian (grounding) untuk proteksi petir?", "id": "Lightning protection system." },
  { "en": "Komponen utama proteksi petir eksternal?", "id": "Air terminal, down conductor, grounding." },
  { "en": "Apa itu 'equipotential bonding'?", "id": "Menghubungkan semua bagian konduktif." },
  { "en": "Tujuan 'equipotential bonding'?", "id": "Mencegah beda tegangan berbahaya." },
  { "en": "Apa itu 'surge protective device' (SPD)?", "id": "Perangkat pelindung surja." },
  { "en": "Fungsi SPD (Surge Protective Device)?", "id": "Membuang tegangan surja ke tanah." },
  { "en": "Kelas SPD (Surge Protective Device) untuk panel utama?", "id": "Tipe 1." },
  { "en": "Kelas SPD (Surge Protective Device) untuk sub-panel?", "id": "Tipe 2." },
  { "en": "Apa itu 'ethernet'?", "id": "Teknologi jaringan kabel (LAN) yang dominan." },
  { "en": "Kecepatan standar 'fast ethernet'?", "id": "100 Mbps." },
  { "en": "Kecepatan standar 'gigabit ethernet'?", "id": "1 Gbps (1000 Mbps)." },
  { "en": "Apa itu 'fiber to the home' (FTTH)?", "id": "Jaringan serat optik sampai ke rumah." },
  { "en": "Kepanjangan dari FTTH?", "id": "Fiber to the Home." },
  { "en": "Apa itu 'digital subscriber line' (DSL)?", "id": "Internet melalui saluran telepon tembaga." },
  { "en": "Kepanjangan dari DSL?", "id": "Digital Subscriber Line." },
  { "en": "Apa itu ' denial-of-service' (DoS) attack?", "id": "Serangan untuk melumpuhkan layanan." },
  { "en": "Apa itu 'man-in-the-middle' (MITM) attack?", "id": "Serangan menyadap di tengah komunikasi." },
  { "en": "Apa itu 'malware signature'?", "id": "Pola unik untuk mengidentifikasi malware." },
  { "en": "Perangkat lunak pendeteksi 'malware signature'?", "id": "Antivirus." },
  { "en": "Sistem kontrol yang menggunakan logika fuzzy?", "id": "Fuzzy logic control." },
  { "en": "Sistem kontrol yang menggunakan jaringan saraf tiruan?", "id": "Neural network control." },
  { "en": "Apa itu 'servomechanism'?", "id": "Sistem kontrol umpan balik untuk posisi/kecepatan." },
  { "en": "Apa itu 'regulator'?", "id": "Sistem kontrol umpan balik untuk menjaga variabel." },
  { "en": "Apa itu 'process control'?", "id": "Kontrol untuk proses industri (suhu, tekanan)." },
  { "en": "Diagram blok digunakan untuk?", "id": "Merepresentasikan sistem kontrol secara grafis." },
  { "en": "Apa itu 'summing point' di diagram blok?", "id": "Titik penjumlahan atau pengurangan sinyal." },
  { "en": "Apa itu 'takeoff point' di diagram blok?", "id": "Titik percabangan sinyal." },
  { "en": "Apa itu 'Laplace transform'?", "id": "Transformasi matematis untuk analisis sistem." },
  { "en": "Domain waktu diubah menjadi apa oleh 'Laplace transform'?", "id": "Domain frekuensi kompleks (s-domain)." },
  { "en": "Apa itu 'poles' dari fungsi transfer?", "id": "Akar dari penyebut (denominator)." },
  { "en": "Apa itu 'zeros' dari fungsi transfer?", "id": "Akar dari pembilang (numerator)." },
  { "en": "Posisi 'poles' menentukan apa?", "id": "Kestabilan sistem." },
  { "en": "Sistem stabil jika semua 'poles' berada di?", "id": "Sisi kiri bidang-s." },
  { "en": "Apa itu 'final value theorem'?", "id": "Menentukan nilai akhir respon sistem." },
  { "en": "Apa itu 'initial value theorem'?", "id": "Menentukan nilai awal respon sistem." },
  { "en": "Apa itu 'multimeter'?", "id": "Alat ukur listrik serbaguna." },
  { "en": "Tiga besaran utama yang diukur multimeter?", "id": "Tegangan, arus, dan hambatan." },
  { "en": "Multimeter analog menggunakan apa untuk penunjukan?", "id": "Jarum penunjuk (pointer)." },
  { "en": "Multimeter digital menggunakan apa untuk penunjukan?", "id": "Tampilan digital (display)." },
  { "en": "Kepanjangan DMM?", "id": "Digital Multimeter." },
  { "en": "Bagaimana cara mengukur tegangan dengan multimeter?", "id": "Dipasang paralel dengan beban." },
  { "en": "Bagaimana cara mengukur arus dengan multimeter?", "id": "Dipasang seri dengan beban." },
  { "en": "Bagaimana cara mengukur hambatan dengan multimeter?", "id": "Beban harus dalam keadaan mati." },
  { "en": "Alat ukur bentuk gelombang listrik?", "id": "Osiloskop." },
  { "en": "Sumbu horizontal pada osiloskop merepresentasikan?", "id": "Waktu." },
  { "en": "Sumbu vertikal pada osiloskop merepresentasikan?", "id": "Tegangan." },
  { "en": "Alat untuk menghasilkan berbagai bentuk gelombang?", "id": "Function generator." },
  { "en": "Contoh bentuk gelombang dari 'function generator'?", "id": "Sinus, kotak, segitiga." },
  { "en": "Apa itu 'Fourier analysis'?", "id": "Memecah sinyal menjadi gelombang sinus." },
  { "en": "Komponen frekuensi terendah dalam 'Fourier analysis'?", "id": "Frekuensi fundamental." },
  { "en": "Komponen frekuensi kelipatan dari fundamental?", "id": "Harmonisa." },
  { "en": "Apa itu 'bandwidth' sinyal?", "id": "Rentang frekuensi yang dikandung sinyal." },
  { "en": "Apa itu 'baseband' signal?", "id": "Sinyal informasi sebelum modulasi." },
  { "en": "Apa itu 'broadband' signal?", "id": "Sinyal yang menempati bandwidth lebar." },
  { "en": "Apa itu 'passband' signal?", "id": "Sinyal yang sudah dimodulasi." },
  { "en": "Apa itu 'channel' dalam komunikasi?", "id": "Media fisik tempat sinyal merambat." },
  { "en": "Apa itu 'noise' dalam komunikasi?", "id": "Sinyal acak yang tidak diinginkan." },
  { "en": "Sumber 'thermal noise'?", "id": "Gerakan acak elektron dalam konduktor." },
  { "en": "Apa itu 'interference'?", "id": "Gangguan dari sinyal lain." },
  { "en": "Apa itu 'Shannon-Hartley theorem'?", "id": "Menentukan kapasitas kanal maksimum." },
  { "en": "Kapasitas kanal bergantung pada?", "id": "Bandwidth dan SNR (Signal-to-Noise Ratio)." },
  { "en": "Apa itu 'bit error rate' (BER)?", "id": "Tingkat kesalahan bit dalam transmisi." },
  { "en": "Kepanjangan dari BER?", "id": "Bit Error Rate." },
  { "en": "Nilai BER (Bit Error Rate) yang baik?", "id": "Nilai yang sangat rendah." },
  { "en": "Apa itu 'powerline communication' (PLC)?", "id": "Komunikasi data melalui jaringan listrik." },
  { "en": "Kepanjangan dari PLC (Powerline Communication)?", "id": "Powerline Communication." },
  { "en": "Apa itu 'magnetic field'?", "id": "Medan magnet." },
  { "en": "Apa itu 'electric field'?", "id": "Medan listrik." },
  { "en": "Gelombang yang terdiri dari medan listrik dan magnet?", "id": "Gelombang elektromagnetik." },
  { "en": "Hukum yang mengatur induksi elektromagnetik?", "id": "Hukum Faraday." },
  { "en": "Arah arus induksi ditentukan oleh hukum?", "id": "Hukum Lenz." },
  { "en": "Apa itu 'reluctance'?", "id": "Reluktansi (tahanan magnetik)." },
  { "en": "Apa itu 'permeability'?", "id": "Permeabilitas (kemampuan material dimagnetisasi)." },
  { "en": "Material dengan permeabilitas sangat tinggi?", "id": "Material feromagnetik (besi, nikel)." },
  { "en": "Apa itu 'hysteresis'?", "id": "Ketinggalan fluks magnet terhadap gaya magnet." },
  { "en": "Kerugian daya akibat histeresis disebut?", "id": "Rugi histeresis." },
  { "en": "Arus sirkuler dalam inti besi akibat medan magnet?", "id": "Arus eddy (eddy current)." },
  { "en": "Kerugian daya akibat arus eddy?", "id": "Rugi arus eddy." },
  { "en": "Cara mengurangi rugi arus eddy?", "id": "Menggunakan inti berlapis (laminasi)." },
  { "en": "Peralatan yang mengubah AC ke DC?", "id": "Rectifier (penyearah)." },
  { "en": "Peralatan yang mengubah DC ke AC?", "id": "Inverter." },
  { "en": "Peralatan yang mengubah DC ke DC tegangan lain?", "id": "DC-DC converter." },
  { "en": "Jenis DC-DC converter penaik tegangan?", "id": "Boost converter." },
  { "en": "Jenis DC-DC converter penurun tegangan?", "id": "Buck converter." },
  { "en": "Jenis DC-DC converter penaik/penurun tegangan?", "id": "Buck-boost converter." },
  { "en": "Elektronika yang berurusan dengan konversi daya?", "id": "Elektronika daya (power electronics)." },
  { "en": "Apa itu 'domain name'?", "id": "Nama unik untuk sebuah website." },
  { "en": "Contoh 'top-level domain' (TLD)?", "id": ".com, .id, .org." },
  { "en": "Apa itu 'web hosting'?", "id": "Layanan penyimpanan data website." },
  { "en": "Apa itu 'IP address' publik?", "id": "Alamat IP yang bisa diakses dari internet." },
  { "en": "Apa itu 'IP address' privat?", "id": "Alamat IP untuk jaringan lokal." },
  { "en": "Proses penerjemahan alamat IP privat ke publik?", "id": "NAT (Network Address Translation)." },
  { "en": "Kepanjangan dari NAT?", "id": "Network Address Translation." },
  { "en": "Apa itu 'cookie'?", "id": "Data kecil yang disimpan browser." },
  { "en": "Apa itu 'script'?", "id": "Program kecil di halaman web." },
  { "en": "Bahasa 'scripting' sisi klien yang populer?", "id": "JavaScript." },
  { "en": "Bahasa 'scripting' sisi server yang populer?", "id": "PHP, Python, Node.js." },
  { "en": "Apa itu 'database'?", "id": "Kumpulan data terstruktur." },
  { "en": "Sistem manajemen database yang umum?", "id": "MySQL, PostgreSQL, Oracle." },
  { "en": "Bahasa untuk berinteraksi dengan database relasional?", "id": "SQL (Structured Query Language)." },
  { "en": "Kepanjangan dari SQL?", "id": "Structured Query Language." },
  { "en": "Apa itu 'Nyquist plot'?", "id": "Plot untuk analisis kestabilan sistem." },
  { "en": "Kriteria kestabilan Nyquist didasarkan pada?", "id": "Encirclement titik (-1, 0)." },
  { "en": "Apa itu 'feedback control system'?", "id": "Sistem kontrol loop tertutup." },
  { "en": "Apa itu 'feedforward control'?", "id": "Kontrol untuk mengantisipasi gangguan." },
  { "en": "Apa itu 'cascade control'?", "id": "Sistem kontrol dengan dua loop." },
  { "en": "Apa itu 'adaptive control'?", "id": "Sistem kontrol yang bisa beradaptasi." },
  { "en": "Apa itu 'robust control'?", "id": "Sistem kontrol yang tangguh terhadap ketidakpastian." },
  { "en": "Apa itu 'optimal control'?", "id": "Sistem kontrol yang dioptimalkan." },
  { "en": "Apa itu 'state-space representation'?", "id": "Model sistem menggunakan variabel state." },
  { "en": "Apa itu 'controllability'?", "id": "Kemampuan mengontrol state sistem." },
  { "en": "Apa itu 'observability'?", "id": "Kemampuan mengamati state sistem." },
  { "en": "Apa itu 'sensor fusion'?", "id": "Menggabungkan data dari beberapa sensor." },
  { "en": "Sensor untuk mengukur percepatan?", "id": "Akselerometer." },
  { "en": "Sensor untuk mengukur orientasi sudut?", "id": "Giroskop (gyroscope)." },
  { "en": "Gabungan akselerometer dan giroskop?", "id": "IMU (Inertial Measurement Unit)." },
  { "en": "Kepanjangan dari IMU?", "id": "Inertial Measurement Unit." },
  { "en": "Apa itu 'Kalman filter'?", "id": "Algoritma untuk estimasi state sistem." },
  { "en": "Aplikasi 'Kalman filter'?", "id": "Navigasi, pelacakan objek." },
  { "en": "Sistem kontrol untuk robot?", "id": "Kontrol robotik." },
  { "en": "Ilmu tentang interaksi manusia dan mesin?", "id": "Human-Machine Interface (HMI)." },
  { "en": "Kepanjangan dari HMI?", "id": "Human-Machine Interface." },
  { "en": "Contoh HMI (Human-Machine Interface)?", "id": "Layar sentuh di panel kontrol." },
  { "en": "Sistem kontrol terdistribusi?", "id": "DCS (Distributed Control System)." },
  { "en": "Kepanjangan dari DCS?", "id": "Distributed Control System." },
  { "en": "Aplikasi DCS (Distributed Control System)?", "id": "Pabrik kimia, pembangkit listrik." },
  { "en": "Apa itu 'biasing' pada transistor?", "id": "Memberi tegangan DC untuk titik kerja." },
  { "en": "Tujuan 'biasing' transistor?", "id": "Agar bisa bekerja sebagai penguat." },
  { "en": "Titik kerja transistor disebut juga?", "id": "Titik Q (Quiescent point)." },
  { "en": "Metode penyambungan antar tingkat penguat?", "id": "Coupling." },
  { "en": "Jenis 'coupling' yang paling umum?", "id": "RC coupling (Resistor-Capacitor)." },
  { "en": "Keuntungan 'transformer coupling'?", "id": "Menyesuaikan impedansi, isolasi galvanis." },
  { "en": "Apa itu 'darlington pair'?", "id": "Dua transistor BJT digabung." },
  { "en": "Tujuan 'darlington pair'?", "id": "Mendapatkan penguatan arus sangat tinggi." },
  { "en": "Apa itu 'cascode amplifier'?", "id": "Konfigurasi penguat untuk frekuensi tinggi." },
  { "en": "Apa itu 'multiplexer' (MUX)?", "id": "Memilih satu dari banyak input." },
  { "en": "Apa itu 'demultiplexer' (DEMUX)?", "id": "Mengirim satu input ke banyak output." },
  { "en": "Rangkaian logika yang bisa menjumlahkan?", "id": "Adder (full adder, half adder)." },
  { "en": "Apa itu 'comparator' digital?", "id": "Membandingkan dua bilangan biner." },
  { "en": "Apa itu 'shift register'?", "id": "Register yang bisa menggeser datanya." },
  { "en": "Aplikasi 'shift register'?", "id": "Konversi data serial-paralel." },
  { "en": "Pita frekuensi VLF (Very Low Frequency)?", "id": "3 - 30 kHz." },
  { "en": "Aplikasi VLF (Very Low Frequency)?", "id": "Komunikasi kapal selam." },
  { "en": "Pita frekuensi SHF (Super High Frequency)?", "id": "3 - 30 GHz." },
  { "en": "Aplikasi SHF (Super High Frequency)?", "id": "Radar, Wi-Fi 5 GHz, satelit." },
  { "en": "Pita frekuensi EHF (Extremely High Frequency)?", "id": "30 - 300 GHz." },
  { "en": "Media transmisi untuk frekuensi sangat tinggi (microwave)?", "id": "Waveguide (pemandu gelombang)." },
  { "en": "Bentuk 'waveguide' (pemandu gelombang)?", "id": "Pipa logam berongga." },
  { "en": "Apa itu 'parity bit'?", "id": "Bit tambahan untuk deteksi error." },
  { "en": "Jenis 'parity check'?", "id": "Paritas ganjil (odd) dan genap (even)." },
  { "en": "Apa itu 'checksum'?", "id": "Metode deteksi error dengan penjumlahan." },
  { "en": "Apa itu 'line coding'?", "id": "Metode merepresentasikan data digital." },
  { "en": "Contoh 'line coding'?", "id": "NRZ, Manchester, Bipolar AMI." },
  { "en": "Apa itu 'scattering'?", "id": "Hamburan gelombang oleh partikel." },
  { "en": "Apa itu 'diffraction'?", "id": "Pembelokan gelombang di sekitar halangan." },
  { "en": "Apa itu 'reflection'?", "id": "Pemantulan gelombang oleh permukaan." },
  { "en": "Teorema superposisi berlaku untuk rangkaian?", "id": "Rangkaian linear." },
  { "en": "Prinsip teorema superposisi?", "id": "Menganalisis satu sumber pada satu waktu." },
  { "en": "Teorema transfer daya maksimum terjadi saat?", "id": "Resistansi beban sama dengan Thevenin." },
  { "en": "Respon rangkaian saat saklar baru ditutup?", "id": "Respon transien." },
  { "en": "Respon rangkaian setelah waktu yang lama?", "id": "Respon steady-state." },
  { "en": "Kapasitor pada rangkaian DC steady-state bersifat?", "id": "Rangkaian terbuka (open circuit)." },
  { "en": "Induktor pada rangkaian DC steady-state bersifat?", "id": "Rangkaian singkat (short circuit)." },
  { "en": "Rangkaian untuk mengubah bentuk gelombang?", "id": "Clamper dan clipper." },
  { "en": "Fungsi rangkaian 'clamper'?", "id": "Menggeser level DC sinyal." },
  { "en": "Fungsi rangkaian 'clipper'?", "id": "Memotong sebagian sinyal." },
  { "en": "Apa itu 'lightning arrester'?", "id": "Pelindung surja petir." },
  { "en": "Di mana 'lightning arrester' dipasang?", "id": "Sebelum peralatan mahal (trafo)." },
  { "en": "Fungsi utama busbar di gardu induk?", "id": "Titik temu koneksi banyak sirkuit." },
  { "en": "Apa itu 'switchgear' metal-clad?", "id": "Switchgear dengan kompartemen logam terpisah." },
  { "en": "Tujuan kompartemen terpisah pada 'switchgear'?", "id": "Meningkatkan keamanan dan keandalan." },
  { "en": "Apa itu 'automatic recloser'?", "id": "PMT yang bisa menutup kembali otomatis." },
  { "en": "Di mana 'automatic recloser' sering dipasang?", "id": "Jaringan distribusi." },
  { "en": "Tujuan 'recloser'?", "id": "Mengatasi gangguan temporer." },
  { "en": "Apa itu 'sectionalizer'?", "id": "Saklar yang membuka saat gangguan permanen." },
  { "en": "Kerja 'sectionalizer' dikoordinasikan dengan?", "id": "Recloser." },
  { "en": "Apa itu sistem operasi (Operating System - OS)?", "id": "Perangkat lunak inti pengelola komputer." },
  { "en": "Fungsi utama OS (Operating System)?", "id": "Manajemen memori, proses, dan perangkat keras." },
  { "en": "Contoh OS (Operating System) desktop?", "id": "Windows, macOS, Linux." },
  { "en": "Topologi jaringan dimana semua terhubung ke kabel utama?", "id": "Topologi bus." },
  { "en": "Topologi jaringan dimana semua terhubung ke pusat?", "id": "Topologi bintang (star)." },
  { "en": "Topologi jaringan berbentuk lingkaran tertutup?", "id": "Topologi cincin (ring)." },
  { "en": "Perbedaan utama router dan switch?", "id": "Router antar jaringan, switch dalam jaringan." },
  { "en": "Router bekerja di lapisan OSI mana?", "id": "Layer 3 (Network Layer)." },
  { "en": "Switch bekerja di lapisan OSI mana?", "id": "Layer 2 (Data Link Layer)." },
  { "en": "Proses membungkus data dengan header protokol?", "id": "Enkapsulasi." },
  { "en": "Apa itu sensor?", "id": "Mengubah besaran fisik menjadi sinyal listrik." },
  { "en": "Apa itu transduser?", "id": "Mengubah satu bentuk energi ke bentuk lain." },
  { "en": "Apakah semua sensor adalah transduser?", "id": "Ya, benar." },
  { "en": "Apakah semua transduser adalah sensor?", "id": "Tidak, contohnya loudspeaker." },
  { "en": "Kriteria kestabilan Routh-Hurwitz didasarkan pada?", "id": "Tabel Routh." },
  { "en": "Sistem stabil menurut Routh-Hurwitz jika?", "id": "Semua elemen kolom pertama positif." },
  { "en": "Apa itu 'servomotor'?", "id": "Motor untuk kontrol posisi presisi." },
  { "en": "Umpan balik pada 'servomotor' biasanya berupa?", "id": "Posisi sudut (dari encoder)." },
  { "en": "Apa itu 'stepper motor'?", "id": "Motor yang bergerak dalam langkah diskrit." },
  { "en": "Keuntungan 'stepper motor'?", "id": "Kontrol posisi loop terbuka presisi." },
  { "en": "Apa itu 'photovoltaic cell'?", "id": "Sel surya." },
  { "en": "Efek yang mendasari kerja sel surya?", "id": "Efek fotovoltaik." },
  { "en": "Gabungan beberapa sel surya?", "id": "Modul surya atau panel surya." },
  { "en": "Gabungan beberapa modul surya?", "id": "Array surya." },
  { "en": "Alat untuk mengisi daya baterai dari panel surya?", "id": "Solar Charge Controller." },
  { "en": "Tujuan 'Solar Charge Controller'?", "id": "Mencegah overcharging dan over-discharging." },
  { "en": "Teknologi untuk memaksimalkan daya panel surya?", "id": "MPPT (Maximum Power Point Tracking)." },
  { "en": "Kepanjangan MPPT?", "id": "Maximum Power Point Tracking." },
  { "en": "Apa itu 'bypass diode' pada panel surya?", "id": "Melindungi sel dari bayangan (shading)." },
  { "en": "Apa itu 'blocking diode'?", "id": "Mencegah arus balik ke panel malam hari." },
  { "en": "Apa itu 'diode bridge'?", "id": "Empat dioda untuk penyearah gelombang penuh." },
  { "en": "Apa itu 'voltage regulator'?", "id": "Sirkuit penjaga tegangan output stabil." },
  { "en": "Jenis regulator tegangan?", "id": "Linear dan switching." },
  { "en": "Regulator tegangan yang lebih efisien?", "id": "Switching regulator." },
  { "en": "Kelebihan regulator linear?", "id": "Noise output sangat rendah." },
  { "en": "Apa itu LDO (Low-Dropout) regulator?", "id": "Regulator linear dengan beda tegangan kecil." },
  { "en": "Apa itu 'baud'?", "id": "Satuan untuk kecepatan simbol." },
  { "en": "Apa itu 'protocol stack'?", "id": "Kumpulan lapisan protokol (misal: TCP/IP)." },
  { "en": "Apa itu 'gateway'?", "id": "Penghubung dua jaringan dengan protokol berbeda." },
  { "en": "Apa itu 'proxy server'?", "id": "Server perantara antara klien dan internet." },
  { "en": "Fungsi 'proxy server'?", "id": "Filtering, caching, keamanan." },
  { "en": "Apa itu 'time constant'?", "id": "Waktu yang dibutuhkan untuk mencapai 63.2%." },
  { "en": "Lima kali 'time constant' dianggap?", "id": "Mencapai kondisi steady-state." },
  { "en": "Apa itu 'dielectric'?", "id": "Bahan isolator dalam kapasitor." },
  { "en": "Fungsi bahan 'dielectric'?", "id": "Meningkatkan kapasitansi." },
  { "en": "Ukuran kemampuan dielektrik menyimpan energi?", "id": "Konstanta dielektrik." },
  { "en": "Representasi digital dari aset fisik jaringan?", "id": "Kembaran digital (digital twin)." },
  { "en": "Aplikasi AI (Artificial Intelligence) untuk prediksi beban?", "id": "Prediksi beban (load forecasting)." },
  { "en": "Jaringan sensor pintar di jaringan listrik?", "id": "IoT (Internet of Things)." },
  { "en": "Penggunaan BESS (Battery Energy Storage System) untuk menstabilkan frekuensi?", "id": "Regulasi frekuensi." },
  { "en": "Penggunaan BESS (Battery Energy Storage System) untuk memotong beban puncak?", "id": "Peak shaving." },
  { "en": "Inverter yang mengikuti sinyal grid?", "id": "Grid-following inverter." },
  { "en": "Inverter yang bisa menciptakan sinyal grid sendiri?", "id": "Grid-forming inverter." },
  { "en": "Pembangkit yang menggabungkan surya, angin, dan baterai?", "id": "Pembangkit listrik hibrida." },
  { "en": "Skema proteksi yang menggunakan data area luas?", "id": "WAPS (Wide Area Protection Schemes)." },
  { "en": "Perangkat pengukur fasor tegangan dan arus tersinkronisasi?", "id": "PMU (Phasor Measurement Unit)." },
  { "en": "Proteksi yang setelannya bisa beradaptasi?", "id": "Adaptive relaying." },
  { "en": "Teknologi 'switchgear' alternatif pengganti gas SF6 (Sulfur Hexafluoride)?", "id": "Udara bersih atau vakum." },
  { "en": "Pengujian untuk mendeteksi kerusakan mekanis belitan trafo?", "id": "SFRA (Sweep Frequency Response Analysis)." },
  { "en": "Pemantauan kondisi peralatan secara terus-menerus?", "id": "Online monitoring." },
  { "en": "Layanan tambahan untuk menjaga keandalan grid?", "id": "Layanan pendukung (ancillary services)." },
  { "en": "Program agar pelanggan mengurangi pemakaian listrik?", "id": "Demand response." },
  { "en": "Transaksi jual beli listrik langsung antar pelanggan?", "id": "P2P (Peer-to-Peer) energy trading." },
  { "en": "Sistem teknologi untuk mengelola operasi fisik?", "id": "OT (Operational Technology)." },
  { "en": "Keamanan siber untuk sistem OT (Operational Technology)?", "id": "OT cybersecurity." },
  { "en": "Kemampuan jaringan untuk pulih dari gangguan parah?", "id": "Ketahanan (resilience)." },
  { "en": "Kemampuan pembangkit untuk hidup tanpa pasokan dari luar?", "id": "Kemampuan black start." },
  { "en": "Teknologi mobil listrik (EV) menyuplai daya ke grid?", "id": "V2G (Vehicle-to-Grid)." },
  { "en": "Dampak pengisian daya cepat pada trafo?", "id": "Beban lebih dan penuaan." },
  { "en": "Pengisian daya mobil listrik (EV) yang dikontrol secara cerdas?", "id": "Smart charging." },
  { "en": "Apa itu 'electromagnetic compatibility' (EMC)?", "id": "Kemampuan alat berfungsi tanpa interferensi." },
  { "en": "Apa itu 'electromagnetic interference' (EMI)?", "id": "Gangguan elektromagnetik." },
  { "en": "Bagaimana cara kerja 'noise cancelling headset'?", "id": "Membuat sinyal anti-noise." },
  { "en": "Apa itu 'signal processing'?", "id": "Pemrosesan atau analisis sinyal." },
  { "en": "Teknik 'digital signal processing' (DSP)?", "id": "Memproses sinyal secara digital." },
  { "en": "Proses mengubah sinyal waktu ke frekuensi?", "id": "Transformasi Fourier." },
  { "en": "Algoritma cepat untuk Transformasi Fourier?", "id": "FFT (Fast Fourier Transform)." },
  { "en": "Papan pengembangan untuk membuat prototipe elektronik?", "id": "Development board (misal: Arduino)." },
  { "en": "Bahasa pemrograman umum untuk Arduino?", "id": "C++." },
  { "en": "Komputer mini seukuran kartu kredit?", "id": "Raspberry Pi." },
  { "en": "Sistem operasi pada Raspberry Pi?", "id": "Linux (Raspberry Pi OS)." },
  { "en": "Apa itu 'time division multiplexing' (TDM)?", "id": "Multiplexing berbasis pembagian waktu." },
  { "en": "Apa itu 'frequency division multiplexing' (FDM)?", "id": "Multiplexing berbasis pembagian frekuensi." },
  { "en": "Apa itu 'orthogonal frequency-division multiplexing' (OFDM)?", "id": "Teknik modulasi untuk 4G/5G/Wi-Fi." },
  { "en": "Keunggulan OFDM (Orthogonal Frequency-Division Multiplexing)?", "id": "Tahan terhadap multipath fading." },
  { "en": "Apa itu 'multiple-input multiple-output' (MIMO)?", "id": "Menggunakan banyak antena pemancar/penerima." },
  { "en": "Keuntungan MIMO (Multiple-Input Multiple-Output)?", "id": "Meningkatkan kecepatan dan kapasitas." },
  { "en": "Teknologi yang memungkinkan banyak pengguna berbagi frekuensi?", "id": "CDMA, TDMA, FDMA." },
  { "en": "Kepanjangan dari CDMA?", "id": "Code Division Multiple Access." },
  { "en": "Nilai tegangan AC yang diukur multimeter?", "id": "Nilai RMS (Root Mean Square)." },
  { "en": "Nilai arus AC yang diukur tang ampere?", "id": "Nilai RMS (Root Mean Square)." },
  { "en": "Peralatan untuk mengukur iluminasi cahaya?", "id": "Lux meter." },
  { "en": "Satuan fluks cahaya?", "id": "Lumen." },
  { "en": "Satuan iluminasi?", "id": "Lux." },
  { "en": "Jenis sambungan tiga fasa berbentuk huruf Y?", "id": "Hubungan Bintang (Star)." },
  { "en": "Jenis sambungan tiga fasa berbentuk segitiga?", "id": "Hubungan Segitiga (Delta)." },
  { "en": "Pada hubungan Bintang, Arus Line sama dengan?", "id": "Arus Fasa." },
  { "en": "Pada hubungan Segitiga, Tegangan Line sama dengan?", "id": "Tegangan Fasa." },
  { "en": "Pengaman arus sisa disebut?", "id": "Residual Current Device (RCD)." },
  { "en": "Fungsi utama RCD (Residual Current Device)?", "id": "Melindungi dari sengatan listrik." },
  { "en": "Sensitivitas RCD (Residual Current Device) untuk manusia?", "id": "30 miliAmpere." },
  { "en": "Sistem kontrol frekuensi beban?", "id": "LFC (Load Frequency Control)." },
  { "en": "LFC (Load Frequency Control) adalah bagian dari?", "id": "AGC (Automatic Generation Control)." },
  { "en": "Tujuan LFC (Load Frequency Control)?", "id": "Menjaga frekuensi sistem tetap stabil." },
  { "en": "Apa itu 'primary frequency control'?", "id": "Respon governor pembangkit." },
  { "en": "Apa itu 'secondary frequency control'?", "id": "Aksi dari LFC/AGC." },
  { "en": "Apa itu 'tertiary frequency control'?", "id": "Redispatch ekonomis secara manual/otomatis." },
  { "en": "Apa itu 'routing' dalam jaringan?", "id": "Proses menentukan jalur paket data." },
  { "en": "Tabel yang digunakan router untuk menentukan jalur?", "id": "Tabel routing." },
  { "en": "Protokol routing dinamis?", "id": "OSPF, BGP, RIP." },
  { "en": "Apa itu 'subnet mask'?", "id": "Menentukan bagian network dan host IP." },
  { "en": "Apa itu 'default gateway'?", "id": "Router jalan keluar dari jaringan lokal." },
  { "en": "Apa itu DHCP (Dynamic Host Configuration Protocol)?", "id": "Protokol pemberi alamat IP otomatis." },
  { "en": "Kepanjangan dari DHCP?", "id": "Dynamic Host Configuration Protocol." },
  { "en": "Apa itu 'ping of death'?", "id": "Serangan DoS menggunakan ping." },
  { "en": "Apa itu 'SQL injection'?", "id": "Serangan pada aplikasi web via database." },
  { "en": "Apa itu 'cross-site scripting' (XSS)?", "id": "Serangan injeksi skrip di sisi klien." },
  { "en": "Apa itu 'deadlock' dalam sistem kontrol?", "id": "Kondisi saling tunggu antar proses." },
  { "en": "Apa itu 'actuator saturation'?", "id": "Aktuator mencapai batas maksimumnya." },
  { "en": "Sistem kontrol yang perilakunya berubah secara diskrit?", "id": "Discrete-time control system." },
  { "en": "Transformasi matematis untuk sistem waktu diskrit?", "id": "Transformasi-Z." },
  { "en": "Apa itu 'aliasing' dalam sampling sinyal?", "id": "Frekuensi tinggi menyamar sebagai frekuensi rendah." },
  { "en": "Filter untuk mencegah 'aliasing'?", "id": "Filter anti-aliasing (low-pass filter)." },
  { "en": "Alat untuk mengukur tahanan isolasi?", "id": "Megohmmeter atau Megger." },
  { "en": "Satuan hasil ukur Megger?", "id": "Mega-ohm (MΩ)." },
  { "en": "Pengujian tahanan isolasi disebut juga?", "id": "Uji Megger." },
  { "en": "Nilai tahanan isolasi yang baik?", "id": "Sangat tinggi." },
  { "en": "Pengujian rasio belitan transformator?", "id": "TTR (Transformer Turn Ratio) test." },
  { "en": "Pengujian tahanan belitan transformator?", "id": "Winding resistance test." },
  { "en": "Tujuan uji tahanan belitan?", "id": "Memeriksa kualitas sambungan internal." },
  { "en": "Uji yang menyuntikkan tegangan tinggi ke peralatan?", "id": "Hi-Pot (High Potential) test." },
  { "en": "Tujuan Hi-Pot (High Potential) test?", "id": "Memastikan kekuatan isolasi." },
  { "en": "Analisis gas terlarut dalam oli trafo?", "id": "DGA (Dissolved Gas Analysis)." },
  { "en": "Tujuan DGA (Dissolved Gas Analysis)?", "id": "Mendeteksi gangguan internal trafo." },
  { "en": "Gas kunci untuk indikasi arcing/sparking?", "id": "Asetilena (C2H2)." },
  { "en": "Gas kunci untuk indikasi overheating?", "id": "Etilena (C2H4) dan Etana (C2H6)." },
  { "en": "Gas kunci untuk indikasi korona?", "id": "Hidrogen (H2)." },
  { "en": "Pengujian faktor daya (tan delta) pada isolasi?", "id": "Untuk mengetahui kualitas dan kontaminasi." },
  { "en": "Nilai faktor daya isolasi yang baik?", "id": "Sangat rendah." },
  { "en": "Apa itu 'power system stabilizer' (PSS)?", "id": "Peredam osilasi daya." },
  { "en": "PSS (Power System Stabilizer) bekerja melalui?", "id": "Sistem eksitasi generator." },
  { "en": "Osilasi frekuensi rendah pada sistem tenaga?", "id": "Electromechanical oscillations." },
  { "en": "Frekuensi osilasi lokal (local mode)?", "id": "1-2 Hz." },
  { "en": "Frekuensi osilasi antar area (inter-area mode)?", "id": "Di bawah 1 Hz." }



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
