# Kuis-Matematika-Kelas-XI-Ganjil-
<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kuis Matematika Interaktif</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- MathJax Configuration -->
  <script>
    window.MathJax = {
      tex: {
        inlineMath: [['$', '$'], ['\\(', '\\)']],
        displayMath: [['$$', '$$'], ['\\[', '\\]']]
      },
      svg: {
        fontCache: 'global'
      }
    };
  </script>
  <script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js" async></script>
  <style>
    .math-content {
      font-size: 1.1rem;
      line-height: 1.6;
    }
  </style>
</head>
<body class="bg-slate-900 text-slate-100 min-h-screen flex flex-col items-center justify-center p-4">

  <div class="w-full max-w-3xl bg-slate-800 rounded-2xl shadow-2xl border border-slate-700 p-6 md:p-8 my-6">
    <!-- Header -->
    <div class="flex flex-col md:flex-row md:items-center justify-between border-b border-slate-700 pb-4 mb-6 gap-2">
      <div>
        <h1 class="text-2xl font-bold text-indigo-400">Kuis Matematika Interaktif</h1>
        <p class="text-sm text-slate-400">Fungsi Komposisi, Invers, dan Lingkaran</p>
      </div>
      <div id="progress-container" class="text-right">
        <span id="question-number" class="text-lg font-semibold text-slate-300">Soal 1 dari 14</span>
        <div class="w-full bg-slate-700 rounded-full h-2.5 mt-2 md:w-36">
          <div id="progress-bar" class="bg-indigo-500 h-2.5 rounded-full transition-all duration-300" style="width: 7%"></div>
        </div>
      </div>
    </div>

    <!-- Quiz Screen -->
    <div id="quiz-box" class="space-y-6">
      <!-- Question -->
      <div class="bg-slate-900/60 p-5 rounded-xl border border-slate-700/80">
        <p id="question-text" class="text-lg font-medium text-slate-200 math-content leading-relaxed"></p>
      </div>

      <!-- Hint Box -->
      <div id="hint-box" class="hidden bg-amber-950/40 border border-amber-500/30 text-amber-200 p-4 rounded-xl text-sm">
        <span class="font-bold text-amber-400">💡 Petunjuk:</span> <span id="hint-text"></span>
      </div>

      <!-- Options -->
      <div id="options-container" class="grid grid-cols-1 gap-3"></div>

      <!-- Rationale / Rincian Jawaban -->
      <div id="rationale-box" class="hidden bg-indigo-950/40 border border-indigo-500/30 text-slate-200 p-5 rounded-xl space-y-2">
        <h4 class="font-semibold text-indigo-300 flex items-center gap-2">
          <span>📘 Pembahasan:</span>
        </h4>
        <p id="rationale-text" class="text-sm math-content leading-relaxed text-slate-300"></p>
      </div>

      <!-- Navigation Buttons -->
      <div class="flex items-center justify-between pt-4 border-t border-slate-700/60">
        <button id="hint-btn" onclick="toggleHint()" class="px-4 py-2 bg-slate-700 hover:bg-slate-600 text-slate-200 text-sm font-medium rounded-lg transition-colors">
          💡 Tampilkan Petunjuk
        </button>
        <button id="next-btn" onclick="nextQuestion()" disabled class="px-6 py-2 bg-indigo-600 hover:bg-indigo-500 disabled:bg-slate-700 disabled:text-slate-500 text-white font-medium rounded-lg transition-all shadow-md">
          Lanjut ➔
        </button>
      </div>
    </div>

    <!-- Results Screen -->
    <div id="result-box" class="hidden text-center py-8 space-y-6">
      <div class="inline-flex items-center justify-center w-20 h-20 bg-indigo-600/20 text-indigo-400 rounded-full border border-indigo-500/30 mb-2">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
        </svg>
      </div>
      <h2 class="text-3xl font-bold text-slate-100">Kuis Selesai!</h2>
      <p class="text-slate-400">Berikut adalah hasil pencapaian Kamu:</p>
      
      <div class="grid grid-cols-3 gap-4 max-w-md mx-auto my-6">
        <div class="bg-slate-900/80 p-4 rounded-xl border border-slate-700">
          <div class="text-xs text-slate-400">Skor Akhir</div>
          <div id="score-text" class="text-2xl font-extrabold text-indigo-400 mt-1">0%</div>
        </div>
        <div class="bg-slate-900/80 p-4 rounded-xl border border-slate-700">
          <div class="text-xs text-slate-400">Benar</div>
          <div id="correct-text" class="text-2xl font-extrabold text-emerald-400 mt-1">0</div>
        </div>
        <div class="bg-slate-900/80 p-4 rounded-xl border border-slate-700">
          <div class="text-xs text-slate-400">Salah</div>
          <div id="wrong-text" class="text-2xl font-extrabold text-rose-400 mt-1">0</div>
        </div>
      </div>

      <button onclick="restartQuiz()" class="px-8 py-3 bg-indigo-600 hover:bg-indigo-500 text-white font-semibold rounded-xl shadow-lg transition-all">
        🔄 Ulangi Kuis
      </button>
    </div>
  </div>

  <script>
    const questionsData = [
      {
        question: "1. Sebuah industri menggunakan Mesin I: $g(x) = \\frac{x - 1}{x + 4}$ dan Mesin II: $f(g) = 2g + 5$. Rumus fungsi total output $(f \\circ g)(x)$ adalah:",
        options: [
          { text: "$\\frac{7x + 18}{x + 4}$", isCorrect: true, rationale: "Substitusi $g(x)$ ke $f(g)$: $2\\left(\\frac{x - 1}{x + 4}\\right) + 5 = \\frac{2x - 2 + 5(x + 4)}{x + 4} = \\frac{7x + 18}{x + 4}$." },
          { text: "$\\frac{7x + 18}{-x + 4}$", isCorrect: false, rationale: "Terjadi kesalahan tanda pada penyebut $-x+4$." },
          { text: "$\\frac{7x + 15}{-x - 4}$", isCorrect: false, rationale: "Kesalahan perhitungan pembilang dan perkalian penyebut." },
          { text: "$\\frac{7x - 18}{x - 4}$", isCorrect: false, rationale: "Terjadi kesalahan pengurangan tanda minus pada pembilang." },
          { text: "$\\frac{7x + 15}{x + 4}$", isCorrect: false, rationale: "Lupa menambahkan perkalian konstanta $5 \\times 4 = 20$ saat menyamakan penyebut." }
        ],
        hint: "Substitusikan bentuk $g(x)$ ke dalam $f(g) = 2g + 5$ lalu samakan penyebutnya."
      },
      {
        question: "2. Diketahui $f(x) = 3x - 5$ dan $g(x) = \\frac{x - 1}{2 - x}$, $x \\ne 2$. Hasil fungsi $(g \\circ f)(x)$ adalah:",
        options: [
          { text: "$\\frac{3x - 6}{7 - 3x}$", isCorrect: true, rationale: "$(g \\circ f)(x) = g(3x - 5) = \\frac{(3x - 5) - 1}{2 - (3x - 5)} = \\frac{3x - 6}{7 - 3x}$." },
          { text: "$\\frac{3x - 6}{7 + 3x}$", isCorrect: false, rationale: "Kesalahan distributive property saat mendistribusikan minus $2 - (3x - 5)$." },
          { text: "$-\\frac{3x + 6}{7 - 3x}$", isCorrect: false, rationale: "Tanda minus pada pembilang tidak difaktorkan dengan benar." },
          { text: "$-\\frac{3x + 6}{7 + 3x}$", isCorrect: false, rationale: "Kesalahan ganda pada pembilang dan tanda penyebut." },
          { text: "$\\frac{-3x - 6}{7 - 3x}$", isCorrect: false, rationale: "Tanda dari $-3x$ di pembilang tertukar." }
        ],
        hint: "Gantikan $x$ pada fungsi $g(x)$ dengan fungsi $f(x) = 3x - 5$."
      },
      {
        question: "3. Jika $f(x) = 2x + 3$ dan $(g \\circ f)(x) = 2x - 1$, maka $g(x)$ adalah:",
        options: [
          { text: "$x - 2$", isCorrect: false, rationale: "Terjadi kesalahan substitusi variabel $x = \\frac{y-3}{2}$." },
          { text: "$2x - 1$", isCorrect: false, rationale: "Hanya menyalin fungsi komposit tanpa memasukkan fungsi dalam." },
          { text: "$2x - 2$", isCorrect: false, rationale: "Kesalahan perhitungan pengoperasian konstanta." },
          { text: "$x + 1$", isCorrect: false, rationale: "Salah tanda saat memindahkan suku konstanta." },
          { text: "$x - 4$", isCorrect: true, rationale: "Misal $y = 2x + 3 \\implies x = \\frac{y-3}{2}$. Maka $g(y) = 2\\left(\\frac{y-3}{2}\\right) - 1 = y - 4$, sehingga $g(x) = x - 4$. (Dipilih opsi e/koreksi matematis $x-4$ / sesuai representasi)." }
        ],
        hint: "Misalkan $y = f(x)$, cari nilai $x$ dalam bentuk $y$, lalu substitusikan ke $(g \\circ f)(x)$."
      },
      {
        question: "4. Jumlah kertas modul $k(x) = 250(x + 1)$. Biaya $b(k) = 400k + 20.000$. Jika pengeluaran Rp 10.120.000,00, banyak eksemplar modul $x$ adalah:",
        options: [
          { text: "$100$", isCorrect: true, rationale: "$400k + 20.000 = 10.120.000 \\implies 400k = 10.100.000 \\implies k = 25.250$. Lalu $250(x+1) = 25.250 \\implies x+1 = 101 \\implies x = 100$." },
          { text: "$150$", isCorrect: false, rationale: "Kesalahan pembagian $10.100.000 / 400$." },
          { text: "$110$", isCorrect: false, rationale: "Lupa mengurangkan $x + 1 = 101$ dengan 1." },
          { text: "$135$", isCorrect: false, rationale: "Kesalahan perhitungan pada langkah pertidaksamaan biaya." },
          { text: "$95$", isCorrect: false, rationale: "Mengurangkan nilai awal modul secara tidak presisi." }
        ],
        hint: "Cari dulu nilai lembar kertas $k$ dari fungsi $b(k) = 10.120.000$, lalu cari $x$ dari $k(x)$."
      },
      {
        question: "5. Jika $f(x) = 2x + 5$ dan $g(x) = \\frac{x - 1}{x + 4}$, $x \\ne -4$, maka $(f \\circ g)(x)$ adalah:",
        options: [
          { text: "$\\frac{7x + 18}{x + 4}$", isCorrect: true, rationale: "$(f \\circ g)(x) = 2\\left(\\frac{x-1}{x+4}\\right) + 5 = \\frac{2x - 2 + 5x + 20}{x + 4} = \\frac{7x + 18}{x + 4}$." },
          { text: "$\\frac{7x + 18}{-x + 4}$", isCorrect: false, rationale: "Tanda pada penyebut salah $(-x+4)$." },
          { text: "$\\frac{7x + 15}{-x - 4}$", isCorrect: false, rationale: "Kesalahan penyebut dan penjumlah konstanta." },
          { text: "$\\frac{7x - 18}{x - 4}$", isCorrect: false, rationale: "Tanda negatif pada $-18$ tidak akurat." },
          { text: "$\\frac{7x + 15}{x + 4}$", isCorrect: false, rationale: "Kesalahan perhitungan $-2 + 20$ menjadi $15$." }
        ],
        hint: "Substitusikan fungsi $g(x)$ ke dalam variabel $x$ pada $f(x)$."
      },
      {
        question: "6. Diketahui $f(x) = 3x + 5$ dan $g(x) = \\frac{2x}{x + 1}$, $x \\ne -1$. Rumus $(g \\circ f)(x)$ adalah:",
        options: [
          { text: "$\\frac{6x + 10}{3x + 6}$", isCorrect: true, rationale: "$(g \\circ f)(x) = \\frac{2(3x + 5)}{(3x + 5) + 1} = \\frac{6x + 10}{3x + 6}$." },
          { text: "$\\frac{6x - 10}{3x + 6}$", isCorrect: false, rationale: "Salah tanda pada pembilang $6x - 10$." },
          { text: "$\\frac{-6x + 10}{3x + 6}$", isCorrect: false, rationale: "Salah tanda minus pada koefisien $x$ pembilang." },
          { text: "$\\frac{6x - 10}{3x - 6}$", isCorrect: false, rationale: "Salah tanda pada penyebut $3x - 6$." },
          { text: "$\\frac{3x + 5}{6x + 10}$", isCorrect: false, rationale: "Posisi pembilang dan penyebut terbalik." }
        ],
        hint: "Gantikan setiap $x$ pada $g(x)$ dengan $(3x + 5)$."
      },
      {
        question: "7. Diketahui $f(x) + g(x) = 2x + 3$, maka nilai $(f + g)(4)$ adalah:",
        options: [
          { text: "$11$", isCorrect: true, rationale: "$(f + g)(4) = f(4) + g(4) = 2(4) + 3 = 8 + 3 = 11$." },
          { text: "$9$", isCorrect: false, rationale: "Kesalahan perkalian $2 \\times 4 = 6$." },
          { text: "$8$", isCorrect: false, rationale: "Hanya menghitung $2 \\times 4$ tanpa menjumlahkan $3$." },
          { text: "$7$", isCorrect: false, rationale: "Kesalahan evaluasi fungsi pada $x = 4$." },
          { text: "$6$", isCorrect: false, rationale: "Perhitungan tidak lengkap." }
        ],
        hint: "Langsung substitusikan $x = 4$ ke dalam persamaan $(2x + 3)$."
      },
      {
        question: "8. Diketahui $f(x) = x - 4$. Nilai $f(x^2) - (f(x))^2 + 3f(x)$ untuk $x = 2$ adalah:",
        options: [
          { text: "$-10$", isCorrect: true, rationale: "Untuk $x=2$: $f(4) = 0$, $f(2) = -2$. Nilainya $0 - (-2)^2 + 3(-2) = 0 - 4 - 6 = -10$." },
          { text: "$-9$", isCorrect: false, rationale: "Kesalahan tanda kuadrat $(-2)^2$." },
          { text: "$-8$", isCorrect: false, rationale: "Kesalahan perkalian $3(-2)$." },
          { text: "$-7$", isCorrect: false, rationale: "Kesalahan saat menjumlahkan $-4 - 6$." },
          { text: "$-6$", isCorrect: false, rationale: "Lupa memperhitungkan nilai $-(f(2))^2$." }
        ],
        hint: "Hitung nilai $f(4)$ dan $f(2)$ terlebih dahulu secara terpisah."
      },
      {
        question: "9. Diketahui $g(x) = 2x + 3$ dan $(f \\circ g)(x) = 12x^2 + 32x + 26$, maka rumus $f(x)$ adalah:",
        options: [
          { text: "$3x^2 - 2x + 5$", isCorrect: true, rationale: "Misal $y = 2x+3 \\implies x = \\frac{y-3}{2}$. $f(y) = 12\\left(\\frac{y-3}{2}\\right)^2 + 32\\left(\\frac{y-3}{2}\\right) + 26 = 3y^2 - 2y + 5$." },
          { text: "$3x^2 - 5x + 5$", isCorrect: false, rationale: "Kesalahan ekspansi linear pada $-18y + 16y = -2y$." },
          { text: "$3x^2 - x + 8$", isCorrect: false, rationale: "Salah penggabungan suku konstanta $27 - 48 + 26$." },
          { text: "$2x^2 - 5x + 3$", isCorrect: false, rationale: "Salah kuadrat awal $12 / 4 = 3$ (bukan $2$)." },
          { text: "$2x^2 - 2x + 8$", isCorrect: false, rationale: "Koefisien $x^2$ tidak tepat." }
        ],
        hint: "Misalkan $y = 2x + 3$, nyatakan $x$ dalam $y$ lalu substitusi ke fungsi komposit."
      },
      {
        question: "10. Diketahui $f(x) = 3x - 5$ dan $g(x) = \\frac{x - 1}{2 - x}$, $x \\ne 2$. Hasil $(f \\circ g)(x)$ adalah:",
        options: [
          { text: "$\\frac{8x - 13}{2 - x}$", isCorrect: true, rationale: "$(f \\circ g)(x) = 3\\left(\\frac{x-1}{2-x}\\right) - 5 = \\frac{3x - 3 - 5(2 - x)}{2 - x} = \\frac{8x - 13}{2 - x}$." },
          { text: "$\\frac{8x + 13}{2 - x}$", isCorrect: false, rationale: "Kesalahan tanda $-3 - 10 = -13$ menjadi $+13$." },
          { text: "$\\frac{8x - 13}{2 + x}$", isCorrect: false, rationale: "Penyebut berubah tanda secara tidak sah." },
          { text: "$-\\frac{8x - 13}{2 - x}$", isCorrect: false, rationale: "Menambahkan tanda minus ekstra di depan pecahan." },
          { text: "$\\frac{8x - 13}{-2 - x}$", isCorrect: false, rationale: "Kesalahan penyebut $-2-x$." }
        ],
        hint: "Substitusikan fungsi $g(x)$ ke dalam $f(x) = 3x - 5$."
      },
      {
        question: "11. Toko S memberikan diskon $20\\%$ dilanjutkan potongan Rp $20.000,00$. Persamaan matematis fungsinya adalah:",
        options: [
          { text: "$0,8x - 20.000$", isCorrect: true, rationale: "Diskon $20\\%$ artinya sisa harga adalah $(100\\% - 20\\%)x = 0,8x$. Dipotong lagi $20.000$ menjadi $0,8x - 20.000$." },
          { text: "$0,18x - 20.000$", isCorrect: false, rationale: "Salah merubah $20\\%$ menjadi $0,18$." },
          { text: "$0,10x - 20.000$", isCorrect: false, rationale: "Menggunakan angka $0,10$." },
          { text: "$0,12x - 20.000$", isCorrect: false, rationale: "Nilai persentase tidak tepat." },
          { text: "$0,14x - 20.000$", isCorrect: false, rationale: "Angka desimal diskon keliru." }
        ],
        hint: "Ingat bahwa harga setelah diskon $20\\%$ adalah $80\\%$ ($0,8$) dari harga awal $x$."
      },
      {
        question: "12. Pendapatan Kotor $f(x) = 5x - 6$ dan Biaya Operasional $g(x) = 2x + 3$. Rumus keuntungan bersih $(f - g)(x)$ adalah:",
        options: [
          { text: "$3x - 9$", isCorrect: true, rationale: "$(f - g)(x) = (5x - 6) - (2x + 3) = 5x - 6 - 2x - 3 = 3x - 9$." },
          { text: "$3x + 7$", isCorrect: false, rationale: "Kesalahan tanda $-6 - (+3) = -9$, bukan $+7$." },
          { text: "$-3x - 9$", isCorrect: false, rationale: "Salah pengurangan $5x - 2x = 3x$." },
          { text: "$3x - 5$", isCorrect: false, rationale: "Salah penjumlahan konstanta." },
          { text: "$3x + 5$", isCorrect: false, rationale: "Lupa mendistribusikan negatif ke angka $3$." }
        ],
        hint: "Kurangkan $(5x - 6)$ dengan $(2x + 3)$. Hati-hati dengan distribusi tanda minus!"
      },
      {
        question: "13. Nilai $f^{-1}(x)$ dari $f(x) = -3x + 1$ adalah:",
        options: [
          { text: "$\\frac{-x + 1}{3}$", isCorrect: true, rationale: "$y = -3x + 1 \\implies 3x = 1 - y \\implies x = \\frac{1 - y}{3} = \\frac{-x + 1}{3}$." },
          { text: "$\\frac{-x - 1}{3}$", isCorrect: false, rationale: "Kesalahan pemindahan konstanta $+1$." },
          { text: "$\\frac{-x + 1}{-3}$", isCorrect: false, rationale: "Tanda pembagi negatif ganda." },
          { text: "$\\frac{-x - 1}{-3}$", isCorrect: false, rationale: "Kesalahan pembagian tanda minus." },
          { text: "$\\frac{x - 1}{3}$", isCorrect: false, rationale: "Terbalik tanda koefisien $-x$." }
        ],
        hint: "Ubah $y = -3x + 1$ menjadi $x = ...$"
      },
      {
        question: "14. Lingkaran A berjari-jari 2 satuan. Jika Panjang BC = 2, besar sudut BDC adalah:",
        options: [
          { text: "$30^\\circ$", isCorrect: true, rationale: "Jari-jari $AB = AC = 2$. Karena $BC = 2$, maka $\\triangle ABC$ sama sisi, sudut pusat $\\angle BAC = 60^\\circ$. Sudut keliling $\\angle BDC = \\frac{1}{2} \\times 60^\\circ = 30^\\circ$." },
          { text: "$45^\\circ$", isCorrect: false, rationale: "Menganggap sudut pusat $90^\\circ$." },
          { text: "$75^\\circ$", isCorrect: false, rationale: "Kesalahan hubungan sudut pusat dan keliling." },
          { text: "$60^\\circ$", isCorrect: false, rationale: "Ini adalah nilai sudut pusat $\\angle BAC$, bukan sudut keliling $\\angle BDC$." },
          { text: "$90^\\circ$", isCorrect: false, rationale: "Menganggap siku-siku di keliling." }
        ],
        hint: "Segitiga $ABC$ dibentuk oleh dua jari-jari dan tali busur $BC$. Sudut keliling adalah setengah sudut pusat."
      }
    ];

    let currentQuestionIndex = 0;
    let score = 0;
    let answered = false;

    function loadQuestion() {
      answered = false;
      const q = questionsData[currentQuestionIndex];

      document.getElementById('question-number').innerText = `Soal ${currentQuestionIndex + 1} dari ${questionsData.length}`;
      document.getElementById('progress-bar').style.width = `${((currentQuestionIndex + 1) / questionsData.length) * 100}%`;
      
      document.getElementById('question-text').innerHTML = q.question;
      document.getElementById('hint-text').innerText = q.hint;

      // Hide elements
      document.getElementById('hint-box').classList.add('hidden');
      document.getElementById('rationale-box').classList.add('hidden');
      document.getElementById('next-btn').disabled = true;

      const optionsContainer = document.getElementById('options-container');
      optionsContainer.innerHTML = '';

      q.options.forEach((opt, idx) => {
        const btn = document.createElement('button');
        btn.className = "option-btn w-full text-left p-4 rounded-xl border border-slate-700 bg-slate-800/80 hover:bg-slate-700/80 transition-all text-slate-200 math-content flex items-center justify-between";
        btn.innerHTML = `<span>${opt.text}</span> <span class="status-icon"></span>`;
        btn.onclick = () => selectOption(idx);
        optionsContainer.appendChild(btn);
      });

      if (window.MathJax) {
        MathJax.typesetPromise();
      }
    }

    function selectOption(index) {
      if (answered) return;
      answered = true;

      const q = questionsData[currentQuestionIndex];
      const optionsBtns = document.querySelectorAll('.option-btn');
      const selectedOption = q.options[index];

      optionsBtns.forEach((btn, idx) => {
        btn.disabled = true;
        if (q.options[idx].isCorrect) {
          btn.classList.remove('bg-slate-800/80', 'border-slate-700');
          btn.classList.add('bg-emerald-950/60', 'border-emerald-500/80', 'text-emerald-200');
          btn.querySelector('.status-icon').innerHTML = '✅';
        } else if (idx === index) {
          btn.classList.remove('bg-slate-800/80', 'border-slate-700');
          btn.classList.add('bg-rose-950/60', 'border-rose-500/80', 'text-rose-200');
          btn.querySelector('.status-icon').innerHTML = '❌';
        }
      });

      if (selectedOption.isCorrect) {
        score++;
      }

      // Show rationale
      document.getElementById('rationale-text').innerHTML = selectedOption.rationale;
      document.getElementById('rationale-box').classList.remove('hidden');

      document.getElementById('next-btn').disabled = false;

      if (window.MathJax) {
        MathJax.typesetPromise();
      }
    }

    function toggleHint() {
      const hintBox = document.getElementById('hint-box');
      hintBox.classList.toggle('hidden');
    }

    function nextQuestion() {
      currentQuestionIndex++;
      if (currentQuestionIndex < questionsData.length) {
        loadQuestion();
      } else {
        showResults();
      }
    }

    function showResults() {
      document.getElementById('quiz-box').classList.add('hidden');
      document.getElementById('progress-container').classList.add('hidden');
      document.getElementById('result-box').classList.remove('hidden');

      const percentage = Math.round((score / questionsData.length) * 100);
      document.getElementById('score-text').innerText = `${percentage}%`;
      document.getElementById('correct-text').innerText = score;
      document.getElementById('wrong-text').innerText = questionsData.length - score;
    }

    function restartQuiz() {
      currentQuestionIndex = 0;
      score = 0;
      document.getElementById('result-box').classList.add('hidden');
      document.getElementById('progress-container').classList.remove('hidden');
      document.getElementById('quiz-box').classList.remove('hidden');
      loadQuestion();
    }

    // Initialize
    window.onload = loadQuestion;
  </script>
</body>
</html>
```eof

### Fitur Utama Kuis:
- **Tampilan Interaktif**: Setiap pilihan jawaban dapat langsung diklik.
- **Umpan Balik Seketika**: Pilihan yang benar akan disorot warna hijau (✅) dan pilihan salah akan disorot warna merah (❌).
- **Pembahasan Otomatis**: Penjelasan dan langkah-langkah penyelesaian muncul secara otomatis setelah Kamu memilih jawaban.
- **Fitur Petunjuk (Hint)**: Tersedia tombol untuk menampilkan petunjuk singkat jika Kamu mengalami kesulitan.
- **Tampilan Matematika Rapi**: Menggunakan library MathJax untuk merender rumus-rumus fungsi dan pecahan dengan jelas.
