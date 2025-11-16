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


  { "en": "Apa Nama Ibukota Negara Indonesia?", "id": "Jakarta." },
  { "en": "Apa Warna Bendera Negara Indonesia?", "id": "Merah Dan Putih." },
  { "en": "Siapa Nama Presiden Pertama Republik Indonesia?", "id": "Soekarno." },
  { "en": "Lagu Kebangsaan Negara Indonesia Adalah?", "id": "Indonesia Raya." },
  { "en": "Apa Nama Semboyan Negara Indonesia?", "id": "Bhinneka Tunggal Ika." },
  { "en": "Apa Nama Mata Uang Negara Indonesia?", "id": "Rupiah." },
  { "en": "Kapan Hari Kemerdekaan Republik Indonesia?", "id": "17 Agustus 1945." },
  { "en": "Apa Nama Lambang Negara Indonesia?", "id": "Garuda Pancasila." },
  { "en": "Dimana Letak Candi Borobudur Berada?", "id": "Magelang, Jawa Tengah." },
  { "en": "Apa Nama Pulau Terbesar Di Indonesia?", "id": "Papua." },
  { "en": "Apa Nama Samudra Yang Mengapit Indonesia?", "id": "Hindia Dan Pasifik." },
  { "en": "Berapa Jumlah Provinsi Di Negara Indonesia?", "id": "38 Provinsi." },
  { "en": "Siapa Yang Menjahit Bendera Merah Putih?", "id": "Fatmawati." },
  { "en": "Siapa Yang Menciptakan Lagu Indonesia Raya?", "id": "Wage Rudolf Soepratman." },
  { "en": "Apa Nama Bunga Nasional Negara Indonesia?", "id": "Melati Putih." },
  { "en": "Apa Nama Hewan Nasional Negara Indonesia?", "id": "Komodo." },
  { "en": "Gunung Tertinggi Di Indonesia Adalah Gunung?", "id": "Puncak Jaya, Papua." },
  { "en": "Danau Terbesar Di Indonesia Adalah Danau?", "id": "Toba, Sumatera Utara." },
  { "en": "Sungai Terpanjang Di Indonesia Adalah Sungai?", "id": "Kapuas, Kalimantan Barat." },
  { "en": "Apa Makanan Khas Daerah Padang?", "id": "Rendang." },
  { "en": "Apa Alat Musik Tradisional Jawa Tengah?", "id": "Gamelan." },
  { "en": "Tarian Tradisional Dari Bali Yang Terkenal?", "id": "Kecak Dan Pendet." },
  { "en": "Rumah Adat Provinsi Sumatera Barat Adalah?", "id": "Rumah Gadang." },
  { "en": "Apa Nama Pahlawan Nasional Dari Aceh?", "id": "Cut Nyak Dien." },
  { "en": "Siapa Bapak Pendidikan Nasional Indonesia?", "id": "Ki Hadjar Dewantara." },
  { "en": "Organisasi Pergerakan Nasional Pertama Di Indonesia?", "id": "Budi Utomo." },
  { "en": "Apa Isi Teks Sumpah Pemuda?", "id": "Satu Tanah Air, Satu Bangsa, Dan Satu Bahasa." },
  { "en": "Peristiwa Penting Sebelum Proklamasi Kemerdekaan Indonesia?", "id": "Rengasdengklok." },
  { "en": "Dimana Teks Proklamasi Kemerdekaan Dibacakan?", "id": "Jalan Pegangsaan Timur Nomor 56." },
  { "en": "Badan Penyelidik Usaha-Usaha Persiapan Kemerdekaan Indonesia (BPUPKI) Diketuai?", "id": "Radjiman Wedyodiningrat." },
  { "en": "Panitia Persiapan Kemerdekaan Indonesia (PPKI) Diketuai?", "id": "Soekarno." },
  { "en": "Berapa Jumlah Sila Dalam Pancasila?", "id": "5 Sila." },
  { "en": "Sila Pertama Pancasila Berbunyi Apa?", "id": "Ketuhanan Yang Maha Esa." },
  { "en": "Apa Lambang Sila Pertama Pancasila?", "id": "Bintang." },
  { "en": "Apa Bunyi Sila Kedua Pancasila?", "id": "Kemanusiaan Yang Adil Dan Beradab." },
  { "en": "Apa Lambang Sila Kedua Pancasila?", "id": "Rantai." },
  { "en": "Apa Bunyi Sila Ketiga Pancasila?", "id": "Persatuan Indonesia." },
  { "en": "Apa Lambang Sila Ketiga Pancasila?", "id": "Pohon Beringin." },
  { "en": "Apa Bunyi Sila Keempat Pancasila?", "id": "Kerakyatan Yang Dipimpin Oleh Hikmat." },
  { "en": "Apa Lambang Sila Keempat Pancasila?", "id": "Kepala Banteng." },
  { "en": "Apa Bunyi Sila Kelima Pancasila?", "id": "Keadilan Sosial Bagi Seluruh Rakyat." },
  { "en": "Apa Lambang Sila Kelima Pancasila?", "id": "Padi Dan Kapas." },
  { "en": "Undang-Undang Dasar (UUD) 1945 Disahkan Kapan?", "id": "18 Agustus 1945." },
  { "en": "Planet Terbesar Dalam Tata Surya Kita?", "id": "Jupiter." },
  { "en": "Planet Terdekat Dengan Matahari Adalah Planet?", "id": "Merkurius." },
  { "en": "Planet Yang Memiliki Cincin Indah?", "id": "Saturnus." },
  { "en": "Benda Langit Yang Memancarkan Cahaya Sendiri?", "id": "Bintang." },
  { "en": "Pusat Tata Surya Kita Adalah?", "id": "Matahari." },
  { "en": "Manusia Bernapas Menggunakan Organ Apa?", "id": "Paru-Paru." },
  { "en": "Ikan Bernapas Menggunakan Organ Apa?", "id": "Insang." },
  { "en": "Air Mendidih Pada Suhu Berapa Derajat?", "id": "100 Derajat Celcius." },
  { "en": "Proses Tumbuhan Membuat Makanan Disebut Apa?", "id": "Fotosintesis." },
  { "en": "Gas Yang Dihirup Manusia Saat Bernapas?", "id": "Oksigen (O2)." },
  { "en": "Gas Yang Dikeluarkan Manusia Saat Bernapas?", "id": "Karbon Dioksida (CO2)." },
  { "en": "Hewan Yang Dapat Hidup Di Darat Air?", "id": "Amfibi." },
  { "en": "Hewan Pemakan Daging Disebut Dengan Apa?", "id": "Karnivora." },
  { "en": "Hewan Pemakan Tumbuhan Disebut Dengan Apa?", "id": "Herbivora." },
  { "en": "Hewan Pemakan Segala Disebut Dengan Apa?", "id": "Omnivora." },
  { "en": "Indra Manusia Untuk Melihat Adalah Apa?", "id": "Mata." },
  { "en": "Indra Manusia Untuk Mendengar Adalah Apa?", "id": "Telinga." },
  { "en": "Indra Manusia Untuk Merasa Adalah Apa?", "id": "Kulit." },
  { "en": "Indra Manusia Untuk Mengecap Adalah Apa?", "id": "Lidah." },
  { "en": "Indra Manusia Untuk Mencium Adalah Apa?", "id": "Hidung." },
  { "en": "Berapa Jumlah Tulang Pada Manusia Dewasa?", "id": "Sekitar 206 Tulang." },
  { "en": "Vitamin Apa Yang Baik Untuk Mata?", "id": "Vitamin A." },
  { "en": "Sumber Energi Terbesar Di Muka Bumi?", "id": "Matahari." },
  { "en": "Bahan Dasar Pembuatan Kaca Adalah Apa?", "id": "Pasir Silika." },
  { "en": "Logam Paling Ringan Di Dunia Adalah?", "id": "Litium." },
  { "en": "Negara Dengan Julukan Negeri Tirai Bambu?", "id": "Tiongkok." },
  { "en": "Negara Dengan Julukan Negeri Matahari Terbit?", "id": "Jepang." },
  { "en": "Negara Dengan Julukan Negeri Kincir Angin?", "id": "Belanda." },
  { "en": "Benua Terluas Di Dunia Adalah Benua?", "id": "Asia." },
  { "en": "Benua Terkecil Di Dunia Adalah Benua?", "id": "Australia." },
  { "en": "Samudra Terluas Di Dunia Adalah Samudra?", "id": "Pasifik." },
  { "en": "Gurun Terluas Di Dunia Adalah Gurun?", "id": "Antartika." },
  { "en": "Air Terjun Tertinggi Di Dunia Adalah?", "id": "Air Terjun Angel, Venezuela." },
  { "en": "Bangunan Tertinggi Di Dunia Saat Ini?", "id": "Burj Khalifa, Dubai." },
  { "en": "Apa Nama Ibukota Negara Amerika Serikat?", "id": "Washington, D.C." },
  { "en": "Apa Nama Ibukota Negara Inggris?", "id": "London." },
  { "en": "Apa Nama Ibukota Negara Perancis?", "id": "Paris." },
  { "en": "Apa Nama Ibukota Negara Jepang?", "id": "Tokyo." },
  { "en": "Apa Nama Ibukota Negara Mesir?", "id": "Kairo." },
  { "en": "Siapa Penemu Benua Amerika Pertama Kali?", "id": "Christopher Columbus." },
  { "en": "Siapa Penemu Lampu Pijar Yang Terkenal?", "id": "Thomas Alva Edison." },
  { "en": "Siapa Penemu Telepon Pertama Kali?", "id": "Alexander Graham Bell." },
  { "en": "Teori Relativitas Ditemukan Oleh Siapa?", "id": "Albert Einstein." },
  { "en": "Apa Nama Satelit Alami Planet Bumi?", "id": "Bulan." },
  { "en": "Berapa Lama Bumi Berotasi Pada Porosnya?", "id": "24 Jam." },
  { "en": "Berapa Lama Bumi Berevolusi Mengelilingi Matahari?", "id": "365 Hari." },
  { "en": "Pergantian Siang Dan Malam Disebabkan Oleh?", "id": "Rotasi Bumi." },
  { "en": "Pergantian Musim Di Bumi Disebabkan Oleh?", "id": "Revolusi Bumi." },
  { "en": "Warna Primer Terdiri Dari Warna Apa?", "id": "Merah, Kuning, Dan Biru." },
  { "en": "Campuran Warna Merah Dan Kuning Menghasilkan?", "id": "Oranye." },
  { "en": "Campuran Warna Kuning Dan Biru Menghasilkan?", "id": "Hijau." },
  { "en": "Campuran Warna Biru Dan Merah Menghasilkan?", "id": "Ungu." },
  { "en": "Berapa Jumlah Huruf Dalam Alfabet Latin?", "id": "26 Huruf." },
  { "en": "Angka Romawi Untuk Angka Seratus Adalah?", "id": "C." },
  { "en": "Siapa Nama Wakil Presiden Pertama Republik Indonesia?", "id": "Mohammad Hatta." },
  { "en": "Di Pulau Manakah Letak Ibukota Jakarta?", "id": "Pulau Jawa." },
  { "en": "Apa Sebutan Untuk Garis Khayal Di Tengah Bumi?", "id": "Garis Khatulistiwa." },
  { "en": "Berapa Jumlah Kaki Pada Serangga Normalnya?", "id": "6 Kaki." },
  { "en": "Siapa Manusia Pertama Yang Pergi Ke Luar Angkasa?", "id": "Yuri Gagarin." },
  { "en": "Apa Nama Benua Yang Paling Dingin?", "id": "Benua Antartika." },
  { "en": "Palung Laut Terdalam Di Dunia Adalah?", "id": "Palung Mariana." },
  { "en": "Senjata Tradisional Dari Daerah Aceh Adalah?", "id": "Rencong." },
  { "en": "Apa Akronim Dari Perserikatan Bangsa-Bangsa?", "id": "PBB." },
  { "en": "Dimana Letak Menara Eiffel Yang Terkenal?", "id": "Paris, Perancis." },
  { "en": "Apa Nama Ilmiah Untuk Manusia Modern?", "id": "Homo Sapiens." },
  { "en": "Planet Yang Dijuluki Sebagai Planet Merah?", "id": "Mars." },
  { "en": "Komponen Darah Yang Berfungsi Membawa Oksigen?", "id": "Sel Darah Merah." },
  { "en": "Siapa Yang Menulis Naskah Proklamasi Kemerdekaan?", "id": "Soekarno, Hatta, Achmad Soebardjo." },
  { "en": "Apa Sebutan Untuk Anak Laki-Laki Raja?", "id": "Pangeran." },
  { "en": "Berapa Jumlah Pemain Dalam Satu Tim Sepak Bola?", "id": "11 Pemain." },
  { "en": "Siapa Penemu Hukum Gravitasi Yang Terkenal?", "id": "Isaac Newton." },
  { "en": "Apa Nama Lautan Terkecil Di Dunia?", "id": "Samudra Arktik." },
  { "en": "Apa Bahasa Resmi Negara Brasil?", "id": "Bahasa Portugis." },
  { "en": "Dari Negara Manakah Kesenian Bela Diri Karate?", "id": "Jepang." },
  { "en": "Apa Nama Tokoh Utama Dalam Cerita Ramayana?", "id": "Rama Dan Sinta." },
  { "en": "Berapa Jumlah Sisi Pada Bangun Datar Segitiga?", "id": "3 Sisi." },
  { "en": "Proses Perubahan Uap Air Menjadi Air Disebut?", "id": "Kondensasi." },
  { "en": "Mamalia Laut Terbesar Di Dunia Adalah?", "id": "Paus Biru." },
  { "en": "Siapa Yang Dijuluki Bapak Proklamator Indonesia?", "id": "Soekarno Dan Hatta." },
  { "en": "Apa Nama Rumah Adat Khas Suku Eskimo?", "id": "Igloo." },
  { "en": "Apa Satuan Dasar Untuk Mengukur Suhu?", "id": "Kelvin." },
  { "en": "Di Benua Manakah Sungai Nil Berada?", "id": "Benua Afrika." },
  { "en": "Penyakit Demam Berdarah Disebabkan Oleh Gigitan Nyamuk?", "id": "Aedes Aegypti." },
  { "en": "Siapakah Dewi Padi Dalam Mitologi Jawa?", "id": "Dewi Sri." },
  { "en": "Berapa Jumlah Huruf Dalam Aksara Jawa?", "id": "20 Huruf." },
  { "en": "Negara Manakah Yang Berbentuk Semenanjung Besar?", "id": "India." },
  { "en": "Apa Nama Bahan Bakar Untuk Pesawat Terbang?", "id": "Avtur." },
  { "en": "Siapakah Nama Dewa Matahari Dalam Mitologi Mesir?", "id": "Dewa Ra." },
  { "en": "Berapa Tahun Dalam Satu Dekade?", "id": "10 Tahun." },
  { "en": "Apa Warna Dominan Pada Taksi Di New York?", "id": "Kuning." },
  { "en": "Siapa Penulis Novel Terkenal Laskar Pelangi?", "id": "Andrea Hirata." },
  { "en": "Organisme Yang Dapat Membuat Makanan Sendiri Disebut?", "id": "Autotrof." },
  { "en": "Negara Dengan Penduduk Terbanyak Di Dunia Adalah?", "id": "India." },
  { "en": "Apa Nama Tarian Selamat Datang Dari Papua?", "id": "Tari Selamat Datang." },
  { "en": "Unsur Kimia Apa Yang Melambangkan Emas?", "id": "Au (Aurum)." },
  { "en": "Siapakah Nama Pahlawan Yang Tertera Di Uang Rp100.000?", "id": "Soekarno Dan Hatta." },
  { "en": "Apa Sebutan Untuk Garis Lintang 0 Derajat?", "id": "Khatulistiwa." },
  { "en": "Berapa Derajat Sudut Siku-Siku Itu?", "id": "90 Derajat." },
  { "en": "Apa Ibukota Dari Negara Bagian California?", "id": "Sacramento." },
  { "en": "Apa Sebutan Untuk Olahraga Menggunakan Papan Seluncur?", "id": "Skateboarding." },
  { "en": "Siapa Yang Mempopulerkan Teori Evolusi Manusia?", "id": "Charles Darwin." },
  { "en": "Apa Nama Kerajaan Hindu Pertama Di Indonesia?", "id": "Kerajaan Kutai." },
  { "en": "Pada Tanggal Berapa Hari Kartini Diperingati?", "id": "21 April." },
  { "en": "Hewan Darat Tercepat Di Dunia Adalah?", "id": "Cheetah." },
  { "en": "Apa Sebutan Untuk Perpindahan Panas Melalui Zat?", "id": "Konduksi." },
  { "en": "Siapa Nama Istri Dari Presiden Soekarno?", "id": "Fatmawati." },
  { "en": "Negara Mana Yang Terkenal Dengan Piramida?", "id": "Mesir." },
  { "en": "Berapa Jumlah Hari Dalam Bulan Februari Kabisat?", "id": "29 Hari." },
  { "en": "Arah Mata Angin Antara Utara Dan Timur?", "id": "Timur Laut." },
  { "en": "Apa Sebutan Untuk Pusat Kegiatan Ekonomi Negara?", "id": "Ibu Kota." },
  { "en": "Karya Sastra Terkenal Gubahan Empu Tantular Adalah?", "id": "Kakawin Sutasoma." },
  { "en": "Siapa Kaisar Terakhir Dari Negara Tiongkok?", "id": "Puyi." },
  { "en": "Apa Nama Bahan Keras Pelindung Gigi?", "id": "Email Gigi." },
  { "en": "Berapa Jumlah Warna Dalam Pelangi?", "id": "7 Warna." },
  { "en": "Organisasi Kesehatan Dunia Disingkat Menjadi Apa?", "id": "WHO." },
  { "en": "Siapa Presiden Amerika Serikat Yang Ke-16?", "id": "Abraham Lincoln." },
  { "en": "Negara Manakah Yang Merupakan Tempat Lahir Pizza?", "id": "Italia." },
  { "en": "Apa Nama Angin Yang Bertiup Dari Darat?", "id": "Angin Darat." },
  { "en": "Siapa Dewa Perang Dalam Mitologi Yunani Kuno?", "id": "Ares." },
  { "en": "Apa Istilah Untuk Studi Tentang Bintang?", "id": "Astronomi." },
  { "en": "Apa Nama Alat Untuk Mengukur Gempa Bumi?", "id": "Seismograf." },
  { "en": "Rempah Asli Indonesia Yang Sangat Terkenal?", "id": "Cengkeh Dan Pala." },
  { "en": "Apa Sebutan Untuk Tulang Terpanjang Manusia?", "id": "Tulang Paha." },
  { "en": "Berapa Jumlah Tuts Putih Pada Sebuah Piano?", "id": "52 Tuts." },
  { "en": "Siapa Penemu Benua Australia Yang Terkenal?", "id": "James Cook." },
  { "en": "Apa Mata Uang Yang Digunakan Di Korea Selatan?", "id": "Won Korea Selatan." },
  { "en": "Apa Nama Patung Singa Berkepala Manusia Mesir?", "id": "Sphinx." },
  { "en": "Gelar Untuk Lulusan Strata Satu Adalah?", "id": "Sarjana." },
  { "en": "Apa Sebutan Untuk Zaman Batu Tua?", "id": "Zaman Paleolitikum." },
  { "en": "Apa Nama Zat Hijau Daun Pada Tumbuhan?", "id": "Klorofil." },
  { "en": "Pada Tahun Berapa Perang Dunia Pertama Dimulai?", "id": "Tahun 1914." },
  { "en": "Siapa Nama Tokoh Wayang Terkuat Pandawa?", "id": "Bima Atau Werkudara." },
  { "en": "Apa Sebutan Untuk Pemimpin Negara Monarki?", "id": "Raja Atau Ratu." },
  { "en": "Dimana Lokasi Tembok Besar Yang Terkenal?", "id": "Tiongkok." },
  { "en": "Apa Akronim Untuk Asam Deoksiribonukleat?", "id": "DNA." },
  { "en": "Berapa Jumlah Benua Yang Ada Di Dunia?", "id": "7 Benua." },
  { "en": "Apa Nama Ibu Kota Negara Australia?", "id": "Canberra." },
  { "en": "Siapa Sutradara Film Terkenal Titanic?", "id": "James Cameron." },
  { "en": "Apa Sebutan Untuk Bayi Kanguru?", "id": "Joey." },
  { "en": "Siapa Yang Terkenal Dengan Lukisan Monalisa?", "id": "Leonardo Da Vinci." },
  { "en": "Apa Nama Perusahaan Teknologi Yang Didirikan Bill Gates?", "id": "Microsoft." },
  { "en": "Paus Pembunuh Sebenarnya Termasuk Jenis Hewan Apa?", "id": "Lumba-Lumba." },
  { "en": "Dimana Letak Jembatan Golden Gate Yang Ikonik?", "id": "San Francisco, Amerika." },
  { "en": "Siapa Penulis Seri Buku Harry Potter?", "id": "J.K. Rowling." },
  { "en": "Apa Nama Bahasa Yang Paling Banyak Digunakan?", "id": "Bahasa Mandarin." },
  { "en": "Berapa Banyak Jantung Yang Dimiliki Gurita?", "id": "3 Jantung." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Cuaca?", "id": "Meteorologi." },
  { "en": "Siapa Yang Dijuluki Sebagai Raja Pop Dunia?", "id": "Michael Jackson." },
  { "en": "Apa Nama Makanan Pokok Sebagian Besar Orang Asia?", "id": "Nasi." },
  { "en": "Hewan Apa Yang Dapat Tidur Sambil Berdiri?", "id": "Kuda Dan Gajah." },
  { "en": "Apa Sebutan Untuk Lapisan Terluar Planet Bumi?", "id": "Kerak Bumi." },
  { "en": "Siapakah Penemu Mesin Uap Yang Efisien?", "id": "James Watt." },
  { "en": "Apa Nama Gurun Pasir Terbesar Di Dunia?", "id": "Gurun Sahara." },
  { "en": "Berapa Jumlah Negara Anggota Asean Saat Ini?", "id": "10 Negara." },
  { "en": "Apa Nama Ibukota Negara Thailand?", "id": "Bangkok." },
  { "en": "Siapakah Yang Dijuluki Sebagai Bapak Evolusi?", "id": "Charles Darwin." },
  { "en": "Apa Nama Sungai Terpanjang Di Dunia?", "id": "Sungai Nil." },
  { "en": "Berapa Jumlah Sisi Pada Bangun Datar Persegi?", "id": "4 Sisi." },
  { "en": "Negara Manakah Yang Terkenal Dengan Menara Pisa?", "id": "Italia." },
  { "en": "Apa Sebutan Untuk Dokter Spesialis Hewan?", "id": "Veteriner." },
  { "en": "Siapa Yang Menemukan Tabel Periodik Unsur Kimia?", "id": "Dmitri Mendeleev." },
  { "en": "Apa Nama Benua Yang Dijuluki Benua Hitam?", "id": "Benua Afrika." },
  { "en": "Kapan Peringatan Hari Pendidikan Nasional Di Indonesia?", "id": "2 Mei." },
  { "en": "Apa Nama Bahan Yang Tidak Dapat Menghantarkan Listrik?", "id": "Isolator." },
  { "en": "Siapa Nama Presiden Amerika Serikat Pertama?", "id": "George Washington." },
  { "en": "Berapa Jumlah Lubang Pada Lapangan Golf Standar?", "id": "18 Lubang." },
  { "en": "Apa Nama Mata Uang Negara Malaysia?", "id": "Ringgit." },
  { "en": "Dari Manakah Asal Tarian Samba Yang Enerjik?", "id": "Brasil." },
  { "en": "Proses Penguapan Air Ke Udara Disebut Apa?", "id": "Evaporasi." },
  { "en": "Siapakah Penulis Naskah Drama Romeo Dan Juliet?", "id": "William Shakespeare." },
  { "en": "Apa Nama Lapisan Pelindung Bumi Dari Sinar Uv?", "id": "Lapisan Ozon." },
  { "en": "Agama Apa Yang Memiliki Kitab Suci Weda?", "id": "Agama Hindu." },
  { "en": "Apa Sebutan Untuk Garis Bujur 0 Derajat?", "id": "Garis Meridian Greenwich." },
  { "en": "Berapa Jumlah Oseania Atau Samudra Di Dunia?", "id": "5 Samudra." },
  { "en": "Siapakah Ratu Mesir Kuno Yang Sangat Terkenal?", "id": "Cleopatra." },
  { "en": "Apa Akronim Untuk North Atlantic Treaty Organization?", "id": "NATO." },
  { "en": "Di Kota Manakah Markas Besar PBB Berada?", "id": "New York, Amerika." },
  { "en": "Binatang Apa Yang Menjadi Lambang Negara Tiongkok?", "id": "Panda Raksasa." },
  { "en": "Apa Nama Permainan Papan Strategi Dengan Raja?", "id": "Catur." },
  { "en": "Siapa Dewa Laut Dalam Mitologi Yunani Kuno?", "id": "Poseidon." },
  { "en": "Berapa Menit Dalam Satu Jam Waktu?", "id": "60 Menit." },
  { "en": "Apa Sebutan Untuk Benda Langit Berkilau Berekor?", "id": "Komet." },
  { "en": "Negara Manakah Yang Beribukota Di Moskow?", "id": "Rusia." },
  { "en": "Apa Nama Alat Musik Yang Dipetik Dari Hawaii?", "id": "Ukulele." },
  { "en": "Siapakah Ilmuwan Penemu Penisilin Pertama Kali?", "id": "Alexander Fleming." },
  { "en": "Apa Sebutan Untuk Garis Vertikal Di Peta?", "id": "Garis Bujur." },
  { "en": "Dalam Satu Tahun Ada Berapa Minggu?", "id": "52 Minggu." },
  { "en": "Jenis Tulang Rawan Yang Ada Di Hidung?", "id": "Tulang Rawan Hialin." },
  { "en": "Negara Manakah Yang Memiliki Bendera Union Jack?", "id": "Inggris Raya." },
  { "en": "Siapakah Yang Melukis Langit-Langit Kapel Sistina?", "id": "Michelangelo." },
  { "en": "Apa Gas Utama Pembentuk Atmosfer Planet Bumi?", "id": "Nitrogen." },
  { "en": "Apa Nama Pulau Terbesar Di Dunia?", "id": "Greenland." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Genetika Modern?", "id": "Gregor Mendel." },
  { "en": "Apa Nama Cabang Biologi Tentang Serangga?", "id": "Entomologi." },
  { "en": "Berapa Jumlah Titik Pada Sebuah Dadu Standar?", "id": "21 Titik." },
  { "en": "Apa Nama Ibukota Negara Kanada?", "id": "Ottawa." },
  { "en": "Siapa Nama Tokoh Detektif Fiksi Terkenal?", "id": "Sherlock Holmes." },
  { "en": "Penyakit Kuning Disebabkan Oleh Tingginya Kadar Apa?", "id": "Bilirubin." },
  { "en": "Apa Nama Tembok Yang Memisahkan Berlin Dulu?", "id": "Tembok Berlin." },
  { "en": "Indra Apa Yang Digunakan Ular Untuk Mencium?", "id": "Lidah." },
  { "en": "Siapakah Pematung Terkenal Yang Membuat Patung David?", "id": "Michelangelo." },
  { "en": "Berapa Jumlah Senar Pada Gitar Akustik Standar?", "id": "6 Senar." },
  { "en": "Dimana Ajang Olimpiade Modern Pertama Kali Diadakan?", "id": "Athena, Yunani." },
  { "en": "Apa Sebutan Untuk Massa Es Yang Bergerak Lambat?", "id": "Gletser." },
  { "en": "Siapakah Perdana Menteri Wanita Pertama Di Dunia?", "id": "Sirimavo Bandaranaike." },
  { "en": "Apa Nama Samudra Yang Mengelilingi Benua Antartika?", "id": "Samudra Selatan." },
  { "en": "Berapa Jumlah Bilangan Prima Di Bawah Angka 10?", "id": "4 Bilangan." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Jamur?", "id": "Mikologi." },
  { "en": "Negara Mana Yang Menjadi Asal Mula Kertas?", "id": "Tiongkok." },
  { "en": "Siapa Nama Pemimpin Uni Soviet Yang Terkenal?", "id": "Joseph Stalin." },
  { "en": "Apa Nama Unit Dasar Kehidupan Suatu Organisme?", "id": "Sel." },
  { "en": "Pegunungan Terpanjang Di Dunia Adalah Pegunungan?", "id": "Pegunungan Andes." },
  { "en": "Siapakah Yang Menemukan Struktur Heliks Ganda DNA?", "id": "Watson Dan Crick." },
  { "en": "Apa Sebutan Untuk Seni Menulis Indah?", "id": "Kaligrafi." },
  { "en": "Berapa Jumlah Huruf Vokal Dalam Alfabet Latin?", "id": "5 Huruf." },
  { "en": "Apa Satuan Ukuran Untuk Intensitas Suara?", "id": "Desibel." },
  { "en": "Siapa Nama Firaun Muda Yang Makamnya Ditemukan?", "id": "Tutankhamun." },
  { "en": "Apa Nama Makanan Khas Negara Jepang?", "id": "Sushi." },
  { "en": "Hewan Mamalia Apa Yang Dapat Terbang?", "id": "Kelelawar." },
  { "en": "Apa Akronim Untuk Central Processing Unit?", "id": "CPU." },
  { "en": "Berapa Sisi Yang Dimiliki Oleh Sebuah Kubus?", "id": "6 Sisi." },
  { "en": "Apa Nama Ibukota Negara Korea Selatan?", "id": "Seoul." },
  { "en": "Siapakah Filsuf Yunani Kuno Guru Alexander Agung?", "id": "Aristoteles." },
  { "en": "Apa Istilah Untuk Studi Tentang Batuan?", "id": "Petrologi." },
  { "en": "Burung Apa Yang Tidak Bisa Terbang?", "id": "Burung Unta, Pinguin." },
  { "en": "Apa Ibukota Provinsi Jawa Barat?", "id": "Bandung." },
  { "en": "Siapakah Dewi Kebijaksanaan Dalam Mitologi Yunani Kuno?", "id": "Athena." },
  { "en": "Apa Elemen Paling Umum Di Alam Semesta?", "id": "Hidrogen." },
  { "en": "Berapa Banyak Gelar Juara Dunia Valentino Rossi?", "id": "9 Gelar." },
  { "en": "Apa Sebutan Untuk Pusat Tata Surya Kita?", "id": "Matahari." },
  { "en": "Siapa Kaisar Romawi Pertama Yang Berkuasa?", "id": "Kaisar Augustus." },
  { "en": "Apa Sebutan Untuk Perpindahan Penduduk Antar Negara?", "id": "Migrasi." },
  { "en": "Dimana Lokasi Patung Liberty Yang Terkenal?", "id": "New York, Amerika." },
  { "en": "Berapa Jumlah Tulang Rusuk Pada Manusia Normal?", "id": "24 Tulang." },
  { "en": "Apa Nama Benua Yang Hilang Menurut Legenda?", "id": "Benua Atlantis." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Komputer Modern?", "id": "Alan Turing." },
  { "en": "Apa Sebutan Untuk Pemanasan Global Secara Umum?", "id": "Global Warming." },
  { "en": "Berapa Jumlah Zona Waktu Resmi Di Rusia?", "id": "11 Zona Waktu." },
  { "en": "Apa Nama Alat Untuk Melihat Benda Sangat Kecil?", "id": "Mikroskop." },
  { "en": "Siapakah Astronot Pertama Yang Mendarat Di Bulan?", "id": "Neil Armstrong." },
  { "en": "Apa Sebutan Untuk Tulisan Mesir Kuno?", "id": "Hieroglif." },
  { "en": "Apa Akronim Untuk Light Emitting Diode?", "id": "LED." },
  { "en": "Pada Tahun Berapa Tembok Berlin Runtuh Total?", "id": "Tahun 1989." },
  { "en": "Siapa Pelukis Terkenal Dengan Aliran Kubisme?", "id": "Pablo Picasso." },
  { "en": "Apa Nama Ibu Kota Negara Spanyol?", "id": "Madrid." },
  { "en": "Berapa Jumlah Minimal Pemain Dalam Tim Basket?", "id": "5 Pemain." },
  { "en": "Apa Nama Proses Perubahan Wujud Zat Padat Ke Gas?", "id": "Menyublim." },
  { "en": "Siapakah Yang Menemukan Benua Amerika Secara Resmi?", "id": "Amerigo Vespucci." },
  { "en": "Apa Nama Gurun Yang Ada Di Tiongkok?", "id": "Gurun Gobi." },
  { "en": "Jenis Gula Apa Yang Terdapat Dalam Buah?", "id": "Fruktosa." },
  { "en": "Siapakah Tokoh Utama Dalam Epos Mahabharata?", "id": "Pandawa Dan Kurawa." },
  { "en": "Di Negara Mana Letak Pegunungan Himalaya?", "id": "Nepal, Tiongkok, India." },
  { "en": "Apa Nama Ibukota Negara Vietnam?", "id": "Hanoi." },
  { "en": "Siapakah Yang Dijuluki Wanita Besi Dari Inggris?", "id": "Margaret Thatcher." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Masyarakat?", "id": "Sosiologi." },
  { "en": "Berapa Jumlah Planet Kerdil Yang Diakui Resmi?", "id": "5 Planet Kerdil." },
  { "en": "Negara Manakah Tempat Asal Jam Tangan Rolex?", "id": "Swiss." },
  { "en": "Apa Nama Angin Yang Bertiup Dari Laut?", "id": "Angin Laut." },
  { "en": "Siapakah Yang Menemukan Teori Heliosentris Tata Surya?", "id": "Nicolaus Copernicus." },
  { "en": "Apa Nama Bahan Keras Utama Pembentuk Tulang?", "id": "Kalsium." },
  { "en": "Kapan Peringatan Hari Buruh Internasional Atau May Day?", "id": "1 Mei." },
  { "en": "Apa Nama Proses Darah Membeku Saat Terluka?", "id": "Koagulasi." },
  { "en": "Siapakah Pemimpin Revolusi Bolshevik Di Rusia?", "id": "Vladimir Lenin." },
  { "en": "Berapa Jumlah Tali Pada Alat Musik Biola?", "id": "4 Tali." },
  { "en": "Apa Nama Mata Uang Resmi Negara India?", "id": "Rupee India." },
  { "en": "Dari Benua Manakah Asal Buah Tomat?", "id": "Amerika Selatan." },
  { "en": "Proses Jatuhnya Air Dari Awan Disebut Apa?", "id": "Presipitasi." },
  { "en": "Siapa Penulis Buku The Origin Of Species?", "id": "Charles Darwin." },
  { "en": "Apa Nama Bagian Telinga Untuk Menjaga Keseimbangan?", "id": "Kanal Semisirkularis." },
  { "en": "Agama Manakah Yang Merayakan Hari Raya Waisak?", "id": "Agama Buddha." },
  { "en": "Apa Sebutan Untuk Garis Horizontal Di Peta?", "id": "Garis Lintang." },
  { "en": "Berapa Jumlah Negara Bagian Di Amerika Serikat?", "id": "50 Negara Bagian." },
  { "en": "Siapakah Firaun Yang Membangun Piramida Giza?", "id": "Firaun Khufu." },
  { "en": "Apa Akronim Untuk Association Of Southeast Asian Nations?", "id": "ASEAN." },
  { "en": "Di Kota Manakah Patung Kristus Penebus Berada?", "id": "Rio De Janeiro, Brasil." },
  { "en": "Binatang Apa Yang Menjadi Simbol Partai Republik AS?", "id": "Gajah." },
  { "en": "Apa Nama Olahraga Mendayung Perahu Panjang?", "id": "Dayung." },
  { "en": "Siapa Dewa Tertinggi Dalam Mitologi Romawi Kuno?", "id": "Jupiter." },
  { "en": "Berapa Detik Dalam Satu Jam Waktu?", "id": "3600 Detik." },
  { "en": "Apa Sebutan Untuk Batuan Cair Di Bawah Bumi?", "id": "Magma." },
  { "en": "Negara Manakah Yang Beribukota Di Kairo?", "id": "Mesir." },
  { "en": "Apa Nama Alat Musik Tradisional Dari Sunda?", "id": "Angklung." },
  { "en": "Siapakah Ilmuwan Yang Merumuskan Hukum Gerak?", "id": "Isaac Newton." },
  { "en": "Apa Sebutan Untuk Daerah Kutub Utara Bumi?", "id": "Arktik." },
  { "en": "Dalam Satu Abad Ada Berapa Tahun?", "id": "100 Tahun." },
  { "en": "Jaringan Apa Yang Menghubungkan Otot Ke Tulang?", "id": "Tendon." },
  { "en": "Negara Mana Yang Punya Julukan Negeri Gajah Putih?", "id": "Thailand." },
  { "en": "Siapa Yang Melukis The Starry Night?", "id": "Vincent Van Gogh." },
  { "en": "Apa Gas Yang Diperlukan Untuk Proses Pembakaran?", "id": "Oksigen." },
  { "en": "Apa Nama Selat Yang Memisahkan Asia Dan Amerika?", "id": "Selat Bering." },
  { "en": "Siapa Yang Dikenal Sebagai Bapak Bom Atom?", "id": "Robert Oppenheimer." },
  { "en": "Apa Cabang Matematika Yang Mempelajari Sudut?", "id": "Trigonometri." },
  { "en": "Berapa Jumlah Pemain Dalam Satu Regu Voli?", "id": "6 Pemain." },
  { "en": "Apa Nama Ibukota Negara Filipina?", "id": "Manila." },
  { "en": "Siapa Nama Tokoh Utama Dalam Novel Don Quixote?", "id": "Don Quixote." },
  { "en": "Kekurangan Vitamin C Dapat Menyebabkan Penyakit Apa?", "id": "Skorbut Atau Sariawan." },
  { "en": "Apa Nama Istana Kediaman Ratu Inggris?", "id": "Istana Buckingham." },
  { "en": "Indra Apa Yang Paling Kuat Pada Anjing?", "id": "Indra Penciuman." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Menara Eiffel?", "id": "Gustave Eiffel." },
  { "en": "Berapa Jumlah Langkah Dalam Permainan Catur?", "id": "Tidak Terbatas." },
  { "en": "Dimana Lokasi Colosseum, Arena Gladiator Kuno?", "id": "Roma, Italia." },
  { "en": "Apa Sebutan Untuk Studi Tentang Fosil?", "id": "Paleontologi." },
  { "en": "Siapa Presiden Afrika Selatan Anti-Apartheid Terkenal?", "id": "Nelson Mandela." },
  { "en": "Apa Nama Titik Terendah Di Permukaan Bumi?", "id": "Laut Mati." },
  { "en": "Berapa Jumlah Sisi Pada Bangun Datar Oktagon?", "id": "8 Sisi." },
  { "en": "Apa Sebutan Untuk Ilmu Mempelajari Gempa Bumi?", "id": "Seismologi." },
  { "en": "Negara Mana Yang Menjadi Asal Seni Origami?", "id": "Jepang." },
  { "en": "Siapa Nama Pemimpin Kuba Yang Terkenal Dahulu?", "id": "Fidel Castro." },
  { "en": "Apa Nama Bagian Berwarna Pada Mata Manusia?", "id": "Iris." },
  { "en": "Pegunungan Apa Yang Memisahkan Eropa Dan Asia?", "id": "Pegunungan Ural." },
  { "en": "Siapa Yang Menemukan Vaksin Polio Pertama?", "id": "Jonas Salk." },
  { "en": "Apa Sebutan Untuk Seni Menata Taman Jepang?", "id": "Ikebana." },
  { "en": "Berapa Jumlah Huruf Konsonan Dalam Alfabet Latin?", "id": "21 Huruf." },
  { "en": "Apa Satuan Ukuran Untuk Tekanan Udara?", "id": "Pascal." },
  { "en": "Siapa Nama Ratu Inggris Dengan Masa Jabatan Terlama?", "id": "Ratu Elizabeth II." },
  { "en": "Apa Nama Makanan Khas Negara Meksiko?", "id": "Taco." },
  { "en": "Hewan Darat Terbesar Di Dunia Saat Ini?", "id": "Gajah Afrika." },
  { "en": "Apa Akronim Untuk Random Access Memory?", "id": "RAM." },
  { "en": "Berapa Sudut Total Dalam Sebuah Lingkaran?", "id": "360 Derajat." },
  { "en": "Apa Nama Ibu Kota Negara Jerman?", "id": "Berlin." },
  { "en": "Siapakah Filsuf Yunani Kuno Yang Dihukum Mati?", "id": "Socrates." },
  { "en": "Apa Istilah Untuk Studi Tentang Gunung Berapi?", "id": "Vulkanologi." },
  { "en": "Burung Apa Yang Dapat Meniru Suara Manusia?", "id": "Beo Dan Kakaktua." },
  { "en": "Apa Ibukota Provinsi Jawa Timur?", "id": "Surabaya." },
  { "en": "Siapa Dewa Petir Dalam Mitologi Nordik?", "id": "Thor." },
  { "en": "Apa Zat Yang Membuat Darah Berwarna Merah?", "id": "Hemoglobin." },
  { "en": "Berapa Gelar Juara Dunia Formula 1 Michael Schumacher?", "id": "7 Gelar." },
  { "en": "Apa Sebutan Untuk Pergerakan Lempeng Bumi?", "id": "Tektonik Lempeng." },
  { "en": "Siapa Pemimpin Militer Perancis Yang Terkenal?", "id": "Napoleon Bonaparte." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Bahasa?", "id": "Linguis." },
  { "en": "Dimana Lokasi Kota Kuno Petra Yang Hilang?", "id": "Yordania." },
  { "en": "Berapa Jumlah Bilah Pada Kincir Angin Tradisional?", "id": "4 Bilah." },
  { "en": "Apa Nama Fenomena Optik Di Gurun Pasir?", "id": "Fata Morgana." },
  { "en": "Siapakah Bapak Kedokteran Modern Yang Terkenal?", "id": "Hippocrates." },
  { "en": "Apa Sebutan Untuk Perubahan Iklim Jangka Panjang?", "id": "Perubahan Iklim." },
  { "en": "Berapa Jumlah Pemain Dalam Tim Hoki Es?", "id": "6 Pemain." },
  { "en": "Apa Nama Alat Untuk Mengukur Kecepatan Angin?", "id": "Anemometer." },
  { "en": "Siapakah Pilot Wanita Pertama Yang Melintasi Atlantik?", "id": "Amelia Earhart." },
  { "en": "Apa Sebutan Untuk Garis Keturunan Seorang Raja?", "id": "Dinasti." },
  { "en": "Apa Akronim Untuk Global Positioning System?", "id": "GPS." },
  { "en": "Pada Tahun Berapa Kapal Titanic Tenggelam?", "id": "Tahun 1912." },
  { "en": "Siapa Pelukis Terkenal Beraliran Surealisme?", "id": "Salvador Dali." },
  { "en": "Apa Nama Ibukota Negara Italia?", "id": "Roma." },
  { "en": "Berapa Jumlah Bintang Pada Bendera Amerika Serikat?", "id": "50 Bintang." },
  { "en": "Apa Proses Perubahan Gas Menjadi Cair?", "id": "Mengembun." },
  { "en": "Siapakah Yang Menemukan Benua Afrika Selatan?", "id": "Bartolomeu Dias." },
  { "en": "Apa Nama Gurun Dingin Terbesar Di Dunia?", "id": "Gurun Arktik." },
  { "en": "Jenis Awan Apa Yang Menandakan Hujan Badai?", "id": "Awan Kumulonimbus." },
  { "en": "Siapa Tokoh Wanita Utama Dalam Kisah Perang Troya?", "id": "Helen." },
  { "en": "Di Negara Manakah Letak Gunung Fuji?", "id": "Jepang." },
  { "en": "Apa Nama Ibukota Negara Turki?", "id": "Ankara." },
  { "en": "Siapakah Yang Dijuluki Bapak Kemerdekaan India?", "id": "Mahatma Gandhi." },
  { "en": "Apa Sebutan Untuk Ilmu Mempelajari Tumbuhan?", "id": "Botani." },
  { "en": "Berapa Jumlah Wilayah Waktu Di Seluruh Dunia?", "id": "24 Zona Waktu." },
  { "en": "Negara Manakah Tempat Asal Mula Olimpiade?", "id": "Yunani." },
  { "en": "Apa Nama Unsur Paling Melimpah Di Kerak Bumi?", "id": "Oksigen." },
  { "en": "Siapakah Yang Menemukan Sinar-X Secara Tidak Sengaja?", "id": "Wilhelm Conrad Rontgen." },
  { "en": "Apa Nama Bahan Dasar Pembuatan Plastik Umumnya?", "id": "Minyak Bumi." },
  { "en": "Kapan Peringatan Hari Bumi Sedunia Diperingati?", "id": "22 April." },
  { "en": "Apa Nama Proses Pemecahan Makanan Dalam Tubuh?", "id": "Pencernaan." },
  { "en": "Siapakah Pemimpin Khmer Merah Di Kamboja?", "id": "Pol Pot." },
  { "en": "Berapa Jumlah Titik Sudut Pada Sebuah Piramida?", "id": "Tergantung Alasnya." },
  { "en": "Apa Nama Mata Uang Resmi Negara Swiss?", "id": "Franc Swiss." },
  { "en": "Dari Benua Manakah Asal Tanaman Kopi?", "id": "Afrika, Ethiopia." },
  { "en": "Proses Pelepasan Air Dari Tumbuhan Disebut Apa?", "id": "Transpirasi." },
  { "en": "Siapa Penulis Buku The Wealth Of Nations?", "id": "Adam Smith." },
  { "en": "Apa Nama Hormon Pemicu Rasa Stres?", "id": "Kortisol." },
  { "en": "Agama Manakah Yang Memiliki Tempat Ibadah Vihara?", "id": "Agama Buddha." },
  { "en": "Apa Sebutan Untuk Titik Tertinggi Di Langit?", "id": "Zenit." },
  { "en": "Berapa Jumlah Tulang Di Tengkorak Manusia Dewasa?", "id": "22 Tulang." },
  { "en": "Siapakah Jenderal Kartago Yang Melawan Roma?", "id": "Hannibal Barca." },
  { "en": "Apa Akronim Untuk United Nations Children's Fund?", "id": "UNICEF." },
  { "en": "Di Kota Manakah Terdapat Bangunan Opera House?", "id": "Sydney, Australia." },
  { "en": "Binatang Apa Yang Menjadi Simbol Partai Demokrat AS?", "id": "Keledai." },
  { "en": "Apa Nama Olahraga Menggunakan Raket Dan Kok?", "id": "Bulu Tangkis." },
  { "en": "Siapa Dewa Matahari Dalam Mitologi Yunani Kuno?", "id": "Apollo." },
  { "en": "Berapa Jam Dalam Satu Hari Waktu?", "id": "24 Jam." },
  { "en": "Apa Sebutan Untuk Batuan Cair Di Permukaan Bumi?", "id": "Lava." },
  { "en": "Negara Manakah Yang Beribukota Di Buenos Aires?", "id": "Argentina." },
  { "en": "Apa Nama Alat Musik Gesek Yang Besar?", "id": "Cello Atau Kontrabas." },
  { "en": "Siapakah Ilmuwan Wanita Peraih Dua Nobel Berbeda?", "id": "Marie Curie." },
  { "en": "Apa Sebutan Untuk Daerah Kutub Selatan Bumi?", "id": "Antartika." },
  { "en": "Dalam Satu Milenium Ada Berapa Abad?", "id": "10 Abad." },
  { "en": "Jaringan Apa Yang Menghubungkan Tulang Dengan Tulang?", "id": "Ligamen." },
  { "en": "Negara Mana Yang Punya Julukan Zamrud Khatulistiwa?", "id": "Indonesia." },
  { "en": "Siapa Pelukis Belanda Yang Memotong Telinganya?", "id": "Vincent Van Gogh." },
  { "en": "Apa Gas Yang Membuat Minuman Bersoda Berbusa?", "id": "Karbon Dioksida." },
  { "en": "Apa Nama Terusan Yang Menghubungkan Laut Merah?", "id": "Terusan Suez." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Psikoanalisis?", "id": "Sigmund Freud." },
  { "en": "Apa Cabang Biologi Yang Mempelajari Hewan?", "id": "Zoologi." },
  { "en": "Berapa Jumlah Kartu Dalam Satu Set Remi?", "id": "52 Kartu." },
  { "en": "Apa Nama Ibukota Negara Brasil?", "id": "Brasilia." },
  { "en": "Siapa Nama Tokoh Bajak Laut Legendaris Karibia?", "id": "Jack Sparrow." },
  { "en": "Kekurangan Vitamin B1 Menyebabkan Penyakit Apa?", "id": "Beri-Beri." },
  { "en": "Apa Nama Jembatan Terkenal Di Kota London?", "id": "Tower Bridge." },
  { "en": "Organ Apa Yang Berfungsi Sebagai Pusat Koordinasi?", "id": "Otak." },
  { "en": "Siapakah Arsitek Yang Merancang Katedral St. Basil?", "id": "Postnik Yakovlev." },
  { "en": "Berapa Jumlah Bidak Catur Pada Awal Permainan?", "id": "32 Bidak." },
  { "en": "Dimana Lokasi Situs Kuno Machu Picchu?", "id": "Peru." },
  { "en": "Apa Sebutan Untuk Studi Tentang Bahasa?", "id": "Linguistik." },
  { "en": "Siapa Aktivis Hak Sipil Amerika Yang Terkenal?", "id": "Martin Luther King Jr." },
  { "en": "Apa Nama Danau Air Tawar Terbesar Di Dunia?", "id": "Danau Superior." },
  { "en": "Berapa Jumlah Sisi Pada Bangun Datar Heksagon?", "id": "6 Sisi." },
  { "en": "Apa Sebutan Untuk Ilmu Mempelajari Iklim?", "id": "Klimatologi." },
  { "en": "Negara Mana Yang Menjadi Asal Mula Pasta?", "id": "Italia." },
  { "en": "Siapa Nama Diktator Italia Selama Perang Dunia 2?", "id": "Benito Mussolini." },
  { "en": "Apa Nama Lensa Mata Alami Pada Manusia?", "id": "Lensa Kristalin." },
  { "en": "Gurun Pasir Panas Terbesar Di Benua Amerika?", "id": "Gurun Chihuahua." },
  { "en": "Siapakah Yang Menemukan Teori Kuantum Fisika?", "id": "Max Planck." },
  { "en": "Apa Sebutan Untuk Seni Melipat Kertas Jepang?", "id": "Origami." },
  { "en": "Berapa Jumlah Angka Pada Bilangan Romawi?", "id": "7 Angka." },
  { "en": "Apa Satuan Ukuran Untuk Frekuensi Suara?", "id": "Hertz." },
  { "en": "Siapakah Raja Inggris Yang Terkenal Memiliki 6 Istri?", "id": "Raja Henry VIII." },
  { "en": "Apa Nama Minuman Khas Negara Rusia?", "id": "Vodka." },
  { "en": "Reptil Terbesar Di Dunia Saat Ini?", "id": "Buaya Air Asin." },
  { "en": "Apa Akronim Untuk Read-Only Memory Dalam Komputer?", "id": "ROM." },
  { "en": "Berapa Sudut Lancip Pada Segitiga Siku-Siku?", "id": "Kurang Dari 90 Derajat." },
  { "en": "Apa Nama Ibu Kota Negara Rusia?", "id": "Moskow." },
  { "en": "Siapakah Filsuf Tiongkok Kuno Yang Terkenal?", "id": "Konfusius." },
  { "en": "Apa Istilah Untuk Studi Tentang Virus?", "id": "Virologi." },
  { "en": "Burung Apa Yang Menjadi Simbol Perdamaian?", "id": "Merpati." },
  { "en": "Apa Ibukota Provinsi Sumatera Utara?", "id": "Medan." },
  { "en": "Siapa Dewi Cinta Dalam Mitologi Romawi?", "id": "Venus." },
  { "en": "Apa Zat Yang Memberi Warna Hijau Pada Tumbuhan?", "id": "Klorofil." },
  { "en": "Berapa Gelar Wimbledon Yang Dimenangkan Roger Federer?", "id": "8 Gelar." },
  { "en": "Apa Sebutan Untuk Titik Pusat Gempa Bumi?", "id": "Episentrum." },
  { "en": "Siapa Pendiri Kekaisaran Mongol Yang Agung?", "id": "Genghis Khan." },
  { "en": "Apa Sebutan Untuk Orang Yang Menguasai Banyak Bahasa?", "id": "Poliglot." },
  { "en": "Dimana Lokasi Stonehenge, Monumen Batu Misterius?", "id": "Inggris." },
  { "en": "Berapa Jumlah Pita Pada Bendera Negara Malaysia?", "id": "14 Pita." },
  { "en": "Apa Nama Fenomena Cahaya Di Langit Kutub?", "id": "Aurora." },
  { "en": "Siapakah Yang Dianggap Bapak Sejarah Dunia?", "id": "Herodotus." },
  { "en": "Apa Sebutan Untuk Perubahan Wujud Zat Cair Ke Padat?", "id": "Membeku." },
  { "en": "Berapa Jumlah Pemain Dalam Tim Rugbi?", "id": "15 Pemain." },
  { "en": "Apa Nama Alat Untuk Mengukur Kelembaban Udara?", "id": "Higrometer." },
  { "en": "Siapakah Wanita Pertama Yang Menerbangkan Pesawat Solo?", "id": "Harriet Quimby." },
  { "en": "Apa Sebutan Untuk Sistem Pemerintahan Oleh Rakyat?", "id": "Demokrasi." },
  { "en": "Apa Akronim Untuk World Wide Web?", "id": "WWW." },
  { "en": "Pada Tahun Berapa Perang Dunia Kedua Berakhir?", "id": "Tahun 1945." },
  { "en": "Siapa Seniman Pop Art Terkenal Dari Amerika?", "id": "Andy Warhol." },
  { "en": "Apa Nama Ibukota Negara Portugal?", "id": "Lisbon." },
  { "en": "Berapa Jumlah Garis Pada Bendera Yunani?", "id": "9 Garis." },
  { "en": "Apa Proses Perubahan Cair Menjadi Gas?", "id": "Menguap." },
  { "en": "Siapakah Navigator Portugis Pertama Keliling Dunia?", "id": "Ferdinand Magellan." },
  { "en": "Apa Nama Dataran Tinggi Terluas Di Dunia?", "id": "Dataran Tinggi Tibet." },
  { "en": "Jenis Tulang Rawan Apa Yang Ada Di Telinga?", "id": "Tulang Rawan Elastis." },
  { "en": "Siapa Tokoh Pewayangan Yang Memiliki Anak Gatotkaca?", "id": "Bima." },
  { "en": "Di Negara Mana Letak Candi Angkor Wat?", "id": "Kamboja." },
  { "en": "Apa Nama Ibukota Negara Yunani?", "id": "Athena." },
  { "en": "Siapa Penulis Manifesto Komunis Yang Terkenal?", "id": "Karl Marx." },
  { "en": "Berapa Jumlah Sisi Pada Bangun Datar Heptagon?", "id": "7 Sisi." },
  { "en": "Dari Negara Manakah Asal Mobil Merek Ferrari?", "id": "Italia." },
  { "en": "Apa Nama Unsur Terlarut Terbanyak Di Laut?", "id": "Klorin." },
  { "en": "Siapakah Yang Dianggap Sebagai Penemu Teleskop?", "id": "Hans Lippershey." },
  { "en": "Apa Nama Bahan Baku Utama Pembuatan Semen?", "id": "Batu Kapur." },
  { "en": "Kapan Peringatan Hari Aids (Acquired Immunodeficiency Syndrome) Sedunia?", "id": "1 Desember." },
  { "en": "Apa Nama Proses Pembentukan Sel Telur Manusia?", "id": "Oogenesis." },
  { "en": "Siapakah Pemimpin Revolusi Terkenal Dari Negara Kuba?", "id": "Fidel Castro." },
  { "en": "Berapa Jumlah Pemain Dalam Satu Tim Polo Air?", "id": "7 Pemain." },
  { "en": "Apa Nama Mata Uang Resmi Negara Turki?", "id": "Lira Turki." },
  { "en": "Dari Benua Manakah Tanaman Kakao Berasal?", "id": "Benua Amerika." },
  { "en": "Proses Perubahan Wujud Zat Es Menjadi Air?", "id": "Mencair." },
  { "en": "Siapa Penulis Novel Epik War And Peace?", "id": "Leo Tolstoy." },
  { "en": "Apa Nama Hormon Yang Mengatur Kadar Gula Darah?", "id": "Insulin." },
  { "en": "Agama Manakah Yang Memiliki Kitab Suci Al-Quran?", "id": "Agama Islam." },
  { "en": "Apa Sebutan Untuk Titik Terendah Di Cakrawala?", "id": "Nadir." },
  { "en": "Berapa Jumlah Tulang Pada Telapak Tangan Manusia?", "id": "27 Tulang." },
  { "en": "Siapakah Jenderal Romawi Yang Menaklukkan Galia?", "id": "Julius Caesar." },
  { "en": "Apa Akronim Untuk Organisasi Pendidikan Dan Kebudayaan PBB?", "id": "UNESCO." },
  { "en": "Di Kota Manakah Letak Bangunan Kremlin Moskow?", "id": "Moskow, Rusia." },
  { "en": "Hewan Apa Yang Terkenal Dapat Mengubah Warna Kulit?", "id": "Bunglon." },
  { "en": "Apa Nama Olahraga Berkuda Yang Menggunakan Tongkat?", "id": "Polo." },
  { "en": "Siapa Nama Dewa Dunia Bawah Mitologi Yunani?", "id": "Hades." },
  { "en": "Berapa Kira-Kira Jumlah Hari Dalam Satu Milenium?", "id": "365.242 Hari." },
  { "en": "Apa Sebutan Untuk Letusan Gunung Di Bawah Laut?", "id": "Erupsi Submarin." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Teheran?", "id": "Iran." },
  { "en": "Apa Nama Alat Musik Tiup Khas Skotlandia?", "id": "Bagpipe." },
  { "en": "Siapakah Fisikawan Penemu Fenomena Radioaktivitas?", "id": "Henri Becquerel." },
  { "en": "Apa Nama Tempat Paling Dingin Di Planet Bumi?", "id": "Dataran Tinggi Antartika Timur." },
  { "en": "Satu Lustrum Terdiri Dari Berapa Tahun?", "id": "5 Tahun." },
  { "en": "Jaringan Apa Yang Mengangkut Air Pada Tumbuhan?", "id": "Xilem." },
  { "en": "Negara Apa Yang Dijuluki Negeri Seribu Danau?", "id": "Finlandia." },
  { "en": "Siapa Pelukis Terkenal Beraliran Surealisme Spanyol?", "id": "Salvador Dali." },
  { "en": "Gas Apa Yang Biasa Digunakan Untuk Mengisi Balon?", "id": "Gas Helium." },
  { "en": "Apa Nama Terusan Yang Menghubungkan Samudra Atlantik?", "id": "Terusan Panama." },
  { "en": "Siapakah Yang Dianggap Sebagai Bapak Psikologi Modern?", "id": "Wilhelm Wundt." },
  { "en": "Apa Cabang Ilmu Kedokteran Yang Mempelajari Jantung?", "id": "Kardiologi." },
  { "en": "Berapa Nilai Kartu As Dalam Permainan Blackjack?", "id": "1 Atau 11." },
  { "en": "Siapa Nama Tokoh Vampir Fiksi Dari Transylvania?", "id": "Drakula." },
  { "en": "Kekurangan Vitamin D Dapat Menyebabkan Penyakit Tulang?", "id": "Rakitis." },
  { "en": "Apa Nama Jembatan Ikonik Di San Francisco?", "id": "Jembatan Golden Gate." },
  { "en": "Organ Dalam Tubuh Manusia Yang Menyaring Darah?", "id": "Ginjal." },
  { "en": "Siapakah Arsitek Utama Dari Bangunan Taj Mahal?", "id": "Ustad Ahmad Lahori." },
  { "en": "Berapa Jumlah Ronde Maksimal Pertandingan Tinju Profesional?", "id": "12 Ronde." },
  { "en": "Dimana Lokasi Situs Warisan Dunia Chichen Itza?", "id": "Meksiko." },
  { "en": "Apa Sebutan Untuk Studi Tentang Asal-Usul Kata?", "id": "Etimologi." },
  { "en": "Siapa Presiden Amerika Saat Terjadi Perang Saudara?", "id": "Abraham Lincoln." },
  { "en": "Apa Nama Danau Air Tawar Terdalam Di Dunia?", "id": "Danau Baikal, Siberia." },
  { "en": "Berapa Jumlah Sisi Pada Bangun Datar Nonagon?", "id": "9 Sisi." },
  { "en": "Apa Sebutan Untuk Ilmu Dan Seni Membuat Peta?", "id": "Kartografi." },
  { "en": "Dari Mana Asal Mula Makanan Keju?", "id": "Eropa Prasejarah." },
  { "en": "Siapa Nama Diktator Jerman Pada Era Perang Dunia 2?", "id": "Adolf Hitler." },
  { "en": "Apa Nama Bagian Otak Untuk Berpikir Dan Nalar?", "id": "Korteks Serebral." },
  { "en": "Apa Nama Gurun Terbesar Di Benua Australia?", "id": "Gurun Victoria Besar." },
  { "en": "Siapakah Yang Dianggap Sebagai Penemu Mesin Cetak?", "id": "Johannes Gutenberg." },
  { "en": "Apa Nama Seni Menghias Permukaan Dengan Potongan Kecil?", "id": "Mosaik." },
  { "en": "Huruf Apa Yang Melambangkan Angka 500 Romawi?", "id": "D." },
  { "en": "Apa Satuan Standar Internasional Untuk Daya Listrik?", "id": "Watt." },
  { "en": "Siapakah Raja Makedonia Yang Menaklukkan Kekaisaran Persia?", "id": "Alexander Agung." },
  { "en": "Apa Nama Makanan Fermentasi Khas Dari Korea?", "id": "Kimchi." },
  { "en": "Kelompok Hewan Mamalia Apa Yang Dapat Bertelur?", "id": "Monotremata." },
  { "en": "Apa Akronim Untuk Dana Moneter Internasional?", "id": "IMF (International Monetary Fund)." },
  { "en": "Berapa Besaran Derajat Untuk Sudut Tumpul?", "id": "Lebih Dari 90 Derajat." },
  { "en": "Apa Nama Ibukota Negara Polandia?", "id": "Warsawa." },
  { "en": "Siapakah Filsuf Yang Mengatakan 'Cogito, Ergo Sum'?", "id": "Rene Descartes." },
  { "en": "Apa Nama Cabang Zoologi Yang Mempelajari Serangga?", "id": "Entomologi." },
  { "en": "Jenis Burung Apa Yang Dianggap Paling Cepat?", "id": "Elang Alap-Alap Peregrine." },
  { "en": "Apa Nama Ibukota Provinsi Bali?", "id": "Denpasar." },
  { "en": "Siapa Nama Dewa Perang Dalam Mitologi Romawi?", "id": "Mars." },
  { "en": "Apa Nama Pigmen Yang Memberi Warna Pada Kulit?", "id": "Melanin." },
  { "en": "Berapa Total Medali Emas Olimpiade Michael Phelps?", "id": "23 Medali Emas." },
  { "en": "Apa Titik Tertinggi Di Permukaan Planet Bumi?", "id": "Puncak Gunung Everest." },
  { "en": "Siapakah Pendiri Dari Kekaisaran Ottoman Turki?", "id": "Osman I." },
  { "en": "Apa Sebutan Untuk Ketidakmampuan Melihat Warna?", "id": "Buta Warna." },
  { "en": "Di Mana Lokasi Makam Napoleon Bonaparte?", "id": "Paris, Perancis." },
  { "en": "Berapa Jumlah Senar Pada Gitar Bass Standar?", "id": "4 Senar." },
  { "en": "Kapan Fenomena Gerhana Matahari Total Terjadi?", "id": "Saat Bulan Menutupi Matahari." },
  { "en": "Siapakah Yang Dianggap Bapak Tabel Periodik Modern?", "id": "Dmitri Mendeleev." },
  { "en": "Apa Proses Perubahan Wujud Padat Menjadi Gas?", "id": "Sublimasi." },
  { "en": "Berapa Jumlah Pemain Dalam Satu Tim Bisbol?", "id": "9 Pemain." },
  { "en": "Apa Nama Alat Untuk Mengukur Tekanan Darah?", "id": "Tensimeter Atau Sfigmomanometer." },
  { "en": "Siapakah Kosmonot Wanita Pertama Di Luar Angkasa?", "id": "Valentina Tereshkova." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Satu Orang?", "id": "Monarki Absolut." },
  { "en": "Apa Kepanjangan Dari Akronim VIP (Very Important Person)?", "id": "Orang Yang Sangat Penting." },
  { "en": "Pada Tahun Berapa Perang Vietnam Secara Resmi Berakhir?", "id": "Tahun 1975." },
  { "en": "Siapa Seniman Prancis Pembuat Patung The Thinker?", "id": "Auguste Rodin." },
  { "en": "Apa Nama Ibukota Negara Austria?", "id": "Wina." },
  { "en": "Berapa Jumlah Garis Horizontal Pada Bendera Spanyol?", "id": "3 Garis." },
  { "en": "Apa Nama Proses Peresapan Air Ke Dalam Tanah?", "id": "Infiltrasi." },
  { "en": "Siapa Penjelajah Eropa Pertama Yang Mencapai India?", "id": "Vasco Da Gama." },
  { "en": "Apa Nama Dataran Rendah Terluas Di Dunia?", "id": "Dataran Siberia Barat." },
  { "en": "Apa Nama Hormon Yang Merangsang Pertumbuhan Manusia?", "id": "Somatotropin." },
  { "en": "Siapakah Tokoh Wayang Yang Menjadi Kusir Pandawa?", "id": "Kresna." },
  { "en": "Di Negara Manakah Letak Kota Kuno Timbuktu?", "id": "Mali." },
  { "en": "Apa Sebutan Lain Untuk Bendera Inggris Raya?", "id": "Union Jack." },
  { "en": "Apa Nama Ibukota Negara Selandia Baru?", "id": "Wellington." },
  { "en": "Apa Nama Ibukota Negara Irlandia?", "id": "Dublin." },
  { "en": "Siapa Penulis Buku The Prince Yang Terkenal?", "id": "Niccolo Machiavelli." },
  { "en": "Apa Sebutan Untuk Studi Tentang Virus Dan Penyakitnya?", "id": "Virologi." },
  { "en": "Berapa Jumlah Sisi Pada Bangun Ruang Kubus?", "id": "6 Sisi." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Volvo?", "id": "Swedia." },
  { "en": "Apa Nama Logam Yang Berwujud Cair Pada Suhu Ruang?", "id": "Raksa (Merkuri)." },
  { "en": "Siapakah Yang Menemukan Struktur Atom Pertama Kali?", "id": "John Dalton." },
  { "en": "Apa Bahan Baku Utama Pembuatan Kertas Tradisional?", "id": "Serat Kayu." },
  { "en": "Kapan Hari Palang Merah Sedunia Diperingati?", "id": "8 Mei." },
  { "en": "Apa Nama Proses Pembelahan Sel Menjadi Dua?", "id": "Pembelahan Biner." },
  { "en": "Siapakah Pemimpin Terakhir Uni Soviet Sebelum Bubar?", "id": "Mikhail Gorbachev." },
  { "en": "Berapa Angka Tertinggi Dalam Sekali Lemparan Dart?", "id": "60 (Triple 20)." },
  { "en": "Apa Nama Mata Uang Resmi Negara Arab Saudi?", "id": "Riyal Saudi." },
  { "en": "Dari Benua Manakah Asal Tanaman Kentang?", "id": "Amerika Selatan." },
  { "en": "Proses Perubahan Wujud Benda Dari Gas Ke Padat?", "id": "Mengkristal." },
  { "en": "Siapa Penulis Novel Petualangan Robinson Crusoe?", "id": "Daniel Defoe." },
  { "en": "Apa Nama Hormon Yang Berperan Dalam Kehamilan?", "id": "Progesteron." },
  { "en": "Agama Manakah Yang Merayakan Hari Raya Natal?", "id": "Agama Kristen." },
  { "en": "Apa Sebutan Untuk Jarak Terjauh Planet Dari Matahari?", "id": "Aphelion." },
  { "en": "Berapa Jumlah Tulang Leher Pada Manusia Dan Jerapah?", "id": "7 Tulang." },
  { "en": "Siapakah Kaisar Romawi Yang Memecah Kekaisaran?", "id": "Diocletian." },
  { "en": "Apa Akronim Untuk Food And Agriculture Organization?", "id": "FAO." },
  { "en": "Di Kota Manakah Terdapat Masjid Biru (Blue Mosque)?", "id": "Istanbul, Turki." },
  { "en": "Hewan Apa Yang Memiliki Leher Paling Panjang?", "id": "Jerapah." },
  { "en": "Apa Nama Olahraga Air Dengan Papan Dan Layar?", "id": "Selancar Angin." },
  { "en": "Siapa Nama Dewa Langit Tertinggi Mitologi Mesir?", "id": "Horus." },
  { "en": "Berapa Hari Dalam Satu Tahun Kabisat?", "id": "366 Hari." },
  { "en": "Apa Sebutan Untuk Gelombang Laut Raksasa?", "id": "Tsunami." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Athena?", "id": "Yunani." },
  { "en": "Apa Nama Alat Musik Petik Khas Jepang?", "id": "Shamisen." },
  { "en": "Siapakah Yang Menemukan Bakteri Pertama Kali?", "id": "Antonie Van Leeuwenhoek." },
  { "en": "Apa Nama Titik Terpanas Di Dalam Planet Bumi?", "id": "Inti Dalam." },
  { "en": "Satu Dasawarsa Terdiri Dari Berapa Tahun?", "id": "10 Tahun." },
  { "en": "Jaringan Apa Yang Mengangkut Hasil Fotosintesis?", "id": "Floem." },
  { "en": "Negara Apa Yang Dijuluki Negeri Kanguru?", "id": "Australia." },
  { "en": "Siapa Pelukis Belanda Yang Ahli Melukis Cahaya?", "id": "Rembrandt." },
  { "en": "Gas Apa Yang Memberi Bau Khas Pada Telur Busuk?", "id": "Hidrogen Sulfida." },
  { "en": "Apa Nama Selat Yang Memisahkan Pulau Jawa Dan Bali?", "id": "Selat Bali." },
  { "en": "Siapakah Yang Dianggap Sebagai Bapak Ekonomi Modern?", "id": "Adam Smith." },
  { "en": "Apa Cabang Ilmu Kedokteran Yang Mempelajari Kulit?", "id": "Dermatologi." },
  { "en": "Berapa Jumlah Maksimal Pemain Dalam Kartu Uno?", "id": "10 Pemain." },
  { "en": "Apa Nama Ibukota Negara Argentina?", "id": "Buenos Aires." },
  { "en": "Siapa Nama Tokoh Utama Dalam Novel Moby Dick?", "id": "Kapten Ahab." },
  { "en": "Kekurangan Zat Besi Menyebabkan Penyakit Apa?", "id": "Anemia." },
  { "en": "Apa Nama Jembatan Ikonik Di Kota Sydney?", "id": "Sydney Harbour Bridge." },
  { "en": "Organ Apa Yang Memompa Darah Ke Seluruh Tubuh?", "id": "Jantung." },
  { "en": "Siapakah Arsitek Yang Merancang Museum Louvre Pyramids?", "id": "I. M. Pei." },
  { "en": "Berapa Jumlah Bola Dalam Permainan Biliar Standar?", "id": "16 Bola." },
  { "en": "Dimana Lokasi Tembok Ratapan (Western Wall) Berada?", "id": "Yerusalem." },
  { "en": "Apa Sebutan Untuk Studi Tentang Racun?", "id": "Toksikologi." },
  { "en": "Siapa Kaisar Jepang Saat Perang Dunia Kedua?", "id": "Kaisar Hirohito." },
  { "en": "Apa Nama Gurun Pasir Terkering Di Dunia?", "id": "Gurun Atacama." },
  { "en": "Berapa Jumlah Sisi Pada Bangun Datar Dekagon?", "id": "10 Sisi." },
  { "en": "Apa Sebutan Untuk Ilmu Mempelajari Hukum Fisika?", "id": "Fisika." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Sosis?", "id": "Jerman." },
  { "en": "Siapa Perdana Menteri Inggris Selama Perang Dunia II?", "id": "Winston Churchill." },
  { "en": "Apa Nama Bagian Putih Pada Bola Mata Manusia?", "id": "Sklera." },
  { "en": "Apa Nama Air Terjun Terbesar Di Dunia?", "id": "Air Terjun Victoria." },
  { "en": "Siapakah Yang Menemukan Telepon Genggam Pertama?", "id": "Martin Cooper." },
  { "en": "Apa Nama Seni Teater Boneka Khas Jepang?", "id": "Bunraku." },
  { "en": "Huruf Apa Yang Melambangkan Angka 1000 Romawi?", "id": "M." },
  { "en": "Apa Satuan Standar Internasional Untuk Energi?", "id": "Joule." },
  { "en": "Siapakah Ratu Terakhir Kerajaan Perancis?", "id": "Marie Antoinette." },
  { "en": "Apa Nama Makanan Khas Negara Spanyol?", "id": "Paella." },
  { "en": "Amfibi Terbesar Di Dunia Saat Ini?", "id": "Salamander Raksasa Tiongkok." },
  { "en": "Apa Akronim Untuk Universal Serial Bus?", "id": "USB." },
  { "en": "Berapa Sudut Refleks Pada Sebuah Garis Lurus?", "id": "180 Derajat." },
  { "en": "Apa Nama Ibukota Negara Swedia?", "id": "Stockholm." },
  { "en": "Siapakah Murid Terkenal Dari Filsuf Plato?", "id": "Aristoteles." },
  { "en": "Apa Istilah Untuk Studi Tentang Darah?", "id": "Hematologi." },
  { "en": "Burung Apa Yang Dijadikan Simbol Negara Amerika?", "id": "Elang Botak." },
  { "en": "Apa Ibukota Provinsi Sulawesi Selatan?", "id": "Makassar." },
  { "en": "Siapa Nama Dewa Kematian Dalam Mitologi Mesir?", "id": "Anubis." },
  { "en": "Apa Zat Yang Memberikan Rasa Pedas Pada Cabai?", "id": "Capsaicin." },
  { "en": "Berapa Jumlah Medali Emas Olimpiade Usain Bolt?", "id": "8 Medali Emas." },
  { "en": "Apa Sebutan Untuk Pergerakan Air Laut Naik Turun?", "id": "Pasang Surut." },
  { "en": "Siapa Pendiri Dinasti Maurya Di India Kuno?", "id": "Chandragupta Maurya." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Membuat Peta?", "id": "Kartografer." },
  { "en": "Dimana Lokasi Istana Terlarang (Forbidden City)?", "id": "Beijing, Tiongkok." },
  { "en": "Berapa Jumlah Atom Oksigen Dalam Molekul Air?", "id": "1 Atom." },
  { "en": "Apa Nama Fenomena Optik Pelangi Di Malam Hari?", "id": "Moonbow." },
  { "en": "Siapakah Bapak Filsafat Barat Yang Terkenal?", "id": "Socrates." },
  { "en": "Apa Sebutan Untuk Perubahan Wujud Zat Gas Ke Cair?", "id": "Mengembun." },
  { "en": "Berapa Jumlah Pemain Dalam Satu Tim Kriket?", "id": "11 Pemain." },
  { "en": "Apa Nama Alat Untuk Mengukur Curah Hujan?", "id": "Ombrometer." },
  { "en": "Siapakah Wanita Pertama Yang Memenangkan Hadiah Nobel?", "id": "Marie Curie." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Bangsawan?", "id": "Aristokrasi." },
  { "en": "Apa Akronim Untuk Frequently Asked Questions?", "id": "FAQ." },
  { "en": "Pada Tahun Berapa Manusia Pertama Mendarat Di Bulan?", "id": "Tahun 1969." },
  { "en": "Siapa Seniman Belanda Yang Terkenal Dengan Grafisnya?", "id": "M.C. Escher." },
  { "en": "Apa Nama Ibukota Negara Belgia?", "id": "Brussel." },
  { "en": "Berapa Jumlah Warna Pada Bendera Negara Jerman?", "id": "3 Warna." },
  { "en": "Apa Proses Pengikisan Tanah Oleh Air Atau Angin?", "id": "Erosi." },
  { "en": "Siapa Penjelajah Viking Yang Mencapai Amerika Utara?", "id": "Leif Erikson." },
  { "en": "Apa Nama Kepulauan Terbesar Di Dunia?", "id": "Kepulauan Melayu." },
  { "en": "Apa Hormon Yang Membuat Seseorang Merasa Bahagia?", "id": "Serotonin." },
  { "en": "Siapa Tokoh Pewayangan Yang Paling Bijaksana?", "id": "Semar." },
  { "en": "Di Negara Mana Letak Gunung Kilimanjaro?", "id": "Tanzania." },
  { "en": "Apa Nama Ibukota Negara Hungaria?", "id": "Budapest." },
  { "en": "Siapakah Komposer Musik Klasik Terkenal Dari Austria?", "id": "Wolfgang Amadeus Mozart." },
  { "en": "Apa Sebutan Untuk Studi Tentang Masyarakat Purba?", "id": "Arkeologi." },
  { "en": "Berapa Jumlah Rusuk Pada Sebuah Bangun Balok?", "id": "12 Rusuk." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Hyundai?", "id": "Korea Selatan." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Fe?", "id": "Besi (Ferrum)." },
  { "en": "Siapakah Yang Menciptakan Karakter Mickey Mouse?", "id": "Walt Disney." },
  { "en": "Apa Bahan Baku Utama Pembuatan Anggur Merah?", "id": "Buah Anggur." },
  { "en": "Kapan Peringatan Hari Lingkungan Hidup Sedunia?", "id": "5 Juni." },
  { "en": "Apa Nama Proses Penggabungan Sel Sperma Dan Ovum?", "id": "Fertilisasi." },
  { "en": "Siapakah Presiden Amerika Serikat Selama Perang Dunia I?", "id": "Woodrow Wilson." },
  { "en": "Apa Istilah Untuk Skor Nol Dalam Tenis?", "id": "Love." },
  { "en": "Apa Nama Mata Uang Resmi Negara Denmark?", "id": "Krone Denmark." },
  { "en": "Dari Benua Manakah Asal Tanaman Tebu?", "id": "Asia Tenggara." },
  { "en": "Proses Pendinginan Magma Menjadi Batuan Disebut Apa?", "id": "Kristalisasi." },
  { "en": "Siapa Penulis Novel Misteri And Then There Were None?", "id": "Agatha Christie." },
  { "en": "Apa Nama Hormon Utama Pada Pria?", "id": "Testosteron." },
  { "en": "Agama Manakah Yang Memiliki Kitab Suci Tripitaka?", "id": "Agama Buddha." },
  { "en": "Apa Sebutan Untuk Jarak Terdekat Planet Dari Matahari?", "id": "Perihelion." },
  { "en": "Berapa Jumlah Tulang Pada Pergelangan Kaki Manusia?", "id": "7 Tulang." },
  { "en": "Siapakah Ratu Terakhir Dari Dinasti Ptolemaik Mesir?", "id": "Cleopatra VII." },
  { "en": "Apa Akronim Untuk Organisasi Negara Pengekspor Minyak?", "id": "OPEC." },
  { "en": "Di Kota Manakah Terdapat Alun-Alun Merah (Red Square)?", "id": "Moskow, Rusia." },
  { "en": "Hewan Apa Yang Dikenal Sebagai Raja Hutan?", "id": "Singa." },
  { "en": "Apa Nama Pukulan Pembuka Dalam Permainan Bulu Tangkis?", "id": "Servis." },
  { "en": "Siapa Nama Dewa Perdagangan Dalam Mitologi Romawi?", "id": "Merkurius." },
  { "en": "Berapa Tahun Dalam Satu Windu Menurut Kalender Jawa?", "id": "8 Tahun." },
  { "en": "Apa Sebutan Untuk Udara Yang Bergerak Cepat?", "id": "Angin." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Oslo?", "id": "Norwegia." },
  { "en": "Apa Nama Alat Musik Tiup Logam Melingkar?", "id": "French Horn." },
  { "en": "Siapakah Yang Menemukan Sel Pertama Kali?", "id": "Robert Hooke." },
  { "en": "Apa Nama Lapisan Tengah Dari Struktur Planet Bumi?", "id": "Mantel Bumi." },
  { "en": "Berapa Jumlah Hari Maksimal Dalam Satu Bulan?", "id": "31 Hari." },
  { "en": "Jaringan Apa Yang Menjadi Tempat Fotosintesis Terjadi?", "id": "Jaringan Palisade." },
  { "en": "Negara Apa Yang Dijuluki Negara Tirai Besi?", "id": "Uni Soviet." },
  { "en": "Siapa Pelukis Terkenal Era Renaisans Italia?", "id": "Leonardo Da Vinci, Michelangelo." },
  { "en": "Gas Apa Yang Paling Ringan Di Alam Semesta?", "id": "Hidrogen." },
  { "en": "Apa Nama Selat Yang Memisahkan Inggris Dan Perancis?", "id": "Selat Inggris." },
  { "en": "Siapakah Bapak Taksonomi Modern Yang Terkenal?", "id": "Carolus Linnaeus." },
  { "en": "Apa Cabang Ilmu Kedokteran Tentang Penyakit Anak?", "id": "Pediatri." },
  { "en": "Berapa Nilai Kartu King Dalam Permainan Remi?", "id": "10 Poin." },
  { "en": "Apa Nama Ibukota Negara Norwegia?", "id": "Oslo." },
  { "en": "Siapa Nama Tokoh Penyihir Muda Dalam Novel Fantasi?", "id": "Harry Potter." },
  { "en": "Kekurangan Vitamin A Dapat Menyebabkan Penyakit Apa?", "id": "Rabun Senja." },
  { "en": "Apa Nama Menara Jam Terkenal Di London?", "id": "Big Ben." },
  { "en": "Organ Apa Yang Memproduksi Empedu Dalam Tubuh?", "id": "Hati." },
  { "en": "Siapakah Arsitek Dari Gereja Sagrada Familia?", "id": "Antoni Gaudi." },
  { "en": "Berapa Jumlah Pemain Dalam Tim Futsal?", "id": "5 Pemain." },
  { "en": "Dimana Lokasi Tembok Besar Tiongkok Berada?", "id": "Tiongkok Utara." },
  { "en": "Apa Sebutan Untuk Studi Tentang Ikan?", "id": "Ikhtiologi." },
  { "en": "Siapa Diktator Uni Soviet Selama Perang Dunia II?", "id": "Joseph Stalin." },
  { "en": "Apa Nama Sungai Terpanjang Di Benua Eropa?", "id": "Sungai Volga." },
  { "en": "Berapa Jumlah Sisi Pada Bangun Datar Endekagon?", "id": "11 Sisi." },
  { "en": "Apa Sebutan Untuk Ilmu Mempelajari Bahan Kimia?", "id": "Kimia." },
  { "en": "Negara Mana Yang Merupakan Tempat Lahir Demokrasi?", "id": "Yunani Kuno." },
  { "en": "Siapa Kaisar Perancis Yang Terkenal Dengan Perawakannya?", "id": "Napoleon Bonaparte." },
  { "en": "Apa Nama Bukaan Pada Mata Yang Mengatur Cahaya?", "id": "Pupil." },
  { "en": "Apa Nama Pegunungan Tertinggi Di Benua Amerika?", "id": "Pegunungan Andes." },
  { "en": "Siapakah Yang Menemukan Pesawat Terbang Pertama?", "id": "Wright Bersaudara." },
  { "en": "Apa Nama Tarian Perang Suku Maori Selandia Baru?", "id": "Haka." },
  { "en": "Huruf Apa Yang Melambangkan Angka 1 Dalam Romawi?", "id": "I." },
  { "en": "Apa Satuan Standar Internasional Untuk Arus Listrik?", "id": "Ampere." },
  { "en": "Siapakah Firaun Wanita Terkenal Dari Mesir Kuno?", "id": "Hatshepsut." },
  { "en": "Apa Nama Makanan Khas Negara Thailand?", "id": "Tom Yum Goong." },
  { "en": "Apa Nama Ular Terbesar Di Dunia?", "id": "Anakonda Hijau." },
  { "en": "Apa Akronim Untuk Hypertext Transfer Protocol?", "id": "HTTP." },
  { "en": "Berapa Total Sudut Dalam Segitiga?", "id": "180 Derajat." },
  { "en": "Apa Nama Ibukota Negara Finlandia?", "id": "Helsinki." },
  { "en": "Siapakah Yang Menulis Buku Republik Di Yunani Kuno?", "id": "Plato." },
  { "en": "Apa Istilah Untuk Studi Tentang Burung?", "id": "Ornitologi." },
  { "en": "Kucing Besar Apa Yang Tidak Bisa Mengaum?", "id": "Cheetah." },
  { "en": "Apa Ibukota Provinsi Kalimantan Timur Saat Ini?", "id": "Samarinda." },
  { "en": "Siapa Nama Dewi Bulan Dalam Mitologi Romawi?", "id": "Luna." },
  { "en": "Apa Senyawa Yang Membuat Daun Berwarna Kuning?", "id": "Karotenoid." },
  { "en": "Berapa Kali Brasil Memenangkan Piala Dunia FIFA?", "id": "5 Kali." },
  { "en": "Apa Sebutan Untuk Gempa Yang Berpusat Di Laut?", "id": "Gempa Laut." },
  { "en": "Siapa Pendiri Kota Roma Menurut Legenda?", "id": "Romulus Dan Remus." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Membuat Patung?", "id": "Pematung." },
  { "en": "Dimana Lokasi Kuil Parthenon Yang Megah?", "id": "Athena, Yunani." },
  { "en": "Berapa Jumlah Atom Hidrogen Dalam Molekul Air?", "id": "2 Atom." },
  { "en": "Apa Nama Fenomena Kilatan Cahaya Di Langit?", "id": "Petir." },
  { "en": "Siapakah Yang Dianggap Bapak Retorika Yunani Kuno?", "id": "Aristoteles." },
  { "en": "Apa Sebutan Untuk Proses Pengeringan Dengan Pembekuan?", "id": "Liofilisasi." },
  { "en": "Berapa Jumlah Pemain Dalam Satu Tim Sofbol?", "id": "9 Pemain." },
  { "en": "Apa Nama Alat Untuk Mengukur Kadar Gula Darah?", "id": "Glukometer." },
  { "en": "Siapakah Presiden Wanita Pertama Di Dunia?", "id": "Isabel Peron." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Militer?", "id": "Junta Militer." },
  { "en": "Apa Akronim Untuk As Soon As Possible?", "id": "ASAP." },
  { "en": "Pada Tahun Berapa Tembok Berlin Mulai Dibangun?", "id": "Tahun 1961." },
  { "en": "Siapa Seniman Yang Melukis The Birth Of Venus?", "id": "Sandro Botticelli." },
  { "en": "Apa Nama Ibukota Negara Denmark?", "id": "Kopenhagen." },
  { "en": "Berapa Jumlah Warna Primer Dalam Seni Rupa?", "id": "3 Warna." },
  { "en": "Apa Proses Penyebaran Biji Tumbuhan Oleh Angin?", "id": "Anemokori." },
  { "en": "Siapa Penjelajah Spanyol Yang Menaklukkan Kerajaan Aztec?", "id": "Hernan Cortes." },
  { "en": "Apa Nama Laut Pedalaman Terbesar Di Dunia?", "id": "Laut Kaspia." },
  { "en": "Apa Hormon Yang Dikenal Sebagai Hormon Cinta?", "id": "Oksitosin." },
  { "en": "Siapa Tokoh Pewayangan Yang Memiliki Sifat Jujur?", "id": "Yudistira." },
  { "en": "Di Negara Mana Letak Gurun Kalahari?", "id": "Botswana, Namibia, Afrika Selatan." },
  { "en": "Apa Nama Ibukota Negara Nigeria?", "id": "Abuja." },
  { "en": "Siapa Pelukis Aliran Impresionisme Terkenal Dari Perancis?", "id": "Claude Monet." },
  { "en": "Apa Sebutan Untuk Studi Tentang Cuaca Dan Iklim?", "id": "Meteorologi Dan Klimatologi." },
  { "en": "Berapa Jumlah Titik Sudut Pada Sebuah Kerucut?", "id": "1 Titik Sudut." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Volkswagen?", "id": "Jerman." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol K?", "id": "Kalium (Potassium)." },
  { "en": "Siapakah Yang Menciptakan Karakter James Bond?", "id": "Ian Fleming." },
  { "en": "Apa Bahan Baku Utama Pembuatan Bir?", "id": "Gandum, Hop, Ragi." },
  { "en": "Kapan Peringatan Hari Kesehatan Dunia Diperingati?", "id": "7 April." },
  { "en": "Apa Nama Proses Pembentukan Sperma Pada Pria?", "id": "Spermatogenesis." },
  { "en": "Siapakah Perdana Menteri Pertama Negara India?", "id": "Jawaharlal Nehru." },
  { "en": "Apa Istilah Untuk Tiga Pukulan Beruntun Dalam Boling?", "id": "Turkey." },
  { "en": "Apa Nama Mata Uang Resmi Negara Swedia?", "id": "Krona Swedia." },
  { "en": "Dari Wilayah Manakah Asal Musik Reggae?", "id": "Jamaika." },
  { "en": "Proses Perubahan Batuan Akibat Suhu Tinggi Disebut?", "id": "Metamorfisme." },
  { "en": "Siapa Penulis Epos Yunani Kuno The Odyssey?", "id": "Homer." },
  { "en": "Apa Nama Hormon Utama Pada Wanita?", "id": "Estrogen." },
  { "en": "Agama Manakah Yang Memiliki Kitab Suci Tanakh?", "id": "Agama Yahudi." },
  { "en": "Apa Sebutan Untuk Titik Terdekat Bulan Ke Bumi?", "id": "Perigee." },
  { "en": "Berapa Jumlah Tulang Wajah Pada Tengkorak Manusia?", "id": "14 Tulang." },
  { "en": "Siapakah Yang Memimpin Penaklukan Konstantinopel Pada 1453?", "id": "Sultan Mehmed II." },
  { "en": "Apa Akronim Untuk Organisasi Perdagangan Dunia?", "id": "WTO (World Trade Organization)." },
  { "en": "Di Kota Manakah Terdapat Gerbang Brandenburg?", "id": "Berlin, Jerman." },
  { "en": "Hewan Apa Yang Dikenal Sebagai Mamalia Laut Cerdas?", "id": "Lumba-Lumba." },
  { "en": "Apa Nama Area Di Belakang Garis Gawang Sepak Bola?", "id": "Area Penalti." },
  { "en": "Siapa Nama Dewi Pertanian Dalam Mitologi Yunani?", "id": "Demeter." },
  { "en": "Berapa Hari Dalam Satu Kuartal Tahun?", "id": "Sekitar 91 Hari." },
  { "en": "Apa Sebutan Untuk Angin Topan Di Samudra Atlantik?", "id": "Hurikan." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Wina?", "id": "Austria." },
  { "en": "Apa Nama Alat Musik Perkusi Berbentuk Bilah Kayu?", "id": "Xilofon." },
  { "en": "Siapakah Yang Merumuskan Teori Elektromagnetisme?", "id": "James Clerk Maxwell." },
  { "en": "Apa Nama Lapisan Terluar Dari Atmosfer Bumi?", "id": "Eksosfer." },
  { "en": "Berapa Jumlah Bulan Dalam Satu Caturwulan?", "id": "4 Bulan." },
  { "en": "Jaringan Apa Yang Memberi Kekuatan Pada Batang Tumbuhan?", "id": "Jaringan Sklerenkim." },
  { "en": "Negara Apa Yang Dijuluki Negeri Pizza?", "id": "Italia." },
  { "en": "Siapa Seniman Yang Melukis The Scream?", "id": "Edvard Munch." },
  { "en": "Apa Nama Gas Mulia Yang Paling Ringan?", "id": "Helium." },
  { "en": "Apa Nama Terusan Yang Menghubungkan Great Lakes?", "id": "Terusan Welland." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Sosiologi?", "id": "Auguste Comte." },
  { "en": "Apa Cabang Ilmu Kedokteran Tentang Penyakit Saraf?", "id": "Neurologi." },
  { "en": "Berapa Jumlah Pion Dalam Permainan Catur?", "id": "16 Pion." },
  { "en": "Siapa Nama Tokoh Penjahat Utama Dalam Harry Potter?", "id": "Lord Voldemort." },
  { "en": "Kekurangan Yodium Dapat Menyebabkan Penyakit Apa?", "id": "Penyakit Gondok." },
  { "en": "Apa Nama Patung Terkenal Di Pelabuhan Kopenhagen?", "id": "The Little Mermaid." },
  { "en": "Organ Apa Yang Berperan Dalam Sistem Imun Tubuh?", "id": "Limpa." },
  { "en": "Siapakah Arsitek Terkenal Dari Aliran Bauhaus?", "id": "Walter Gropius." },
  { "en": "Berapa Jarak Lari Maraton Secara Resmi?", "id": "42,195 Kilometer." },
  { "en": "Dimana Lokasi Piramida Matahari Dan Bulan?", "id": "Teotihuacan, Meksiko." },
  { "en": "Apa Sebutan Untuk Studi Tentang Masyarakat Manusia?", "id": "Antropologi." },
  { "en": "Siapa Pemimpin Vietnam Utara Selama Perang Vietnam?", "id": "Ho Chi Minh." },
  { "en": "Apa Nama Sungai Terpanjang Di Benua Amerika Utara?", "id": "Sungai Missouri." },
  { "en": "Berapa Jumlah Sisi Pada Bangun Datar Dodekagon?", "id": "12 Sisi." },
  { "en": "Apa Sebutan Untuk Ilmu Mempelajari Benda Langit?", "id": "Astronomi." },
  { "en": "Negara Mana Yang Merupakan Tempat Lahir Filsafat?", "id": "Yunani Kuno." },
  { "en": "Siapakah Ratu Terakhir Yang Memerintah Kerajaan Hawaii?", "id": "Liliuokalani." },
  { "en": "Apa Nama Partikel Subatomik Yang Tidak Bermuatan?", "id": "Neutron." },
  { "en": "Apa Nama Samudra Terkecil Dan Terdangkal Di Dunia?", "id": "Samudra Arktik." },
  { "en": "Siapakah Penemu World Wide Web (WWW)?", "id": "Tim Berners-Lee." },
  { "en": "Apa Nama Festival Melempar Tomat Di Spanyol?", "id": "La Tomatina." },
  { "en": "Huruf Apa Yang Melambangkan Angka 5 Dalam Romawi?", "id": "V." },
  { "en": "Apa Satuan Standar Internasional Untuk Hambatan Listrik?", "id": "Ohm." },
  { "en": "Siapakah Jenderal Sparta Yang Terkenal Di Thermopylae?", "id": "Raja Leonidas I." },
  { "en": "Apa Nama Saus Khas Dari Negara Italia?", "id": "Saus Bolognese." },
  { "en": "Apa Nama Burung Terbesar Di Dunia?", "id": "Burung Unta." },
  { "en": "Apa Akronim Untuk Light Amplification by Stimulated Emission of Radiation?", "id": "LASER." },
  { "en": "Berapa Total Sudut Dalam Sebuah Persegi?", "id": "360 Derajat." },
  { "en": "Apa Nama Ibukota Negara Korea Utara?", "id": "Pyongyang." },
  { "en": "Siapakah Yang Menulis Buku The Art Of War?", "id": "Sun Tzu." },
  { "en": "Apa Istilah Untuk Studi Tentang Reptil Dan Amfibi?", "id": "Herpetologi." },
  { "en": "Hewan Apa Yang Memiliki Tiga Kelopak Mata?", "id": "Unta." },
  { "en": "Apa Ibukota Provinsi Papua Saat Ini?", "id": "Jayapura." },
  { "en": "Siapa Nama Dewa Anggur Dalam Mitologi Yunani?", "id": "Dionysus." },
  { "en": "Apa Senyawa Kimia Yang Menjadi Komponen Utama Pasir?", "id": "Silikon Dioksida." },
  { "en": "Negara Mana Yang Paling Sering Juara Piala Dunia?", "id": "Brasil." },
  { "en": "Apa Sebutan Untuk Gelombang Energi Dari Gempa?", "id": "Gelombang Seismik." },
  { "en": "Siapa Raja Terkenal Dari Kerajaan Majapahit?", "id": "Hayam Wuruk." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Membuat Pakaian?", "id": "Penjahit." },
  { "en": "Dimana Lokasi Patung Moai Yang Misterius?", "id": "Pulau Paskah, Chili." },
  { "en": "Berapa Jumlah Kromosom Pada Manusia Normal?", "id": "46 Kromosom." },
  { "en": "Apa Nama Istilah Untuk Bulan Purnama Kedua?", "id": "Bulan Biru (Blue Moon)." },
  { "en": "Siapakah Bapak Kedokteran Yunani Yang Terkenal?", "id": "Hippocrates." },
  { "en": "Apa Sebutan Untuk Perubahan Wujud Zat Padat Ke Cair?", "id": "Mencair." },
  { "en": "Berapa Jumlah Babak Dalam Pertandingan Hoki Es?", "id": "3 Babak." },
  { "en": "Apa Nama Alat Untuk Mengukur Ketinggian Suatu Tempat?", "id": "Altimeter." },
  { "en": "Siapakah Ratu Mesir Yang Membangun Deir el-Bahari?", "id": "Hatshepsut." },
  { "en": "Apa Sistem Pemerintahan Yang Berbasis Agama?", "id": "Teokrasi." },
  { "en": "Apa Akronim Untuk By The Way?", "id": "BTW." },
  { "en": "Pada Tahun Berapa Penjelajahan Columbus Mencapai Amerika?", "id": "Tahun 1492." },
  { "en": "Siapa Seniman Yang Memahat Patung Pieta?", "id": "Michelangelo." },
  { "en": "Apa Nama Ibukota Negara Swiss?", "id": "Bern." },
  { "en": "Berapa Jumlah Warna Pada Bendera Negara Italia?", "id": "3 Warna." },
  { "en": "Apa Proses Penyebaran Biji Oleh Hewan?", "id": "Zookori." },
  { "en": "Siapa Penjelajah Spanyol Yang Menaklukkan Kerajaan Inka?", "id": "Francisco Pizarro." },
  { "en": "Apa Nama Laut Terbesar Di Dunia?", "id": "Laut Filipina." },
  { "en": "Apa Hormon Yang Dikenal Sebagai Hormon Stres?", "id": "Adrenalin Dan Kortisol." },
  { "en": "Siapa Nama Patih Terkenal Dari Kerajaan Majapahit?", "id": "Gajah Mada." },
  { "en": "Di Negara Mana Karnaval Rio De Janeiro Diadakan?", "id": "Brasil." },
  { "en": "Apa Nama Ibukota Negara Kenya?", "id": "Nairobi." },
  { "en": "Siapa Pelukis Belanda Yang Terkenal Dengan Potret Dirinya?", "id": "Rembrandt Van Rijn." },
  { "en": "Apa Sebutan Untuk Studi Tentang Asal Usul Kehidupan?", "id": "Abiogenesis." },
  { "en": "Berapa Jumlah Titik Sudut Pada Sebuah Prisma Segitiga?", "id": "6 Titik Sudut." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Toyota?", "id": "Jepang." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Ag?", "id": "Perak (Argentum)." },
  { "en": "Siapakah Yang Menciptakan Karakter Sherlock Holmes?", "id": "Arthur Conan Doyle." },
  { "en": "Apa Bahan Dasar Dari Makanan Khas Italia, Pasta?", "id": "Tepung, Telur, Air." },
  { "en": "Kapan Peringatan Hari Hak Asasi Manusia Sedunia?", "id": "10 Desember." },
  { "en": "Apa Nama Proses Sintesis Gula Pada Tumbuhan?", "id": "Fotosintesis." },
  { "en": "Siapakah Kanselir Pertama Republik Federal Jerman?", "id": "Konrad Adenauer." },
  { "en": "Apa Istilah Untuk Kemenangan Mutlak Dalam Catur?", "id": "Skakmat." },
  { "en": "Apa Nama Mata Uang Resmi Negara Rusia?", "id": "Rubel Rusia." },
  { "en": "Dari Benua Manakah Asal Buah Pisang?", "id": "Asia Tenggara." },
  { "en": "Proses Bertambah Besarnya Batuan Oleh Mineral Disebut?", "id": "Akresi." },
  { "en": "Siapa Penulis Novel Fiksi Ilmiah Frankenstein?", "id": "Mary Shelley." },
  { "en": "Apa Nama Hormon Yang Dikenal Sebagai Hormon Tidur?", "id": "Melatonin." },
  { "en": "Agama Manakah Yang Memiliki Kitab Suci Guru Granth Sahib?", "id": "Agama Sikh." },
  { "en": "Apa Sebutan Untuk Titik Terjauh Bulan Dari Bumi?", "id": "Apogee." },
  { "en": "Berapa Jumlah Tulang Pada Rangka Aksial Manusia?", "id": "80 Tulang." },
  { "en": "Siapakah Yang Memimpin Pasukan Sekutu Di Eropa?", "id": "Dwight D. Eisenhower." },
  { "en": "Apa Akronim Untuk Badan Antariksa Nasional Amerika?", "id": "NASA." },
  { "en": "Di Kota Manakah Terdapat Hagia Sophia Yang Megah?", "id": "Istanbul, Turki." },
  { "en": "Hewan Apa Yang Dikenal Dapat Menyemprotkan Tinta Hitam?", "id": "Cumi-Cumi Dan Gurita." },
  { "en": "Apa Nama Pukulan Keras Menukik Dalam Bola Voli?", "id": "Spike Atau Smash." },
  { "en": "Siapa Nama Dewi Perburuan Dalam Mitologi Romawi?", "id": "Diana." },
  { "en": "Berapa Tahun Dalam Satu Abad Atau Century?", "id": "100 Tahun." },
  { "en": "Apa Sebutan Untuk Angin Topan Di Samudra Pasifik?", "id": "Tifun." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Kopenhagen?", "id": "Denmark." },
  { "en": "Apa Nama Alat Musik Tiup Terbuat Dari Kerang?", "id": "Terompet Kerang." },
  { "en": "Siapakah Yang Pertama Kali Mengemukakan Teori Atom?", "id": "Democritus." },
  { "en": "Apa Nama Lapisan Atmosfer Tempat Terjadinya Aurora?", "id": "Termosfer." },
  { "en": "Berapa Jumlah Hari Dalam Satu Semester Akademik?", "id": "Sekitar 180 Hari." },
  { "en": "Jaringan Apa Yang Melindungi Bagian Dalam Tumbuhan?", "id": "Jaringan Epidermis." },
  { "en": "Negara Apa Yang Dijuluki Negeri Samba?", "id": "Brasil." },
  { "en": "Siapa Seniman Yang Terkenal Dengan Patung David?", "id": "Michelangelo." },
  { "en": "Apa Nama Gas Yang Mengisi Sebagian Besar Udara?", "id": "Nitrogen." },
  { "en": "Apa Nama Samudra Yang Memisahkan Benua Amerika Eropa?", "id": "Samudra Atlantik." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Geometri?", "id": "Euclid." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Kanker?", "id": "Onkologi." },
  { "en": "Berapa Jumlah Pemain Dalam Tim Polo Berkuda?", "id": "4 Pemain." },
  { "en": "Apa Nama Ibukota Negara Iran?", "id": "Teheran." },
  { "en": "Siapa Nama Tokoh Hobbit Pembawa Cincin?", "id": "Frodo Baggins." },
  { "en": "Kekurangan Vitamin B12 Dapat Menyebabkan Penyakit?", "id": "Anemia Pernisiosa." },
  { "en": "Apa Nama Istana Terkenal Di Kota Paris?", "id": "Istana Versailles." },
  { "en": "Organ Apa Yang Berfungsi Menyimpan Urine Sementara?", "id": "Kandung Kemih." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Fallingwater?", "id": "Frank Lloyd Wright." },
  { "en": "Apa Sebutan Untuk Skor Imbang Dalam Tenis?", "id": "Deuce." },
  { "en": "Dimana Lokasi Kuil Emas Amritsar Berada?", "id": "Amritsar, India." },
  { "en": "Apa Sebutan Untuk Studi Tentang Jamur Dan Fungi?", "id": "Mikologi." },
  { "en": "Siapa Pemimpin Revolusi Tiongkok Pada Tahun 1949?", "id": "Mao Zedong." },
  { "en": "Apa Nama Sungai Terpanjang Di Benua Australia?", "id": "Sungai Murray." },
  { "en": "Berapa Jumlah Rusuk Pada Sebuah Limas Segi Empat?", "id": "8 Rusuk." },
  { "en": "Apa Sebutan Untuk Ilmu Mempelajari Bintang Dan Planet?", "id": "Astronomi." },
  { "en": "Negara Mana Yang Merupakan Tempat Asal Celana Jeans?", "id": "Amerika Serikat." },
  { "en": "Siapakah Presiden Amerika Serikat Ke-32?", "id": "Franklin D. Roosevelt." },
  { "en": "Apa Nama Partikel Subatomik Yang Bermuatan Positif?", "id": "Proton." },
  { "en": "Apa Nama Titik Terendah Di Benua Afrika?", "id": "Danau Assal." },
  { "en": "Siapakah Yang Menemukan Bahasa Pemrograman Java?", "id": "James Gosling." },
  { "en": "Apa Nama Festival Cahaya Yang Dirayakan Umat Hindu?", "id": "Diwali." },
  { "en": "Huruf Apa Yang Melambangkan Angka 10 Dalam Romawi?", "id": "X." },
  { "en": "Apa Satuan Standar Internasional Untuk Tegangan Listrik?", "id": "Volt." },
  { "en": "Siapakah Panglima Perang Terkenal Dari Kartago?", "id": "Hannibal Barca." },
  { "en": "Apa Nama Hidangan Nasi Khas Dari Spanyol?", "id": "Paella." },
  { "en": "Apa Nama Hewan Pengerat Terbesar Di Dunia?", "id": "Kapibara." },
  { "en": "Apa Akronim Untuk Compact Disc Read-Only Memory?", "id": "CD-ROM." },
  { "en": "Berapa Jumlah Sumbu Simetri Pada Lingkaran?", "id": "Tak Terhingga." },
  { "en": "Apa Nama Ibukota Negara Kolombia?", "id": "Bogota." },
  { "en": "Siapa Yang Menulis Buku On The Origin Of Species?", "id": "Charles Darwin." },
  { "en": "Apa Istilah Untuk Studi Tentang Otak Dan Saraf?", "id": "Neurologi." },
  { "en": "Hewan Apa Yang Dikenal Sebagai 'Kapal Gurun'?", "id": "Unta." },
  { "en": "Apa Ibukota Provinsi Riau Saat Ini?", "id": "Pekanbaru." },
  { "en": "Siapa Nama Dewa Matahari Dalam Mitologi Jepang?", "id": "Amaterasu." },
  { "en": "Apa Senyawa Kimia Yang Dikenal Sebagai Garam Dapur?", "id": "Natrium Klorida." },
  { "en": "Tim Mana Yang Paling Sering Juara Liga Champions?", "id": "Real Madrid." },
  { "en": "Apa Sebutan Untuk Titik Di Permukaan Bumi Bawah Episentrum?", "id": "Hiposentrum." },
  { "en": "Siapa Raja Inggris Yang Menandatangani Magna Carta?", "id": "Raja John." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Seni Lukis?", "id": "Pelukis." },
  { "en": "Dimana Lokasi Acropolis Athena Yang Terkenal?", "id": "Athena, Yunani." },
  { "en": "Berapa Jumlah Katup Yang Ada Di Jantung Manusia?", "id": "4 Katup." },
  { "en": "Apa Istilah Untuk Hujan Yang Membeku Di Udara?", "id": "Hujan Es." },
  { "en": "Siapakah Bapak Kimia Modern Yang Terkenal?", "id": "Antoine Lavoisier." },
  { "en": "Apa Sebutan Untuk Perubahan Wujud Zat Cair Ke Gas?", "id": "Menguap." },
  { "en": "Berapa Jumlah Babak Dalam Pertandingan Bola Tangan?", "id": "2 Babak." },
  { "en": "Apa Nama Alat Untuk Mengukur Keasaman (PH)?", "id": "PH Meter." },
  { "en": "Siapakah Kaisar Romawi Pertama Yang Menganut Kristen?", "id": "Konstantinus Agung." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Sedikit Orang?", "id": "Oligarki." },
  { "en": "Apa Akronim Untuk For Your Information?", "id": "FYI." },
  { "en": "Pada Tahun Berapa Proklamasi Kemerdekaan Amerika Serikat?", "id": "Tahun 1776." },
  { "en": "Siapa Seniman Surealisme Yang Terkenal Dari Belgia?", "id": "Rene Magritte." },
  { "en": "Apa Nama Ibukota Negara Ceko?", "id": "Praha." },
  { "en": "Berapa Jumlah Warna Pada Bendera Negara Perancis?", "id": "3 Warna." },
  { "en": "Apa Proses Penyerapan Air Oleh Akar Tumbuhan?", "id": "Osmosis." },
  { "en": "Siapa Penjelajah Portugis Yang Menemukan Tanjung Harapan?", "id": "Bartolomeu Dias." },
  { "en": "Apa Nama Gurun Pasir Terbesar Di Benua Asia?", "id": "Gurun Gobi." },
  { "en": "Apa Hormon Yang Mengatur Siklus Menstruasi Wanita?", "id": "Estrogen Dan Progesteron." },
  { "en": "Siapa Tokoh Pewayangan Yang Dijuluki 'Putri Drupadi'?", "id": "Drupadi." },
  { "en": "Di Negara Mana Festival Thaipusam Dirayakan?", "id": "Malaysia, India, Singapura." },
  { "en": "Apa Nama Ibukota Negara Peru?", "id": "Lima." },
  { "en": "Siapa Pelukis Terkenal Dengan Julukan 'El Greco'?", "id": "Domḗnikos Theotokópoulos." },
  { "en": "Apa Sebutan Untuk Studi Tentang Sel Dan Fungsinya?", "id": "Sitologi." },
  { "en": "Berapa Jumlah Sisi Pada Sebuah Bangun Prisma Segilima?", "id": "7 Sisi." },
  { "en": "Dari Negara Manakah Asal Merek Jam Tangan Swatch?", "id": "Swiss." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol W?", "id": "Wolfram (Tungsten)." },
  { "en": "Siapakah Yang Menciptakan Karakter Tintin Yang Terkenal?", "id": "Herge." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Sake?", "id": "Beras." },
  { "en": "Kapan Peringatan Hari Laut Sedunia Diperingati?", "id": "8 Juni." },
  { "en": "Apa Nama Proses Penuaan Alami Pada Sel?", "id": "Senescence." },
  { "en": "Siapakah Kaisar Terakhir Dinasti Qing Di Tiongkok?", "id": "Puyi." },
  { "en": "Apa Istilah Untuk Pukulan Yang Tidak Bisa Dikembalikan?", "id": "Ace (Servis)." },
  { "en": "Apa Nama Mata Uang Resmi Negara Brasil?", "id": "Real Brasil." },
  { "en": "Dari Benua Manakah Asal Buah Nanas?", "id": "Amerika Selatan." },
  { "en": "Proses Penghancuran Batuan Oleh Faktor Alam Disebut?", "id": "Pelapukan." },
  { "en": "Siapa Penulis Novel The Great Gatsby?", "id": "F. Scott Fitzgerald." },
  { "en": "Apa Nama Hormon Yang Memicu Rasa Lapar?", "id": "Ghrelin." },
  { "en": "Agama Manakah Yang Memiliki Kitab Suci Avesta?", "id": "Zoroastrianisme." },
  { "en": "Apa Sebutan Untuk Orbit Bumi Mengelilingi Matahari?", "id": "Ekliptika." },
  { "en": "Berapa Jumlah Tulang Pada Rangka Apendikular Manusia?", "id": "126 Tulang." },
  { "en": "Siapakah Pemimpin Perang Kemerdekaan Amerika Serikat?", "id": "George Washington." },
  { "en": "Apa Akronim Untuk Organisasi Kesehatan Pan Amerika?", "id": "PAHO." },
  { "en": "Di Kota Manakah Terdapat Jembatan Ponte Vecchio?", "id": "Florence, Italia." },
  { "en": "Hewan Apa Yang Dikenal Dapat Tidur Selama 3 Tahun?", "id": "Siput." },
  { "en": "Apa Nama Garis Awal Dalam Lomba Lari?", "id": "Garis Start." },
  { "en": "Siapa Nama Dewi Fajar Dalam Mitologi Yunani?", "id": "Eos." },
  { "en": "Berapa Detik Dalam Satu Hari Penuh?", "id": "86.400 Detik." },
  { "en": "Apa Sebutan Untuk Badai Pasir Di Gurun?", "id": "Haboob." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Brussel?", "id": "Belgia." },
  { "en": "Apa Nama Alat Musik Tiup Dari Bambu Khas Sunda?", "id": "Suling." },
  { "en": "Siapakah Yang Mengembangkan Vaksin Cacar Pertama?", "id": "Edward Jenner." },
  { "en": "Apa Nama Lapisan Atmosfer Paling Bawah?", "id": "Troposfer." },
  { "en": "Berapa Jumlah Menit Dalam Satu Semester Kuliah?", "id": "Tergantung Mata Kuliah." },
  { "en": "Jaringan Apa Yang Berfungsi Sebagai Tempat Penyimpanan Cadangan?", "id": "Jaringan Parenkim." },
  { "en": "Negara Apa Yang Dijuluki Negeri Seribu Pagoda?", "id": "Myanmar." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Seni Pop?", "id": "Andy Warhol." },
  { "en": "Apa Nama Gas Rumah Kaca Yang Paling Umum?", "id": "Uap Air." },
  { "en": "Apa Nama Semenanjung Tempat Negara Spanyol Dan Portugal?", "id": "Semenanjung Iberia." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Penerbangan Modern?", "id": "George Cayley." },
  { "en": "Apa Cabang Kedokteran Yang Mempelajari Penyakit Tua?", "id": "Geriatri." },
  { "en": "Berapa Jumlah Kartu Domino Dalam Satu Set?", "id": "28 Kartu." },
  { "en": "Apa Nama Ibukota Negara Chili?", "id": "Santiago." },
  { "en": "Siapa Nama Detektif Belgia Ciptaan Agatha Christie?", "id": "Hercule Poirot." },
  { "en": "Kekurangan Vitamin K Dapat Menyebabkan Masalah Apa?", "id": "Pendarahan." },
  { "en": "Apa Nama Patung Terkenal Karya Auguste Rodin?", "id": "The Thinker." },
  { "en": "Organ Apa Yang Mengatur Kadar Gula Darah?", "id": "Pankreas." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Guggenheim Museum?", "id": "Frank Gehry." },
  { "en": "Apa Istilah Untuk Skor 40-40 Dalam Tenis?", "id": "Deuce." },
  { "en": "Dimana Lokasi Tembok Berlin Yang Bersejarah?", "id": "Berlin, Jerman." },
  { "en": "Apa Sebutan Untuk Studi Tentang Mamalia?", "id": "Mamalogi." },
  { "en": "Siapa Pendiri Kekaisaran Persia Pertama?", "id": "Koresh Agung." },
  { "en": "Apa Nama Sungai Terpanjang Di Amerika Selatan?", "id": "Sungai Amazon." },
  { "en": "Berapa Jumlah Rusuk Pada Sebuah Prisma Segi Enam?", "id": "18 Rusuk." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Kehidupan?", "id": "Biologi." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Kincir Angin?", "id": "Persia." },
  { "en": "Siapakah Penjelajah Yang Memberi Nama Samudra Pasifik?", "id": "Ferdinand Magellan." },
  { "en": "Apa Nama Unsur Teringan Dalam Tabel Periodik?", "id": "Hidrogen." },
  { "en": "Apa Nama Titik Tertinggi Di Benua Australia?", "id": "Gunung Kosciuszko." },
  { "en": "Siapakah Yang Menciptakan Bahasa Pemrograman C++?", "id": "Bjarne Stroustrup." },
  { "en": "Apa Nama Festival Bir Terbesar Di Jerman?", "id": "Oktoberfest." },
  { "en": "Huruf Apa Yang Melambangkan Angka 50 Dalam Romawi?", "id": "L." },
  { "en": "Apa Satuan Standar Internasional Untuk Gaya?", "id": "Newton." },
  { "en": "Siapakah Firaun Mesir Yang Membangun Kuil Abu Simbel?", "id": "Ramses II." },
  { "en": "Apa Nama Roti Pipih Khas Timur Tengah?", "id": "Pita." },
  { "en": "Apa Nama Kadal Terbesar Di Dunia?", "id": "Komodo." },
  { "en": "Apa Akronim Untuk Digital Versatile Disc?", "id": "DVD." },
  { "en": "Berapa Jumlah Diagonal Dalam Sebuah Segi Lima?", "id": "5 Diagonal." },
  { "en": "Apa Nama Ibukota Negara Venezuela?", "id": "Caracas." },
  { "en": "Siapa Penulis Novel To Kill A Mockingbird?", "id": "Harper Lee." },
  { "en": "Apa Istilah Untuk Studi Tentang Pohon?", "id": "Dendrologi." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional Kanada?", "id": "Berang-Berang." },
  { "en": "Apa Ibukota Provinsi Kalimantan Barat?", "id": "Pontianak." },
  { "en": "Siapa Nama Dewa Perang Dalam Mitologi Nordik?", "id": "Odin Dan Tyr." },
  { "en": "Apa Senyawa Kimia Yang Membuat Langit Berwarna Biru?", "id": "Penyebaran Rayleigh." },
  { "en": "Tim Sepak Bola Mana Yang Dijuluki 'The Red Devils'?", "id": "Manchester United." },
  { "en": "Apa Sebutan Untuk Peristiwa Bulan Menutupi Matahari?", "id": "Gerhana Matahari." },
  { "en": "Siapa Sultan Demak Pertama Yang Memerintah?", "id": "Raden Patah." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Membuat Keramik?", "id": "Pengrajin Keramik." },
  { "en": "Dimana Lokasi Grand Canyon Yang Terkenal?", "id": "Arizona, Amerika Serikat." },
  { "en": "Berapa Jumlah Bilik Yang Dimiliki Jantung Buaya?", "id": "4 Bilik." },
  { "en": "Apa Istilah Untuk Pola Cuaca Jangka Panjang?", "id": "Iklim." },
  { "en": "Siapakah Bapak Revolusi Industri Yang Terkenal?", "id": "Tidak Ada Satu Tokoh." },
  { "en": "Apa Sebutan Untuk Proses Pembusukan Tanpa Oksigen?", "id": "Dekomposisi Anaerobik." },
  { "en": "Berapa Jumlah Ronde Dalam Pertandingan Gulat Amatir?", "id": "3 Ronde." },
  { "en": "Apa Nama Alat Untuk Mengukur Kelembaban Relatif?", "id": "Higrometer." },
  { "en": "Siapakah Firaun Mesir Yang Dikenal Sebagai Raja Sesat?", "id": "Akhenaten." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Raja?", "id": "Monarki." },
  { "en": "Apa Akronim Untuk 'Laughing Out Loud'?", "id": "LOL." },
  { "en": "Pada Tahun Berapa Revolusi Perancis Dimulai?", "id": "Tahun 1789." },
  { "en": "Siapa Seniman Meksiko Yang Terkenal Dengan Potret Dirinya?", "id": "Frida Kahlo." },
  { "en": "Berapa Jumlah Bintang Pada Bendera Negara Tiongkok?", "id": "5 Bintang." },
  { "en": "Apa Proses Pengangkutan Endapan Oleh Air Atau Angin?", "id": "Transportasi Sedimen." },
  { "en": "Siapa Penjelajah Inggris Yang Mengelilingi Dunia?", "id": "Sir Francis Drake." },
  { "en": "Apa Nama Gurun Terpanas Di Amerika Utara?", "id": "Gurun Mojave." },
  { "en": "Apa Hormon Yang Dilepaskan Saat Merasa Takut?", "id": "Adrenalin." },
  { "en": "Siapa Tokoh Pewayangan Yang Memiliki Panah Pasupati?", "id": "Arjuna." },
  { "en": "Di Negara Mana Kota Kuno Babilonia Berada?", "id": "Irak." },
  { "en": "Apa Nama Ibukota Negara Ethiopia?", "id": "Addis Ababa." },
  { "en": "Siapa Pelukis Terkenal Yang Menciptakan Karya Guernica?", "id": "Pablo Picasso." },
  { "en": "Apa Sebutan Untuk Studi Tentang Jaringan Tubuh?", "id": "Histologi." },
  { "en": "Berapa Jumlah Sisi Pada Sebuah Bangun Tabung?", "id": "3 Sisi." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Fiat?", "id": "Italia." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Sn?", "id": "Timah (Stannum)." },
  { "en": "Siapakah Yang Menciptakan Karakter Alice In Wonderland?", "id": "Lewis Carroll." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Tequila?", "id": "Tanaman Agave Biru." },
  { "en": "Kapan Peringatan Hari Populasi Dunia Diperingati?", "id": "11 Juli." },
  { "en": "Apa Nama Proses Matinya Sel Terprogram?", "id": "Apoptosis." },
  { "en": "Siapakah Raja Babilonia Yang Membuat Hukum Terkenal?", "id": "Hammurabi." },
  { "en": "Apa Istilah Untuk Poin Terakhir Dalam Pertandingan?", "id": "Match Point." },
  { "en": "Apa Nama Mata Uang Resmi Negara Meksiko?", "id": "Peso Meksiko." },
  { "en": "Dari Benua Manakah Asal Buah Semangka?", "id": "Afrika." },
  { "en": "Proses Pengendapan Material Oleh Angin Dan Air Disebut?", "id": "Sedimentasi." },
  { "en": "Siapa Penulis Novel The Catcher In The Rye?", "id": "J. D. Salinger." },
  { "en": "Apa Nama Hormon Yang Memicu Rasa Kenyang?", "id": "Leptin." },
  { "en": "Agama Manakah Yang Memiliki Kitab Suci Kojiki?", "id": "Shinto." },
  { "en": "Apa Sebutan Untuk Titik Tinggi Gelombang Laut?", "id": "Puncak Gelombang." },
  { "en": "Berapa Jumlah Tulang Pada Bagian Dada Manusia?", "id": "25 Tulang." },
  { "en": "Siapakah Pemimpin Revolusi Amerika Latin Yang Terkenal?", "id": "Simon Bolivar." },
  { "en": "Apa Akronim Untuk Badan Narkotika Internasional PBB?", "id": "INCB." },
  { "en": "Di Kota Manakah Terdapat Air Mancur Trevi?", "id": "Roma, Italia." },
  { "en": "Hewan Apa Yang Dikenal Dapat Berlari Di Atas Air?", "id": "Kadal Basilisk." },
  { "en": "Apa Nama Garis Akhir Dalam Suatu Perlombaan?", "id": "Garis Finis." },
  { "en": "Siapa Nama Dewi Pelangi Dalam Mitologi Yunani?", "id": "Iris." },
  { "en": "Berapa Hari Dalam Satu Triwulan Kalender?", "id": "Sekitar 91 Hari." },
  { "en": "Apa Sebutan Untuk Hujan Sangat Ringan?", "id": "Gerimis." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Lisbon?", "id": "Portugal." },
  { "en": "Apa Nama Alat Musik Pukul Khas Jawa Dan Bali?", "id": "Gamelan." },
  { "en": "Siapakah Yang Menemukan Hukum Kekekalan Energi?", "id": "James Prescott Joule." },
  { "en": "Apa Nama Lapisan Atmosfer Tempat Meteor Terbakar?", "id": "Mesosfer." },
  { "en": "Berapa Jumlah Minimal Hari Dalam Satu Bulan?", "id": "28 Hari." },
  { "en": "Jaringan Apa Yang Memberi Bentuk Pada Daun?", "id": "Jaringan Tulang Daun." },
  { "en": "Negara Apa Yang Dijuluki Negeri Matahari Tengah Malam?", "id": "Norwegia." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Kubisme?", "id": "Pablo Picasso, Georges Braque." },
  { "en": "Apa Nama Gas Yang Berbau Seperti Klorin?", "id": "Gas Ozon." },
  { "en": "Apa Nama Tanjung Paling Selatan Benua Afrika?", "id": "Tanjung Agulhas." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Fisika Nuklir?", "id": "Ernest Rutherford." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Gangguan Jiwa?", "id": "Psikiatri." },
  { "en": "Berapa Jumlah Pemain Dalam Tim Sepak Takraw?", "id": "3 Pemain." },
  { "en": "Apa Nama Ibukota Negara Kuba?", "id": "Havana." },
  { "en": "Siapa Nama Kapten Kapal Nautilus Dalam Novel Fiksi?", "id": "Kapten Nemo." },
  { "en": "Kekurangan Vitamin E Dapat Menyebabkan Masalah Apa?", "id": "Kerusakan Saraf." },
  { "en": "Apa Nama Istana Musim Dingin Di St. Petersburg?", "id": "Istana Musim Dingin." },
  { "en": "Organ Apa Yang Berfungsi Sebagai Kelenjar Endokrin Terbesar?", "id": "Pankreas." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Villa Savoye?", "id": "Le Corbusier." },
  { "en": "Apa Istilah Untuk Pelanggaran Dalam Olahraga Basket?", "id": "Foul." },
  { "en": "Dimana Lokasi Kuil Karnak Yang Luas Berada?", "id": "Luxor, Mesir." },
  { "en": "Apa Sebutan Untuk Studi Tentang Burung Dan Telurnya?", "id": "Oologi." },
  { "en": "Siapa Kaisar Romawi Yang Memulai Pembangunan Colosseum?", "id": "Vespasian." },
  { "en": "Apa Nama Gurun Pasir Terluas Di Amerika Selatan?", "id": "Gurun Patagonia." },
  { "en": "Berapa Jumlah Rusuk Pada Sebuah Limas Segi Lima?", "id": "10 Rusuk." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Perilaku?", "id": "Psikologi." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Kopi?", "id": "Ethiopia." },
  { "en": "Siapakah Ratu Inggris Yang Dijuluki 'The Virgin Queen'?", "id": "Ratu Elizabeth I." },
  { "en": "Apa Nama Unsur Kimia Terberat Yang Ada Di Alam?", "id": "Uranium." },
  { "en": "Apa Nama Titik Terendah Di Benua Amerika Utara?", "id": "Badwater Basin, Death Valley." },
  { "en": "Siapakah Yang Menciptakan Bahasa Pemrograman Python?", "id": "Guido Van Rossum." },
  { "en": "Apa Nama Festival Lentera Di Thailand?", "id": "Yi Peng Dan Loi Krathong." },
  { "en": "Huruf Apa Yang Melambangkan Angka 500 Dalam Romawi?", "id": "D." },
  { "en": "Apa Satuan Standar Internasional Untuk Tekanan?", "id": "Pascal." },
  { "en": "Siapakah Firaun Mesir Yang Terkenal Dengan Pertempuran Kadesh?", "id": "Ramses II." },
  { "en": "Apa Nama Sup Dingin Khas Spanyol?", "id": "Gazpacho." },
  { "en": "Apa Nama Jenis Rusa Terbesar Di Dunia?", "id": "Moose." },
  { "en": "Apa Akronim Untuk Graphics Interchange Format?", "id": "GIF." },
  { "en": "Berapa Jumlah Sumbu Simetri Pada Segitiga Sama Sisi?", "id": "3 Sumbu." },
  { "en": "Apa Nama Ibukota Negara Ekuador?", "id": "Quito." },
  { "en": "Siapa Penulis Novel The Lord Of The Rings?", "id": "J.R.R. Tolkien." },
  { "en": "Apa Istilah Untuk Studi Tentang Gua?", "id": "Speleologi." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional India?", "id": "Harimau Benggala." },
  { "en": "Apa Ibukota Provinsi Maluku Saat Ini?", "id": "Ambon." },
  { "en": "Siapa Nama Dewa Laut Dan Gempa Bumi Yunani?", "id": "Poseidon." },
  { "en": "Apa Senyawa Kimia Yang Menjadi Komponen Utama Cuka?", "id": "Asam Asetat." },
  { "en": "Tim Basket Mana Yang Paling Banyak Juara NBA?", "id": "Boston Celtics, LA Lakers." },
  { "en": "Apa Sebutan Untuk Peristiwa Bumi Menutupi Bulan?", "id": "Gerhana Bulan." },
  { "en": "Siapa Sultan Mataram Islam Yang Paling Terkenal?", "id": "Sultan Agung." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Membuat Jam?", "id": "Horologis." },
  { "en": "Dimana Lokasi Kota Kuno Pompeii Yang Terkubur?", "id": "Napoli, Italia." },
  { "en": "Berapa Jumlah Bilik Yang Dimiliki Jantung Ikan?", "id": "2 Bilik." },
  { "en": "Apa Istilah Untuk Rata-Rata Cuaca Jangka Panjang?", "id": "Iklim." },
  { "en": "Siapakah Bapak Genetika Yang Terkenal?", "id": "Gregor Mendel." },
  { "en": "Apa Sebutan Untuk Proses Penguraian Dengan Air?", "id": "Hidrolisis." },
  { "en": "Berapa Durasi Satu Babak Dalam Permainan Polo Air?", "id": "8 Menit." },
  { "en": "Apa Nama Alat Untuk Mengukur Intensitas Cahaya?", "id": "Light Meter." },
  { "en": "Siapakah Kaisar Romawi Yang Terkenal Kejam Dan Gila?", "id": "Caligula." },
  { "en": "Apa Sistem Pemerintahan Yang Berdasarkan Keturunan?", "id": "Monarki." },
  { "en": "Apa Akronim Untuk 'In Case You Missed It'?", "id": "ICYMI." },
  { "en": "Pada Tahun Berapa Revolusi Industri Dimulai?", "id": "Sekitar Tahun 1760." },
  { "en": "Siapa Seniman Yang Terkenal Dengan Lukisan Impresionisnya?", "id": "Claude Monet." },
  { "en": "Apa Nama Ibukota Negara Rumania?", "id": "Bucharest." },
  { "en": "Berapa Jumlah Bintang Pada Bendera Negara Australia?", "id": "6 Bintang." },
  { "en": "Apa Proses Penguapan Air Dari Permukaan Daun?", "id": "Transpirasi." },
  { "en": "Siapa Penjelajah Yang Membuktikan Bumi Itu Bulat?", "id": "Ekspedisi Magellan." },
  { "en": "Apa Nama Danau Kawah Terdalam Di Dunia?", "id": "Crater Lake, Oregon." },
  { "en": "Apa Hormon Yang Mengatur Keseimbangan Air Tubuh?", "id": "Hormon Antidiuretik (ADH)." },
  { "en": "Siapa Tokoh Pewayangan Yang Dijuluki 'Bima Suci'?", "id": "Bima." },
  { "en": "Di Negara Mana Festival Holi Dirayakan?", "id": "India, Nepal." },
  { "en": "Apa Nama Ibukota Negara Maroko?", "id": "Rabat." },
  { "en": "Siapa Pelukis Terkenal Yang Menciptakan Karya The Last Supper?", "id": "Leonardo Da Vinci." },
  { "en": "Apa Sebutan Untuk Studi Tentang Awan?", "id": "Nefologi." },
  { "en": "Berapa Jumlah Rusuk Pada Sebuah Kubus?", "id": "12 Rusuk." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Renault?", "id": "Perancis." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Au?", "id": "Emas (Aurum)." },
  { "en": "Siapakah Yang Menciptakan Tokoh Winnie-the-Pooh?", "id": "A. A. Milne." },
  { "en": "Apa Bahan Baku Utama Pembuatan Kaca Modern?", "id": "Pasir Silika." },
  { "en": "Kapan Peringatan Hari Remaja Internasional Diperingati?", "id": "12 Agustus." },
  { "en": "Apa Nama Proses Reaksi Kimia Menggunakan Cahaya?", "id": "Fotokimia." },
  { "en": "Siapakah Ratu Terakhir Dari Kerajaan Mesir Kuno?", "id": "Cleopatra VII." },
  { "en": "Apa Istilah Untuk Cedera Otot Yang Tegang?", "id": "Keseleo." },
  { "en": "Apa Nama Mata Uang Resmi Negara Afrika Selatan?", "id": "Rand Afrika Selatan." },
  { "en": "Dari Benua Manakah Asal Buah Kiwi?", "id": "Tiongkok, Asia." },
  { "en": "Proses Pendinginan Cepat Batuan Cair Di Permukaan?", "id": "Pembekuan Ekstrusif." },
  { "en": "Siapa Penulis Novel Horor The Shining?", "id": "Stephen King." },
  { "en": "Apa Nama Hormon Yang Dikenal Sebagai Hormon Pertumbuhan?", "id": "Somatotropin." },
  { "en": "Agama Manakah Yang Memiliki Kitab Suci Alkitab?", "id": "Kristen." },
  { "en": "Apa Sebutan Untuk Titik Rendah Gelombang Laut?", "id": "Lembah Gelombang." },
  { "en": "Berapa Jumlah Tulang Pada Pinggul Manusia?", "id": "2 Tulang." },
  { "en": "Siapakah Bapak Pendiri Negara Turki Modern?", "id": "Mustafa Kemal Ataturk." },
  { "en": "Apa Akronim Untuk Organisasi Kesehatan Dunia?", "id": "WHO (World Health Organization)." },
  { "en": "Di Kota Manakah Terdapat Jembatan Charles Yang Bersejarah?", "id": "Praha, Ceko." },
  { "en": "Hewan Apa Yang Dikenal Dapat Menyemprotkan Bau Busuk?", "id": "Sigung." },
  { "en": "Apa Nama Alat Yang Digunakan Untuk Memukul Bola Golf?", "id": "Stik Golf." },
  { "en": "Siapa Nama Dewi Pernikahan Dalam Mitologi Yunani?", "id": "Hera." },
  { "en": "Berapa Jam Perbedaan Waktu Antara WIB Dan WIT?", "id": "2 Jam." },
  { "en": "Apa Sebutan Untuk Kabut Yang Sangat Tebal?", "id": "Kabut Tebal." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Seoul?", "id": "Korea Selatan." },
  { "en": "Apa Nama Alat Musik Gesek Tradisional Tiongkok?", "id": "Erhu." },
  { "en": "Siapakah Yang Menemukan Teori Relativitas Umum?", "id": "Albert Einstein." },
  { "en": "Apa Nama Lapisan Atmosfer Tempat Lapisan Ozon Berada?", "id": "Stratosfer." },
  { "en": "Berapa Jumlah Total Hari Dalam Satu Tahun?", "id": "365 Hari." },
  { "en": "Jaringan Apa Yang Berfungsi Menopang Tumbuhan?", "id": "Jaringan Kolenkim." },
  { "en": "Negara Apa Yang Dijuluki Negeri K-Pop?", "id": "Korea Selatan." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Abstrak?", "id": "Wassily Kandinsky." },
  { "en": "Apa Nama Gas Yang Digunakan Dalam Las Karbit?", "id": "Asetilena." },
  { "en": "Apa Nama Gurun Terbesar Di Jazirah Arab?", "id": "Gurun Arab." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Anatomi Modern?", "id": "Andreas Vesalius." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Tulang?", "id": "Ortopedi." },
  { "en": "Berapa Jumlah Bidak Hitam Dalam Permainan Catur?", "id": "16 Bidak." },
  { "en": "Siapa Nama Penjahat Utama Dalam Dunia DC Comics?", "id": "Joker." },
  { "en": "Kekurangan Protein Dapat Menyebabkan Penyakit Apa?", "id": "Kwashiorkor." },
  { "en": "Apa Nama Istana Kepresidenan Di Korea Selatan?", "id": "Rumah Biru." },
  { "en": "Organ Apa Yang Mengontrol Suhu Tubuh Manusia?", "id": "Hipotalamus." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Menara Kembar Petronas?", "id": "Cesar Pelli." },
  { "en": "Apa Istilah Untuk Pelanggaran Pemain Dalam Sepak Bola?", "id": "Pelanggaran (Foul)." },
  { "en": "Dimana Lokasi St Basil's Cathedral Yang Berwarna-warni?", "id": "Moskow, Rusia." },
  { "en": "Apa Sebutan Untuk Studi Tentang Fungi?", "id": "Mikologi." },
  { "en": "Siapa Jenderal Vietnam Utara Yang Mengalahkan Perancis?", "id": "Vo Nguyen Giap." },
  { "en": "Apa Nama Danau Terbesar Di Benua Afrika?", "id": "Danau Victoria." },
  { "en": "Berapa Jumlah Sisi Pada Limas Segitiga?", "id": "4 Sisi." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Energi?", "id": "Termodinamika." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Teh?", "id": "Tiongkok." },
  { "en": "Siapakah Ratu Victoria Dari Kerajaan Inggris?", "id": "Ratu Inggris Raya." },
  { "en": "Apa Nama Batuan Yang Berasal Dari Magma?", "id": "Batuan Beku." },
  { "en": "Apa Nama Titik Tertinggi Di Benua Antartika?", "id": "Vinson Massif." },
  { "en": "Siapakah Yang Menciptakan Sistem Operasi Linux?", "id": "Linus Torvalds." },
  { "en": "Apa Nama Festival Seni Terbesar Di Dunia?", "id": "Edinburgh Festival Fringe." },
  { "en": "Huruf Apa Yang Melambangkan Angka 1000 Dalam Romawi?", "id": "M." },
  { "en": "Apa Satuan Standar Internasional Untuk Muatan Listrik?", "id": "Coulomb." },
  { "en": "Siapakah Kaisar Romawi Yang Membakar Kota Roma?", "id": "Nero." },
  { "en": "Apa Nama Keju Biru Terkenal Dari Perancis?", "id": "Roquefort." },
  { "en": "Apa Nama Ikan Tercepat Di Lautan?", "id": "Ikan Layaran." },
  { "en": "Apa Akronim Untuk Portable Document Format?", "id": "PDF." },
  { "en": "Berapa Jumlah Sumbu Simetri Pada Persegi Panjang?", "id": "2 Sumbu." },
  { "en": "Siapa Penulis Novel 1984 Yang Terkenal?", "id": "George Orwell." },
  { "en": "Apa Istilah Untuk Studi Tentang Gletser?", "id": "Glasiologi." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional Skotlandia?", "id": "Unicorn." },
  { "en": "Apa Ibukota Provinsi Nusa Tenggara Timur?", "id": "Kupang." },
  { "en": "Apa Senyawa Kimia Yang Membuat Tanaman Berfotosintesis?", "id": "Klorofil." },
  { "en": "Klub Sepak Bola Mana Yang Paling Banyak Juara Serie A?", "id": "Juventus." },
  { "en": "Apa Sebutan Untuk Peristiwa Matahari Di Atas Khatulistiwa?", "id": "Ekuinoks." },
  { "en": "Siapa Pendiri Kerajaan Singasari Yang Terkenal?", "id": "Ken Arok." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Filateli?", "id": "Filatelis." },
  { "en": "Dimana Lokasi Kota Petra Yang Bersejarah?", "id": "Yordania." },
  { "en": "Berapa Jumlah Ruang Yang Ada Di Jantung Amfibi?", "id": "3 Ruang." },
  { "en": "Apa Istilah Untuk Pergerakan Udara Vertikal?", "id": "Konveksi." },
  { "en": "Siapakah Bapak Mikrobiologi Yang Terkenal?", "id": "Antonie Van Leeuwenhoek." },
  { "en": "Apa Sebutan Untuk Proses Penguraian Dengan Oksigen?", "id": "Oksidasi." },
  { "en": "Berapa Durasi Satu Ronde Dalam Tinju Amatir?", "id": "3 Menit." },
  { "en": "Apa Nama Alat Untuk Mengukur Kecepatan Suara?", "id": "Fonométer." },
  { "en": "Siapakah Raja Makedonia Ayah Dari Alexander Agung?", "id": "Philip II." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Elit?", "id": "Aristokrasi." },
  { "en": "Apa Akronim Untuk 'Oh My God'?", "id": "OMG." },
  { "en": "Pada Tahun Berapa Revolusi Rusia Dimulai?", "id": "Tahun 1917." },
  { "en": "Siapa Seniman Yang Terkenal Dengan Lukisan Water Lilies?", "id": "Claude Monet." },
  { "en": "Apa Nama Ibukota Negara Hongaria?", "id": "Budapest." },
  { "en": "Berapa Jumlah Bintang Pada Bendera Selandia Baru?", "id": "4 Bintang." },
  { "en": "Apa Proses Pengendapan Partikel Sedimen?", "id": "Deposisi." },
  { "en": "Siapa Penjelajah Yang Mencapai Kutub Selatan Pertama?", "id": "Roald Amundsen." },
  { "en": "Apa Nama Danau Terdalam Di Amerika Utara?", "id": "Great Slave Lake." },
  { "en": "Apa Hormon Yang Dikenal Sebagai Hormon Darurat?", "id": "Adrenalin." },
  { "en": "Siapa Tokoh Pewayangan Yang Dijuluki 'Putra Sang Fajar'?", "id": "Karna." },
  { "en": "Di Negara Mana Festival Songkran Dirayakan?", "id": "Thailand." },
  { "en": "Apa Nama Ibukota Negara Ghana?", "id": "Accra." },
  { "en": "Siapa Pelukis Terkenal Yang Menciptakan Karya The Kiss?", "id": "Gustav Klimt." },
  { "en": "Apa Sebutan Untuk Studi Tentang Danau?", "id": "Limnologi." },
  { "en": "Berapa Jumlah Sisi Pada Sebuah Bangun Bola?", "id": "1 Sisi." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Saab?", "id": "Swedia." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Pb?", "id": "Timbal (Plumbum)." },
  { "en": "Siapakah Yang Menciptakan Tokoh Pippi Longstocking?", "id": "Astrid Lindgren." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Wiski?", "id": "Sereal Fermentasi." },
  { "en": "Kapan Peringatan Hari Demokrasi Internasional Diperingati?", "id": "15 September." },
  { "en": "Apa Nama Proses Pembentukan Jaringan Baru?", "id": "Histogenesis." },
  { "en": "Siapakah Presiden Pertama Republik Rakyat Tiongkok?", "id": "Mao Zedong." },
  { "en": "Apa Istilah Untuk Skor Seri Dalam Catur?", "id": "Remis." },
  { "en": "Apa Nama Mata Uang Resmi Negara Nigeria?", "id": "Naira Nigeria." },
  { "en": "Dari Benua Manakah Asal Buah Alpukat?", "id": "Amerika Tengah." },
  { "en": "Proses Perubahan Bentuk Batuan Oleh Tekanan Disebut?", "id": "Deformasi." },
  { "en": "Siapa Penulis Novel Animal Farm Yang Terkenal?", "id": "George Orwell." },
  { "en": "Apa Nama Hormon Yang Mengatur Tekanan Darah?", "id": "Aldosteron." },
  { "en": "Agama Manakah Yang Memiliki Kitab Suci Tao Te Ching?", "id": "Taoisme." },
  { "en": "Apa Sebutan Untuk Jalur Semu Matahari Di Langit?", "id": "Ekliptika." },
  { "en": "Berapa Jumlah Tulang Pada Tulang Belakang Manusia?", "id": "33 Tulang." },
  { "en": "Siapakah Jenderal Terkenal Dari Kekaisaran Mongol?", "id": "Subutai." },
  { "en": "Apa Akronim Untuk Dana Anak-Anak Perserikatan Bangsa-Bangsa?", "id": "UNICEF." },
  { "en": "Di Kota Manakah Terdapat Patung David Karya Michelangelo?", "id": "Florence, Italia." },
  { "en": "Hewan Apa Yang Dikenal Dapat Menumbuhkan Kembali Ekornya?", "id": "Cicak Dan Kadal." },
  { "en": "Apa Nama Bola Yang Digunakan Dalam Tenis Meja?", "id": "Bola Pingpong." },
  { "en": "Siapa Nama Dewi Kecantikan Dalam Mitologi Yunani?", "id": "Aphrodite." },
  { "en": "Berapa Jumlah Jam Dalam Satu Minggu?", "id": "168 Jam." },
  { "en": "Apa Sebutan Untuk Pusaran Angin Yang Kuat?", "id": "Tornado." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Amsterdam?", "id": "Belanda." },
  { "en": "Apa Nama Alat Musik Dawai Dari Timur Tengah?", "id": "Oud." },
  { "en": "Siapakah Yang Menemukan Elektron Pada Tahun 1897?", "id": "J. J. Thomson." },
  { "en": "Apa Nama Fenomena Optik Yang Menghasilkan Pelangi?", "id": "Dispersi Cahaya." },
  { "en": "Berapa Jumlah Provinsi Awal Kemerdekaan Indonesia?", "id": "8 Provinsi." },
  { "en": "Jaringan Apa Yang Berfungsi Mengisi Ruang Antar Jaringan?", "id": "Jaringan Ikat." },
  { "en": "Negara Apa Yang Dijuluki Negeri Naga Biru?", "id": "Wales." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Dadaisme?", "id": "Marcel Duchamp." },
  { "en": "Apa Nama Gas Yang Digunakan Dalam Minuman Bersoda?", "id": "Karbon Dioksida." },
  { "en": "Apa Nama Sungai Terpanjang Di Tiongkok?", "id": "Sungai Yangtze." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Imunologi?", "id": "Edward Jenner." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Ginjal?", "id": "Nefrologi." },
  { "en": "Berapa Jumlah Kartu Pada Permainan Bridge?", "id": "52 Kartu." },
  { "en": "Siapa Nama Pahlawan Super Yang Berasal Dari Krypton?", "id": "Superman." },
  { "en": "Kekurangan Kalsium Dapat Menyebabkan Penyakit Apa?", "id": "Osteoporosis." },
  { "en": "Apa Nama Museum Seni Terkenal Di Kota Paris?", "id": "Louvre." },
  { "en": "Organ Apa Yang Berfungsi Menyerap Nutrisi Makanan?", "id": "Usus Halus." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Burj Khalifa?", "id": "Adrian Smith." },
  { "en": "Apa Istilah Untuk Jatuhan Bola Diluar Garis Lapangan?", "id": "Out." },
  { "en": "Dimana Lokasi Taj Mahal Yang Ikonik Berada?", "id": "Agra, India." },
  { "en": "Apa Sebutan Untuk Studi Tentang Parasit?", "id": "Parasitologi." },
  { "en": "Siapa Pemimpin Terakhir Apartheid Di Afrika Selatan?", "id": "F. W. de Klerk." },
  { "en": "Apa Nama Danau Terbesar Di Amerika Selatan?", "id": "Danau Maracaibo." },
  { "en": "Berapa Jumlah Titik Sudut Pada Limas Segi Enam?", "id": "7 Titik Sudut." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Materi?", "id": "Kimia Dan Fisika." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Cokelat?", "id": "Meksiko." },
  { "en": "Siapakah Raja Perancis Yang Dijuluki 'Sun King'?", "id": "Louis XIV." },
  { "en": "Apa Nama Batuan Yang Terbentuk Dari Sedimen?", "id": "Batuan Sedimen." },
  { "en": "Apa Nama Titik Tertinggi Di Benua Eropa?", "id": "Gunung Elbrus." },
  { "en": "Siapakah Yang Menciptakan Sistem Operasi Microsoft Windows?", "id": "Bill Gates, Paul Allen." },
  { "en": "Apa Nama Festival Topeng Terkenal Di Venesia?", "id": "Karnaval Venesia." },
  { "en": "Apa Satuan Standar Internasional Untuk Jumlah Zat?", "id": "Mol." },
  { "en": "Siapakah Kaisar Romawi Yang Paling Terkenal Bijaksana?", "id": "Marcus Aurelius." },
  { "en": "Apa Nama Roti Khas Dari Negara Perancis?", "id": "Baguette." },
  { "en": "Apa Nama Hewan Marsupial Terbesar Di Dunia?", "id": "Kanguru Merah." },
  { "en": "Apa Akronim Untuk Joint Photographic Experts Group?", "id": "JPEG." },
  { "en": "Berapa Jumlah Diagonal Ruang Pada Sebuah Kubus?", "id": "4 Diagonal Ruang." },
  { "en": "Siapa Penulis Novel The Hobbit Yang Terkenal?", "id": "J.R.R. Tolkien." },
  { "en": "Apa Istilah Untuk Studi Tentang Koin Dan Uang?", "id": "Numismatik." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional Amerika Serikat?", "id": "Elang Botak." },
  { "en": "Apa Ibukota Provinsi Banten Saat Ini?", "id": "Serang." },
  { "en": "Siapa Nama Dewa Keadilan Dalam Mitologi Mesir?", "id": "Ma'at." },
  { "en": "Apa Senyawa Kimia Yang Menjadi Komponen Utama Gula?", "id": "Sukrosa." },
  { "en": "Tim Baseball Mana Yang Paling Banyak Juara World Series?", "id": "New York Yankees." },
  { "en": "Apa Sebutan Untuk Peristiwa Matahari Tepat Di Zenit?", "id": "Kulminasi." },
  { "en": "Siapa Pendiri Kerajaan Pajajaran Yang Terkenal?", "id": "Sri Jayabhupati." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Perangko?", "id": "Filatelis." },
  { "en": "Dimana Lokasi Pulau Paskah Yang Misterius?", "id": "Samudra Pasifik, Chili." },
  { "en": "Berapa Jumlah Ruang Yang Ada Di Jantung Reptil?", "id": "3 Atau 4 Ruang." },
  { "en": "Apa Istilah Untuk Angin Lokal Yang Kering?", "id": "Angin Fohn." },
  { "en": "Siapakah Bapak Botani Yang Terkenal?", "id": "Theophrastus." },
  { "en": "Apa Sebutan Untuk Proses Penguraian Dengan Panas?", "id": "Pirolisis." },
  { "en": "Berapa Durasi Maksimal Pertandingan Catur Klasik?", "id": "Beberapa Jam." },
  { "en": "Apa Nama Alat Untuk Mengukur Radiasi Elektromagnetik?", "id": "Radiometer." },
  { "en": "Siapakah Presiden Amerika Serikat Ke-16?", "id": "Abraham Lincoln." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Rakyat?", "id": "Demokrasi." },
  { "en": "Apa Akronim Untuk 'Rolling On The Floor Laughing'?", "id": "ROFL." },
  { "en": "Pada Tahun Berapa Revolusi Amerika Dimulai?", "id": "Tahun 1775." },
  { "en": "Siapa Seniman Yang Terkenal Dengan Patung Perunggunya?", "id": "Auguste Rodin." },
  { "en": "Berapa Jumlah Bintang Pada Bendera Negara Brazil?", "id": "27 Bintang." },
  { "en": "Apa Proses Penguapan Total Dari Permukaan Bumi?", "id": "Evapotranspirasi." },
  { "en": "Siapa Penjelajah Yang Mencapai Kutub Utara Pertama?", "id": "Robert Peary." },
  { "en": "Apa Nama Danau Garam Terbesar Di Belahan Barat?", "id": "Great Salt Lake." },
  { "en": "Apa Hormon Yang Dikenal Sebagai Hormon Kebahagiaan?", "id": "Endorfin." },
  { "en": "Siapa Tokoh Pewayangan Yang Memiliki Senjata Gada Rujakpolo?", "id": "Bima." },
  { "en": "Di Negara Mana Festival San Fermin Dirayakan?", "id": "Spanyol." },
  { "en": "Apa Nama Ibukota Negara Bulgaria?", "id": "Sofia." },
  { "en": "Siapa Komposer Terkenal Dengan Karya 'Für Elise'?", "id": "Ludwig Van Beethoven." },
  { "en": "Apa Sebutan Untuk Studi Tentang Lautan?", "id": "Oseanografi." },
  { "en": "Berapa Jumlah Sisi Pada Sebuah Limas Segi Empat?", "id": "5 Sisi." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Jaguar?", "id": "Inggris." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Hg?", "id": "Raksa (Hydrargyrum)." },
  { "en": "Siapakah Yang Menulis Buku Dongeng The Little Mermaid?", "id": "Hans Christian Andersen." },
  { "en": "Apa Bahan Baku Utama Pembuatan Minuman Anggur?", "id": "Buah Anggur Fermentasi." },
  { "en": "Kapan Peringatan Hari Penerjemahan Internasional Diperingati?", "id": "30 September." },
  { "en": "Apa Nama Proses Reproduksi Tanpa Fertilisasi?", "id": "Partenogenesis." },
  { "en": "Siapakah Kaisar Romawi Pertama Yang Beragama Kristen?", "id": "Konstantinus Agung." },
  { "en": "Apa Istilah Untuk Pukulan Kemenangan Dalam Tinju?", "id": "Knockout (KO)." },
  { "en": "Apa Nama Mata Uang Resmi Negara Uni Emirat Arab?", "id": "Dirham UEA." },
  { "en": "Dari Benua Manakah Asal Tanaman Jagung?", "id": "Amerika." },
  { "en": "Proses Pelepasan Energi Dari Batuan Bumi Disebut?", "id": "Peluruhan Radioaktif." },
  { "en": "Siapa Penulis Novel Fiksi Ilmiah Brave New World?", "id": "Aldous Huxley." },
  { "en": "Apa Nama Hormon Yang Mengatur Metabolisme Tubuh?", "id": "Tiroksin." },
  { "en": "Agama Manakah Yang Merayakan Hari Raya Idul Fitri?", "id": "Agama Islam." },
  { "en": "Apa Sebutan Untuk Kumpulan Bintang Yang Membentuk Pola?", "id": "Konstelasi Atau Rasi Bintang." },
  { "en": "Berapa Jumlah Ruas Tulang Ekor Pada Manusia?", "id": "4 Ruas." },
  { "en": "Siapakah Ratu Terakhir Dinasti Tudor Di Inggris?", "id": "Ratu Elizabeth I." },
  { "en": "Apa Akronim Untuk Organisasi Penerbangan Sipil Internasional?", "id": "ICAO." },
  { "en": "Di Kota Manakah Terdapat Katedral Notre Dame?", "id": "Paris, Perancis." },
  { "en": "Hewan Apa Yang Dikenal Sebagai 'Tikus Terbang'?", "id": "Kelelawar." },
  { "en": "Apa Nama Arena Pacuan Kuda Terkenal Di Inggris?", "id": "Ascot Racecourse." },
  { "en": "Siapa Nama Dewi Kebijaksanaan Dalam Mitologi Romawi?", "id": "Minerva." },
  { "en": "Berapa Jumlah Menit Dalam Satu Derajat Bujur?", "id": "60 Menit." },
  { "en": "Apa Sebutan Untuk Salju Yang Turun Sangat Lebat?", "id": "Badai Salju." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Bern?", "id": "Swiss." },
  { "en": "Apa Nama Alat Musik Petik Berbentuk Setengah Labu?", "id": "Mandolin." },
  { "en": "Siapakah Yang Menemukan Proton Dalam Inti Atom?", "id": "Ernest Rutherford." },
  { "en": "Apa Nama Fenomena Optik Yang Menghasilkan Fata Morgana?", "id": "Pembiasan Cahaya." },
  { "en": "Berapa Jumlah Kabupaten Dan Kota Di Indonesia?", "id": "514 (Data Bisa Berubah)." },
  { "en": "Jaringan Apa Yang Membawa Makanan Pada Tumbuhan?", "id": "Jaringan Floem." },
  { "en": "Negara Apa Yang Dijuluki Negeri Tulip?", "id": "Belanda." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Futurisme?", "id": "Filippo Tommaso Marinetti." },
  { "en": "Apa Nama Gas Yang Berbau Seperti Buah Busuk?", "id": "Gas Etilena." },
  { "en": "Apa Nama Sungai Terpanjang Di Semenanjung Iberia?", "id": "Sungai Tagus." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Genetika?", "id": "Gregor Mendel." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Kelenjar Endokrin?", "id": "Endokrinologi." },
  { "en": "Berapa Jarak Lari Gawang Putra Dalam Atletik?", "id": "110 Meter." },
  { "en": "Apa Nama Ibukota Negara Skotlandia?", "id": "Edinburgh." },
  { "en": "Siapa Nama Musuh Bebuyutan Spider-Man Yang Berwarna Hijau?", "id": "Green Goblin." },
  { "en": "Kekurangan Fosfor Dapat Menyebabkan Masalah Apa?", "id": "Kelemahan Tulang." },
  { "en": "Apa Nama Jembatan Gantung Terkenal Di Brooklyn?", "id": "Jembatan Brooklyn." },
  { "en": "Organ Apa Yang Berfungsi Sebagai Tempat Produksi Insulin?", "id": "Pankreas." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Sydney Opera House?", "id": "Jørn Utzon." },
  { "en": "Apa Istilah Untuk Gol Bunuh Diri Dalam Sepak Bola?", "id": "Gol Bunuh Diri (Own Goal)." },
  { "en": "Dimana Lokasi Tembok Hadrian Yang Bersejarah?", "id": "Inggris." },
  { "en": "Apa Sebutan Untuk Studi Tentang Serangga?", "id": "Entomologi." },
  { "en": "Siapa Panglima Tertinggi Pasukan Vietnam Utara?", "id": "Vo Nguyen Giap." },
  { "en": "Apa Nama Danau Terbesar Di Amerika Tengah?", "id": "Danau Nikaragua." },
  { "en": "Berapa Jumlah Titik Sudut Pada Prisma Segi Delapan?", "id": "16 Titik Sudut." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Bumi?", "id": "Geologi." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Kertas Toilet?", "id": "Tiongkok." },
  { "en": "Siapakah Ratu Perancis Terakhir Sebelum Revolusi?", "id": "Marie Antoinette." },
  { "en": "Apa Nama Batuan Yang Berubah Bentuk Karena Panas?", "id": "Batuan Metamorf." },
  { "en": "Apa Nama Titik Tertinggi Di Benua Amerika Selatan?", "id": "Gunung Aconcagua." },
  { "en": "Siapakah Yang Menciptakan Email Pertama Kali?", "id": "Ray Tomlinson." },
  { "en": "Apa Nama Festival Pelepasan Sapi Jantan Di Spanyol?", "id": "San Fermín." },
  { "en": "Apa Satuan Standar Internasional Untuk Intensitas Cahaya?", "id": "Candela." },
  { "en": "Siapakah Kaisar Mongol Yang Mendirikan Dinasti Yuan?", "id": "Kublai Khan." },
  { "en": "Apa Nama Hidangan Daging Asap Khas Amerika?", "id": "Barbecue." },
  { "en": "Apa Nama Hewan Darat Paling Berbisa Di Dunia?", "id": "Ular Taipan Pedalaman." },
  { "en": "Apa Akronim Untuk International Atomic Energy Agency?", "id": "IAEA." },
  { "en": "Berapa Jumlah Sisi Sejajar Pada Trapesium?", "id": "2 Sisi." },
  { "en": "Apa Nama Ibukota Negara Kroasia?", "id": "Zagreb." },
  { "en": "Siapa Penulis Novel Pride And Prejudice?", "id": "Jane Austen." },
  { "en": "Apa Istilah Untuk Studi Tentang Cap Jempol?", "id": "Daktiloskopi." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional Thailand?", "id": "Gajah." },
  { "en": "Apa Ibukota Provinsi Aceh Saat Ini?", "id": "Banda Aceh." },
  { "en": "Siapa Nama Dewa Bawah Laut Dalam Mitologi Nordik?", "id": "Aegir." },
  { "en": "Klub Hoki Es Mana Yang Paling Banyak Juara Stanley Cup?", "id": "Montreal Canadiens." },
  { "en": "Apa Sebutan Untuk Peristiwa Matahari Terjauh Dari Khatulistiwa?", "id": "Solstis." },
  { "en": "Siapa Pahlawan Nasional Dari Maluku Yang Terkenal?", "id": "Pattimura." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Kaligrafi?", "id": "Kaligrafer." },
  { "en": "Dimana Lokasi Air Terjun Iguazu Yang Spektakuler?", "id": "Argentina, Brasil." },
  { "en": "Berapa Jumlah Ruang Yang Ada Di Jantung Burung?", "id": "4 Ruang." },
  { "en": "Apa Istilah Untuk Pergerakan Lempeng Bumi Saling Menjauh?", "id": "Divergen." },
  { "en": "Siapakah Bapak Taksonomi Yang Mengklasifikasikan Makhluk Hidup?", "id": "Carolus Linnaeus." },
  { "en": "Apa Sebutan Untuk Proses Pelepasan Energi Tanpa Oksigen?", "id": "Fermentasi." },
  { "en": "Berapa Jarak Lari Estafet Dalam Kompetisi Resmi?", "id": "4x100 Dan 4x400 Meter." },
  { "en": "Apa Nama Alat Untuk Mengukur Tekanan Gas Tertutup?", "id": "Manometer." },
  { "en": "Siapakah Raja Babilonia Yang Membangun Taman Gantung?", "id": "Nebukadnezar II." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Satu Partai?", "id": "Sistem Satu Partai." },
  { "en": "Apa Akronim Untuk 'As Far As I Know'?", "id": "AFAIK." },
  { "en": "Pada Tahun Berapa Tembok Berlin Runtuh?", "id": "Tahun 1989." },
  { "en": "Siapa Seniman Terkenal Yang Melukis Campbell's Soup Cans?", "id": "Andy Warhol." },
  { "en": "Apa Nama Ibukota Negara Ukraina?", "id": "Kyiv." },
  { "en": "Berapa Jumlah Bintang Pada Bendera Negara Vietnam?", "id": "1 Bintang." },
  { "en": "Apa Proses Pembentukan Gua Oleh Air?", "id": "Pelarutan." },
  { "en": "Siapa Penjelajah Yang Mencapai Puncak Gunung Everest Pertama?", "id": "Edmund Hillary, Tenzing Norgay." },
  { "en": "Apa Nama Lembah Terdalam Di Dunia?", "id": "Lembah Yarlung Tsangpo." },
  { "en": "Apa Hormon Yang Dihasilkan Oleh Kelenjar Tiroid?", "id": "Tiroksin." },
  { "en": "Siapa Tokoh Pewayangan Yang Dijuluki 'Dewi Keadilan'?", "id": "Dewi Kunti." },
  { "en": "Di Negara Mana Festival Balon Udara Terbesar Diadakan?", "id": "Albuquerque, Amerika Serikat." },
  { "en": "Apa Nama Ibukota Negara Aljazair?", "id": "Algiers." },
  { "en": "Siapa Pelukis Terkenal Yang Menciptakan Karya The Anatomy Lesson?", "id": "Rembrandt." },
  { "en": "Apa Sebutan Untuk Studi Tentang Rawa?", "id": "Limnologi." },
  { "en": "Berapa Jumlah Sisi Pada Sebuah Prisma Segi Enam?", "id": "8 Sisi." },
  { "en": "Dari Negara Manakah Asal Merek Mobil Aston Martin?", "id": "Inggris." },
  { "en": "Apa Nama Unsur Kimia Dengan Simbol Pt?", "id": "Platina (Platinum)." },
  { "en": "Siapakah Yang Menulis Buku Petualangan The Wizard of Oz?", "id": "L. Frank Baum." },
  { "en": "Apa Bahan Baku Utama Pembuatan Kertas Daur Ulang?", "id": "Kertas Bekas." },
  { "en": "Kapan Peringatan Hari Pangan Sedunia Diperingati?", "id": "16 Oktober." },
  { "en": "Apa Nama Proses Penyesuaian Diri Makhluk Hidup?", "id": "Adaptasi." },
  { "en": "Siapakah Yang Dianggap Kaisar Pertama Kekaisaran Romawi?", "id": "Augustus." },
  { "en": "Apa Istilah Untuk Poin Terakhir Dalam Babak Tie-Break?", "id": "Set Point." },
  { "en": "Apa Nama Mata Uang Resmi Negara Israel?", "id": "Shekel Baru Israel." },
  { "en": "Dari Benua Manakah Asal Tanaman Vanila?", "id": "Amerika Tengah." },
  { "en": "Proses Pengerasan Sedimen Menjadi Batuan Disebut?", "id": "Lifikasi." },
  { "en": "Siapa Penulis Novel The Picture Of Dorian Gray?", "id": "Oscar Wilde." },
  { "en": "Apa Nama Hormon Yang Dihasilkan Kelenjar Adrenal?", "id": "Adrenalin." },
  { "en": "Agama Manakah Yang Merayakan Hari Raya Hanukkah?", "id": "Agama Yahudi." },
  { "en": "Apa Sebutan Untuk Galaksi Tempat Tata Surya Kita?", "id": "Galaksi Bima Sakti." },
  { "en": "Berapa Jumlah Tulang Kelangkang Pada Manusia?", "id": "1 Tulang (5 menyatu)." },
  { "en": "Siapakah Presiden Kulit Hitam Pertama Afrika Selatan?", "id": "Nelson Mandela." },
  { "en": "Apa Akronim Untuk Dana Moneter Internasional PBB?", "id": "IMF (International Monetary Fund)." },
  { "en": "Di Kota Manakah Terdapat Jembatan Golden Gate?", "id": "San Francisco, Amerika." },
  { "en": "Hewan Apa Yang Dikenal Memiliki Cangkang Keras?", "id": "Kura-Kura Dan Penyu." },
  { "en": "Apa Nama Trofi Turnamen Tenis Wimbledon?", "id": "Challenge Cup, Venus Rosewater Dish." },
  { "en": "Siapa Nama Dewa Api Dalam Mitologi Yunani?", "id": "Hephaestus." },
  { "en": "Berapa Jumlah Zona Waktu Utama Di Amerika Serikat?", "id": "6 Zona Waktu." },
  { "en": "Apa Sebutan Untuk Siklus Air Di Planet Bumi?", "id": "Siklus Hidrologi." },
  { "en": "Negara Manakah Yang Memiliki Ibukota Di Canberra?", "id": "Australia." },
  { "en": "Apa Nama Alat Musik Petik Khas Dari India?", "id": "Sitar." },
  { "en": "Siapakah Yang Menemukan Neutron Dalam Inti Atom?", "id": "James Chadwick." },
  { "en": "Apa Nama Fenomena Optik Lingkaran Cahaya Mengelilingi Matahari?", "id": "Halo." },
  { "en": "Berapa Jumlah Huruf Dalam Aksara Arab?", "id": "28 Huruf." },
  { "en": "Jaringan Apa Yang Berfungsi Melindungi Biji?", "id": "Kulit Biji." },
  { "en": "Negara Apa Yang Dijuluki Negeri Paman Sam?", "id": "Amerika Serikat." },
  { "en": "Siapa Seniman Yang Mempelopori Gerakan Surealisme?", "id": "Andre Breton." },
  { "en": "Apa Nama Gas Yang Digunakan Untuk Fotosintesis?", "id": "Karbon Dioksida." },
  { "en": "Apa Nama Sungai Terpanjang Di Kepulauan Inggris?", "id": "Sungai Severn." },
  { "en": "Siapakah Yang Dikenal Sebagai Bapak Bom Hidrogen?", "id": "Edward Teller." },
  { "en": "Apa Cabang Kedokteran Yang Menangani Sistem Pernapasan?", "id": "Pulmonologi." },
  { "en": "Berapa Skor Tertinggi Dalam Satu Lemparan Boling?", "id": "10 Poin (Strike)." },
  { "en": "Apa Nama Ibukota Negara Qatar?", "id": "Doha." },
  { "en": "Siapa Nama Tokoh Penjahat Dalam Film Star Wars?", "id": "Darth Vader." },
  { "en": "Kekurangan Vitamin B3 Dapat Menyebabkan Penyakit Apa?", "id": "Pellagra." },
  { "en": "Apa Nama Patung Terkenal Di Pulau Paskah?", "id": "Moai." },
  { "en": "Organ Apa Yang Berfungsi Sebagai Tempat Penyimpanan Darah?", "id": "Limpa." },
  { "en": "Siapakah Arsitek Terkenal Yang Merancang Gedung Putih?", "id": "James Hoban." },
  { "en": "Apa Istilah Untuk Pelanggaran Awal Start Lari?", "id": "Start Salah (False Start)." },
  { "en": "Dimana Lokasi Piramida Agung Giza Berada?", "id": "Giza, Mesir." },
  { "en": "Apa Sebutan Untuk Studi Tentang Alga?", "id": "Fikologi." },
  { "en": "Siapa Pemimpin Revolusi Bolshevik Di Rusia?", "id": "Vladimir Lenin." },
  { "en": "Apa Nama Danau Terbesar Di Dunia Berdasarkan Volume?", "id": "Danau Baikal." },
  { "en": "Berapa Jumlah Rusuk Pada Sebuah Prisma Segi Delapan?", "id": "24 Rusuk." },
  { "en": "Apa Sebutan Untuk Ilmu Yang Mempelajari Lingkungan?", "id": "Ekologi." },
  { "en": "Negara Mana Yang Merupakan Asal Mula Angka Nol?", "id": "India." },
  { "en": "Siapakah Ratu Skotlandia Yang Dieksekusi Di Inggris?", "id": "Mary, Ratu Skotlandia." },
  { "en": "Apa Nama Batuan Yang Berasal Dari Pendinginan Lava?", "id": "Batuan Beku Ekstrusif." },
  { "en": "Apa Nama Titik Tertinggi Di Benua Amerika Utara?", "id": "Denali (Gunung McKinley)." },
  { "en": "Siapakah Yang Menciptakan Merek Apple Computer?", "id": "Steve Jobs, Steve Wozniak." },
  { "en": "Apa Nama Festival Seni Membakar Patung Pria?", "id": "Burning Man." },
  { "en": "Huruf Apa Yang Melambangkan Angka 100 Dalam Romawi?", "id": "C." },
  { "en": "Apa Satuan Standar Internasional Untuk Suhu Termodinamika?", "id": "Kelvin." },
  { "en": "Siapakah Kaisar Romawi Terakhir Dari Kekaisaran Barat?", "id": "Romulus Augustulus." },
  { "en": "Apa Nama Sup Daging Khas Dari Hungaria?", "id": "Goulash." },
  { "en": "Apa Nama Hewan Darat Paling Lambat Di Dunia?", "id": "Kungkang." },
  { "en": "Apa Akronim Untuk Asynchronous JavaScript And XML?", "id": "AJAX." },
  { "en": "Berapa Jumlah Titik Sudut Pada Sebuah Balok?", "id": "8 Titik Sudut." },
  { "en": "Apa Nama Ibukota Negara Mongolia?", "id": "Ulaanbaatar." },
  { "en": "Siapa Penulis Novel The Old Man And The Sea?", "id": "Ernest Hemingway." },
  { "en": "Apa Istilah Untuk Studi Tentang Prangko?", "id": "Filateli." },
  { "en": "Hewan Apa Yang Menjadi Simbol Nasional Finlandia?", "id": "Beruang Coklat." },
  { "en": "Apa Ibukota Provinsi Sulawesi Tengah?", "id": "Palu." },
  { "en": "Siapa Nama Dewa Anggur Dalam Mitologi Romawi?", "id": "Bacchus." },
  { "en": "Apa Senyawa Kimia Yang Menjadi Komponen Utama Batu Kapur?", "id": "Kalsium Karbonat." },
  { "en": "Tim Formula 1 Mana Yang Paling Sukses?", "id": "Scuderia Ferrari." },
  { "en": "Apa Sebutan Untuk Titik Terdekat Matahari Ke Pusat Galaksi?", "id": "Perigalacticon." },
  { "en": "Siapa Pahlawan Nasional Yang Dijuluki Ayam Jantan Dari Timur?", "id": "Sultan Hasanuddin." },
  { "en": "Apa Sebutan Untuk Orang Yang Ahli Numismatik?", "id": "Numismatis." },
  { "en": "Dimana Lokasi Air Terjun Niagara Yang Terkenal?", "id": "Amerika Serikat, Kanada." },
  { "en": "Berapa Jumlah Otot Yang Ada Di Tubuh Manusia?", "id": "Lebih Dari 600 Otot." },
  { "en": "Apa Istilah Untuk Pergerakan Lempeng Bumi Saling Bertumbukan?", "id": "Konvergen." },
  { "en": "Siapakah Bapak Fisiologi Eksperimental Yang Terkenal?", "id": "Claude Bernard." },
  { "en": "Apa Sebutan Untuk Proses Penguraian Senyawa Oleh Listrik?", "id": "Elektrolisis." },
  { "en": "Berapa Jarak Lari Sprint Terpendek Dalam Olimpiade?", "id": "100 Meter." },
  { "en": "Apa Nama Alat Untuk Mengukur Gelombang Otak?", "id": "Elektroensefalograf (EEG)." },
  { "en": "Siapakah Raja Mesir Kuno Yang Membangun Piramida Agung?", "id": "Khufu." },
  { "en": "Apa Sistem Pemerintahan Yang Dipimpin Oleh Tuhan?", "id": "Teokrasi." },
  { "en": "Apa Akronim Untuk 'Talk To You Later'?", "id": "TTYL." },
  { "en": "Pada Tahun Berapa Revolusi Kuba Berakhir?", "id": "Tahun 1959." },
  { "en": "Siapa Seniman Yang Terkenal Dengan Lukisan Abstraknya?", "id": "Wassily Kandinsky." },
  { "en": "Apa Nama Ibukota Negara Kazakhstan?", "id": "Astana." },
  { "en": "Berapa Jumlah Warna Pada Bendera Afrika Selatan?", "id": "6 Warna." },
  { "en": "Apa Proses Pembentukan Tanah Dari Batuan?", "id": "Pelapukan." },
  { "en": "Siapa Penjelajah Yang Memberi Nama Kepulauan Filipina?", "id": "Ruy Lopez de Villalobos." },
  { "en": "Apa Nama Lembah Retakan Terbesar Di Dunia?", "id": "Lembah Retakan Besar." },
  { "en": "Apa Hormon Yang Dihasilkan Kelenjar Pituitari?", "id": "Hormon Pertumbuhan." },
  { "en": "Siapa Tokoh Pewayangan Yang Memiliki Ajian Pancasona?", "id": "Gatotkaca." },
  { "en": "Di Negara Mana Festival Día de los Muertos Dirayakan?", "id": "Meksiko." }


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
