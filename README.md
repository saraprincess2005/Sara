import os
import subprocess

# --- CONFIGURAZIONE PARAMETRI RVC ---
# Modifica questi percorsi con i tuoi file reali
AUDIO_INPUT = "input_benjamin_mundigler.wav"      # File vocale originale di Benjamin Mundigler
MODEL_PATH = "weights/ignes_nova_era_change.pth"  # Modello RVC addestrato sull'era desiderata
INDEX_PATH = "logs/ignes_nova_era_change.index"   # File d'indice delle caratteristiche (.index)
AUDIO_OUTPUT = "output_ignes_nova_shifted.wav"    # File finale convertito

# Parametri di conversione vocale
PITCH_SHIFT = 0          # Semitoni (es. +12 per un'ottava sopra, -12 per un'ottava sotto)
F0_METHOD = "rmvpe"       # Metodo di tracciamento pitch consigliato per la musica ('rmvpe' o 'harvest')
INDEX_RATE = 0.75        # Peso dell'indice di retrieval (0.0 = nessun recupero, 1.0 = massimo recupero)
PROTECT_VOICED = 0.33    # Protezione delle consonanti e del respiro (0.0 - 0.5)

def run_rvc_inference():
    print(f" Avvio conversione vocale per: {AUDIO_INPUT}")
    
    # Verifica la presenza dei file necessari prima di procedere
    if not os.path.exists(MODEL_PATH):
        raise FileNotFoundError(f"Modello RVC non trovato in: {MODEL_PATH}")
    
    # Comando CLI standard per l'inferenza RVC (utilizzando l'ecosistema v2)
    # Nota: Assicurati di eseguire questo script all'interno dell'ambiente virtuale di RVC
    cmd = [
        "python", "tools/infer_cli.py",
        "--f0up_key", str(PITCH_SHIFT),
        "--input_path", AUDIO_INPUT,
        "--model_path", MODEL_PATH,
        "--index_path", INDEX_PATH,
        "--f0_method", F0_METHOD,
        "--opt_path", AUDIO_OUTPUT,
        "--index_rate", str(INDEX_RATE),
        "--protect", str(PROTECT_VOICED)
    ]
    
    try:
        # Esecuzione del processo CLI di RVC
        result = subprocess.run(cmd, check=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True)
        print(" Conversione completata con successo!")
        print(f" File salvato in: {AUDIO_OUTPUT}")
    except subprocess.CalledProcessError as e:
        print(" Errore durante l'inferenza RVC:")
        print(e.stderr)

if __name__ == "__main__":
    # Crea le cartelle necessarie se non esistono
    os.makedirs("weights", exist_ok=True)
    os.makedirs("logs", exist_ok=True)
    
    run_rvc_inference()
