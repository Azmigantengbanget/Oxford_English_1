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


    { "en": "Absorb?", "id": "Menyerap." },
    { "en": "Abstract?", "id": "Abstrak." },
    { "en": "Accent?", "id": "Aksen." },
    { "en": "Accidentally?", "id": "Secara tidak sengaja." },
    { "en": "Accommodate?", "id": "Mengakomodasi." },
    { "en": "Accomplish?", "id": "Mencapai." },
    { "en": "Accountant?", "id": "Akuntan." },
    { "en": "Accuracy?", "id": "Akurasi." },
    { "en": "Accurately?", "id": "Dengan akurat." },
    { "en": "Acid?", "id": "Asam." },
    { "en": "Activate?", "id": "Mengaktifkan." },
    { "en": "Addiction?", "id": "Kecanduan." },
    { "en": "Additionally?", "id": "Selain itu." },
    { "en": "Adequate?", "id": "Memadai." },
    { "en": "Adequately?", "id": "Dengan memadai." },
    { "en": "Adjust?", "id": "Menyesuaikan." },
    { "en": "Affordable?", "id": "Terjangkau." },
    { "en": "Agriculture?", "id": "Pertanian." },
    { "en": "AIDS?", "id": "AIDS." },
    { "en": "Alien?", "id": "Makhluk asing." },
    { "en": "Alongside?", "id": "Di samping." },
    { "en": "Altogether?", "id": "Secara keseluruhan." },
    { "en": "Ambulance?", "id": "Ambulans." },
    { "en": "Amusing?", "id": "Menghibur." },
    { "en": "Analyst?", "id": "Analis." },
    { "en": "Ancestor?", "id": "Leluhur." },
    { "en": "Animation?", "id": "Animasi." },
    { "en": "Annually?", "id": "Setiap tahun." },
    { "en": "Anticipate?", "id": "Mengantisipasi." },
    { "en": "Anxiety?", "id": "Kecemasan." },
    { "en": "Apology?", "id": "Permintaan maaf." },
    { "en": "Applicant?", "id": "Pelamar." },
    { "en": "Appropriately?", "id": "Secara tepat." },
    { "en": "Arrow?", "id": "Panah." },
    { "en": "Artwork?", "id": "Karya seni." },
    { "en": "Aside?", "id": "Di samping." },
    { "en": "Asset?", "id": "Aset." },
    { "en": "Assign?", "id": "Menugaskan." },
    { "en": "Assistance?", "id": "Bantuan." },
    { "en": "Assumption?", "id": "Asumsi." },
    { "en": "Assure?", "id": "Meyakinkan." },
    { "en": "Astonishing?", "id": "Mencengangkan." },
    { "en": "Attachment?", "id": "Lampiran." },
    { "en": "Auction?", "id": "Lelang." },
    { "en": "Audio?", "id": "Audio." },
    { "en": "Automatic?", "id": "Otomatis." },
    { "en": "Automatically?", "id": "Secara otomatis." },
    { "en": "Awareness?", "id": "Kesadaran." },
    { "en": "Awkward?", "id": "Canggung." },
    { "en": "Badge?", "id": "Lencana." },
    { "en": "Balanced?", "id": "Seimbang." },
    { "en": "Ballet?", "id": "Balet." },
    { "en": "Balloon?", "id": "Balon." },
    { "en": "Barely?", "id": "Hampir tidak." },
    { "en": "Bargain?", "id": "Tawar menawar." },
    { "en": "Basement?", "id": "Ruang bawah tanah." },
    { "en": "Basket?", "id": "Keranjang." },
    { "en": "Bat?", "id": "Kelelawar." },
    { "en": "Beneficial?", "id": "Bermanfaat." },
    { "en": "Beside?", "id": "Di samping." },
    { "en": "Besides?", "id": "Selain itu." },
    { "en": "Bias?", "id": "Bias." },
    { "en": "Bid?", "id": "Tawaran, menawar." },
    { "en": "Biological?", "id": "Biologis." },
    { "en": "Blanket?", "id": "Selimut." },
    { "en": "Blow?", "id": "Pukulan/tiupan." },
    { "en": "Bold?", "id": "Berani/tebal." },
    { "en": "Bombing?", "id": "Pemboman." },
    { "en": "Booking?", "id": "Pemesanan." },
    { "en": "Boost?", "id": "Meningkatkan, dorongan." },
    { "en": "Bound?", "id": "Terikat." },
    { "en": "Brick?", "id": "Batu bata." },
    { "en": "Briefly?", "id": "Secara singkat." },
    { "en": "Broadcaster?", "id": "Penyiar." },
    { "en": "Broadly?", "id": "Secara luas." },
    { "en": "Bug?", "id": "Serangga/kerusakan." },
    { "en": "Cabin?", "id": "Kabin." },
    { "en": "Canal?", "id": "Kanal/saluran air." },
    { "en": "Candle?", "id": "Lilin." },
    { "en": "Carbon?", "id": "Karbon." },
    { "en": "Casual?", "id": "Santai/tidak resmi." },
    { "en": "Cave?", "id": "Gua." },
    { "en": "Certainty?", "id": "Kepastian." },
    { "en": "Certificate?", "id": "Sertifikat." },
    { "en": "Challenging?", "id": "Menantang." },
    { "en": "Championship?", "id": "Kejuaraan." },
    { "en": "Charming?", "id": "Menawan." },
    { "en": "Chase?", "id": "Mengejar, pengejaran." },
    { "en": "Cheek?", "id": "Pipi." },
    { "en": "Cheer?", "id": "Bersorak, sorakan." },
    { "en": "Choir?", "id": "Paduan suara." },
    { "en": "Chop?", "id": "Mencincang." },
    { "en": "Circuit?", "id": "Sirkuit/rangkaian." },
    { "en": "Civilization?", "id": "Peradaban." },
    { "en": "Clarify?", "id": "Menjelaskan." },
    { "en": "Classify?", "id": "Mengklasifikasikan." },
    { "en": "Clerk?", "id": "Pegawai/penjaga toko." },
    { "en": "Cliff?", "id": "Tebing." },
    { "en": "Clinic?", "id": "Klinik." },
    { "en": "Clip?", "id": "Klip/cuplikan." },
    { "en": "Coincidence?", "id": "Kebetulan." },
    { "en": "Collector?", "id": "Kolektor." },
    { "en": "Colony?", "id": "Koloni." },
    { "en": "Colourful?", "id": "Berwarna-warni." },
    { "en": "Comic?", "id": "Lucu, komik." },
    { "en": "Commander?", "id": "Komandan." },
    { "en": "Comparative?", "id": "Perbandingan." },
    { "en": "Completion?", "id": "Penyelesaian." },
    { "en": "Compose?", "id": "Menyusun." },
    { "en": "Composer?", "id": "Komposer/pencipta lagu." },
    { "en": "Compound?", "id": "Gabungan/senyawa." },
    { "en": "Comprehensive?", "id": "Menyeluruh." },
    { "en": "Comprise?", "id": "Mencakup." },
    { "en": "Compulsory?", "id": "Wajib." },
    { "en": "Concrete?", "id": "Konkret, beton." },
    { "en": "Confess?", "id": "Mengakui/mengaku." },
    { "en": "Confusion?", "id": "Kebingungan." },
    { "en": "Consequently?", "id": "Akibatnya." },
    { "en": "Conservation?", "id": "Konservasi." },
    { "en": "Considerable?", "id": "Cukup besar." },
    { "en": "Considerably?", "id": "Secara signifikan." },
    { "en": "Consistently?", "id": "Secara konsisten." },
    { "en": "Conspiracy?", "id": "Konspirasi." },
    { "en": "Consult?", "id": "Berkonsultasi." },
    { "en": "Consultant?", "id": "Konsultan." },
    { "en": "Consumption?", "id": "Konsumsi." },
    { "en": "Controversial?", "id": "Kontroversi." },
    { "en": "Controversy?", "id": "Kontroversial." },
    { "en": "Convenience?", "id": "Kenyamanan." },
    { "en": "Convention?", "id": "Konvensi." },
    { "en": "Conventional?", "id": "Konvensional." },
    { "en": "Convey?", "id": "Menyampaikan." },
    { "en": "Convincing?", "id": "Meyakinkan." },
    { "en": "Cope?", "id": "Mengatasi." },
    { "en": "Corporation?", "id": "Korporasi." },
    { "en": "Corridor?", "id": "Koridor." },
    { "en": "Coverage?", "id": "Cakupan." },
    { "en": "Crack?", "id": "Meretakkan, retakan." },
    { "en": "Craft?", "id": "Kerajinan." },
    { "en": "Creativity?", "id": "Kreatifitas." },
    { "en": "Critically?", "id": "Secara kritis." },
    { "en": "Cruise?", "id": "Kapal pesiar, berlayar." },
    { "en": "Cue?", "id": "Isyarat." },
    { "en": "Curious?", "id": "Penasaran." },
    { "en": "Curriculum?", "id": "Kurikulum." },
    { "en": "Cute?", "id": "Lucu." },
    { "en": "Dairy?", "id": "Perusahaan susu." },
    { "en": "Dare?", "id": "Berani." },
    { "en": "Darkness?", "id": "Kegelapan." },
    { "en": "Database?", "id": "Basis data." },
    { "en": "Deadline?", "id": "Batas waktu." },
    { "en": "Deadly?", "id": "Mematikan." },
    { "en": "Dealer?", "id": "Pedagang." },
    { "en": "Deck?", "id": "Dek." },
    { "en": "Defender?", "id": "Pembela." },
    { "en": "Delete?", "id": "Menghapus." },
    { "en": "Democracy?", "id": "Demokrasi." },
    { "en": "Democratic?", "id": "Demokratis." },
    { "en": "Demonstration?", "id": "Demonstrasi." },
    { "en": "Depart?", "id": "Berangkat." },
    { "en": "Dependent?", "id": "Bergantung." },
    { "en": "Deposit?", "id": "Setoran." },
    { "en": "Depression?", "id": "Depresi." },
    { "en": "Derive?", "id": "Berasal." },
    { "en": "Desperately?", "id": "Dengan putus asa." },
    { "en": "Destruction?", "id": "Kehancuran." },
    { "en": "Determination?", "id": "Tekad." },
    { "en": "Devote?", "id": "Mengabdikan." },
    { "en": "Differ?", "id": "Berbeda." },
    { "en": "Disability?", "id": "Disabilitas." },
    { "en": "Disabled?", "id": "Penyandang disabilitas." },
    { "en": "Disagreement?", "id": "Ketidaksepakatan." },
    { "en": "Disappoint?", "id": "Mengecewakan." },
    { "en": "Disappointment?", "id": "Kekecewaan." },
    { "en": "Discourage?", "id": "Melemahkan semangat." },
    { "en": "Disorder?", "id": "Gangguan." },
    { "en": "Distant?", "id": "Jauh." },
    { "en": "Distinct?", "id": "Berbeda." },
    { "en": "Distinguish?", "id": "Membedakan." },
    { "en": "Distract?", "id": "Mengalihkan perhatian." },
    { "en": "Disturb?", "id": "Mengganggu." },
    { "en": "Dive?", "id": "Menyelam, penyelaman." },
    { "en": "Diverse?", "id": "Beragam." },
    { "en": "Diversity?", "id": "Keberagaman." },
    { "en": "Divorce?", "id": "Perceraian, bercerai." },
    { "en": "Dominant?", "id": "Dominan." },
    { "en": "Donation?", "id": "Donasi." },
    { "en": "Dot?", "id": "Titik." },
    { "en": "Downtown?", "id": "Pusat kota." },
    { "en": "Dramatically?", "id": "Secara dramastis." },
    { "en": "Drought?", "id": "Kekeringan." },
    { "en": "Dull?", "id": "Membosankan." },
    { "en": "Dump?", "id": "Membuang." },
    { "en": "Duration?", "id": "Durasi." },
    { "en": "Dynamic?", "id": "Dinamis." },
    { "en": "Economics?", "id": "Ilmu ekonomi." },
    { "en": "Economist?", "id": "Ekonom." },
    { "en": "Editorial?", "id": "Editoral." },
    { "en": "Efficiently?", "id": "Secara efisien." },
    { "en": "Elbow?", "id": "Siku." },
    { "en": "Electronics?", "id": "Elektronik." },
    { "en": "Elegant?", "id": "Elegan." },
    { "en": "Elementary?", "id": "Dasar." },
    { "en": "Eliminate?", "id": "Menghilangkan." },
    { "en": "Embrace?", "id": "Merangkul." },
    { "en": "Emission?", "id": "Emisi." },
    { "en": "Emotionally?", "id": "Secara emosional." },
    { "en": "Empire?", "id": "Kekaisaran." },
    { "en": "Enjoyable?", "id": "Menyenangkan." },
    { "en": "Entertaining?", "id": "Menghibur." },
    { "en": "Entrepreneur?", "id": "Pengusaha." },
    { "en": "Envelope?", "id": "Amplop." },
    { "en": "Equip?", "id": "Melengkapi." },
    { "en": "Equivalent?", "id": "Kesetaraan, setara." },
    { "en": "Era?", "id": "Era." },
    { "en": "Erupt?", "id": "Meletus." },
    { "en": "Essentially?", "id": "Pada dasarnya." },
    { "en": "Ethic?", "id": "Etika." },
    { "en": "Ethnic?", "id": "Etnis." },
    { "en": "Evaluation?", "id": "Evaluasi." },
    { "en": "Evident?", "id": "Jelas." },
    { "en": "Evolution?", "id": "Evolusi." },
    { "en": "Evolve?", "id": "Berkembang." },
    { "en": "Exceed?", "id": "Melebihi." },
    { "en": "Exception?", "id": "Pengecualian." },
    { "en": "Excessive?", "id": "Berlebihan." },
    { "en": "Exclude?", "id": "Mengecualikan." },
    { "en": "Exhibit?", "id": "Memamerkan, pameran." },
    { "en": "Exit?", "id": "Pintu keluar." },
    { "en": "Exotic?", "id": "Eksotis." },
    { "en": "Expansion?", "id": "Ekspansi." },
    { "en": "Expertise?", "id": "Keahlian." },
    { "en": "Exploit?", "id": "Mengeksploitasi." },
    { "en": "Exposure?", "id": "Paparan." },
    { "en": "Extension?", "id": "Perpanjangan." },
    { "en": "Extensive?", "id": "Luas." },
    { "en": "Extensively?", "id": "Secara luas." },
    { "en": "Extract?", "id": "Ekstrak." },
    { "en": "Fabric?", "id": "Kain." },
    { "en": "Fabulous?", "id": "Luar biasa." },
    { "en": "Failed?", "id": "Gagal." },
    { "en": "Fake?", "id": "Palsu." },
    { "en": "Fame?", "id": "Ketenaran." },
    { "en": "Fantasy?", "id": "Fantasi." },
    { "en": "Fare?", "id": "Ongkos." },
    { "en": "Federal?", "id": "Federal." },
    { "en": "Fever?", "id": "Demam." },
    { "en": "Firefighter?", "id": "Pemadam kebakaran." },
    { "en": "Firework?", "id": "Kembang api." },
    { "en": "Firm?", "id": "Tegas." },
    { "en": "Firmly?", "id": "Dengan tegas." },
    { "en": "Flavour?", "id": "Rasa." },
    { "en": "Fond?", "id": "Menyukai." },
    { "en": "Fool?", "id": "Orang bodoh." },
    { "en": "Forbid?", "id": "Melarang." },
    { "en": "Forecast?", "id": "Ramalan, meramalkan." },
    { "en": "Format?", "id": "Format." },
    { "en": "Formation?", "id": "Formasi." },
    { "en": "Formerly?", "id": "Sebelumnya." },
    { "en": "Fortunate?", "id": "Beruntung." },
    { "en": "Forum?", "id": "Forum." },
    { "en": "Fossil?", "id": "Fosil." },
    { "en": "Foundation?", "id": "Fondasi." },
    { "en": "Founder?", "id": "Pendiri." },
    { "en": "Fraction?", "id": "Pecahan." },
    { "en": "Fragment?", "id": "Fragmen." },
    { "en": "Framework?", "id": "Kerangka." },
    { "en": "Fraud?", "id": "Penipuan." },
    { "en": "Freely?", "id": "Secara bebas." },
    { "en": "Frequent?", "id": "Sering." },
    { "en": "Fulfil?", "id": "Memenuhi." },
    { "en": "Full-time?", "id": "Penuh waktu." },
    { "en": "Fundamentally?", "id": "Secara mendasar." },
    { "en": "Furious?", "id": "Sangat marah." },
    { "en": "Gaming?", "id": "Permainan." },
    { "en": "Gay?", "id": "Gay." },
    { "en": "Gender?", "id": "Jenis kelamin." },
    { "en": "Gene?", "id": "Gen." },
    { "en": "Genetic?", "id": "Genetik." },
    { "en": "Genius?", "id": "Jenius." },
    { "en": "Genuine?", "id": "Asli." },
    { "en": "Genuinely?", "id": "Dengan tulus." },
    { "en": "Gesture?", "id": "Gerakan." },
    { "en": "Gig?", "id": "Pertunjukan kecil." },
    { "en": "Globalization?", "id": "Globalisasi." },
    { "en": "Globe?", "id": "Bola dunia." },
    { "en": "Golden?", "id": "Berwarna emas." },
    { "en": "Goodness?", "id": "Kebaikan." },
    { "en": "Gorgeous?", "id": "Sangat cantik." },
    { "en": "Governor?", "id": "Gubernur." },
    { "en": "Graphic?", "id": "Grafis." },
    { "en": "Graphics?", "id": "Grafik." },
    { "en": "Greatly?", "id": "Dengan sangat." },
    { "en": "Greenhouse?", "id": "Rumah kaca." },
    { "en": "Grocery?", "id": "Toko bahan makanan." },
    { "en": "Guideline?", "id": "Pedoman." },
    { "en": "Habitat?", "id": "Habitat." },
    { "en": "Harbour?", "id": "Pelabuhan." },
    { "en": "Headquarters?", "id": "Markas besar." },
    { "en": "Heal?", "id": "Menyembuhkan." },
    { "en": "Healthcare?", "id": "Layanan kesehatan." },
    { "en": "Helmet?", "id": "Helm." },
    { "en": "Hence?", "id": "Oleh karena itu." },
    { "en": "Herb?", "id": "Tanaman obat." },
    { "en": "Hidden?", "id": "Tersembunyi." },
    { "en": "Highway?", "id": "Jalan raya." },
    { "en": "Hilarious?", "id": "Sangat lucu." },
    { "en": "Hip?", "id": "Pinggul." },
    { "en": "Historian?", "id": "Sejarawan." },
    { "en": "Homeless?", "id": "Tunawisma." },
    { "en": "Honesty?", "id": "Kejujuran." },
    { "en": "Hook?", "id": "Kait." },
    { "en": "Hopefully?", "id": "Semoga." },
    { "en": "Hunger?", "id": "Kelaparan." },
    { "en": "Hypothesis?", "id": "Hipotesis." },
    { "en": "Icon?", "id": "Ikon." },
    { "en": "ID?", "id": "Kartu identitas." },
    { "en": "Identical?", "id": "Identik." },
    { "en": "Illusion?", "id": "Ilusi." },
    { "en": "Immigration?", "id": "Imigrasi." },
    { "en": "Immune?", "id": "Kebal." },
    { "en": "Implement?", "id": "Melaksanakan." },
    { "en": "Implication?", "id": "Implikasi." },
    { "en": "Incentive?", "id": "Insentif." },
    { "en": "Incorporate?", "id": "Menggabungkan." },
    { "en": "Incorrect?", "id": "Tidak benar." },
    { "en": "Independence?", "id": "Kemerdekaan." },
    { "en": "Index?", "id": "Indeks." },
    { "en": "Indication?", "id": "Indikasi." },
    { "en": "Inevitable?", "id": "Tidak terhindarkan." },
    { "en": "Inevitably?", "id": "Pasti terjadi." },
    { "en": "Infer?", "id": "Menyimpulkan." },
    { "en": "Inflation?", "id": "Inflasi." },
    { "en": "Info?", "id": "Informasi." },
    { "en": "Infrastructure?", "id": "Infrastruktur." },
    { "en": "Inhabitant?", "id": "Penduduk." },
    { "en": "Inherit?", "id": "Mewarisi." },
    { "en": "Ink?", "id": "Tinta." },
    { "en": "Innovation?", "id": "Inovasi." },
    { "en": "Innovative?", "id": "Inovatif." },
    { "en": "Input?", "id": "Masukan." },
    { "en": "Insert?", "id": "Memasukkan." },
    { "en": "Inspector?", "id": "Inspektur." },
    { "en": "Installation?", "id": "Instalasi." },
    { "en": "Instant?", "id": "Seketika." },
    { "en": "Instantly?", "id": "Segera." },
    { "en": "Integrate?", "id": "Mengintegrasikan." },
    { "en": "Intellectual?", "id": "Intelektual." },
    { "en": "Interact?", "id": "Berinteraksi." },
    { "en": "Interaction?", "id": "Interaksi." },
    { "en": "Interpretation?", "id": "Interpretasi." },
    { "en": "Interval?", "id": "Jeda." },
    { "en": "Invade?", "id": "Menyerbu." },
    { "en": "Invasion?", "id": "Invasi." },
    { "en": "Investor?", "id": "Investor." },
    { "en": "Isolate?", "id": "Mengisolasi." },
    { "en": "Isolated?", "id": "Terpencil." },
    { "en": "Jail?", "id": "Penjara, memenjarakan." },
    { "en": "Jet?", "id": "Pesawat jet." },
    { "en": "Joint?", "id": "Bersama, sambungan." },
    { "en": "Journalism?", "id": "Jurnalisme." },
    { "en": "Jury?", "id": "Juri." },
    { "en": "Kit?", "id": "Perlengkapan." },
    { "en": "Ladder?", "id": "Tangga." },
    { "en": "Landing?", "id": "Pendaratan." },
    { "en": "Lane?", "id": "Jalur." },
    { "en": "Lately?", "id": "Belakangan ini." },
    { "en": "Leaflet?", "id": "Selebaran." },
    { "en": "Legend?", "id": "Legenda." },
    { "en": "Lens?", "id": "Lensa." },
    { "en": "Lifetime?", "id": "Seumur hidup." },
    { "en": "Lighting?", "id": "Pencahayaan." },
    { "en": "Likewise?", "id": "Demikian juga." },
    { "en": "Limitation?", "id": "Keterbatasan." },
    { "en": "Literally?", "id": "Secara harfiah." },
    { "en": "Literary?", "id": "Sastra." },
    { "en": "Litre?", "id": "Liter." },
    { "en": "Litter?", "id": "Sampah." },
    { "en": "Logo?", "id": "Logo." },
    { "en": "Lottery?", "id": "Undian." },
    { "en": "Loyal?", "id": "Setia." },
    { "en": "Lyric?", "id": "Lirik." },
    { "en": "Magnificent?", "id": "Megah." },
    { "en": "Make-up?", "id": "Riasan." },
    { "en": "Making?", "id": "Pembuatan." },
    { "en": "Manufacture?", "id": "Memproduksi." },
    { "en": "Manufacturing?", "id": "Produksi." },
    { "en": "Marathon?", "id": "Maraton." },
    { "en": "Margin?", "id": "Batas pinggir." },
    { "en": "Marker?", "id": "Spidol." },
    { "en": "Martial?", "id": "Berhubungan dengan militer." },
    { "en": "Mate?", "id": "Pasangan, berpasangan." },
    { "en": "Mayor?", "id": "Walikota." },
    { "en": "Mechanic?", "id": "Montir." },
    { "en": "Mechanical?", "id": "Mekanis." },
    { "en": "Mechanism?", "id": "Mekanisme." },
    { "en": "Medal?", "id": "Medali." },
    { "en": "Medication?", "id": "Pengobatan." },
    { "en": "Membership?", "id": "Keanggotaan." },
    { "en": "Memorable?", "id": "Berkesan." },
    { "en": "Metaphor?", "id": "Metafora." },
    { "en": "Miner?", "id": "Penambang." },
    { "en": "Miserable?", "id": "Sengsara." },
    { "en": "Mode?", "id": "Mode." },
    { "en": "Modest?", "id": "Sederhana." },
    { "en": "Monster?", "id": "Monster." },
    { "en": "Monthly?", "id": "Bulanan." },
    { "en": "Monument?", "id": "Monumen." },
    { "en": "Moreover?", "id": "Terlebih lagi." },
    { "en": "Mortgage?", "id": "Hipotek." },
    { "en": "Mosque?", "id": "Masjid." },
    { "en": "Motion?", "id": "Gerakan." },
    { "en": "Motivate?", "id": "Memotivasi." },
    { "en": "Motivation?", "id": "Motivasi." },
    { "en": "Moving?", "id": "Mengharukan." },
    { "en": "Myth?", "id": "Mitos." },
    { "en": "Naked?", "id": "Telanjang." },
    { "en": "Nasty?", "id": "Buruk." },
    { "en": "Navigation?", "id": "Navigasi." },
    { "en": "Nearby?", "id": "Di dekat/di sekitar." },
    { "en": "Necessity?", "id": "Kebutuhan." },
    { "en": "Negotiate?", "id": "Bernegosiasi." },
    { "en": "Negotiation?", "id": "Negosiasi." },
    { "en": "Neutral?", "id": "Netral." },
    { "en": "Newly?", "id": "Baru saja." },
    { "en": "Norm?", "id": "Norma." },
    { "en": "Notebook?", "id": "Buku catatan." },
    { "en": "Novelist?", "id": "Novelis." },
    { "en": "Nowadays?", "id": "Zaman sekarang." },
    { "en": "Nursing?", "id": "Perawatan." },
    { "en": "Nutrition?", "id": "Nutrisi." },
    { "en": "Obesity?", "id": "Obesitas." },
    { "en": "Observer?", "id": "Pengamat." },
    { "en": "Obstacle?", "id": "Hambatan." },
    { "en": "Occupation?", "id": "Pekerjaan." },
    { "en": "Occupy?", "id": "Menduduki." },
    { "en": "Offender?", "id": "Pelanggar." },
    { "en": "Ongoing?", "id": "Sedang berlangsung." },
    { "en": "Openly?", "id": "Secara terbuka." },
    { "en": "Opera?", "id": "Opera." },
    { "en": "Operator?", "id": "Operator." },
    { "en": "Optimistic?", "id": "Optimistis." },
    { "en": "Orchestra?", "id": "Orkestra." },
    { "en": "Organic?", "id": "Organik." },
    { "en": "Outfit?", "id": "Pakaian." },
    { "en": "Output?", "id": "Hasil keluaran." },
    { "en": "Outstanding?", "id": "Luar biasa." },
    { "en": "Overcome?", "id": "Mengatasi." },
    { "en": "Overnight?", "id": "Semalaman." },
    { "en": "Overseas?", "id": "Di luar negeri, luar negeri." },
    { "en": "Ownership?", "id": "Kepemilikan." },
    { "en": "Oxygen?", "id": "Oksigen." },
    { "en": "Packet?", "id": "Bungkus kecil." },
    { "en": "Palm?", "id": "Telapak tangan." },
    { "en": "Panic?", "id": "Panik." },
    { "en": "Parade?", "id": "Parade." },
    { "en": "Parallel?", "id": "Sejajar, garis sejajar." },
    { "en": "Participation?", "id": "Partisipasi." },
    { "en": "Partnership?", "id": "Kemitraan." },
    { "en": "Part-time?", "id": "Paruh waktu." },
    { "en": "Passionate?", "id": "Penuh gairah." },
    { "en": "Password?", "id": "Kata sandi." },
    { "en": "Patience?", "id": "Kesabaran." },
    { "en": "Pause?", "id": "Berhenti sejenak, jeda." },
    { "en": "Peer?", "id": "Rekan." },
    { "en": "Penalty?", "id": "Hukuman." },
    { "en": "Perceive?", "id": "Memahami." },
    { "en": "Perception?", "id": "Persepsi." },
    { "en": "Permanently?", "id": "Secara permanen." },
    { "en": "Pill?", "id": "Pil." },
    { "en": "Pity?", "id": "Rasa kasihan." },
    { "en": "Placement?", "id": "Penempatan." },
    { "en": "Portion?", "id": "Porsi." },
    { "en": "Potentially?", "id": "Berpotensi." },
    { "en": "Precede?", "id": "Mendahului." },
    { "en": "Precious?", "id": "Berharga." },
    { "en": "Precise?", "id": "Tepat." },
    { "en": "Precisely?", "id": "Dengan tepat." },
    { "en": "Predictable?", "id": "Dapat diprediksi." },
    { "en": "Preference?", "id": "Preferensi." },
    { "en": "Pride?", "id": "Kebanggaan." },
    { "en": "Primarily?", "id": "Terutama." },
    { "en": "Principal?", "id": "Utama." },
    { "en": "Prior?", "id": "Sebelumnya." },
    { "en": "Probability?", "id": "Kemungkinan." },
    { "en": "Probable?", "id": "Mungkin." },
    { "en": "Proceed?", "id": "Melanjutkan." },
    { "en": "Programming?", "id": "Pemrograman." },
    { "en": "Progressive?", "id": "Progresif." },
    { "en": "Promising?", "id": "Menjanjikan." },
    { "en": "Promotion?", "id": "Promosi." },
    { "en": "Prompt?", "id": "Segera." },
    { "en": "Prohibit?", "id": "Melarang." },
    { "en": "Proportion?", "id": "Proporsi." },
    { "en": "Protein?", "id": "Protein." },
    { "en": "Protester?", "id": "Pemrotes." },
    { "en": "Psychological?", "id": "Psikologis." },
    { "en": "Publicity?", "id": "Publisitas." },
    { "en": "Publishing?", "id": "Penerbitan." },
    { "en": "Punk?", "id": "Punk." },
    { "en": "Purely?", "id": "Murni." },
    { "en": "Pursuit?", "id": "Pengejaran." },
    { "en": "Puzzle?", "id": "Teka teki." },
    { "en": "Questionnaire?", "id": "Kuesioner." },
    { "en": "Racial?", "id": "Rasial." },
    { "en": "Racism?", "id": "Rasisme." },
    { "en": "Racist?", "id": "Rasis, orang rasis." },
    { "en": "Radiation?", "id": "Radiasi." },
    { "en": "Rail?", "id": "Rel." },
    { "en": "Random?", "id": "Acak." },
    { "en": "Rat?", "id": "Tikus." },
    { "en": "Rating?", "id": "Peringkat." },
    { "en": "Reasonably?", "id": "Secara wajar." },
    { "en": "Rebuild?", "id": "Membangun kembali." },
    { "en": "Receiver?", "id": "Penerima." },
    { "en": "Recession?", "id": "Resesi." },
    { "en": "Reckon?", "id": "Memperkirakan." },
    { "en": "Recognition?", "id": "Pengakuan." },
    { "en": "Recovery?", "id": "Pemulihan." },
    { "en": "Recruit?", "id": "Merekrut, perekrutan." },
    { "en": "Recruitment?", "id": "Perekrutan." },
    { "en": "Referee?", "id": "Wasit." },
    { "en": "Refugee?", "id": "Pengungsi." },
    { "en": "Registration?", "id": "Pendaftaran." },
    { "en": "Regulate?", "id": "Mengatur." },
    { "en": "Reinforce?", "id": "Memperkuat." },
    { "en": "Relieve?", "id": "Meringankan." },
    { "en": "Relieved?", "id": "Merasa lega." },
    { "en": "Remarkable?", "id": "Luar biasa." },
    { "en": "Remarkably?", "id": "Dengan luar biasa." },
    { "en": "Resign?", "id": "Mengundurkan diri." },
    { "en": "Resolution?", "id": "Resolusi." },
    { "en": "Restore?", "id": "Memulihkan." },
    { "en": "Restrict?", "id": "Membatasi." },
    { "en": "Restriction?", "id": "Pembatasan." },
    { "en": "Retail?", "id": "Eceran." },
    { "en": "Retirement?", "id": "Pensiun." },
    { "en": "Revenue?", "id": "Pendapatan." },
    { "en": "Revision?", "id": "Revisi." },
    { "en": "Ridiculous?", "id": "Konyol." },
    { "en": "Risky?", "id": "Berisiko." },
    { "en": "Rival?", "id": "Saingan, pesaing." },
    { "en": "Rob?", "id": "Merampok." },
    { "en": "Robbery?", "id": "Perampokan." },
    { "en": "Rocket?", "id": "Roket." },
    { "en": "Romance?", "id": "Percintaan." },
    { "en": "Rose?", "id": "Mawar." },
    { "en": "Roughly?", "id": "Kira-kira/secara kasar." },
    { "en": "Ruin?", "id": "Menghancurkan, kehancuran." },
    { "en": "Satisfaction?", "id": "Kepuasan." },
    { "en": "Scandal?", "id": "Skandal." },
    { "en": "Scare?", "id": "Menakut-nakuti, ketakutan." },
    { "en": "Scenario?", "id": "Skenario." },
    { "en": "Scholar?", "id": "Cendekiawan." },
    { "en": "Scholarship?", "id": "Beasiswa." },
    { "en": "Scratch?", "id": "Menggaruk, goresan." },
    { "en": "Screening?", "id": "Penyaringan." },
    { "en": "Seeker?", "id": "Pencari." },
    { "en": "Seminar?", "id": "Seminar." },
    { "en": "Settler?", "id": "Pemukim." },
    { "en": "Severely?", "id": "Dengan parah." },
    { "en": "Sexy?", "id": "Seksi." },
    { "en": "Shaped?", "id": "Berbentuk." },
    { "en": "Shocking?", "id": "Mengejutkan." },
    { "en": "Shore?", "id": "Pantai/tepi laut." },
    { "en": "Shortage?", "id": "Kekurangan." },
    { "en": "Shortly?", "id": "Sebentar lagi." },
    { "en": "Short-term?", "id": "Jangka pendek." },
    { "en": "Sibling?", "id": "Saudara kandung." },
    { "en": "Signature?", "id": "Tanda tangan." },
    { "en": "Significance?", "id": "Arti penting." },
    { "en": "Skilled?", "id": "Terampil." },
    { "en": "Skull?", "id": "Tengkorak." },
    { "en": "Slogan?", "id": "Slogan." },
    { "en": "So-called?", "id": "Yang disebut/disebutkan." },
    { "en": "Somehow?", "id": "Entah bagaimana." },
    { "en": "Sometime?", "id": "Suatu saat." },
    { "en": "Sophisticated?", "id": "Canggih." },
    { "en": "Spare?", "id": "Cadangan." },
    { "en": "Specialize?", "id": "Mengkhususkan diri." },
    { "en": "Specify?", "id": "Menentukan secara spesifik." },
    { "en": "Spectacular?", "id": "Spektakuler." },
    { "en": "Spectator?", "id": "Penonton." },
    { "en": "Speculate?", "id": "Berspekulasi." },
    { "en": "Speculation?", "id": "Spekulasi." },
    { "en": "Spice?", "id": "Rempah-rempah." },
    { "en": "Spill?", "id": "Menumpahkan." },
    { "en": "Spite?", "id": "Dendam." },
    { "en": "Spoil?", "id": "Merusak/memanjakan." },
    { "en": "Spokesman?", "id": "Juru bicara (pria)." },
    { "en": "Spokesperson?", "id": "Juru bicara." },
    { "en": "Spokeswoman?", "id": "Juru bicara (wanita)." },
    { "en": "Sponsorship?", "id": "Sponsor." },
    { "en": "Sporting?", "id": "Berhubungan dengan olahraga." },
    { "en": "Stall?", "id": "Kios/stan." },
    { "en": "Stance?", "id": "Sikap/posisi." },
    { "en": "Starve?", "id": "Kelaparan." },
    { "en": "Steadily?", "id": "Dengan stabil." },
    { "en": "Steam?", "id": "Uap." },
    { "en": "Stimulate?", "id": "Merangsang." },
    { "en": "Strengthen?", "id": "Memperkuat." },
    { "en": "Strictly?", "id": "Dengan ketat." },
    { "en": "Stroke?", "id": "Serangan strok." },
    { "en": "Stunning?", "id": "Memukau." },
    { "en": "Subsequent?", "id": "Selanjutnya." },
    { "en": "Subsequently?", "id": "Kemudian." },
    { "en": "Suburb?", "id": "Daerah pinggiran kota." },
    { "en": "Suffering?", "id": "Penderitaan." },
    { "en": "Sufficient?", "id": "Cukup." },
    { "en": "Sufficiently?", "id": "Dengan cukup." },
    { "en": "Super?", "id": "Luar biasa." },
    { "en": "Surgeon?", "id": "Ahli bedah." },
    { "en": "Survival?", "id": "Kelangsungan hidup." },
    { "en": "Survivor?", "id": "Penyintas." },
    { "en": "Suspend?", "id": "Menangguhkan." },
    { "en": "Sustainable?", "id": "Berkelanjutan." },
    { "en": "Swallow?", "id": "Menelan." },
    { "en": "Sympathetic?", "id": "Simpatik." },
    { "en": "Tackle?", "id": "Mengatasi." },
    { "en": "Tag?", "id": "Label, tanda." },
    { "en": "Tap?", "id": "Mengetuk, ketukan." },
    { "en": "Technological?", "id": "Berhubungan dengan teknologi." },
    { "en": "Teens?", "id": "Masa remaja." },
    { "en": "Temple?", "id": "Kuil/candi." },
    { "en": "Temporarily?", "id": "Untuk sementara." },
    { "en": "Tendency?", "id": "Kecenderungan." },
    { "en": "Tension?", "id": "Ketegangan." },
    { "en": "Terminal?", "id": "Terminal." },
    { "en": "Terms?", "id": "Persyaratan." },
    { "en": "Terribly?", "id": "Dengan sangat buruk." },
    { "en": "Terrify?", "id": "Menakutkan." },
    { "en": "Territory?", "id": "Wilayah." },
    { "en": "Terror?", "id": "Teror." },
    { "en": "Terrorism?", "id": "Terorisme." },
    { "en": "Terrorist?", "id": "Teroris." },
    { "en": "Testing?", "id": "Pengujian." },
    { "en": "Textbook?", "id": "Buku pelajaran." },
    { "en": "Theft?", "id": "Pencurian." },
    { "en": "Therapist?", "id": "Terapis." },
    { "en": "Thesis?", "id": "Tesis." },
    { "en": "Thorough?", "id": "Menyeluruh." },
    { "en": "Thoroughly?", "id": "Secara meyeluruh." },
    { "en": "Thumb?", "id": "Ibu jari." },
    { "en": "Timing?", "id": "Waktu pelaksanaan." },
    { "en": "Tissue?", "id": "Jaringan tubuh/tisu." },
    { "en": "Ton?", "id": "Ton." },
    { "en": "Tonne?", "id": "Ton metrik." },
    { "en": "Tournament?", "id": "Turnamen." },
    { "en": "Trace?", "id": "Melacak." },
    { "en": "Trading?", "id": "Perdagangan." },
    { "en": "Tragedy?", "id": "Tragedi." },
    { "en": "Tragic?", "id": "Tragis." },
    { "en": "Trait?", "id": "Sifat/ciri khas." },
    { "en": "Transmit?", "id": "Menularkan/mengirimkan." },
    { "en": "Transportation?", "id": "Transportasi." },
    { "en": "Trap?", "id": "Menjebak, jebakan." },
    { "en": "Treasure?", "id": "Harta karun." },
    { "en": "Tribe?", "id": "Suku." },
    { "en": "Trigger?", "id": "Memicu." },
    { "en": "Trillion?", "id": "Triliun." },
    { "en": "Troop?", "id": "Pasukan." },
    { "en": "Tsunami?", "id": "Tsunami." },
    { "en": "Ultimate?", "id": "Tertinggi." },
    { "en": "Unacceptable?", "id": "Tidak dapat diterima." },
    { "en": "Uncertainty?", "id": "Ketidakpastian." },
    { "en": "Undergo?", "id": "Menjalani." },
    { "en": "Undertake?", "id": "Melakukan." },
    { "en": "Unfold?", "id": "Membuka/mengungkapkan." },
    { "en": "Unfortunate?", "id": "Malang." },
    { "en": "Unite?", "id": "Menyatukan." },
    { "en": "Unity?", "id": "Persatuan." },
    { "en": "Universal?", "id": "Universal/berlaku umum." },
    { "en": "Urgent?", "id": "Mendesak." },
    { "en": "Usage?", "id": "Penggunaan." },
    { "en": "Useless?", "id": "Tidak berguna." },
    { "en": "Valid?", "id": "Berlaku/sah." },
    { "en": "Variation?", "id": "Variasi." },
    { "en": "Vertical?", "id": "Vertikal/tegak lurus." },
    { "en": "Viewpoint?", "id": "Sudut pandang." },
    { "en": "Visa?", "id": "Visa." },
    { "en": "Visible?", "id": "Terlihat." },
    { "en": "Voluntary?", "id": "Sukarela." },
    { "en": "Voting?", "id": "Pemungutan suara." },
    { "en": "Wander?", "id": "Berkeliaran." },
    { "en": "Warming?", "id": "Pemanasan." },
    { "en": "Weekly?", "id": "Mingguan." },
    { "en": "Weird?", "id": "Aneh." },
    { "en": "Welfare?", "id": "Kesejahteraan." },
    { "en": "Wheat?", "id": "Gandum." },
    { "en": "Whoever?", "id": "Siapa pun." },
    { "en": "Widespread?", "id": "Tersebar luas." },
    { "en": "Wisdom?", "id": "Kebijaksanaan." },
    { "en": "Withdraw?", "id": "Menarik diri/mundur." },
    { "en": "Workforce?", "id": "Tenaga kerja." },
    { "en": "Workplace?", "id": "Tempat kerja." },
    { "en": "Workshop?", "id": "Lokakarya." },
    { "en": "Worm?", "id": "Cacing." },
    { "en": "Wrist?", "id": "Pergelangan tangan." },
    { "en": "Abolish?", "id": "Menghapuskan." },
    { "en": "Abortion?", "id": "Aborsi." },
    { "en": "Absence?", "id": "Ketidakhadiran." },
    { "en": "Absent?", "id": "Tidak hadir." },
    { "en": "Absurd?", "id": "Tidak masuk akal." },
    { "en": "Abundance?", "id": "Kelimpahan." },
    { "en": "Abuse?", "id": "Penyalahgunaan." },
    { "en": "Academy?", "id": "Akademi." },
    { "en": "Accelerate?", "id": "Mempercepat." },
    { "en": "Acceptance?", "id": "Penerimaan." },
    { "en": "Accessible?", "id": "Dapat diakses." },
    { "en": "Accomplishment?", "id": "Pencapaian." },
    { "en": "Accordance?", "id": "Kesesuaian." },
    { "en": "Accordingly?", "id": "Sesuai." },
    { "en": "Accountability?", "id": "Tanggung jawab." },
    { "en": "Accountable?", "id": "Bertanggung jawab." },
    { "en": "Accumulate?", "id": "Mengumpulkan." },
    { "en": "Accumulation?", "id": "Penumpukan." },
    { "en": "Accusation?", "id": "Tuduhan." },
    { "en": "Accused?", "id": "Terdakwa." },
    { "en": "Acid?", "id": "Asam." },
    { "en": "Acquisition?", "id": "Perolehan." },
    { "en": "Acre?", "id": "Hektar." },
    { "en": "Activation?", "id": "Pengaktifan." },
    { "en": "Activist?", "id": "Aktivis." },
    { "en": "Acute?", "id": "Akut/tajam." },
    { "en": "Adaptation?", "id": "Adaptasi." },
    { "en": "Adhere?", "id": "Melekat/mematuhi." },
    { "en": "Adjacent?", "id": "Berdekatan." },
    { "en": "Adjustment?", "id": "Penyesuaian." },
    { "en": "Administer?", "id": "Mengelola." },
    { "en": "Administrative?", "id": "Administratif." },
    { "en": "Administrator?", "id": "Pengelola." },
    { "en": "Admission?", "id": "Penerimaan." },
    { "en": "Adolescent?", "id": "Remaja." },
    { "en": "Adoption?", "id": "Adopsi." },
    { "en": "Adverse?", "id": "Merugikan." },
    { "en": "Advocate?", "id": "Pendukung." },
    { "en": "Aesthetic?", "id": "Estetis." },
    { "en": "Affection?", "id": "Kasih sayang." },
    { "en": "Aftermath?", "id": "Akibat/dampak." },
    { "en": "Aggression?", "id": "Agresi." },
    { "en": "Agricultural?", "id": "Pertanian." },
    { "en": "Aide?", "id": "Pembantu/asisten." },
    { "en": "Albeit?", "id": "Meskipun." },
    { "en": "Alert?", "id": "Waspada, kewaspadaan, siaga." },
    { "en": "Alien?", "id": "Asing." },
    { "en": "Align?", "id": "Menyelaraskan." },
    { "en": "Alignment?", "id": "Penyelarasan." },
    { "en": "Alike?", "id": "Serupa, mirip." },
    { "en": "Allegation?", "id": "Tuduhan." },
    { "en": "Allege?", "id": "Menuduh." },
    { "en": "Allegedly?", "id": "Diduga." },
    { "en": "Alliance?", "id": "Aliansi." },
    { "en": "Allocate?", "id": "Mengalokasikan." },
    { "en": "Allocation?", "id": "Alokasi." },
    { "en": "Allowance?", "id": "Tunjangan/uang saku." },
    { "en": "Ally?", "id": "Sekutu." },
    { "en": "Aluminium?", "id": "Aluminium." },
    { "en": "Amateur?", "id": "Amatir." },
    { "en": "Ambassador?", "id": "Duta besar." },
    { "en": "Amend?", "id": "Memperbaiki." },
    { "en": "Amendment?", "id": "Amendemen/perubahan." },
    { "en": "Amid?", "id": "Di tengah-tengah." },
    { "en": "Analogy?", "id": "Analogi/perbandingan." },
    { "en": "Anchor?", "id": "Jangkar." },
    { "en": "Angel?", "id": "Malaikat." },
    { "en": "Anonymous?", "id": "Anonim/tanpa nama." },
    { "en": "Apparatus?", "id": "Alat." },
    { "en": "Appealing?", "id": "Menarik." },
    { "en": "Appetite?", "id": "Nafsu makan." },
    { "en": "Applaud?", "id": "Bertepuk tangan/memuji." },
    { "en": "Applicable?", "id": "Berlaku." },
    { "en": "Appoint?", "id": "Menunjuk." },
    { "en": "Appreciation?", "id": "Apresiasi." },
    { "en": "Arbitrary?", "id": "Sewenang-wenang." },
    { "en": "Architectural?", "id": "Berhubungan dengan arsitektur." },
    { "en": "Archive?", "id": "Arsip." },
    { "en": "Arena?", "id": "Arena." },
    { "en": "Arguably?", "id": "Bisa dibilang." },
    { "en": "Arm?", "id": "Mempersenjatai." },
    { "en": "Array?", "id": "Deretan." },
    { "en": "Articulate?", "id": "Mengungkapkan dengan jelas." },
    { "en": "Ash?", "id": "Abu." },
    { "en": "Aspiration?", "id": "Aspirasi." },
    { "en": "Aspire?", "id": "Bercita-cita." },
    { "en": "Assassination?", "id": "Pembunuhan (terencana)." },
    { "en": "Assault?", "id": "Serangan, menyerang." },
    { "en": "Assemble?", "id": "Merakit." },
    { "en": "Assembly?", "id": "Pertemuan." },
    { "en": "Assert?", "id": "Menegaskan." },
    { "en": "Assertion?", "id": "Penegasan." },
    { "en": "Assurance?", "id": "Jaminan." },
    { "en": "Asylum?", "id": "Suaka/perlindungan." },
    { "en": "Atrocity?", "id": "Kekejaman." },
    { "en": "Attain?", "id": "Mencapai." },
    { "en": "Attendance?", "id": "Kehadiran." },
    { "en": "Attorney?", "id": "Pengacara." },
    { "en": "Attribute?", "id": "Menghubungkan, atribut." },
    { "en": "Audit?", "id": "Audit." },
    { "en": "Authentic?", "id": "Autentik." },
    { "en": "Authorize?", "id": "Mengesahkan." },
    { "en": "Auto?", "id": "Mobil." },
    { "en": "Autonomy?", "id": "Otonomi." },
    { "en": "Availability?", "id": "Ketersediaan." },
    { "en": "Await?", "id": "Menunggu." },
    { "en": "Backdrop?", "id": "Latar belakang." },
    { "en": "Backing?", "id": "Dukungan." },
    { "en": "Backup?", "id": "Cadangan." },
    { "en": "Bail?", "id": "Jaminan." },
    { "en": "Ballot?", "id": "Surat suara." },
    { "en": "Banner?", "id": "Spanduk." },
    { "en": "Bare?", "id": "Telanjang/polos." },
    { "en": "Barrel?", "id": "Tong." },
    { "en": "Bass?", "id": "Suara bass." },
    { "en": "Bat (verb)?", "id": "Memukul." },
    { "en": "Battlefield?", "id": "Medan perang." },
    { "en": "Bay?", "id": "Teluk." },
    { "en": "Beam?", "id": "Balok/sinar." },
    { "en": "Beast?", "id": "Binatang buas." },
    { "en": "Behalf?", "id": "Atas nama." },
    { "en": "Beloved?", "id": "Tercinta." },
    { "en": "Bench?", "id": "Bangku." },
    { "en": "Benchmark?", "id": "Tolak ukur." },
    { "en": "Beneath?", "id": "Di bawah." },
    { "en": "Beneficiary?", "id": "Penerima manfaat." },
    { "en": "Betray?", "id": "Mengkhianati." },
    { "en": "Bind?", "id": "Mengikat." },
    { "en": "Biography?", "id": "Biografi." },
    { "en": "Bishop?", "id": "Uskup." },
    { "en": "Bizarre?", "id": "Aneh." },
    { "en": "Blade?", "id": "Bilah/pisau." },
    { "en": "Blast?", "id": "Ledakan, meledakkan." },
    { "en": "Bleed?", "id": "Berdarah." },
    { "en": "Blend?", "id": "Mencampur, campuran." },
    { "en": "Bless?", "id": "Memberkati." },
    { "en": "Blessing?", "id": "Berkah." },
    { "en": "Boast?", "id": "Membanggakan." },
    { "en": "Bonus?", "id": "Bonus." },
    { "en": "Boom?", "id": "Ledakan." },
    { "en": "Bounce?", "id": "Memantul." },
    { "en": "Boundary?", "id": "Batas." },
    { "en": "Bow?", "id": "Membungkuk, busur." },
    { "en": "Breach?", "id": "Pelanggaran, melanggar." },
    { "en": "Breakdown?", "id": "Kerusakan/kegagalan." },
    { "en": "Breakthrough?", "id": "Terobosan." },
    { "en": "Breed?", "id": "Berkembang biak, ras." },
    { "en": "Broadband?", "id": "Jaringan internet cepat." },
    { "en": "Browser?", "id": "Peramban." },
    { "en": "Brutal?", "id": "Brutal/kejam." },
    { "en": "Buck?", "id": "Dolar/rusa jantan." },
    { "en": "Buddy?", "id": "Teman akrab." },
    { "en": "Buffer?", "id": "Penyangga." },
    { "en": "Bulk?", "id": "Sebagian besar/massa." },
    { "en": "Burden?", "id": "Beban." },
    { "en": "Bureaucracy?", "id": "Birokrasi." },
    { "en": "Burial?", "id": "Pemakaman." },
    { "en": "Burst?", "id": "Meledak." },
    { "en": "Cabinet?", "id": "Kabinet/lemari." },
    { "en": "Calculation?", "id": "Perhitungan." },
    { "en": "Canvas?", "id": "Kanvas." },
    { "en": "Capability?", "id": "Kemampuan." },
    { "en": "Capitalism?", "id": "Kapitalisme." },
    { "en": "Capitalist?", "id": "Kapitalis." },
    { "en": "Cargo?", "id": "Kargo." },
    { "en": "Carriage?", "id": "Gerbong/kendaraan." },
    { "en": "Carve?", "id": "Mengukir." },
    { "en": "Casino?", "id": "Kasino." },
    { "en": "Casualty?", "id": "Korban." },
    { "en": "Catalogue?", "id": "Katalog." },
    { "en": "Cater?", "id": "Menyediakan makanan, melayani." },
    { "en": "Cattle?", "id": "Ternak." },
    { "en": "Caution?", "id": "Peringatan." },
    { "en": "Cautious?", "id": "Berhati-hati." },
    { "en": "Cease?", "id": "Menghentikan." },
    { "en": "Cemetery?", "id": "Pemakaman." },
    { "en": "Chamber?", "id": "Ruangan." },
    { "en": "Chaos?", "id": "Kekacauan." },
    { "en": "Characterize?", "id": "Mencirikan." },
    { "en": "Charm?", "id": "Daya tarik." },
    { "en": "Charter?", "id": "Piagam/sewa khusus." },
    { "en": "Chronic?", "id": "Kronis." },
    { "en": "Chunk?", "id": "Potongan besar." },
    { "en": "Circulate?", "id": "Mengedarkan." },
    { "en": "Circulation?", "id": "Sirkulasi." },
    { "en": "Citizenship?", "id": "Kewarganegaraan." },
    { "en": "Civic?", "id": "Terkait masyarakat." },
    { "en": "Civilian?", "id": "Warga sipil, sipil." },
    { "en": "Clarity?", "id": "Kejelasan." },
    { "en": "Clash?", "id": "Perselisihan." },
    { "en": "Classification?", "id": "Klasifikasi." },
    { "en": "Cling?", "id": "Melekat." },
    { "en": "Clinical?", "id": "Klinis." },
    { "en": "Closure?", "id": "Penutupan." },
    { "en": "Cluster?", "id": "Kelompok, gugus." },
    { "en": "Coalition?", "id": "Koalisi." },
    { "en": "Coastal?", "id": "Pesisir." },
    { "en": "Cocktail?", "id": "Koktail." },
    { "en": "Cognitive?", "id": "Kognitif." },
    { "en": "Coincide?", "id": "Bertepatan." },
    { "en": "Collaborate?", "id": "Berkolaborasi." },
    { "en": "Collaboration?", "id": "Kolaborasi." },
    { "en": "Collective?", "id": "Kolektif." },
    { "en": "Collision?", "id": "Tabrakan." },
    { "en": "Colonial?", "id": "Kolonial." },
    { "en": "Columnist?", "id": "Kolumnis." },
    { "en": "Combat?", "id": "Pertempuran, melawan." },
    { "en": "Commence?", "id": "Memulai." },
    { "en": "Commentary?", "id": "Komentar." },
    { "en": "Commentator?", "id": "Komentator." },
    { "en": "Commerce?", "id": "Perdagangan." },
    { "en": "Commissioner?", "id": "Komisaris." },
    { "en": "Commodity?", "id": "Komoditas." },
    { "en": "Communist?", "id": "Komunis." },
    { "en": "Companion?", "id": "Teman." },
    { "en": "Comparable?", "id": "Sebanding." },
    { "en": "Compassion?", "id": "Belas kasihan." },
    { "en": "Compel?", "id": "Memaksa." },
    { "en": "Compelling?", "id": "Menarik." },
    { "en": "Compensate?", "id": "Mengganti rugi." },
    { "en": "Compensation?", "id": "Kompensasi." },
    { "en": "Competence?", "id": "Kompetensi." },
    { "en": "Competent?", "id": "Kompeten." },
    { "en": "Compile?", "id": "Menyusun." },
    { "en": "Complement?", "id": "Pelengkap." },
    { "en": "Complexity?", "id": "Kompleksitas." },
    { "en": "Compliance?", "id": "Kepatuhan." },
    { "en": "Complication?", "id": "Komplikasi." },
    { "en": "Comply?", "id": "Mematuhi." },
    { "en": "Composition?", "id": "Komposisi." },
    { "en": "Compromise?", "id": "Kompromi." },
    { "en": "Compute?", "id": "Menghitung." },
    { "en": "Conceal?", "id": "Menyembunyikan." },
    { "en": "Concede?", "id": "Mengakui." },
    { "en": "Conceive?", "id": "Mengandung/membayangkan." },
    { "en": "Conception?", "id": "Gagasan." },
    { "en": "Concession?", "id": "Konsesi/hak istimewa." },
    { "en": "Condemn?", "id": "Mengutuk." },
    { "en": "Confer?", "id": "Memberikan." },
    { "en": "Confession?", "id": "Pengakuan." },
    { "en": "Configuration?", "id": "Konfigurasi." },
    { "en": "Confine?", "id": "Membatasi." },
    { "en": "Confirmation?", "id": "Konfirmasi." },
    { "en": "Confront?", "id": "Menghadapi." },
    { "en": "Confrontation?", "id": "Konfrontasi." },
    { "en": "Congratulate?", "id": "Memberi selamat." },
    { "en": "Congregation?", "id": "Perkumpulan." },
    { "en": "Congressional?", "id": "Terkait kongres." },
    { "en": "Conquer?", "id": "Menaklukkan." },
    { "en": "Conscience?", "id": "Hati nurani." },
    { "en": "Consciousness?", "id": "Kesadaran." },
    { "en": "Consecutive?", "id": "Berturut-turut." },
    { "en": "Consensus?", "id": "Kesepakatan." },
    { "en": "Consent?", "id": "Persetujuan, menyetujui." },
    { "en": "Conserve?", "id": "Melestarikan." },
    { "en": "Consistency?", "id": "Konsistensi." },
    { "en": "Consolidate?", "id": "Memperkuat." },
    { "en": "Constituency?", "id": "Daerah pemilihan." },
    { "en": "Constitute?", "id": "Membentuk." },
    { "en": "Constitution?", "id": "Konstitusi." },
    { "en": "Constitutional?", "id": "Konstitusional." },
    { "en": "Constraint?", "id": "Batasan." },
    { "en": "Consultation?", "id": "Konsultasi." },
    { "en": "Contemplate?", "id": "Merenungkan." },
    { "en": "Contempt?", "id": "Penghinaan." },
    { "en": "Contend?", "id": "Berpendapat." },
    { "en": "Contender?", "id": "Pesaing." },
    { "en": "Content?", "id": "Puas." },
    { "en": "Contention?", "id": "Perselisihan." },
    { "en": "Continually?", "id": "Terus-menerus." },
    { "en": "Contractor?", "id": "Kontraktor." },
    { "en": "Contradiction?", "id": "Kontradiksi." },
    { "en": "Contrary?", "id": "Bertentangan." },
    { "en": "Contributor?", "id": "Penyumbang." },
    { "en": "Conversion?", "id": "Konversi." },
    { "en": "Convict?", "id": "Menghukum." },
    { "en": "Conviction?", "id": "Keyakinan/vonis." },
    { "en": "Cooperate?", "id": "Bekerja sama." },
    { "en": "Cooperative?", "id": "Kooperatif." },
    { "en": "Coordinate?", "id": "Mengoordinasikan." },
    { "en": "Coordination?", "id": "Koordinasi." },
    { "en": "Coordinator?", "id": "Koordinator." },
    { "en": "Cop?", "id": "Polisi." },
    { "en": "Copper?", "id": "Tembaga." },
    { "en": "Copyright?", "id": "Hak cipta." },
    { "en": "Correction?", "id": "Koreksi." },
    { "en": "Correlate?", "id": "Berkorelasi." },
    { "en": "Correlation?", "id": "Hubungan." },
    { "en": "Correspond?", "id": "Bersesuaian." },
    { "en": "Correspondence?", "id": "Korespondensi." },
    { "en": "Correspondent?", "id": "Wartawan." },
    { "en": "Corresponding?", "id": "Bersesuaian." },
    { "en": "Corrupt?", "id": "Tidak jujur." },
    { "en": "Corruption?", "id": "Korupsi." },
    { "en": "Costly?", "id": "Mahal." },
    { "en": "Councillor?", "id": "Anggota dewan." },
    { "en": "Counselling?", "id": "Konseling." },
    { "en": "Counsellor?", "id": "Konselor." },
    { "en": "Counter (argue against)?", "id": "Melawan." },
    { "en": "Counterpart?", "id": "Rekan." },
    { "en": "Countless?", "id": "Tak terhitung." },
    { "en": "Coup?", "id": "Kudeta." },
    { "en": "Courtesy?", "id": "Kesopanan." },
    { "en": "Craft (verb)?", "id": "Merancang." },
    { "en": "Crawl?", "id": "Merangkak." },
    { "en": "Creator?", "id": "Pencipta." },
    { "en": "Credibility?", "id": "Kredibilitas." },
    { "en": "Credible?", "id": "Kredibel." },
    { "en": "Creep?", "id": "Menjalar." },
    { "en": "Critique?", "id": "Kritik." },
    { "en": "Crown?", "id": "Mahkota." },
    { "en": "Crude?", "id": "Kasar." },
    { "en": "Crush?", "id": "Menghancurkan." },
    { "en": "Crystal?", "id": "Kristal." },
    { "en": "Cult?", "id": "Kultus." },
    { "en": "Cultivate?", "id": "Membudidayakan." },
    { "en": "Curiosity?", "id": "Rasa ingin tahu." },
    { "en": "Custody?", "id": "Pengawasan." },
    { "en": "Cutting?", "id": "Pemotongan." },
    { "en": "Cynical?", "id": "Sinis." },
    { "en": "Dam?", "id": "Bendungan." },
    { "en": "Damaging?", "id": "Merusak." },
    { "en": "Dawn?", "id": "Fajar." },
    { "en": "Debris?", "id": "Puing." },
    { "en": "Debut?", "id": "Debut." },
    { "en": "Decision-making?", "id": "Pengambilan keputusan." },
    { "en": "Decisive?", "id": "Tegas." },
    { "en": "Declaration?", "id": "Deklarasi." },
    { "en": "Dedicated?", "id": "Berdedikasi." },
    { "en": "Dedication?", "id": "Dedikasi." },
    { "en": "Deed?", "id": "Perbuatan." },
    { "en": "Deem?", "id": "Menganggap." },
    { "en": "Default?", "id": "Standar." },
    { "en": "Defect?", "id": "Cacat." },
    { "en": "Defensive?", "id": "Defensif." },
    { "en": "Deficiency?", "id": "Kekurangan." },
    { "en": "Deficit?", "id": "Defisit." },
    { "en": "Defy?", "id": "Menentang." },
    { "en": "Delegate?", "id": "Delegasi." },
    { "en": "Delegation?", "id": "Delegasi." },
    { "en": "Delicate?", "id": "Halus." },
    { "en": "Demon?", "id": "Iblis." },
    { "en": "Denial?", "id": "Penyangkalan." },
    { "en": "Denounce?", "id": "Mencela." },
    { "en": "Dense?", "id": "Padat." },
    { "en": "Density?", "id": "Kepadatan." },
    { "en": "Dependence?", "id": "Ketergantungan." },
    { "en": "Depict?", "id": "Menggambarkan." },
    { "en": "Deploy?", "id": "Mengerahkan." },
    { "en": "Deployment?", "id": "Penyebaran." },
    { "en": "Deposit (verb)?", "id": "Menyimpan." },
    { "en": "Deprive?", "id": "Mencabut." },
    { "en": "Deputy?", "id": "Wakil." },
    { "en": "Descend?", "id": "Turun." },
    { "en": "Descent?", "id": "Keturunan." },
    { "en": "Designate?", "id": "Menunjuk." },
    { "en": "Desirable?", "id": "Diinginkan." },
    { "en": "Desktop?", "id": "Desktop." },
    { "en": "Destructive?", "id": "Destruktif." },
    { "en": "Detain?", "id": "Menahan." },
    { "en": "Detection?", "id": "Deteksi." },
    { "en": "Detention?", "id": "Penahanan." },
    { "en": "Deteriorate?", "id": "Memburuk." },
    { "en": "Devastate?", "id": "Menghancurkan." },
    { "en": "Devil?", "id": "Setan." },
    { "en": "Devise?", "id": "Merancang." },
    { "en": "Diagnose?", "id": "Mendiagnosis." },
    { "en": "Diagnosis?", "id": "Diagnosis." },
    { "en": "Dictate?", "id": "Mendikte." },
    { "en": "Dictator?", "id": "Diktator." },
    { "en": "Differentiate?", "id": "Membedakan." },
    { "en": "Dignity?", "id": "Martabat." },
    { "en": "Dilemma?", "id": "Dilema." },
    { "en": "Dimension?", "id": "Dimensi." },
    { "en": "Diminish?", "id": "Berkurang." },
    { "en": "Dip?", "id": "Mencelupkan." },
    { "en": "Diplomat?", "id": "Diplomat." },
    { "en": "Diplomatic?", "id": "Diplomatik." },
    { "en": "Directory?", "id": "Direktori." },
    { "en": "Disastrous?", "id": "Bencana." },
    { "en": "Discard?", "id": "Membuang." },
    { "en": "Discharge?", "id": "Melepaskan." },
    { "en": "Disclose?", "id": "Mengungkapkan." },
    { "en": "Disclosure?", "id": "Pengungkapan." },
    { "en": "Discourse?", "id": "Wacana." },
    { "en": "Discretion?", "id": "Kebijaksanaan." },
    { "en": "Discrimination?", "id": "Diskriminasi." },
    { "en": "Dismissal?", "id": "Pemecatan." },
    { "en": "Displace?", "id": "Menggantikan." },
    { "en": "Disposal?", "id": "Pembuangan." },
    { "en": "Dispose?", "id": "Mengatur." },
    { "en": "Dispute?", "id": "Perselisihan, berselisih." },
    { "en": "Disrupt?", "id": "Mengganggu." },
    { "en": "Disruption?", "id": "Gangguan." },
    { "en": "Dissolve?", "id": "Larut." },
    { "en": "Distinction?", "id": "Perbedaan." },
    { "en": "Distinctive?", "id": "Khas." },
    { "en": "Distort?", "id": "Memutarbalikkan." },
    { "en": "Distress?", "id": "Kesulitan, menyusahkan." },
    { "en": "Disturbing?", "id": "Mengganggu." },
    { "en": "Divert?", "id": "Mengalihkan." },
    { "en": "Divine?", "id": "Ilahi." },
    { "en": "Doctrine?", "id": "Doktrin." },
    { "en": "Documentation?", "id": "Dokumentasi." },
    { "en": "Domain?", "id": "Domain." },
    { "en": "Dominance?", "id": "Dominasi." },
    { "en": "Donor?", "id": "Donor." },
    { "en": "Dose?", "id": "Dosis." },
    { "en": "Drain?", "id": "Menguras." },
    { "en": "Drift?", "id": "Hanyut." },
    { "en": "Driving?", "id": "Mengemudi." },
    { "en": "Drown?", "id": "Tenggelam." },
    { "en": "Dual?", "id": "Ganda." },
    { "en": "Dub?", "id": "Menyulih suara." },
    { "en": "Dumb?", "id": "Bodoh." },
    { "en": "Duo?", "id": "Duo." },
    { "en": "Dynamic (noun)?", "id": "Dinamis." },
    { "en": "Eager?", "id": "Bersemangat." },
    { "en": "Earnings?", "id": "Penghasilan." },
    { "en": "Ease?", "id": "Kemudahan, mempermudah." },
    { "en": "Echo?", "id": "Bergema, gema." },
    { "en": "Ecological?", "id": "Ekologis." },
    { "en": "Educator?", "id": "Pendidik." },
    { "en": "Effectiveness?", "id": "Efektivitas." },
    { "en": "Efficiency?", "id": "Efisiensi." },
    { "en": "Ego?", "id": "Ego." },
    { "en": "Elaborate?", "id": "Rumit." },
    { "en": "Embassy?", "id": "Kedutaan." },
    { "en": "Embed?", "id": "Menanamkan." },
    { "en": "Embody?", "id": "Mewujudkan." },
    { "en": "Emergence?", "id": "Kemunculan." },
    { "en": "Empirical?", "id": "Empiris." },
    { "en": "Empower?", "id": "Memberdayakan." },
    { "en": "Enact?", "id": "Menetapkan." },
    { "en": "Encompass?", "id": "Mencakup." },
    { "en": "Encouragement?", "id": "Dorongan." },
    { "en": "Encouraging?", "id": "Menggembirakan." },
    { "en": "Endeavour?", "id": "Usaha." },
    { "en": "Endless?", "id": "Tak berujung." },
    { "en": "Endorse?", "id": "Mendukung." },
    { "en": "Endorsement?", "id": "Dukungan." },
    { "en": "Endure?", "id": "Bertahan." },
    { "en": "Enforce?", "id": "Menegakkan." },
    { "en": "Enforcement?", "id": "Penegakan." },
    { "en": "Engagement?", "id": "Keterlibatan." },
    { "en": "Engaging?", "id": "Menarik." },
    { "en": "Enquire?", "id": "Menanyakan." },
    { "en": "Enrich?", "id": "Memperkaya." },
    { "en": "Enrol?", "id": "Mendaftar." },
    { "en": "Ensue?", "id": "Terjadi kemudian." },
    { "en": "Enterprise?", "id": "Perusahaan." },
    { "en": "Enthusiast?", "id": "Penggemar." },
    { "en": "Entitle?", "id": "Memberi hak." },
    { "en": "Entity?", "id": "Entitas." },
    { "en": "Epidemic?", "id": "Wabah." },
    { "en": "Equality?", "id": "Kesetaraan." },
    { "en": "Equation?", "id": "Persamaan." },
    { "en": "Erect?", "id": "Mendirikan." },
    { "en": "Escalate?", "id": "Meningkat." },
    { "en": "Essence?", "id": "Inti." },
    { "en": "Establishment?", "id": "Pendirian." },
    { "en": "Eternal?", "id": "Abadi." },
    { "en": "Evacuate?", "id": "Mengungsi." },
    { "en": "Evoke?", "id": "Membangkitkan." },
    { "en": "Evolutionary?", "id": "Evolusioner." },
    { "en": "Exaggerate?", "id": "Melebih-lebihkan." },
    { "en": "Excellence?", "id": "Keunggulan." },
    { "en": "Exceptional?", "id": "Luar biasa." },
    { "en": "Excess?", "id": "Kelebihan." },
    { "en": "Exclusion?", "id": "Pengucilan." },
    { "en": "Exclusive?", "id": "Eksklusif." },
    { "en": "Exclusively?", "id": "Secara eksklusif." },
    { "en": "Execute?", "id": "Mengeksekusi." },
    { "en": "Execution?", "id": "Eksekusi." },
    { "en": "Exert?", "id": "Mengarahkan." },
    { "en": "Exile?", "id": "Pengasingan." },
    { "en": "Exit (verb)?", "id": "Keluar." },
    { "en": "Expenditure?", "id": "Pengeluaran." },
    { "en": "Experimental?", "id": "Eksperimental." },
    { "en": "Expire?", "id": "Kadaluwarsa." },
    { "en": "Explicit?", "id": "Jelas." },
    { "en": "Explicitly?", "id": "Secara jelas." },
    { "en": "Exploitation?", "id": "Eksploitasi." },
    { "en": "Explosive?", "id": "Eksplosif." },
    { "en": "Extract (verb)?", "id": "Mengekstrak." },
    { "en": "Extremist?", "id": "Ekstremis." },
    { "en": "Facilitate?", "id": "Memfasilitasi." },
    { "en": "Faction?", "id": "Faksi." },
    { "en": "Faculty?", "id": "Fakultas." },
    { "en": "Fade?", "id": "Memudar." },
    { "en": "Fairness?", "id": "Keadilan." },
    { "en": "Fatal?", "id": "Fatal." },
    { "en": "Fate?", "id": "Takdir." },
    { "en": "Favourable?", "id": "Menguntungkan." },
    { "en": "Feat?", "id": "Prestasi." },
    { "en": "Feminist?", "id": "Feminis." },
    { "en": "Fibre?", "id": "Serat." },
    { "en": "Fierce?", "id": "Sengit." },
    { "en": "Film-maker?", "id": "Pembuat film." },
    { "en": "Filter?", "id": "Penyaring, menyaring." },
    { "en": "Fine (noun, verb)?", "id": "Denda, menghukum." },
    { "en": "Firearm?", "id": "Senjata api." },
    { "en": "Fit (noun)?", "id": "Cocok." },
    { "en": "Fixture?", "id": "Perlengkapan." },
    { "en": "Flaw?", "id": "Cacat." },
    { "en": "Flawed?", "id": "Cacat." },
    { "en": "Flee?", "id": "Melarikan diri." },
    { "en": "Fleet?", "id": "Armada." },
    { "en": "Flesh?", "id": "Daging." },
    { "en": "Flexibility?", "id": "Fleksibilitas." },
    { "en": "Flourish?", "id": "Berkembang." },
    { "en": "Fluid?", "id": "Cairan." },
    { "en": "Footage?", "id": "Rekaman." },
    { "en": "Foreigner?", "id": "Orang asing." },
    { "en": "Forge?", "id": "Memalsukan." },
    { "en": "Formula?", "id": "Rumus." },
    { "en": "Formulate?", "id": "Merumuskan." },
    { "en": "Forth?", "id": "Seterusnya." },
    { "en": "Forthcoming?", "id": "Yang akan datang." },
    { "en": "Foster?", "id": "Memupuk." },
    { "en": "Fragile?", "id": "Rapuh." },
    { "en": "Franchise?", "id": "Waralaba." },
    { "en": "Frankly?", "id": "Terus terang." },
    { "en": "Frustrated?", "id": "Frustasi." },
    { "en": "Frustrating?", "id": "Membuat frustasi." },
    { "en": "Frustration?", "id": "Kefrustrasian." },
    { "en": "Functional?", "id": "Fungsional." },
    { "en": "Fundraising?", "id": "Penggalangan dana." },
    { "en": "Funeral?", "id": "Pemakaman." },
    { "en": "Gallon?", "id": "Galon." },
    { "en": "Gambling?", "id": "Perjudian." },
    { "en": "Gathering?", "id": "Pertemuan." },
    { "en": "Gaze?", "id": "Tatapan, menatap." },
    { "en": "Gear?", "id": "Perlengkapan." },
    { "en": "Generic?", "id": "Generik." },
    { "en": "Genocide?", "id": "Genosida." },
    { "en": "Glance?", "id": "Pandangan sekilas, melirik." },
    { "en": "Glimpse?", "id": "Sekilas." },
    { "en": "Glorious?", "id": "Mulia." },
    { "en": "Glory?", "id": "Kejayaan." },
    { "en": "Governance?", "id": "Tata kelola." },
    { "en": "Grace?", "id": "Rahmat." },
    { "en": "Grasp?", "id": "Menggenggam." },
    { "en": "Grave (for dead person)?", "id": "Kuburan." },
    { "en": "Grave (serious)?", "id": "Serius." },
    { "en": "Gravity?", "id": "Gravitasi." },
    { "en": "Grid?", "id": "Grid." },
    { "en": "Grief?", "id": "Kesedihan." },
    { "en": "Grin?", "id": "Menyeringai, senyum lebar." },
    { "en": "Grind?", "id": "Menggiling." },
    { "en": "Grip?", "id": "Cengkeraman, mencengkram." },
    { "en": "Gross?", "id": "Kotor." },
    { "en": "Guerrilla?", "id": "Gerilya." },
    { "en": "Guidance?", "id": "Bimbingan." },
    { "en": "Guilt?", "id": "Rasa bersalah." },
    { "en": "Gut?", "id": "Usus." },
    { "en": "Hail?", "id": "Memanggil." },
    { "en": "Halfway?", "id": "Setengah jalan." },
    { "en": "Halt?", "id": "Berhenti." },
    { "en": "Handful?", "id": "Segenggam." },
    { "en": "Handling?", "id": "Penanganan." },
    { "en": "Handy?", "id": "Praktis." },
    { "en": "Harassment?", "id": "Pelecehan." },
    { "en": "Hardware?", "id": "Perangkat keras." },
    { "en": "Harmony?", "id": "Harmoni." },
    { "en": "Harsh?", "id": "Keras." },
    { "en": "Harvest?", "id": "Panen, memanen." },
    { "en": "Hatred?", "id": "Kebencian." },
    { "en": "Haunt?", "id": "Menghantui." },
    { "en": "Hazard?", "id": "Bahaya." },
    { "en": "Heighten?", "id": "Meningkatkan." },
    { "en": "Heritage?", "id": "Warisan." },
    { "en": "Hierarchy?", "id": "Hierarki." },
    { "en": "High-profile?", "id": "Profil tinggi." },
    { "en": "Hint?", "id": "Petunjuk, mengisyaratkan." },
    { "en": "Homeland?", "id": "Tanah air." },
    { "en": "Hook (verb)?", "id": "Mengaitkan." },
    { "en": "Hopeful?", "id": "Penuh harapan." },
    { "en": "Horizon?", "id": "Cakrawala." },
    { "en": "Horn?", "id": "Tanduk." },
    { "en": "Hostage?", "id": "Sandera." },
    { "en": "Hostile?", "id": "Bermusuhan." },
    { "en": "Hostility?", "id": "Permusuhan." },
    { "en": "Humanitarian?", "id": "Kemanusiaan." },
    { "en": "Humanity?", "id": "Kemanusiaan." },
    { "en": "Humble?", "id": "Rendah hati." },
    { "en": "Hydrogen?", "id": "Hidrogen." },
    { "en": "Identification?", "id": "Identifikasi." },
    { "en": "Ideological?", "id": "Ideologis." },
    { "en": "Ideology?", "id": "Ideologi." },
    { "en": "Idiot?", "id": "Idiot." },
    { "en": "Ignorance?", "id": "Ketidaktahuan." },
    { "en": "Imagery?", "id": "Pencitraan." },
    { "en": "Immense?", "id": "Sangat besar." },
    { "en": "Imminent?", "id": "Segera terjadi." },
    { "en": "Implementation?", "id": "Implementasi." },
    { "en": "Imprison?", "id": "Memenjarakan." },
    { "en": "Imprisonment?", "id": "Pemenjaraan." },
    { "en": "Inability?", "id": "Ketidakmampuan." },
    { "en": "Inadequate?", "id": "Tidak memadai." },
    { "en": "Inappropriate?", "id": "Tidak pantas." },
    { "en": "Incidence?", "id": "Kejadian." },
    { "en": "Inclined?", "id": "Cenderung." },
    { "en": "Inclusion?", "id": "Penyertaan." },
    { "en": "Incur?", "id": "Menanggung." },
    { "en": "Indicator?", "id": "Indikator." },
    { "en": "Indictment?", "id": "Dakwaan." },
    { "en": "Indigenous?", "id": "Pribumi." },
    { "en": "Induce?", "id": "Mendorong." },
    { "en": "Indulge?", "id": "Memanjakan." },
    { "en": "Inequality?", "id": "Ketidaksetaraan." },
    { "en": "Infamous?", "id": "Terkenal buruk." },
    { "en": "Infant?", "id": "Bayi." },
    { "en": "Infect?", "id": "Menginfeksi." },
    { "en": "Inflict?", "id": "Menimbulkan." },
    { "en": "Influential?", "id": "Berpengaruh." },
    { "en": "Inherent?", "id": "Melekat." },
    { "en": "Inhibit?", "id": "Menghambat." },
    { "en": "Initiate?", "id": "Memulai." },
    { "en": "Inject?", "id": "Menyuntikkan." },
    { "en": "Injection?", "id": "Suntikan." },
    { "en": "Injustice?", "id": "Ketidakadilan." },
    { "en": "Inmate?", "id": "Narapidana." },
    { "en": "Insertion?", "id": "Penyisipan." },
    { "en": "Insider?", "id": "Orang dalam." },
    { "en": "Inspect?", "id": "Memeriksa." },
    { "en": "Inspection?", "id": "Pemeriksaan." },
    { "en": "Inspiration?", "id": "Inspirasi." },
    { "en": "Instinct?", "id": "Naluri." },
    { "en": "Institutional?", "id": "Kelembagaan." },
    { "en": "Instruct?", "id": "Menginstruksikan." },
    { "en": "Instrumental?", "id": "Instrumental." },
    { "en": "Insufficient?", "id": "Tidak cukup." },
    { "en": "Insult?", "id": "Penghinaan, menghina." },
    { "en": "Intact?", "id": "Utuh." },
    { "en": "Intake?", "id": "Asupan." },
    { "en": "Integral?", "id": "Integral." },
    { "en": "Integrated?", "id": "Terintegrasi." },
    { "en": "Integration?", "id": "Integrasi." },
    { "en": "Integrity?", "id": "Integritas." },
    { "en": "Intellectual (noun)?", "id": "Intelektual." },
    { "en": "Intensify?", "id": "Mengintensifkan." },
    { "en": "Intensity?", "id": "Meningkatkan." },
    { "en": "Intensive?", "id": "Intensif." },
    { "en": "Intent?", "id": "Niat." },
    { "en": "Interactive?", "id": "Interaktif." },
    { "en": "Interface?", "id": "Antarmuka." },
    { "en": "Interfere?", "id": "Berinteraksi." },
    { "en": "Interference?", "id": "Gangguan." },
    { "en": "Interim?", "id": "Sementara." },
    { "en": "Interior?", "id": "Dalam, interior." },
    { "en": "Intermediate?", "id": "Menengah." },
    { "en": "Intervene?", "id": "Campur tangan." },
    { "en": "Intervention?", "id": "Intervensi." },
    { "en": "Intimate?", "id": "Intim." },
    { "en": "Intriguing?", "id": "Menarik." },
    { "en": "Investigator?", "id": "Penyelidik." },
    { "en": "Invisible?", "id": "Tak terlihat." },
    { "en": "Invoke?", "id": "Memanggil." },
    { "en": "Involvement?", "id": "Keterlibatan." },
    { "en": "Ironic?", "id": "Ionis." },
    { "en": "Ironically?", "id": "Secara ironis." },
    { "en": "Irony?", "id": "Ironi." },
    { "en": "Irrelevant?", "id": "Tidak relevan." },
    { "en": "Isolation?", "id": "Isolasi." },
    { "en": "Judicial?", "id": "Yudisial." },
    { "en": "Junction?", "id": "Persimpangan." },
    { "en": "Jurisdiction?", "id": "Yurisdiksi." },
    { "en": "Just?", "id": "Adil." },
    { "en": "Justification?", "id": "Pembenaran." },
    { "en": "Kidnap?", "id": "Menculik." },
    { "en": "Kidney?", "id": "Ginjal." },
    { "en": "Kingdom?", "id": "Kerajaan." },
    { "en": "Lad?", "id": "Pemuda." },
    { "en": "Landlord?", "id": "Tuan tanah." },
    { "en": "Landmark?", "id": "Tengara." },
    { "en": "Lap?", "id": "Pangkuan." },
    { "en": "Large-scale?", "id": "Skala besar." },
    { "en": "Laser?", "id": "Laser." },
    { "en": "Latter?", "id": "Yang terakhir." },
    { "en": "Lawn?", "id": "Halaman rumput." },
    { "en": "Lawsuit?", "id": "Gugatan hukum." },
    { "en": "Layout?", "id": "Tata letak." },
    { "en": "Leak?", "id": "Bocor, kebocoran." },
    { "en": "Leap?", "id": "Melompat, lompatan." },
    { "en": "Legacy?", "id": "Warisan." },
    { "en": "Legendary?", "id": "Legendaris." },
    { "en": "Legislation?", "id": "Undang-undang." },
    { "en": "Legislative?", "id": "Legislatif." },
    { "en": "Legislature?", "id": "Badan legislatif." },
    { "en": "Legitimate?", "id": "Sah." },
    { "en": "Lengthy?", "id": "Panjang." },
    { "en": "Lesbian?", "id": "Lesbian." },
    { "en": "Lesser?", "id": "Lebih kecil." },
    { "en": "Lethal?", "id": "Mematikan." },
    { "en": "Liable?", "id": "Bertanggung jawab." },
    { "en": "Liberal?", "id": "Liberal." },
    { "en": "Liberation?", "id": "Pembebasan." },
    { "en": "Liberty?", "id": "Kebebasan." },
    { "en": "License?", "id": "Melisensikan." },
    { "en": "Lifelong?", "id": "Seumur hidup." },
    { "en": "Likelihood?", "id": "Kemungkinan." },
    { "en": "Limb?", "id": "Anggota badan." },
    { "en": "Linear?", "id": "Linear." },
    { "en": "Line-up?", "id": "Barisan." },
    { "en": "Linger?", "id": "Berlama-lama." },
    { "en": "Listing?", "id": "Daftar." },
    { "en": "Literacy?", "id": "Literasi." },
    { "en": "Liver?", "id": "Hati." },
    { "en": "Lobby?", "id": "Lobi, melobi." },
    { "en": "Log?", "id": "Catatan, mencatat." },
    { "en": "Logic?", "id": "Logika." },
    { "en": "Long-standing?", "id": "Sudah lama." },
    { "en": "Long-time?", "id": "Lama." },
    { "en": "Loom?", "id": "Membayang." },
    { "en": "Loop?", "id": "Lingkaran." },
    { "en": "Loyalty?", "id": "Kesetiaan." },
    { "en": "Machinery?", "id": "Mesin." },
    { "en": "Magical?", "id": "Ajaib." },
    { "en": "Magistrate?", "id": "Hakim." },
    { "en": "Magnetic?", "id": "Magnetis." },
    { "en": "Magnitude?", "id": "Besaran." },
    { "en": "Mainland?", "id": "Daratan utama." },
    { "en": "Mainstream?", "id": "Arus utama." },
    { "en": "Maintenance?", "id": "Pemeliharaan." },
    { "en": "Mandate?", "id": "Mandat." },
    { "en": "Mandatory?", "id": "Wajib." },
    { "en": "Manifest?", "id": "Mewujudkan." },
    { "en": "Manipulate?", "id": "Memanipulasi." },
    { "en": "Manipulation?", "id": "Manipulasi." },
    { "en": "Manuscript?", "id": "Naskah." },
    { "en": "March?", "id": "Pawai, berbaris." },
    { "en": "Marginal?", "id": "Marjinal." },
    { "en": "Marine?", "id": "Kelautan." },
    { "en": "Marketplace?", "id": "Pasar." },
    { "en": "Mask?", "id": "Topeng." },
    { "en": "Massacre?", "id": "Pembantaian." },
    { "en": "Mathematical?", "id": "Matematis." },
    { "en": "Mature?", "id": "Matang, menjadi matang." },
    { "en": "Maximize?", "id": "Memaksimalkan." },
    { "en": "Meaningful?", "id": "Bermakna." },
    { "en": "Meantime?", "id": "Sementara itu." },
    { "en": "Medieval?", "id": "Abad pertengahan." },
    { "en": "Meditation?", "id": "Meditasi." },
    { "en": "Melody?", "id": "Melodi." },
    { "en": "Memo?", "id": "Memo." },
    { "en": "Memoir?", "id": "Memoar." },
    { "en": "Memorial?", "id": "Peringatan." },
    { "en": "Mentor?", "id": "Mentor." },
    { "en": "Merchant?", "id": "Pedagang." },
    { "en": "Mercy?", "id": "Belas kasihan." },
    { "en": "Mere?", "id": "Semata." },
    { "en": "Merely?", "id": "Hanya." },
    { "en": "Merge?", "id": "Bergabung." },
    { "en": "Merger?", "id": "Penggabungan." },
    { "en": "Merit?", "id": "Keunggulan." },
    { "en": "Methodology?", "id": "Metodologi." },
    { "en": "Midst?", "id": "Tengah-tengah." },
    { "en": "Migration?", "id": "Migrasi." },
    { "en": "Militant?", "id": "Militan." },
    { "en": "Militia?", "id": "Milisi." },
    { "en": "Mill?", "id": "Pabrik." },
    { "en": "Minimal?", "id": "Minimal." },
    { "en": "Minimize?", "id": "Meminimalkan." },
    { "en": "Mining?", "id": "Pertambangan." },
    { "en": "Ministry?", "id": "Kementerian." },
    { "en": "Minute (adjective)?", "id": "Sangat kecil." },
    { "en": "Miracle?", "id": "Mukjizat." },
    { "en": "Misery?", "id": "Penderitaan." },
    { "en": "Misleading?", "id": "Menyesatkan." },
    { "en": "Missile?", "id": "Rudal." },
    { "en": "Mob?", "id": "Kerumunan." },
    { "en": "Mobility?", "id": "Mobilitas." },
    { "en": "Mobilize?", "id": "Memobilisasi." },
    { "en": "Moderate?", "id": "Moderat." },
    { "en": "Modification?", "id": "Modifikasi." },
    { "en": "Momentum?", "id": "Momentum." },
    { "en": "Monk?", "id": "Biarawan." },
    { "en": "Monopoly?", "id": "Monopoli." },
    { "en": "Morality?", "id": "Moralitas." },
    { "en": "Motive?", "id": "Motif." },
    { "en": "Motorist?", "id": "Pengendara mobil." },
    { "en": "Municipal?", "id": "Kotamadya." },
    { "en": "Mutual?", "id": "Timbal balik." },
    { "en": "Namely?", "id": "Yaitu." },
    { "en": "Nationwide?", "id": "Nasional." },
    { "en": "Naval?", "id": "Angkatan laut." },
    { "en": "Neglect?", "id": "Mengabaikan, pengabaian." },
    { "en": "Neighbouring?", "id": "Tetangga." },
    { "en": "Nest?", "id": "Sarang." },
    { "en": "Net?", "id": "Bersih." },
    { "en": "Newsletter?", "id": "Buletin." },
    { "en": "Niche?", "id": "Ceruk." },
    { "en": "Noble?", "id": "Mulia." },
    { "en": "Nod?", "id": "Mengangguk." },
    { "en": "Nominate?", "id": "Mencalonkan." },
    { "en": "Nomination?", "id": "Nominasi." },
    { "en": "Nominee?", "id": "Calon." },
    { "en": "Nonetheless?", "id": "Meskipun demikian." },
    { "en": "Non-profit?", "id": "Nirlaba." },
    { "en": "Nonsense?", "id": "Omong kosong." },
    { "en": "Noon?", "id": "Tengah hari." },
    { "en": "Notable?", "id": "Terkemuka." },
    { "en": "Notably?", "id": "Terutama." },
    { "en": "Notify?", "id": "Memberi tahu." },
    { "en": "Notorious?", "id": "Terkenal (negatif)." },
    { "en": "Novel (adjective)?", "id": "Baru." },
    { "en": "Nursery?", "id": "Penitipan anak." },
    { "en": "Objection?", "id": "Keberatan." },
    { "en": "Oblige?", "id": "Mewajibkan." },
    { "en": "Obsess?", "id": "Terobsesi." },
    { "en": "Obsession?", "id": "Obsesi." },
    { "en": "Occasional?", "id": "Kadang-kadang." },
    { "en": "Occurrence?", "id": "Kejadian." },
    { "en": "Odds?", "id": "Kemungkinan." },
    { "en": "Offering?", "id": "Persembahan." },
    { "en": "Offspring?", "id": "Keturunan." },
    { "en": "Operational?", "id": "Operasional." },
    { "en": "Opt?", "id": "Memilih." },
    { "en": "Optical?", "id": "Optik." },
    { "en": "Optimism?", "id": "Optimisme." },
    { "en": "Oral?", "id": "Lisan." },
    { "en": "Organizational?", "id": "Organisasional." },
    { "en": "Orientation?", "id": "Orientasi." },
    { "en": "Originate?", "id": "Berasal." },
    { "en": "Outbreak?", "id": "Wabah." },
    { "en": "Outing?", "id": "Jalan-jalan." },
    { "en": "Outlet?", "id": "Saluran." },
    { "en": "Outlook?", "id": "Pandangan." },
    { "en": "Outrage?", "id": "Kemarahan." },
    { "en": "Outsider?", "id": "Orang luar." },
    { "en": "Overlook?", "id": "Mengabaikan." },
    { "en": "Overly?", "id": "Terlalu." },
    { "en": "Oversee?", "id": "Mengawasi." },
    { "en": "Overturn?", "id": "Membatalkan." },
    { "en": "Overwhelm?", "id": "Mengalahkan." },
    { "en": "Overwhelming?", "id": "Luar biasa." },
    { "en": "Pad?", "id": "Bantalan." },
    { "en": "Parameter?", "id": "Parameter." },
    { "en": "Parental?", "id": "Orang tua." },
    { "en": "Parish?", "id": "Paroki." },
    { "en": "Parliamentary?", "id": "Parlementer." },
    { "en": "Partial?", "id": "Sebagian." },
    { "en": "Partially?", "id": "Sebagian." },
    { "en": "Passing?", "id": "Kematian." },
    { "en": "Passive?", "id": "Pasif." },
    { "en": "Pastor?", "id": "Pendeta." },
    { "en": "Patch?", "id": "Tambalan." },
    { "en": "Patent?", "id": "Paten." },
    { "en": "Pathway?", "id": "Jalur." },
    { "en": "Patrol?", "id": "Patroli, berpatroli." },
    { "en": "Patron?", "id": "Pelindung." },
    { "en": "Peak?", "id": "Puncak." },
    { "en": "Peasant?", "id": "Petani." },
    { "en": "Peculiar?", "id": "Aneh." },
    { "en": "Persist?", "id": "Bertahan." },
    { "en": "Persistent?", "id": "Gigih." },
    { "en": "Personnel?", "id": "Personel." },
    { "en": "Petition?", "id": "Petisi." },
    { "en": "Philosopher?", "id": "Filsuf." },
    { "en": "Philosophical?", "id": "Filosofis." },
    { "en": "Physician?", "id": "Dokter." },
    { "en": "Pioneer?", "id": "Pelopor, merintis." },
    { "en": "Pipeline?", "id": "Pipa." },
    { "en": "Pirate?", "id": "Bajak laut." },
    { "en": "Pit?", "id": "Lubang." },
    { "en": "Plea?", "id": "Permohonan." },
    { "en": "Plead?", "id": "Mengajukan permohonan." },
    { "en": "Pledge?", "id": "Janji, berjanji." },
    { "en": "Plug?", "id": "Colokan, memasang." },
    { "en": "Plunge?", "id": "Terjun." },
    { "en": "Pole?", "id": "Kutub." },
    { "en": "Poll?", "id": "Jajak pendapat." },
    { "en": "Pond?", "id": "Kolam." },
    { "en": "Pop?", "id": "Meletus." },
    { "en": "Portfolio?", "id": "Portofolio." },
    { "en": "Portray?", "id": "Menggambarkan." },
    { "en": "Postpone?", "id": "Menunda." },
    { "en": "Post-war?", "id": "Pascaperang." },
    { "en": "Practitioner?", "id": "Praktisi." },
    { "en": "Preach?", "id": "Berkhotbah." },
    { "en": "Precedent?", "id": "Preseden." },
    { "en": "Precision?", "id": "Presisi." },
    { "en": "Predator?", "id": "Predator." },
    { "en": "Predecessor?", "id": "Pendahulu." },
    { "en": "Predominantly?", "id": "Terutama." },
    { "en": "Pregnancy?", "id": "Kehamilan." },
    { "en": "Prejudice?", "id": "Prasangka." },
    { "en": "Preliminary?", "id": "Pendahuluan." },
    { "en": "Premier?", "id": "Perdana menteri." },
    { "en": "Premise?", "id": "Premis." },
    { "en": "Premium?", "id": "Premi." },
    { "en": "Prescribe?", "id": "Meresepkan." },
    { "en": "Prescription?", "id": "Resep." },
    { "en": "Presently?", "id": "Saat ini." },
    { "en": "Preservation?", "id": "Pelestarian." },
    { "en": "Preside?", "id": "Memimpin." },
    { "en": "Presidency?", "id": "Kepresidenan." },
    { "en": "Presidential?", "id": "Presidensial." }

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
