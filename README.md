# Render Video dengan GitHub Actions (ffmpeg) — Gratis

Repo ini merender video Shorts dari klip + voiceover langsung di GitHub Actions. Tidak memakai kredit Flow atau layanan render berbayar.

## Cara pakai (mode real)

1. Unggah klip video (`.mp4`, masing-masing di bawah 100 MB) ke folder `clips/`, dengan nama berurutan supaya urutannya benar, misalnya: `scene01.mp4`, `scene02.mp4`, `scene03.mp4`, ...
2. Unggah voiceover (`.mp3`) ke `voiceover/voiceover.mp3` (opsional — kalau tidak ada, audio asli klip yang dipakai).
3. Push ke branch `main`. Workflow **Render Video** jalan otomatis. Atau jalankan manual dari tab **Actions → Render Video → Run workflow**.
4. Kalau selesai, unduh hasilnya di tab Actions → run tersebut → bagian **Artifacts → video-final**. Hasil berupa satu file `.mp4` 1080x1920, 24 FPS.

Unggah file bisa lewat web GitHub (drag & drop di folder `clips/`), masing-masing file maksimal 100 MB. Untuk file yang lebih besar, pakai Git LFS (belum disiapkan di repo ini).

## Mode test

Workflow punya mode `test` (bawaan saat pertama kali di-push): klip contoh dan voiceover contoh dibuat otomatis di dalam runner dengan ffmpeg (`testsrc2` + nada `sine`), lalu dirender. Mode ini untuk membuktikan pipeline jalan tanpa harus mengunggah file apa pun.

Jalankan manual: **Actions → Render Video → Run workflow → mode: test**.

## Yang dilakukan workflow

- `ffmpeg -f concat` menggabungkan semua `clips/*.mp4` sesuai urutan nama file.
- Video di-scale ke **1080x1920**, **24 FPS**, codec `libx264` + `yuv420p`.
- Kalau ada `voiceover/voiceover.mp3`, audio klip diganti voiceover (`-shortest`, jadi durasi mengikuti yang lebih pendek).
- Hasil dicek dengan `ffprobe`, lalu diunggah sebagai artifact (tersimpan 7 hari).

## Batasan gratis GitHub Actions

- Repo **publik**: menit Actions gratis tidak terbatas.
- Repo privat: 2.000 menit/bulan gratis.
- Artifact hasil render tersimpan 7 hari di repo ini — unduh sebelum habis.
- Runner `ubuntu-latest` sudah menyediakan `ffmpeg`, tidak perlu install.

## Struktur repo

```text
.github/workflows/render.yml   # workflow render
clips/                          # taruh klip .mp4 di sini (scene01.mp4, ...)
voiceover/                      # taruh voiceover.mp3 di sini (opsional)
```
