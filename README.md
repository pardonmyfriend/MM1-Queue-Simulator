# Symulacja routera w modelach kolejkowych M/M/1 i M/M/1/N

## 📌 Opis projektu
Projekt zrealizowany w ramach przedmiotu **Statystyka i Teoria Obsługi Masowej**.  
Celem jest **zamodelowanie routera** jako systemu kolejkowego oraz **symulacyjne porównanie** dwóch modeli:
- **M/M/1** – jeden serwer, nieskończony bufor,
- **M/M/1/N** – jeden serwer, **skończony bufor** o pojemności *N* (możliwe odrzucenia pakietów).

W projekcie implementuję własny **symulator zdarzeniowy** (event-driven), a następnie porównuję metryki empiryczne z wartościami **teoretycznymi** i badam wpływ obciążenia oraz rozmiaru bufora na zachowanie systemu.

---

## 🛠 Implementacja
1. **Klasy i struktury:**
   - `class Event` – reprezentacja zdarzeń *(ARRIVAL/DEPARTURE)*, kolejność czasowa realizowana przez **`heapq`** (kolejka priorytetowa).
   - `class MM1QueueSimulator` – symulator **M/M/1**:
     - planowanie kolejnych przyjść i odejść,
     - obsługa serwera i kolejki, akumulacja statystyk w czasie.
   - `class MM1NQueueSimulator(MM1QueueSimulator)` – symulator **M/M/1/N**:
     - dziedziczy logikę M/M/1,
     - dodatkowo kontrola rozmiaru bufora i **zliczanie odrzuceń**.
2. **Wejście symulatora:** `arrival_rate (λ)`, `service_rate (μ)`, `max_time`, oraz dla M/M/1/N: `buffer_size = N`.
3. **Zbierane statystyki (empiryczne):**
   - **średnia długość kolejki/systemu**,
   - dla M/M/1/N – **prawdopodobieństwo odrzuceń**.

---

## 🔬 Plan eksperymentów i powtarzalność
- **Dobór liczby powtórzeń**: funkcja `estimate_se_for_model(...)` szacuje **błąd standardowy (SE)** metryki (np. \(E(L)\)) w funkcji liczby uruchomień i pozwala ustalić minimalną liczbę powtórzeń zapewniającą stabilny wynik.
- **Porównania modeli**: `compare_models_with_repeats(rho_values, buffer_sizes, sim_time, n_runs)` - siatka eksperymentów po **ρ** i **N**, powtarzana **n\_runs** razy; wyniki agregowane i wizualizowane.

---

## 📈 Wyniki
- Dla **M/M/1** przy ρ < 1 metryki stabilizują się blisko wartości teoretycznych; przy ρ dążącym do 1 rosną gwałtownie.
- Dla **M/M/1/N** mniejszy bufor - **większe prawdopodobieństwo straty**; rośnie też średnie opóźnienie w warunkach dużego obciążenia.

---

## 🚀 Uruchamianie
```bash
git clone https://github.com/<twoja-nazwa-uzytkownika>/SiTOM-Queue.git
cd SiTOM-Queue

python -m venv .venv
# Linux/macOS
source .venv/bin/activate
# Windows
.venv\Scripts\activate

pip install -r requirements.txt
jupyter notebook SiTOM_queue.ipynb
```

---

## 📦 Wymagania
Plik `requirements.txt` zawiera minimalny zestaw bibliotek użytych w notatniku:
- `numpy`
- `matplotlib`
- `jupyter`
