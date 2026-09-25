# tugas-gamedev-pt1
Tugas Game Development Pt 1

# Tugas Algoritma Pathfinding & AI Enemy dalam Dungeon

## Soal / Studi Kasus
Player bergerak di dalam sebuah dungeon. Enemy harus mendeteksi player, menentukan apakah player berada dalam jangkauan, mencari jalur menuju player, kemudian bergerak menuju player.

---

## 1. Identifikasi Algoritma yang Digunakan

Untuk menyelesaikan permasalahan ini, digunakan kombinasi beberapa algoritma:

1. Algoritma Deteksi Jangkauan (Distance Calculation):
   - Euclidean Distance atau Manhattan Distance: Digunakan untuk menghitung jarak antara posisi Enemy $(x_1, y_1)$ dan posisi Player $(x_2, y_2)$ guna menentukan apakah Player berada dalam jangkauan deteksi/serang (*Detection Range*).

2. Algoritma Pencarian Jalur (Pathfinding Algorithm):
   - Algoritma A (A-Star): Digunakan untuk mencari rute atau jalur terpendek dari posisi Enemy menuju Player di dalam dungeon bertipe grid/tile-based dengan memperhitungkan rintangan (obstacles/walls).
   - (Alternatif sederhana): Breadth-First Search (BFS) jika graf tidak berbobot.

3. Algoritma Pengambil Keputusan (State/Behavior Machine):
   - Finite State Machine (FSM): Mengatur status Enemy (misal: `PATROL` / `IDLE` -> `CHASE` ketika player terdeteksi -> `ATTACK` ketika player dalam jangkauan serang).

---

## 2. Flowchart Algoritma

```text
[Mulai]
   │
   ▼
[Dapatkan Posisi Enemy & Posisi Player]
   │
   ▼
[Hitung Jarak (Distance = |Enemy_X - Player_X| + |Enemy_Y - Player_Y|)]
   │
   ▼
/ Apakah Jarak <= Jangkauan Deteksi? \
\             (Detection Range)       /
   ├─── Tidak ───► [Enemy Tetap Diam / Patroli] ───► [Selesai]
   │
  Ya
   │
   ▼
[Cari Jalur Terpendek Menggunakan Algoritma A* (Pathfinding)]
   │
   ▼
/ Apakah Jalur Ditemukan? \
\                        /
   ├─── Tidak ───► [Enemy Tetap Diam / Mencari Jalur Lain] ───► [Selesai]
   │
  Ya
   │
   ▼
[Gerakkan Enemy Satu Langkah Mengikuti Jalur A*]
   │
   ▼
/ Apakah Player Berada dalam Jangkauan Serang? \
\               (Attack Range)                  /
   ├─── Ya ──────► [Lakukan Serangan ke Player] ───► [Selesai]
   │
 Mendarat / Tidak
   │
   ▼
[Selesai]
```


## 3. Code Snippet (Bahasa Pemrograman: Python)

Berikut adalah contoh kode Python untuk deteksi dan pergerakan enemy:

```python
import math

class Enemy:
    def __init__(self, x, y, detection_range, attack_range):
        self.x = x
        self.y = y
        self.detection_range = detection_range
        self.attack_range = attack_range

    def calculate_distance(self, player_x, player_y):
        return math.sqrt((self.x - player_x)**2 + (self.y - player_y)**2)

    def update(self, player_x, player_y):
        distance = self.calculate_distance(player_x, player_y)
        
        if distance <= self.detection_range:
            if distance <= self.attack_range:
                print("Enemy menyerang Player!")
            else:
                print("Enemy bergerak mendekati Player...")
                self.move_towards(player_x, player_y)
        else:
            print("Enemy patroli / diam.")

    def move_towards(self, target_x, target_y):
        if self.x < target_x: self.x += 1
        elif self.x > target_x: self.x -= 1
        if self.y < target_y: self.y += 1
        elif self.y > target_y: self.y -= 1
```
