# Q_Learning_Train

# Batch-aware Q-Learning Server with Telemetry Logging

Sebuah server Q-Learning berbasis batch yang menerima data telemetry dari klien (seperti MATLAB), melakukan pembelajaran penguatan (reinforcement learning) secara online, dan mengembalikan rekomendasi pengaturan parameter.

---

## Fitur

- Menerima batch data kendaraan lewat TCP socket (port 5000 default) dalam format JSON.
- Menerima nilai *transmission power*, *beacon rate*, *channel busy ratio (CBR)*, *neighbors*, dan *SNR* dari klien.
- Melakukan Q-Learning untuk mengoptimasi parameter *power* dan *beacon rate* demi mencapai target CBR.
- Mengembalikan nilai *transmissionPower*, *beaconRate*, dan *MCS* hasil pembelajaran ke klien.
- Logging lengkap ke terminal dan file (`train.log`).
- Menyimpan data telemetry dan reward per kendaraan ke file CSV (`metrics.csv`).
- Menyimpan dan memuat Q-table ke/dari file `q_table.npy`.
- Komunikasi menggunakan protokol dengan header 4-byte panjang pesan.

---

## Cara Pakai

1. Jalankan server:

```bash
python train.py

Hubungkan klien (misal MATLAB) ke alamat IP server dan port 5000.

Kirim data batch JSON dengan format berikut:
`````````

{
  "veh_id1": {
    "power": 15.0,
    "beacon": 10,
    "cbr": 0.72,
    "neighbors": 3,
    "snr": 18.2
  },
  "veh_id2": {
    "transmissionPower": 10.0,
    "beaconRate": 5,
    "CBR": 0.58,
    "neigh": 1,
    "SNR": 24.5
  }
}

    Terima response JSON berisi pengaturan baru:

{
  "veh_id1": {
    "transmissionPower": 16,
    "beaconRate": 11,
    "MCS": 5
  },
  "veh_id2": {
    "transmissionPower": 9,
    "beaconRate": 4,
    "MCS": 6
  }
}

Struktur File

    ```bash
    train.py — kode server Q-Learning
    `````

    ```bash
    train.log — file log aktivitas server
    `````

    ```bash
    metrics.csv — data telemetry dan reward per kendaraan
    ````

    ```bash
    q_table.npy — file penyimpanan Q-table (otomatis dibuat dan digunakan)
    ```````

Konfigurasi

    HOST dan PORT di train.py untuk mengatur alamat dan port server.

    Parameter RL seperti learning rate, discount factor, epsilon (exploration rate), dan target CBR bisa diubah di bagian awal script.

    Discretization bins dan action space juga dapat disesuaikan sesuai kebutuhan.

Dependencies

    Python 3.7+

    Numpy

Catatan

    Server mengandalkan protokol TCP dengan 4-byte header panjang pesan.

    Pastikan klien mengikuti protokol yang sama agar komunikasi berjalan lancar.

    Server menyimpan Q-table secara otomatis saat server dihentikan (misal dengan Ctrl+C).
