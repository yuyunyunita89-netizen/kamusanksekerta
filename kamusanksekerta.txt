import streamlit as st
import random
import pandas as pd

# Konfigurasi halaman
st.set_page_config(
    page_title="Kamus Sanskerta-Indonesia",
    page_icon="🕉️",
    layout="wide"
)

# Kamus Sanskerta-Indonesia dengan 200 kata
kamus_sanskerta = {
    # Kata Benda Umum
    "nara": "pria", "nari": "wanita", "balaka": "anak laki-laki", "balika": "anak perempuan",
    "pita": "ayah", "mata": "ibu", "bhrata": "saudara laki-laki", "bhagini": "saudara perempuan",
    "putra": "putra", "putri": "putri", "grha": "rumah", "vana": "hutan",
    "nagara": "kota", "grama": "desa", "jala": "air", "agni": "api",
    "vayu": "angin", "prthvi": "bumi", "akasha": "langit", "surya": "matahari",
    "chandra": "bulan", "tara": "bintang", "parvata": "gunung", "nadi": "sungai",
    "sagara": "laut", "vrksha": "pohon", "pushpa": "bunga", "phala": "buah",
    "patra": "daun", "mula": "akar",
    
    # Hewan
    "gaja": "gajah", "ashva": "kuda", "go": "sapi", "shuna": "anjing",
    "marjara": "kucing", "simha": "singa", "vyaghra": "harimau", "vanara": "monyet",
    "kaka": "gagak", "mayura": "merak", "hamsa": "angsa", "garuda": "garuda",
    "sarpa": "ular", "matsya": "ikan", "makshika": "lalat", "bhramara": "lebah",
    "kukuta": "ayam", "shvapada": "hewan berkaki empat", "pakshi": "burung", "mrga": "rusa",
    
    # Kata Sifat
    "sundara": "cantik/indah", "vishala": "besar", "laghu": "kecil", "uccha": "tinggi",
    "nicha": "rendah", "dirgha": "panjang", "hrasva": "pendek", "nutana": "baru",
    "puratana": "lama/kuno", "shubha": "baik", "ashubha": "buruk", "satya": "benar",
    "asatya": "salah", "priya": "sayang/kesayangan", "shuddha": "bersih/murni", "madhura": "manis",
    "tikshna": "tajam", "komala": "lembut", "kathora": "keras", "ushna": "panas",
    "shita": "dingin", "gambhira": "dalam", "uttama": "terbaik", "adhama": "terburuk",
    
    # Kata Kerja
    "gam": "pergi", "agam": "datang", "khad": "makan", "pa": "minum",
    "path": "membaca", "likh": "menulis", "kr": "membuat", "bhu": "menjadi",
    "as": "adalah/ada", "vad": "berkata", "shru": "mendengar", "drsh": "melihat",
    "has": "tertawa", "rud": "menangis", "stha": "berdiri", "uttha": "bangun",
    "shi": "tidur", "jag": "terjaga", "dhav": "berlari", "chal": "berjalan",
    "jiv": "hidup", "mr": "mati", "labh": "mendapat", "da": "memberi", "grah": "mengambil",
    
    # Angka
    "eka": "satu", "dvi": "dua", "tri": "tiga", "chatur": "empat",
    "pancha": "lima", "shat": "enam", "sapta": "tujuh", "ashta": "delapan",
    "nava": "sembilan", "dasha": "sepuluh", "ekadasha": "sebelas", "dvadasha": "dua belas",
    "vimsati": "dua puluh", "trimsat": "tiga puluh", "shata": "seratus", "sahasra": "seribu",
    
    # Waktu
    "dina": "hari", "ratri": "malam", "pratah": "pagi", "sayam": "sore",
    "madhyahna": "siang", "masa": "bulan (waktu)", "varsha": "tahun", "kala": "waktu",
    "adya": "hari ini", "shvah": "besok", "hyah": "kemarin", "sandhi": "senja", "samaya": "waktu/saat",
    
    # Warna
    "shveta": "putih", "krishna": "hitam", "rakta": "merah", "nila": "biru",
    "pita": "kuning", "harita": "hijau", "dhusara": "abu-abu", "kapila": "cokelat",
    
    # Tubuh
    "shira": "kepala", "mukha": "wajah/mulut", "netra": "mata", "karna": "telinga",
    "nasika": "hidung", "danta": "gigi", "jihva": "lidah", "hasta": "tangan",
    "pada": "kaki", "anga": "anggota tubuh", "hrdaya": "hati/jantung", "anguli": "jari",
    "skandha": "bahu", "griva": "leher", "prshtha": "punggung", "udara": "perut",
    
    # Emosi dan Perasaan
    "sukha": "kebahagiaan", "duhkha": "kesedihan", "krodha": "kemarahan", "bhaya": "ketakutan",
    "prema": "cinta", "ghrina": "kebencian", "shanti": "kedamaian", "ananda": "kegembiraan",
    "harsha": "kesenangan", "vishada": "keputusasaan",
    
    # Makanan
    "anna": "makanan/beras", "dugdha": "susu", "ghrta": "ghee/mentega", "madhu": "madu",
    "phala": "buah", "shaka": "sayuran", "paya": "minuman", "bhojana": "makanan",
    
    # Pakaian
    "vastra": "pakaian", "uttariya": "selendang atas", "adhavastra": "pakaian bawah", "kanaka": "emas (perhiasan)",
    
    # Tempat
    "desha": "negara", "pradesha": "wilayah", "mandira": "kuil", "vidyalaya": "sekolah",
    "rajadhani": "ibukota", "patha": "jalan", "dvara": "pintu", "vaataayana": "jendela",
    
    # Konsep Abstrak
    "jnana": "pengetahuan", "vidya": "pembelajaran", "buddhi": "kecerdasan", "dharma": "kebajikan/agama",
    "karma": "perbuatan", "moksha": "pembebasan", "atma": "jiwa", "brahma": "Brahman/Yang Mutlak",
    "maya": "ilusi", "yoga": "yoga/penyatuan", "tapas": "tapa/pertapaan", "samadhi": "meditasi mendalam",
    
    # Kata Ganti
    "aham": "saya", "tvam": "kamu", "sah": "dia (laki-laki)", "sa": "dia (perempuan)",
    "tat": "itu", "idam": "ini", "vayam": "kami/kita", "yuyam": "kalian", "te": "mereka",
    
    # Preposisi dan Partikel
    "cha": "dan", "va": "atau", "na": "tidak", "saha": "dengan",
    "vina": "tanpa", "upari": "di atas", "adhah": "di bawah", "antah": "di dalam",
    "bahih": "di luar", "sarvatra": "di mana-mana",
    
    # Kata Tanya
    "kim": "apa", "kah": "siapa (laki-laki)", "ka": "siapa (perempuan)", "kutra": "di mana",
    "kada": "kapan", "katham": "bagaimana", "kimartham": "mengapa", "kiyat": "berapa banyak",
    
    # Kualitas
    "bala": "kekuatan", "shakti": "daya/energi", "vira": "keberanian", "dhairya": "kesabaran",
    "daya": "belas kasihan", "karuna": "kasih sayang", "kshama": "pengampunan", "satva": "kebaikan/kemurnian",
    
    # Aktivitas
    "yuddha": "perang", "khela": "permainan", "nrtya": "tarian", "gita": "lagu",
    "katha": "cerita", "yatra": "perjalanan", "krida": "olahraga/permainan", "karya": "pekerjaan",
    
    # Sosial
    "raja": "raja", "rani": "ratu", "sevaka": "pelayan", "mitra": "teman",
    "shatru": "musuh", "guru": "guru", "shishya": "murid", "pitamaha": "kakek", "pitamahi": "nenek",
    
    # Tambahan
    "pustaka": "buku", "patra": "surat", "dhana": "kekayaan", "dana": "pemberian",
    "yajna": "pengorbanan ritual", "mantra": "mantra", "stotra": "pujian/hymne", "shraddha": "keyakinan",
    "loka": "dunia", "svarga": "surga", "naraka": "neraka",
}

# Inisialisasi session state
if 'quiz_started' not in st.session_state:
    st.session_state.quiz_started = False
if 'quiz_questions' not in st.session_state:
    st.session_state.quiz_questions = []
if 'current_question' not in st.session_state:
    st.session_state.current_question = 0
if 'score' not in st.session_state:
    st.session_state.score = 0
if 'answered' not in st.session_state:
    st.session_state.answered = False

# Header
st.title("🕉️ Kamus Sanskerta-Indonesia")
st.markdown("---")

# Sidebar
with st.sidebar:
    st.header("📚 Menu")
    menu = st.radio(
        "Pilih Menu:",
        ["🏠 Beranda", "🔍 Cari Kata", "📖 Daftar Kata", "🎯 Quiz", "ℹ️ Info"]
    )
    
    st.markdown("---")
    st.info(f"**Total Kata:** {len(kamus_sanskerta)} kata")
    st.markdown("---")
    st.markdown("### 🙏 Namaste!")
    st.caption("Aplikasi pembelajaran bahasa Sanskerta")

# MENU BERANDA
if menu == "🏠 Beranda":
    col1, col2, col3 = st.columns([1, 2, 1])
    with col2:
        st.image("https://via.placeholder.com/400x200/FF9933/FFFFFF?text=Sanskerta", use_container_width=True)
    
    st.header("Selamat Datang di Kamus Sanskerta!")
    st.write("""
    Aplikasi ini menyediakan **200 kata Sanskerta** dengan terjemahan dalam Bahasa Indonesia.
    Sanskerta adalah bahasa kuno India yang digunakan dalam teks-teks Hindu, Buddha, dan Jain.
    """)
    
    col1, col2, col3, col4 = st.columns(4)
    with col1:
        st.metric("Total Kata", len(kamus_sanskerta))
    with col2:
        st.metric("Kategori", "15+")
    with col3:
        st.metric("Kata Kerja", "25")
    with col4:
        st.metric("Kata Sifat", "24")
    
    st.markdown("---")
    st.subheader("📚 Kategori Kata")
    
    col1, col2, col3 = st.columns(3)
    with col1:
        st.info("**Kata Benda**\nManusia, alam, tempat")
        st.info("**Hewan**\nGajah, singa, burung")
        st.info("**Waktu**\nHari, bulan, tahun")
    with col2:
        st.success("**Kata Sifat**\nBesar, kecil, indah")
        st.success("**Warna**\nMerah, biru, hijau")
        st.success("**Tubuh**\nKepala, tangan, kaki")
    with col3:
        st.warning("**Kata Kerja**\nPergi, datang, makan")
        st.warning("**Angka**\n1-1000")
        st.warning("**Emosi**\nSenang, sedih, marah")

# MENU CARI KATA
elif menu == "🔍 Cari Kata":
    st.header("🔍 Cari Kata Sanskerta")
    
    search_term = st.text_input("Masukkan kata Sanskerta (contoh: surya, prema, gaja):", "")
    
    if search_term:
        search_lower = search_term.lower().strip()
        if search_lower in kamus_sanskerta:
            st.success(f"**Kata ditemukan!** ✅")
            
            col1, col2 = st.columns(2)
            with col1:
                st.info(f"**Sanskerta:** {search_lower}")
            with col2:
                st.success(f"**Arti:** {kamus_sanskerta[search_lower]}")
        else:
            st.error("❌ Kata tidak ditemukan dalam kamus")
            st.info("💡 **Saran:** Coba kata lain seperti surya, chandra, prthvi, jala, atau agni")
            
            # Cari kata mirip
            similar = [k for k in kamus_sanskerta.keys() if search_lower in k or k in search_lower]
            if similar:
                st.write("**Kata yang mirip:**")
                for word in similar[:5]:
                    st.write(f"- {word} = {kamus_sanskerta[word]}")

# MENU DAFTAR KATA
elif menu == "📖 Daftar Kata":
    st.header("📖 Daftar Lengkap Kata Sanskerta")
    
    # Filter kategori
    filter_option = st.selectbox(
        "Filter berdasarkan:",
        ["Semua Kata", "Dimulai dengan huruf..."]
    )
    
    if filter_option == "Dimulai dengan huruf...":
        letter = st.text_input("Masukkan huruf awal (a-z):", "").lower()
        if letter:
            filtered = {k: v for k, v in kamus_sanskerta.items() if k.startswith(letter)}
            if filtered:
                df = pd.DataFrame(list(filtered.items()), columns=["Sanskerta", "Arti Indonesia"])
            else:
                st.warning(f"Tidak ada kata yang dimulai dengan '{letter}'")
                df = pd.DataFrame(columns=["Sanskerta", "Arti Indonesia"])
        else:
            df = pd.DataFrame(list(kamus_sanskerta.items()), columns=["Sanskerta", "Arti Indonesia"])
    else:
        df = pd.DataFrame(list(kamus_sanskerta.items()), columns=["Sanskerta", "Arti Indonesia"])
    
    # Tambahkan nomor
    df.insert(0, "No", range(1, len(df) + 1))
    
    # Tampilkan tabel
    st.dataframe(df, use_container_width=True, height=600)
    
    # Tombol download
    csv = df.to_csv(index=False).encode('utf-8')
    st.download_button(
        label="📥 Download CSV",
        data=csv,
        file_name="kamus_sanskerta.csv",
        mime="text/csv"
    )

# MENU QUIZ
elif menu == "🎯 Quiz":
    st.header("🎯 Quiz Kamus Sanskerta")
    
    if not st.session_state.quiz_started:
        st.write("Uji pengetahuan Anda tentang kosakata Sanskerta!")
        
        num_questions = st.slider("Jumlah soal:", 5, 20, 10)
        
        if st.button("🚀 Mulai Quiz", type="primary"):
            # Siapkan soal
            all_words = list(kamus_sanskerta.items())
            random.shuffle(all_words)
            st.session_state.quiz_questions = all_words[:num_questions]
            st.session_state.current_question = 0
            st.session_state.score = 0
            st.session_state.quiz_started = True
            st.session_state.answered = False
            st.rerun()
    
    else:
        # Quiz sedang berlangsung
        current = st.session_state.current_question
        total = len(st.session_state.quiz_questions)
        
        if current < total:
            question = st.session_state.quiz_questions[current]
            sanskerta, answer = question
            
            st.progress((current) / total)
            st.subheader(f"Soal {current + 1} dari {total}")
            
            st.info(f"### Apa arti dari: **{sanskerta}** ?")
            
            user_answer = st.text_input("Jawaban Anda:", key=f"answer_{current}")
            
            col1, col2 = st.columns(2)
            
            with col1:
                if st.button("✅ Submit Jawaban", type="primary"):
                    if user_answer.strip().lower() == answer.lower():
                        st.success("✅ BENAR!")
                        st.session_state.score += 1
                    else:
                        st.error(f"❌ SALAH! Jawaban yang benar: **{answer}**")
                    st.session_state.answered = True
            
            with col2:
                if st.session_state.answered:
                    if st.button("➡️ Soal Berikutnya"):
                        st.session_state.current_question += 1
                        st.session_state.answered = False
                        st.rerun()
            
            st.markdown("---")
            st.write(f"**Skor saat ini:** {st.session_state.score}/{current + (1 if st.session_state.answered else 0)}")
        
        else:
            # Quiz selesai
            st.balloons()
            st.success("🎉 Quiz Selesai!")
            
            score = st.session_state.score
            total = len(st.session_state.quiz_questions)
            percentage = (score / total) * 100
            
            col1, col2, col3 = st.columns(3)
            with col1:
                st.metric("Skor Anda", f"{score}/{total}")
            with col2:
                st.metric("Persentase", f"{percentage:.1f}%")
            with col3:
                if percentage >= 80:
                    st.metric("Grade", "A 🌟")
                elif percentage >= 60:
                    st.metric("Grade", "B 👍")
                else:
                    st.metric("Grade", "C 💪")
            
            if st.button("🔄 Quiz Lagi", type="primary"):
                st.session_state.quiz_started = False
                st.rerun()

# MENU INFO
elif menu == "ℹ️ Info":
    st.header("ℹ️ Tentang Kamus Sanskerta")
    
    st.write("""
    ### Tentang Aplikasi
    Aplikasi Kamus Sanskerta-Indonesia ini dibuat untuk membantu pembelajaran bahasa Sanskerta,
    bahasa kuno yang memiliki pengaruh besar terhadap bahasa-bahasa di Asia Tenggara.
    
    ### Fitur Aplikasi
    - 🔍 **Pencarian Kata**: Cari arti kata dengan mudah
    - 📖 **Daftar Lengkap**: Lihat semua 200 kata dalam tabel
    - 🎯 **Quiz Interaktif**: Uji pengetahuan Anda
    - 📥 **Download CSV**: Ekspor kamus ke file CSV
    
    ### Tentang Bahasa Sanskerta
    Sanskerta (संस्कृतम्) adalah bahasa Indo-Arya kuno yang berasal dari India.
    Bahasa ini digunakan dalam teks-teks suci Hindu, Buddha, dan Jain, serta literatur klasik India.
    
    ### Kategori Kata (200 kata)
    - **Kata Benda**: Manusia, alam, tempat (30 kata)
    - **Hewan**: Berbagai jenis hewan (20 kata)
    - **Kata Sifat**: Sifat dan kualitas (24 kata)
    - **Kata Kerja**: Aksi dan aktivitas (25 kata)
    - **Angka**: 1 hingga 1000 (16 kata)
    - **Waktu**: Hari, bulan, tahun (13 kata)
    - **Warna**: Berbagai warna (8 kata)
    - **Tubuh**: Anggota tubuh (16 kata)
    - **Emosi**: Perasaan (10 kata)
    - Dan kategori lainnya...
    
    ### Transliterasi
    Aplikasi ini menggunakan sistem transliterasi Latin (romanisasi) untuk memudahkan
    pembacaan kata-kata Sanskerta bagi pemula.
    
    ### 🙏 Namaste
    Terima kasih telah menggunakan aplikasi ini. Semoga bermanfaat untuk pembelajaran Anda!
    """)
    
    st.markdown("---")
    st.caption("Dikembangkan dengan ❤️ menggunakan Streamlit")

# Footer
st.markdown("---")
st.caption("© 2024 Kamus Sanskerta-Indonesia | 🕉️ Namaste")
