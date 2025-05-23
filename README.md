# codeprogramintensitas
Algoritma Fuzzy

# Install scikit-fuzzy jika belum terpasang
# !pip install scikit-fuzzy

import numpy as np
import skfuzzy as fuzz
from skfuzzy import control as ctrl
import matplotlib.pyplot as plt

# 1. Definisi variabel linguistik
cahaya = ctrl.Antecedent(np.arange(0, 1001, 1), 'cahaya')  # Tingkat cahaya lingkungan (lux)
lampu = ctrl.Consequent(np.arange(0, 101, 1), 'lampu')     # Tingkat kecerahan lampu (% PWM)

# 2. Fungsi keanggotaan untuk input cahaya
cahaya['gelap'] = fuzz.trimf(cahaya.universe, [0, 0, 300])
cahaya['redup'] = fuzz.trimf(cahaya.universe, [200, 500, 700])
cahaya['terang'] = fuzz.trimf(cahaya.universe, [600, 1000, 1000])

# Fungsi keanggotaan untuk output lampu
lampu['rendah'] = fuzz.trimf(lampu.universe, [0, 0, 40])
lampu['sedang'] = fuzz.trimf(lampu.universe, [30, 50, 70])
lampu['tinggi'] = fuzz.trimf(lampu.universe, [60, 100, 100])

# 3. Aturan fuzzy
rule1 = ctrl.Rule(cahaya['gelap'], lampu['tinggi'])
rule2 = ctrl.Rule(cahaya['redup'], lampu['sedang'])
rule3 = ctrl.Rule(cahaya['terang'], lampu['rendah'])

# 4. Sistem kontrol fuzzy
sistem_lampu_ctrl = ctrl.ControlSystem([rule1, rule2, rule3])
sistem_lampu = ctrl.ControlSystemSimulation(sistem_lampu_ctrl)

# 5. Simulasi dengan input contoh
input_cahaya = 250  # contoh input cahaya dalam lux
sistem_lampu.input['cahaya'] = input_cahaya
sistem_lampu.compute()

# 6. Output hasil defuzzifikasi
print(f"Tingkat kecerahan lampu untuk cahaya {input_cahaya} lux: {sistem_lampu.output['lampu']:.2f}%")

# Visualisasi fungsi keanggotaan output dengan hasil simulasi
lampu.view(sim=sistem_lampu)
plt.show()
