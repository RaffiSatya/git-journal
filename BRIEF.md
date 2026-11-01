# Project 02 — `git-journal`

**Durasi:** 2–3 hari · **Fase:** 0 (Fondasi Tools)

## 🎯 Tujuan
Setelah project ini kamu bisa:
1. Menjelaskan apa itu Git, repository, commit, dan kenapa developer tidak bisa hidup tanpanya.
2. Menjalankan siklus harian Git: **ubah → `add` → `commit`**.
3. Membuat **branch**, **merge**, dan menyelesaikan **merge conflict**.
4. Push repo ke **GitHub** sehingga bisa dilihat orang lain (dan calon employer).

**Hasil akhir:** 2 repo publik di GitHub:
- `git-journal` → jurnal belajarmu, di-update di setiap project sampai project 50.
- `hello-terminal` → Project 01 kamu.

---

## 📚 Bagian 1 — Konsep

### 1.1 Git itu apa?
Git = **mesin waktu untuk folder project**. Setiap kali kamu "menyimpan titik" (commit), Git mencatat isi semua file pada saat itu. Kamu bisa:
- melihat apa yang berubah, kapan, dan oleh siapa,
- kembali ke versi kemarin kalau hari ini rusak,
- bekerja paralel di beberapa "jalur" (branch) tanpa saling ganggu.

Analogi GIS: mirip **versioning di geodatabase** — ada versi default, kamu edit di versi sendiri, lalu reconcile & post (= merge).

Bandingkan dengan cara manual:
```
peta_final.qgz
peta_final_revisi.qgz
peta_final_revisi2_FIX.qgz
peta_final_revisi2_FIX_beneran.qgz   ← tanpa Git 😅
```

### 1.2 Tiga "area" Git (paling penting!)

```
 Working Directory        Staging Area          Repository (.git)
 (file yang kamu edit) →  (siap di-commit)  →   (riwayat permanen)
                git add               git commit
```

- **Working directory:** file biasa di folder kamu.
- **Staging area:** "keranjang" berisi perubahan yang mau masuk commit berikutnya. Kenapa ada? Supaya kamu bisa memilih — misalnya ubah 5 file tapi commit 2 dulu.
- **Repository:** semua commit tersimpan di folder tersembunyi `.git/`. **Hapus folder `.git` = hapus seluruh riwayat.**

### 1.3 Commit
Satu commit = satu snapshot + pesan + penulis + waktu + ID unik (hash, mis. `a3f9c21`).

Pesan commit yang baik menjelaskan **apa dan kenapa**, dalam bentuk perintah:
- ✅ `Add LiDAR and QGIS categories to organizer`
- ❌ `update`, `fix`, `asdf`, `revisi lagi`

### 1.4 Branch & merge
**Branch** = jalur kerja paralel. Branch utama biasanya bernama `main`.

```
main:        A ─── B ─────────── M      ← M = merge commit
                    \           /
glossary:            C ─── D ──┘
```

Kerjakan fitur baru di branch sendiri → kalau sudah beres, **merge** ke `main`. Kalau gagal, branch-nya dibuang saja, `main` tetap aman.

**Merge conflict** terjadi kalau **baris yang sama** diubah berbeda di dua branch. Git tidak bisa menebak mana yang benar, jadi kamu yang memutuskan.

### 1.5 Git vs GitHub
| Git | GitHub |
|---|---|
| Program di laptop kamu | Website / server di internet |
| Mencatat riwayat | Menyimpan salinan repo (**remote**) + kolaborasi + portofolio |
| Bisa jalan offline | Butuh internet |

- `git push` = kirim commit dari laptop → GitHub
- `git pull` = ambil commit dari GitHub → laptop
- `origin` = nama default untuk alamat repo di GitHub

### 1.6 `.gitignore`
Daftar file yang **tidak boleh** masuk Git: `.venv/` (besar, bisa dibuat ulang), file cache, data besar, dan **file rahasia** (password, API key — nanti di fase deploy ini sangat penting). Lihat contoh di `01-hello-terminal/.gitignore`.

### 1.7 Markdown
Format teks sederhana yang dipakai README, jurnal, dan dokumentasi. GitHub menampilkannya dengan rapi.
```markdown
# Judul besar        ## Sub judul
**tebal**  *miring*  `kode`
- list item
[teks link](https://contoh.com)
```

### ⚠️ Satu aturan struktur
Setiap project = **repo sendiri**. Jangan jalankan `git init` di folder induk `E:\03_Raffi's Project` — nanti semua project tercampur jadi satu repo.

---

## 🛠️ Bagian 2 — Praktik (kamu yang ketik)

### Langkah 0 — Setup sekali seumur laptop
Identitas Git kamu sudah diatur (`RaffiSatya`). Tambahkan dua pengaturan ini:
```powershell
git config --global init.defaultBranch main
git config --global core.editor "code --wait"
git config --global --list
```
- Baris 1: branch utama bernama `main` (standar GitHub). Default laptopmu masih `master`.
- Baris 2: kalau Git butuh kamu menulis pesan, ia membuka VS Code. Tanpa ini Git membuka **Vim**, editor yang terkenal susah ditutup (`:q!` kalau terjebak 😄).

Pastikan juga kamu punya akun di **github.com** dan tahu username-nya. Di BRIEF ini aku pakai `RaffiSatya` — ganti kalau username GitHub kamu berbeda.

### Langkah 1 — Repo pertama
```powershell
cd "E:\03_Raffi's Project\02-git-journal"
git init
git status
```
`git status` = perintah yang paling sering kamu ketik. Ia menjelaskan kondisi sekarang. Semua file masih **untracked** (merah).

### Langkah 2 — Commit pertama
```powershell
git add README.md
git status                     # README hijau (staged), sisanya masih merah
git add .                      # . = semua file di folder ini
git status
git commit -m "Initial journal structure with Project 01 entry"
git log
git log --oneline
```
> Kalau muncul `warning: ... LF will be replaced by CRLF`, abaikan saja. Itu soal perbedaan karakter akhir baris antara Windows (CRLF) dan Linux/Mac (LF), dan Git menanganinya otomatis.

### Langkah 3 — Lihat perubahan
Buka `entries/2026-10-27-project-01.md` di VS Code, isi bagian **"Masih bingung"** dengan 1–2 poin milikmu sendiri. Simpan, lalu:
```powershell
git status
git diff                       # baris + hijau = ditambah, - merah = dihapus
git add .
git commit -m "Add open questions to Project 01 entry"
git log --oneline
```

### Langkah 4 — Branch & merge
```powershell
git branch                     # lihat branch (* = posisi kamu)
git switch -c add-glossary     # buat branch baru & pindah ke sana
```
Buat file baru `glossary.md` dengan isi:
```markdown
# Glosarium

| Istilah | Arti |
|---|---|
| Repository | Folder project yang riwayatnya dicatat Git |
| Commit | Snapshot project pada satu titik waktu |
| Branch | Jalur kerja paralel |
| venv | Python terisolasi khusus satu project |
```
```powershell
git add glossary.md
git commit -m "Add glossary"
ls                             # glossary.md ada
git switch main
ls                             # glossary.md HILANG? (tenang, lihat kuis)
git merge add-glossary
ls                             # muncul lagi
git branch -d add-glossary     # hapus branch yang sudah di-merge
git log --oneline --graph
```

### Langkah 5 — Merge conflict (sengaja!)
```powershell
git switch -c ganti-judul
```
Di `README.md`, ubah baris pertama jadi `# 📓 Jurnal Belajar Raffi`, simpan.
```powershell
git commit -am "Rename journal title"
git switch main
```
Di `README.md` (versi main), ubah baris pertama jadi `# 📓 Learning Journal`, simpan.
```powershell
git commit -am "Translate journal title"
git merge ganti-judul          # 💥 CONFLICT
git status
```
Buka `README.md` di VS Code. Kamu akan lihat:
```
<<<<<<< HEAD
# 📓 Learning Journal
=======
# 📓 Jurnal Belajar Raffi
>>>>>>> ganti-judul
```
VS Code menampilkan tombol **Accept Current / Accept Incoming / Accept Both**. Pilih salah satu (atau tulis judul baru), pastikan tanda `<<<<<<<`, `=======`, `>>>>>>>` hilang semua, simpan, lalu:
```powershell
git add README.md
git commit -m "Merge ganti-judul, resolve title conflict"
git branch -d ganti-judul
git log --oneline --graph
```

### Langkah 6 — Push ke GitHub
1. Buka github.com → tombol **New repository**.
2. Nama: `git-journal`, pilih **Public**, **JANGAN** centang "Add a README" (repo harus kosong, karena isinya sudah ada di laptop).
3. Klik **Create repository**, lalu:
```powershell
git remote add origin https://github.com/RaffiSatya/git-journal.git
git remote -v
git push -u origin main
```
Saat push pertama, akan muncul jendela login GitHub (Git Credential Manager). Login lewat browser — **jangan** ketik password di terminal. Setelah itu login tersimpan.

`-u` artinya "ingat bahwa `main` di laptop terhubung dengan `main` di origin", jadi push berikutnya cukup `git push`.

Refresh halaman GitHub — repo kamu sudah online! 🎉

### Langkah 7 — Ulangi untuk Project 01 (latihan mandiri)
Ini pengulangan supaya otot tangan hafal:
```powershell
cd "..\01-hello-terminal"
git init
git status                     # .venv dan sample_data TIDAK muncul — berkat .gitignore
git add .
git commit -m "Add GIS folder organizer CLI"
```
Buat repo `hello-terminal` di GitHub (kosong, Public), lalu `git remote add origin ...` dan `git push -u origin main` seperti Langkah 6.

---

## 🔍 Bagian 3 — Contekan Perintah

| Perintah | Fungsi |
|---|---|
| `git init` | Jadikan folder sebagai repo (sekali per project) |
| `git status` | Kondisi sekarang — ketik sesering mungkin |
| `git add <file>` / `git add .` | Masukkan perubahan ke staging |
| `git commit -m "pesan"` | Simpan snapshot |
| `git commit -am "pesan"` | add + commit sekaligus (hanya file yang **sudah** di-track) |
| `git diff` | Lihat perubahan yang belum di-stage |
| `git log --oneline --graph` | Riwayat commit ringkas |
| `git switch -c <nama>` | Buat branch baru & pindah |
| `git switch <nama>` | Pindah branch |
| `git merge <nama>` | Gabungkan branch ke branch saat ini |
| `git branch -d <nama>` | Hapus branch (yang sudah di-merge) |
| `git remote add origin <url>` | Hubungkan ke GitHub |
| `git push` / `git pull` | Kirim / ambil commit |
| `git restore <file>` | Buang perubahan yang belum di-commit (**hati-hati**, tidak bisa di-undo) |

---

## ❓ Bagian 4 — Kuis

1. Kamu edit 3 file, lalu `git add a.py`, lalu `git commit -m "..."`. File mana yang masuk commit? Bagaimana status 2 file lainnya?
2. Di Langkah 4, `glossary.md` "hilang" setelah `git switch main`. Apakah file-nya terhapus? Jelaskan apa yang sebenarnya terjadi.
3. Apa beda `git commit` dan `git push`? Kalau laptopmu hilang setelah commit tapi sebelum push, apa yang hilang?
4. Kenapa `.venv/` harus masuk `.gitignore`? Sebutkan 2 alasan.
5. Temanmu tidak sengaja commit file `.env` berisi password database, lalu push. Dia langsung menghapus file itu dan commit lagi. Apakah password-nya sudah aman? Kenapa?
6. Kenapa repo di GitHub harus kosong (tanpa README) sebelum push pertama dari laptop?
7. **Tebak:** kamu menjalankan `git init` di `E:\03_Raffi's Project` (folder induk). Apa masalahnya?
8. Pilih pesan commit terbaik dan jelaskan: (a) `update file` (b) `Fix crash when organizing folder twice` (c) `fixed bug yg kemaren`

### 🐛 Cari Bug
Seseorang menjalankan perintah ini dan bingung kenapa GitHub masih kosong:
```powershell
git init
git add .
git remote add origin https://github.com/RaffiSatya/project-x.git
git push -u origin main
```
Error: `src refspec main does not match any`. Apa yang terlupa?

---

## ✍️ Bagian 5 — Modifikasi Kecil (bagian kamu)
1. Tambahkan **3 istilah baru** di `glossary.md` (mis. *staging area*, *remote*, *merge conflict*) — lakukan di **branch baru** bernama `glossary-v2`, lalu merge ke main.
2. Push ke GitHub dan pastikan glossary terbaru tampil di sana.

## 🗣️ Bagian 6 — Explain-back
> "Jelaskan perjalanan sebuah perubahan dari saat aku mengetik di VS Code sampai muncul di GitHub."

## ✅ Kriteria Selesai
- [ ] Repo `git-journal` dan `hello-terminal` online di GitHub
- [ ] Riwayat `git-journal` menunjukkan minimal 1 merge (cek: `git log --oneline --graph`)
- [ ] Kuis + Cari Bug dijawab
- [ ] Modifikasi `glossary-v2` selesai & ter-push
- [ ] Explain-back dikirim

Kirim link kedua repo GitHub kamu di chat, aku akan review isinya.

Selanjutnya → **Project 03 `coord-converter-cli`**: Pull Request pertama, CLI yang lebih serius, dan unit test.
