from pydub import AudioSegment

# Carica la voce creata dall'AI e la base musicale strumentale
voce_ai = AudioSegment.from_wav("lettera22_tiziano_vocal.wav")
base_musicale = AudioSegment.from_file("lettera22_strumentale.wav")

# Mixa le tracce sovrapponendole
canzone_completa = base_musicale.overlay(voce_ai, position=0)

# Esporta il file finale
canzone_completa.export("Lettera_22_Tiziano_Leonardi_RVC.mp3", format="mp3")
print("Traccia finale mixata ed esportata!")
